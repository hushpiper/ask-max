+++
title = "Mixtral Spiral"
date = "2024-01-02T07:20:44-07:00"
author = "hushpiper"
authorTwitter = "" #do not include @
cover = ""
tags = ["mixtral"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
draft = false
+++

Mixtral is awesome. It has a huge flaw though, which many of you have already noticed. It's been referred to in a lot of ways: looping, parroting, going psycho, going schizo, throwing a tantrum, etc. All of them refer to the phenomenon where, without warning, and often around the 4k token mark, Mixtral will suddenly start repeating or paraphrasing the prompt, failing in reading comprehension, speaking out of character, speaking for the user, sprinkling its responses with random punctuation, and sometimes losing coherence completely and descending into word salad. In these tantrums, it will partake in basically every form of misbehavior I've documented on the Symptoms page, and then make up some extras, just to spite you.

Personally, I like to call this the Mixtral death spiral (or just parroting, if I'm lazy). Some friends and I decided to do a deep dive and a shitton of tests to try to figure out how to address this issue. Here's what we came up with.

## Why does this happen?

Look, I'm not an expert, so take this with a grain of salt. As far as I can tell, something in the Mixture of Experts architecture Mixtral uses is extremely sensitive to patterns. This is part of its charm: it can follow a card so well it sometimes seems like it's reading your mind. Writing for Mixtral, or talking to Mixtral, is so often effortless and exciting. But this also means that sometimes it detects a pattern that isn't there and suddenly it's seeing secret messages in its Cheerios. I think this has something to do with the way that it routes tokens to different experts: if it gets enough of them wrong, it gets worse and worse. Someone who knows the architecture better will have to chime in; if that's you, ping me on Discord.

## Prompting

Mixtral wants a place for everything and everything in place. If it finds that its peas are touching its mashed potatoes, it will wail and knock the whole plate onto the floor. What I mean by this is that it wants to know exactly what is what. Roleplaying prompts are complicated: there are a lot of stray pieces, and it's easy to get a pea or two into the mashed potatoes. So here's the One Weird Trick I found to make things clear:

Use the Mistral context preset in SillyTavern (which wraps your prompt in `[INST][/INST]`), then put `<ASSISTANT'S ROLEPLAYING PROMPT>` at the very top of your Story String after the `[INST]`, and `</ASSISTANT'S ROLEPLAYING PROMPT>` as the Chat Start. That's it, that's the trick. When I did this, parroting on my long context test prompts immediately disappeared. No more spirals. (Except on sampler presets using Min P. More on that later.)

Would something more token-efficient work just as well here? Probably. This particular formulation is one that Mixtral itself suggested, as I was using a kind of jury-rigged OPRO method to essentially query it as to what sorts of roleplay prompts strike it as good: the example prompts I gave it had no such tags, but it suggested them on its own, and was _very_ insistent about keeping them--in exactly that form--across rerolls. I think it just really really expects something like that. Here's what that tends to look like, in raw prompt format:

```
[INST]<ASSISTANT'S ROLEPLAYING PROMPT>
(system prompt that doesn't seem to be important)
(everything other than dialogue examples: world info, character profile, persona, etc.)
Examples:
[INST]{{user}}: (example dialogue)[/INST]
{{char}}: (example reply)
</ASSISTANT'S ROLEPLAYING PROMPT>
This is the history of the roleplay:
[INST]{{user}}: (something)[/INST]
{{char}}: (reply)
(rinse and repeat)
```

## Turn off Min P

Mixtral _loves_ Min P, absolutely adores it--until it hates it. I ran [tests](/posts/what-tests) with a wide variety of settings on both short and long (4k to 8k tokens) prompts, and we were able to eliminate the parroting on almost every sampler combination we tested. The outliers? All utilized Min P. And more: every single preset that utilized Min P misbehaved, consistently--including the many I tested that are not available in SillyTavern or any of its backends.

(This is true of Mixtral Instruct, but much less true of non-DPO Bagel 8x7b.)

This is unfortunate, because as I said, Mixtral loves Min P. There's no other sampler that will give that same luscious feel while still sticking so beautifully to the prompt. It might as well have been tailor made for Mixtral. But they're that couple that fights and breaks up and gets back together and yells at each other in public: you just wanna see them happy, and it just doesn't seem possible.

I've made some attempts to fix this using other samplers, but so far results are inconclusive. So for the moment, my suggestion is that when you hit a death spiral, you go into your sampler settings, set Min P between 0.05 and 0, and TFS to about 0.95, then adjust downward until the spiral stops. If you want to stick with Min P, you will need to push it way, WAY up: about 0.3 was the sweet spot. But a Min P that high (like a lowered TFS) will tend to produce boring text, so it's a tradeoff.

Over the past few weeks, a friend developed a preset that allows for a more measured approach, called Amphibian-Frozen WoodFrog. Download it [here](/presets/Amphibian-FrozenWoodFrog.json). It's straightforward: Temp = 1.25, Min P = 0.08, TFS = 0.98. The TFS pulls most of the weight here: it tamps down the parroting enough to allow you to lower the Min P a bit, and counteracts the downsides of higher temperature enough that you can get away with raising it to boost the creativity you lose by having Min P higher than 0.05 or so.

**Why the name?**

The creator used the word "amphibian" to indicate that it utilizes both Min P and TFS, while the rest of the name indicates that the temperature was lower on it than on competing amphibian presets. (Some had temperature greater than 5! But this one outcompeted the rest.)

**Why TFS?**

TFS is very similar to Min P, conceptually speaking, which is why it was the focus of our testing efforts. In practice, Min P tends to act like a creativity enhancer while TFS tends to be more of a nonsense inhibitor. Still, the theory is that if Mixtral responds well to Min P, maybe it will also respond well to TFS. In my own experience, they don't have quite the same chemistry, but they do have a very stable, domestic sort of relationship. It makes a good rebound, you know?

Also, repetition penalty behaves nothing like it should on Mixtral, and TFS does a great job of standing in for it.

## So what now?

Look, outros are hard, but I want to get this out rather than spend too much time polishing it. Go and do, and if you have questions or suggestions, ping me in the SillyTavern Discord.
