# Two-Host Audio Overview: Carmack on HBM vs Flash — Spoken Draft

A: Okay, so I want to unpack this John Carmack post, because it sounds very chip-industry-specific, but the idea is actually pretty intuitive.

B: Yeah. The simple version is: AI chips are not just limited by how much math they can do. They’re also limited by how fast they can get the model data they need.

A: So this is a memory story.

B: Exactly. A big AI model is mostly a huge pile of numbers. Those numbers are called weights. When the model answers you, the chip is constantly reading those weights and doing math with them.

A: And if the chip can do the math, but the weights arrive too slowly...

B: Then the chip waits. And that’s painful, because these accelerators are extremely expensive. You don’t want them sitting around waiting for data.

A: Carmack is comparing HBM and flash. Let’s make those concrete.

B: HBM stands for High Bandwidth Memory. It’s the very fast memory that sits close to the GPU or AI accelerator. It’s one reason modern AI chips are so useful: they can pull in a huge amount of data very quickly.

A: So HBM is the premium stuff.

B: Exactly. Fast, close to the chip, very high bandwidth. But also expensive, and limited in capacity.

A: And flash is what we have in SSDs and phones?

B: Right. NAND flash. It’s much cheaper per gigabyte and you can have a lot more of it. But it’s slower, especially if you ask it for little pieces of data from random places.

A: The analogy that helped me was kitchen counter versus pantry.

B: That’s the right one. HBM is the counter next to the chef. It’s where you put ingredients the chef needs immediately. Flash is the big pantry. It holds a lot, and it’s cheap, but it takes longer to get things from there.

A: So Carmack’s question is: do all the model weights need to be on the counter?

B: Exactly. Or can some of them stay in the pantry, as long as we bring them out just before the chef needs them?

A: And this is specifically about inference, right?

B: Yes. Inference means using a trained model. You ask a question, it generates an answer. During inference, the weights mostly don’t change. They’re being read.

A: Training is different because you’re constantly updating the weights.

B: Right. Training does tons of reads and writes. Flash is not great for that. Writes are slower, and flash wears out over time. So Carmack’s idea is much stronger for inference than for training.

A: The key phrase in his post is that inference can have a deterministic memory access pattern. What does that mean in plain English?

B: It means the system often knows what data it will need next. A transformer model moves through layers in a pretty predictable order. So the hardware or runtime can say: okay, I’m going to need this block of weights soon, then this next block, then the next one.

A: That sounds very different from random access.

B: Exactly. Random access is like saying: get page 900, then page 4, then page 70,000, then page 13. Flash hates that. Sequential access is more like: page 1, page 2, page 3, page 4. Flash is much happier with that.

A: So if inference is more sequential, flash becomes less ridiculous.

B: That’s the idea. Not easy. But less ridiculous.

A: He also talks about streaming weights into scratchpad memory. What’s scratchpad memory?

B: Think of it as a tiny, very fast staging area controlled by software. In the kitchen analogy, it’s the part of the counter where you put the next few ingredients the chef is about to use.

A: So the system is trying to do this little dance: fetch from flash, stage in scratchpad or HBM, feed the compute cores, repeat.

B: Yes. And the timing is everything. If the data arrives just in time, the chip keeps working. If it arrives late, the chip stalls.

A: This feels like an old-school systems problem coming back.

B: It is. Carmack even mentions old optical-drive-style optimization. When access is sequential and seeks are expensive, you sometimes duplicate data or lay it out in weird ways to avoid jumping around.

A: That’s funny. It’s like CD-ROM game optimization, but for AI models.

B: Pretty much. Same kind of thinking: the hardware has a weird performance shape, so software has to respect it.

A: He also gives two possible ways this could be exposed. One is a custom streaming interface. The other is making flash look like normal memory, even though random access would be terrible.

B: Right. The streaming interface is cleaner if you’re designing from scratch. The software explicitly says, move these pages here, then these pages there. But existing code won’t naturally work that way.

A: And if flash pretends to be RAM?

B: Then existing software can at least run. It might run horribly at first, but it gives you a migration path. You can gradually optimize the layout and access patterns until it performs well.

A: So it’s the difference between “perfect but requires a rewrite” and “messy but adoptable.”

B: Exactly.

A: Where do you think this actually makes sense first?

B: I’d look at places where capacity matters more than ultra-low latency. Multi-model serving is one. Imagine a server that hosts many models or many adapters, but only a few are active at a time.

A: Like a model library sitting near the accelerator.

B: Yes. Another good fit is mixture-of-experts models. If only a subset of experts are used for each token, then a giant cheap local weight store becomes interesting.

A: So the machine becomes less like one GPU holding one hot model, and more like a jukebox for model weights.

B: That’s a good way to say it. Fast compute, plus a big local library, plus a runtime that knows what to pull next.

A: But for a dense model serving tons of users with strict latency requirements, this is harder.

B: Much harder. Dense models want lots of weights all the time. And real inference stacks are messy. It’s not just weights. You have KV cache, activations, batching, speculative decoding, attention kernels, quantization layouts, and routing decisions.

A: So the clean story is “just stream the weights,” but the real system has a lot more going on.

B: Exactly. That’s why I wouldn’t say flash replaces HBM.

A: What would you say instead?

B: Flash becomes another tier in the memory hierarchy. Flash holds huge cheap capacity. HBM holds hot data and things that need fast access. SRAM or scratchpad holds the immediate working set. The compute chip does the math.

A: So it’s not HBM versus flash. It’s HBM plus flash, if the software can make the hierarchy work.

B: Yes. And the software is the hard part.

A: What does that software need to do?

B: It needs to arrange weights on flash, predict the access order, prefetch the right pages, manage scratchpad space, avoid evicting useful data, and keep the accelerator busy.

A: That sounds like the compiler and runtime become part of the hardware design.

B: They do. The chip alone doesn’t win. The chip, memory controller, compiler, model layout, and serving runtime all have to cooperate.

A: What would this mean for datacenters if it worked?

B: Potentially larger models on fewer accelerators, less pressure to shard everything across many GPUs, and cheaper hosting for lots of models or adapters. But the bottleneck shifts. Instead of only asking “how much HBM do we have,” you start asking “can we schedule the memory stream well enough?”

A: So it could reduce some interconnect pain, but add memory-layout pain.

B: Exactly. You trade one class of complexity for another.

A: Final takeaway?

B: HBM is the fast, expensive counter. Flash is the huge, cheap pantry. Carmack is saying inference may be predictable enough that you can move ingredients from pantry to counter just in time.

A: And if you get the timing right, you get more model capacity per dollar.

B: Yes. If you get the timing wrong, your expensive AI chip waits around, and the savings disappear.

A: So the idea is promising, but it lives or dies on scheduling.

B: Exactly. It’s a memory architecture idea, but the deciding factor may be software.