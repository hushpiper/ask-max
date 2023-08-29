---
title: "Hush's Big-Little Book of Symptoms"
draft: false
---

This section is extremely under construction, but aspires to be a diagnostic manual for different issues you may see in your outputs. Keep in mind that as your prompt to the model changes, your settings needs will also change: an opening message that says "Oh, hi! How are you doing today?" is going to shine best with different settings than when you're 90 messages deep into the chat with detailed character cards and ChromaDB running. Most models are flexible enough for a "set it and forget it" approach, but if you're having trouble, it's worth taking a look at your settings.

One note: keep in mind that many of these issues can also be addressed (at least in part) by enabling Mirostat. Mirostat essentially disables the Top-Whatever sampler settings and controls them dynamically for you. It takes away some of your control, but nevertheless gives excellent results, particularly on extremely fussy models like Llama 2. From there you can fuck around with temperature or repetition penalty to taste. Mirostat 2 with Tau 5 is usually a good place to start.

## Looping

Looping is usually a problem of your repetition penalty being too low, since that's the issue repetition penalty was meant to solve. You can also try lowering temperature or lowering Top-P. It can also be caused n by *very* low temperature, so if you're under 0.7 or so, raise it--and it doesn't hurt to raise both temperature or repetition penalty. In a pinch, you can also add in some Top-A to introduce some more diverse possibilities into the pool and shake it out of the loop.

## Disobedience

Instruct-tuned models are supposed to follow instructions. If you find they're running wildly off script and ignoring your Instruct Mode prompt, it's usually approached as a prompting issue, but might also be a settings issue. There are two ways to address this: Top-K or Top-A. Yes, this makes no sense since those two oppose each other; I'm confused by it too. If you are on an L2 Airoboros model, raise Top-A first and see what happens. On anything else, try Top-K first and Top-A if that's not enough.

## Parroting

This refers to the model echoing or paraphrasing parts of your prompt, such as your character cards or author's notes. I'm unclear on exactly what causes or contributes to this, but my gut says it's usually due to the model not having any good answers to your prompt, such that the most likely answer is simply to echo what was already said. (That might seem counter-intuitive, but to a language model, a word or phrase gets more and more probable as it repeats; this is why the repetition penalty is so important.) Running with that theory, try raising the temperature, then Top-A, then Top-P. You might also try cautiously raising your repetition penalty. I suspect that No Repeat Ngram Size will help a substantially with that as well, but haven't tested.

But don't discount prompt-based remedies either: turning on Instruct Mode with an appropriate instruct preset (or even a custom system prompt) can help, and certain Author's Notes can be helpful as well. It could also be the case that something in your prompt is getting it turned around, for example if you have something like Smart Context (the SillyTavern extras version, not Kobold.cpp) injecting its context into the middle of the prompt. There are, unfortunately, many possible causes and therefore many possible remedies. All you can do is mess around until you find something.

## Short Responses

This is often because the model gets to a certain point and just doesn't know where to go from there, so it stops. The quickest fix is to lower repetition penalty and raise temperature; an alternative might be to raise Top-A and/or Top-P.

With that said though, this is often an issue that responds really well to prompt-based solutions. An author's note instructing the model to give detailed description, and then giving examples of how to do so ("describe sights, sounds, sensations, and the thoughts and feelings of the characters in detail") will often go a very long way.

## Booooriiiiing

Sometimes your model will give you answers that are... *fine,* but boring. Soulless, one might say. A low Typical P will often cause this, and on modern models, there's not much call for that, so just raise it. In most cases your Typical P should be around 0.95 to 1. Alternatively, raising temperature or Top-A can introduce variety and interest into an otherwise bland model/chat. If that causes incoherence or word salad, the various repetition penalties can do a lot to help.

A related problem is answers that don't vary on rerolls. I do need to do more testing with this, but it seems to be due to the same factors, and probably has the same solutions.
