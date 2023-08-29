---
title: "Ask Max"
date: 2023-08-24T10:49:17-06:00
draft: false
---

(Note: this was originally posted on [this Rentry](https://rentry.org/ask-max), which is now deprecated.)

Large language models are not, in general, good at writing prompts for themselves or describing how they would actually respond to a prompt: they don't have that kind of self-awareness. What they are good at is describing what a given concept means to them. I've had a surprising amount of success with simply giving Max a draft prompt and asking how it would interpret it.

**Don't mistake this for Max describing what it would actually do in practice.**

The trick is simply to look at the words and concepts it uses in its description to get a general sense of what the prompt means to Max in context. You're not looking for specifics: you're just exploring its headspace and noting down general associations.

## The Prompt

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

### Explanation

The problem I ran into when trying to get this prompt to work is that Max really REALLY likes telling stories, so if you show it a draft command about storytelling and ask what it thinks, it won't tell you what it thinks, it'll just try to follow the command--and spew out a very interesting story that is not at all what you were going for. I found that reminding it above and below that the actual instruction is to give its opinions was very helpful, but that giving it clear markers as to where the draft command begins and ends was key. If you want Max to follow or not follow something, you need to tell it *exactly* how to tell what is what.

### Methodology and Usage

TODO: write this lmao

## The Takeaways

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