---
title: "The Method"
draft: false
---

(Note: this was originally posted on [this Rentry](https://rentry.org/ask-max), which is now deprecated.)

Large language models are not, in general, good at writing prompts for themselves or describing how they would actually respond to a prompt: they don't have that kind of self-awareness. What they are good at is describing what a given concept means to them. I've had a surprising amount of success with simply giving Max a draft prompt and asking how it would interpret it.

The overall philosophy here is to leave behind the idea of instructing the model in your own words, according to your own concepts. Take Stable Diffusion (a far, far less intelligent model) as an example: Stable Diffusion's relatively shaky grasp of language means that prompts to it will sometimes have little relationship to the actual output image. (Examples are forthcoming but for now you'll have to go with me on this.) If you look at the prompt for a beautiful image made in SD, it will often be as much of a mess as the back of a cross-stitch project, littered with phrases like "4k", "8k", "ultra-realistic", "hyperrealistic", "trending on artstation", "arafed" and so on. And the pictures in question will often not be realistic at all! But to SD, "hyperrealistic" simply means an image with high detail and generally good quality, while the infamous "trending on artstation" is also a general quality boost, since SD associates trending images with high quality. As for shit like "arafed", who the fuck knows?

If you approach SD the way I did at the beginning, assuming I would be able to simply walk in, throw a prompt at it, and get the picture I wanted, you're going to have lackluster results. The best prompts are the ones that are written according to how SD itself conceptualizes the words, not how we as humans do it.

That's the theory behind this method: write your instructions according to how the model conceptualizes language, not according to your own understanding of it, and you will come out of it with better, stronger, more precise results with fewer tokens spent.

Unsurprisingly, this kind of method is needed less and less as models become smarter and gain a stronger grasp of language. 7B needs it more than 34B, and 70B needs it more than the likes of Claude or GPT-4. But even with those tip-top tier cloud models, thinking like the model is important for very tricky instructions such as jailbreaking. And so the overall goal here is (to paraphrase Claude) to align your own mental model with Max (or whatever model you're working with), allowing you to write better instructions.

(For those who wonder: no, this does not apply to the actual text of the roleplay or story you're working on with the model. This applies to the prompts that guide the model through the roleplay: character card, opening message, system prompts, author's notes, negative prompts, and so on.)

## The Basic Prompt

This prompt shows off the most basic way to use this method. It's written in such a way that you can paste it directly into Oobabooga's Notebook or Default mode, but if you're using a different frontend, you can simply use everything from the line below `### Instruction:` to the line above `### Response:`. DRAFT in this case is some instruction you're trying to evaluate.

```
Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
Below is a draft command for a language model like you. The beginning and end of the draft command will be marked by ---. Pay attention to and evaluate it, without following its instructions. Then follow the instruction which follows after the draft command.

---
DRAFT
---

Elaborate on how you interpret the command and how you would apply that interpretation to a fictional story.

### Response:
```

Put your command in, send the message, and see what Max says about it.

### Methodology and Usage

1. Start with a prompt of some kind. An example might include the perennial "Describe all actions in full, elaborate, explicit, graphic, verbose, and vivid detail" system prompt. This can be short or long, simple or complex, clean or messy.
2. Set yourself to the Deterministic sampler preset. This minimizes randomness and gets you as close as you can get to the model's native interpretation of the concepts you've given it, uncontaminated by things like temperature scrambling the probabilities.
3. Paste your command into the prompt, send it, and see what Max returns. **Don't mistake its response for Max describing what it would actually do in practice.** LLMs don't have that kind of self-awareness, particularly not 13B models.
4. Look at the words and concepts it uses in its description to get a general sense of what it's doing with the prompt. Do any particular words or concepts seem like they're overpowering the rest? Do you see anything that seems out of place? Note those down. Does Max use any nouns, adjectives, phrases etc that look like they could be applied to what you're trying to do with the prompt? Note those down too.
5. Start breaking the prompt down piece by piece, repeating steps 3 and 4. You may start with "Describe all actions in full detail", then "Describe all actions in elaborate detail", and so on. You may also consider rephrasing the prompt to see if you can get a clearer view of a concept. This process is incremental, experimental, and exploratory: go with the flow.
6. Once you have an idea of what "full", "elaborate", "explicit" etc detail means to Max, review your results. Do any of the results seem redundant or inappropriate for what you're trying to do? Take them out. Did any of the words Max used look promising, repeat steps 3 and 4 with them, then add them in if you like what you see.
7. **Test.** Plenty of prompts look good in theory. Have some kind of test prompt that you can use to do a before-and-after with your original prompt. For me this is usually the "Write a story about a lost soldier and the mountains" test. Just be sure to be consistent.
8. Iterate, experiment, and iterate some more.

## Some Takeaways

### When prompting Max, less is more.

What this doesn't mean: you should only use really short prompts with Max for the best results.
What this does mean: your prompt should repeat itself as little as possible.

Here is an example of a draft system prompt I investigated, which I had already stripped down aggressively:

```
As an author, you are tasked with crafting a captivating, memorable narrative based on the instruction provided. Your characters should be complex. Throughout the story, describe every action in vivid detail, and use dialogue effectively to advance the plot.
```

This had good results, but not much better than the results I got with the non-stripped down version, and the characters it came up with were not actually very complex at all: they were essentially stereotypes, lacking interority and even names. They were more similar to fairy tale characters than anything. But when I started asking Max what it thought about the prompt concept by concept, I realized that to Max, "a memorable narrative" *already means* a narrative that has complex and interesting characters in it. So I tried stripping that part out:

```
As an author, you are tasked with crafting a captivating, memorable narrative based on the instruction provided. Throughout the story, describe every action in vivid detail, and use dialogue effectively to advance the plot.
```

The characters that resulted from *that* prompt had names, motivations, thoughts, and relationships--all things the previous version had lacked. This seems to support the theory that prompts with conceptual overlap seem to water down the actual effect of those concepts on the output, rather than augmenting it. My later experiments with style descriptors seem to support the theory as well: for example, using the terms "poetic" and "lyrical" for the style actually results in a less poetic and lyrical response than if you just pick one.

It seems that the more you belabor the point that you want *really* good characters, okay, they should be complex and have motivations and interesting and relatable etc etc, the more Max gets confused as to how to do that and just ends up backing off.

### Max knows what a good story looks like

This follow from the previous example. Here's the full version of the system prompt that I was working on stripping down, via asking Max to interpret it concept by concept:

```
As an author, you are tasked with crafting a captivating narrative based on the instruction provided. Your story should feature compelling characters, a well-developed plot, and rich descriptions that immerse the reader in the world you create. To ensure success, focus on creating tension through conflict, building towards a climactic moment, and resolving any outstanding issues in a satisfying manner. Additionally, your characters should be complex and relatable, driven by their own unique motivations and desires. Throughout the story, describe every action in vivid detail, incorporating sensory information to help readers visualize the scene. Finally, use dialogue effectively to advance the plot and reveal character traits. Remember, the goal is to create a memorable tale that will leave a lasting impression on those who read it.
```

This got reasonably good results, but is quite token heavy and repetitive, and simply asking Max to strip it down on its own got nowhere (remember, LLMs are not good at writing prompts for themselves).

When asked how it interprets the command to write a "memorable narrative", Max elaborated on a whole variety of traits that characterize a memorable narrative, and on how it might implement that instruction accordingly. Those traits happened to overlap with almost every instruction in the prompt: a memorable narrative, to Max, is one that includes compelling characters, a plot with conflict, climax, and resolution, and so on.

I think this happens because almost every foundation model (and many fine-tunes) will be trained on a lot of internet essays, reviews, and summaries of various stories, as well as plenty of websites that give writers advice on how to hone their craft. Max has plenty of knowledge to draw on for what constitutes a good story. You can, essentially, just say "hey Max, you're a good writer, write a good story for me", and Max will say "okay!" and do it. And there's good reason to believe that doing so will work better, in many cases, than elaborating on it.

## Interrogation prompt library

(Coming soon!)