---
title: "Hush's Big-Little Book of Symptoms"
draft: false
---

This section is extremely under construction, but aspires to be a diagnostic manual for different issues you may see in your outputs. Keep in mind that as your prompt to the model changes, your settings needs will also change: an opening message that says "Oh, hi! How are you doing today?" is going to shine best with different settings than when you're 90 messages deep into the chat with detailed character cards and ChromaDB running. Most models are flexible enough for a "set it and forget it" approach, but if you're having trouble, it's worth taking a look at your settings.

One note: keep in mind that many of these issues can also be addressed (at least in part) by enabling Mirostat. Mirostat essentially disables the Top-Whatever sampler settings and controls them dynamically for you. It takes away some of your control, but nevertheless gives excellent results, particularly on extremely fussy models like Llama 2. From there you can fuck around with temperature or repetition penalty to taste. Mirostat 2 with Tau 5 is usually a good place to start.

## Looping

Looping is what happens when the model loses coherence and begins repeating one word of sequence of words over and over. Example:

> What time are you coming home today or tomorrow morning and I will be there in about an hour and a half hour and a half hour and a half hour and a half hour an a half hour and a half

Etc. It's usually a problem of your repetition penalty being too low, since that's the issue repetition penalty was meant to solve. You can also try lowering temperature or lowering Top-P. It can also be caused by *very* low temperature, so if you're under 0.7 or so (which isn't super low, but some models can't handle the cold), raise it --- and it doesn't hurt to raise *both* temperature or repetition penalty, either. In a pinch, you can also add in some Top-A to introduce some more diverse possibilities into the pool and shake it out of the loop.

## Parroting

Though it's easy to confuse this with looping, parroting refers to the model echoing or paraphrasing parts of your prompt, such as your character cards, author's notes, or earlier parts of the chat history. It can also show up as the model seeming to become obsessed with particular phrases or bits of wording. I'm unclear on exactly what causes or contributes to this, but my gut says it's usually due to the model not having any good answers to your prompt, such that the most likely answer is simply to echo what was already said. That might seem counter-intuitive, but to a language model, a word or phrase gets more and more probable as it repeats; this is why the repetition penalty is so important. My guess is that anytime the model is too confused to give a strong answer or otherwise is short on possibilities, it will tend to take the "safe" way out and parrot.

Running with that theory, there are a ton of possible causes, and so a ton of possible remedies. The first possibility is that the model is confused. Your context template will often contribute to this, since it governs the structure of the prompt and thus is huge in terms of how clear the prompt is to the model. In my tests, roleplay and simple-proxy parrot quite readily, but the parroting is usually invisible to the user. I've been working on an anti-parroting context template, but it's in very early stages. I suspect that most parroting issues can be addressed with this. You should also check other aspects of your prompt: if you have Smart Context (the SillyTavern Extras version, not Kobold.cpp's version) injecting its context into the middle of the prompt, try turning it off. Double check your Author's Note and make sure it's clear and coherent, and check for any jailbreaks or system prompts that might be causing issues.

The other major possibility is that it's due to the model simply not having enough probable possibilities to respond with. This seems to be more likely to happen when you're a long ways into the chat and the context is significantly filled. So, theoretically: try raising the temperature, then Top-P, then Top-A. You might also try cautiously raising your repetition penalty. I suspect that a fairly high No Repeat Ngram Size setting can help substantially with this as well, but in situations where the phrase being repeated is short (say, less than 5 words), it can easily hurt coherence.

## Disobedience

Instruct-tuned models are supposed to follow instructions. If you find they're running wildly off script and ignoring your Instruct Mode prompt, it's usually approached as a prompting issue, but might also be a settings issue. There are two ways to address this: Top-K or Top-A. Yes, this makes no sense since those two oppose each other; I'm confused by it too. If you are on an L2 Airoboros model older than 2.1, raise Top-A first and see what happens. On anything else, try Top-K first and Top-A if that's not enough. You can also try the Titanic preset: it uses a cautious amount of encoder repetition penalty, which penalizes words that weren't in the original prompt, and has the effect of making it follow instructions better and hallucinate less. If you're super attached to your current preset, try 1.07 encoder repetition penalty and see what it does; don't be too aggresssive though, as it will very reliably cause parroting if it's too high.

## Short Responses

This is often because the model gets to a certain point and just doesn't know where to go from there, so it stops. The quickest fix is to lower repetition penalty and raise temperature; an alternative might be to raise Top-A and/or Top-P.

With that said though, this is often an issue that responds really well to prompt-based solutions. The most common advice for this is to make sure that *you* are writing long responses yourself, particularly at the beginning of the chat. This can require a lot of effort, and doesn't always appear to have an effect, but I can confirm that it does. An author's note instructing the model to give detailed description, and then giving examples of how to do so ("describe sights, sounds, sensations, and the thoughts and feelings of the characters in detail") will often go a very long way as well.

## Long Responses

Long responses are usually considered desirable in the RP hobby, but there are times where they aren't desirable: for example, in a pure dialogue-style chat, paragraphs of description just get in the way. If you want to nudge the model toward being a little less verbose, you can do the opposite of the things that would give you longer responses: lower temperature, raise repetition penalty, lower Top-A and Top-P. You can also instruct the model to give simple, brief descriptions, and focus on dialogue.

## Booooriiiiing

Sometimes your model will give you answers that are... *fine,* but boring. Soulless, one might say. A low Typical P will often cause this, and on modern models, there's not much call for that, so just raise it. In most cases your Typical P should be around 0.95 to 1. Alternatively, raising temperature or Top-A can introduce variety and interest into an otherwise bland model/chat. If that causes incoherence or word salad, the various repetition penalties can do a lot to help.

A related problem is answers that don't vary on rerolls. I do need to do more testing with this, but it seems to be due to the same factors, and probably has the same solutions.
