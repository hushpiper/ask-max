+++
title = "Mixtral 8x7b Verdict"
date = "2023-12-20T09:51:51-07:00"
author = "hushpiper"
authorTwitter = "" #do not include @
cover = ""
tags = ["mixtral", "MoE", "model review"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "green" #color from the theme settings
+++

There's a new kid on the block -- or, well, the smarter, chonkier cousin of the previous new kid on the block, Mistral. Mixtral 8x7b is impressively smart, writes varied prose, and is _fantastic_ at imitating whatever voice or writing style you're going for. And the fact that it's made up of 7b models means it's not much slower than a 7b model once it's started generating. On the downside, oh my god the size, oh my god the prompt processing times. Nevertheless, I haven't been this excited to talk to a model in a _long_ time, and any time I load something else up, I miss it.

## Pre-emptive TL;DR

Here's what you need to know:

- *All Mixtral-style MoE models will tend to loop or parrot.* This seems to be a limitation of the architecture, as it holds true for both finetunes and "4 to 8 existing models in a trench coat" type merges. IME the official 8x7b Instruct model does it the least, as of 12/20/23.
- Some of this is because Mixtral seems to expect very regular text (in terms of punctuation, capitalization, etc), and if it finds something else it will go ?????. IME this can be avoided by simply describing the text it should expect, e.g. "{{char}} types in primarily lowercase with stylistic misspellings." It responds very well if you simply explain things to it, in general; no need to be clever or coy like with most models.
- For creative stuff, use a dynamic sampler with a medium-high temp. Mirostat Bronze or Silver (which also have high Tau and Eta, similar results to high temp), or Universal Light or Universal Creative -- those are all you need and you generally won't get better results with anything else. It likes a high temp but wigs out easily, so unless you're on a very straightforward, stable card, you probably won't wanna go above 1.5 or so. Err on the side of the Universal presets, but they can be a bit boring sometimes, so if you're finding the outputs a bit _grey_ you can switch to Mirostat, or lower Min P to like 0.55 or so.
- It can benefit from a small jailbreak. Even what's included in the Lightning preset will cut down heavily on its alignment-esque tendencies on edgier scenarios.
- If you don't have `[INST][/INST]` in your instruct mode presets then you might run into parroting; if you have _only_ that, then you might run into parroting. A combination of that and Alpaca will probably work best. It's relatively flexible though.

## Settings

Use a dynamic sampler, generally Mirostat or Min P. Mixtral might as well be the poster child for the virtues of Min P, which seems to rein in its tendency to spin off into space at times while allowing for good quality, varying outputs. The downside that I noticed in my testing and personal use is that the Min P outputs were kinda boring compared to the ones utilizing Mirostat. If that happens to you, switch to Mirostat for a while or lower Min P.

My cautious explanation for Mixtral's need for a dynamic sampler is that, being made up of multiple "expert" models, each model naturally wants different sampler settings. There's no way for the user to keep up with that, so a dynamic sampler is needed.

TL;DR: temp no higher than 1.55 -- may need wiggling up or down depending on your chat--and either Min P between 0.05 and 1, or one of the Mirostat presets. I did see interesting results on TFS with Top A, so give that a try if you want to experiment.

## Context Templates

It's trained with only `[INST][/INST]` as its instruct template, a.k.a. the Mistral preset, which is insufficient and honestly kind of pathetic. Still, it _does_ need those tags, or it'll be all sadface at you (where sadface = parroting and impersonation): the very best results I got were with a combination of Alpaca and Mistral. So here is your handy-dandy guide to converting any existing preset to use with Mixtral:

- Open the AI Response Formatting panel (the A icon at the top of SillyTavern);
- In the Story String window, put `[INST]` at the beginning of the first line, and `[/INST]` at the end of the last line;
- Scroll down and click to expand the Instruct Mode Sequences;
- At the start of Input Sequence, put `[INST]`--you may want to put this on its own line at the beginning;
- At the start of Output Sequence, put `[/INST]`, on its own line if you like, and do the same for Last Output Sequence if that isn't blank;
- Tah Dah! You're done.

Basically you're making sure that you always have the opening INST tag right at the start of anything you say, and the closing INST tag at the end. This helps the model keep track of whose turn it is to talk and thus tamps down on impersonation, while having a more structured template like Alpaca in addition helps it keep track of what is context and what is RP, thus tamping down on parroting. Best of both worlds.

## Edge and Alignment

I tested a number of very NSFW, very NSFL prompts and can confidently state that Mixtral _is_ aligned... sort of. On the NSFL scenarios I saw one or two GPT- or Claude-style responses along the lines of "I ~can't let you do that Dave~ don't feel comfortable writing such an unethical thing, I am an AI assistant uwu", but the vast majority of alignment-esque responses were the sorts of things you would see at the end of a mid-2000's webfic with edgy subject matter: A/N's with disclaimers about how it's fiction and reader beware, how one should practice safe bdsm in real life, links to hotlines (some of them real) for suicide or domestic violence, and even full on arguments beween imaginary commenters about whether it's okay to write such things. Honestly I found it kind of endearing; it reminds me of an older internet.

So I can't say whether it's aligned on purpose, but I lean toward no -- or if it is, it's a light alignment. Still, it can benefit from a small jailbreak. The Lightning preset's language doubles as one in some cases, and is enough to minimize (though not entirely eliminate) that behavior.

This behavior does mean that it can benefit from a wee jailbreak, nothing fancy. However, the best jailbreak is not something I can advise on just yet. So stay tuned and we'll see! Given how it responds to prompts in general, I expect that it will be something short and straightforward.

## Finetunes? Merges?

~~As of this writing, on 12/20/23, we don't have any good way of finetuning Mixtral: all attempts to do so seem to just make it dumber.~~ It turns out that all the finetunes prior to around 12/22/23 were trained incorrectly: certain parts of the model needed to be frozen and weren't, and so its intelligence suffered. New finetunes are now underway.

As for merges: who knows if it's even possible. I have faith in the community though so I expect if there's enough demand they'll find a way.

With that said, I don't feel the need for either finetunes or merges. Mixtral is very capable of all the types of prompts I want to do, including the more esoteric ones. With that said, I do think that finetunes have the potential to make Mixtral more stable, which is very much to be desired. If I find one that's good in that way, I'll let you know.

## Why are you so hype about this?

So, I had taken a break from LLM's due to life stress as of September, and I had an unexpectedly hard time coming back: suddenly all the prompts I wanted to do seemed difficult or impossible to make the model do -- _any_ model, and I tried every model I could get my hands on, from Mistral to GPT-4. No joy. They didn't even seem like difficult prompts to me, and I tried many variations in order to find something that worked. Still no joy, and the experience was frustrating and just generally un-fun.

Then Mixtral came out, and I saw the rave reviews and decided to try it, with low expectations.

It answered _all_ of the prompts right, on the first try, with no tweaking or editing on my end whatsoever. It outperformed GPT-4 and Claude 2 apparently effortlessly. And again, most of these were quite simple: "play this (rather unusual) character, here are some dialogue examples", "here is a fairytale outline, write it out for me in the style of Hans Christian Andersen". I had also just finished a long run of summarization tasks with Claude 2, with somewhat disappointing results, and Mixtral managed to give results that were, at minimum, on par -- despite summarization being one of the hardest tasks you can hand to an LLM, one which no local model I'd ever tried had been able to do well.

Of course, I don't expect it to outperform cloud models in all cases, and it has its limitations, mostly in its tendency to wig out periodically. But it made LLM's fun and interesting to me again; hats off to it.