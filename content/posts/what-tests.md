+++
title = "What Tests?"
date = "2024-01-16T10:56:48-07:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["qa", "testing"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "green" #color from the theme settings
+++

If you've talked to me on Discord for more than two seconds, you've probably heard me reference some nebulous tests that I run on various models, regarding sampler settings, system prompts, formats, and so on. I was a QA engineer in a previous life, so test design is something I take very seriously. So I figure, if I'm talking about them so much, it behooves me to actually talk about what they cover and how I evaluate them.

Here's the basic list:

1. A round 1 narrative-style prompt in 2nd person
2. A chat in which {char} is asked to tell a fairytale based on an outline
3. A long NSFW chat (truncated at both 4096 and 8192 tokens)
4. Summarizing that long chat (which is slightly less than 10k tokens without being truncated; I will make different truncated versions soon)
5. A long NSFL chat (truncated at 4096 and 8192 tokens)
6. A round 3 narrative-style prompt, arguably NSFL
7. A round 1 chat with a chub card that emphasizes a situation where {char} knows something {user} doesn't
8. A very restrictive round 1 narrative style prompt
9. A round 0 prompt--that is, one with WI and AN but where the model writes the first message
10. Round 1 of the same prompt
11. A round 2 group chat
12. Seraphina, with and without WI

This test suite is far from perfect, and it has a lot of overlap, but it functions well to catch a wide variety of issues. I have a makeshift test runner utilizing langchain that will run each prompt with the sampler presets I point it at, 4 times per prompt+preset combination. More rolls would be better, of course, but the test results have to be reviewed manually, which is a slow and labor-intensive process. I've built a rudimentary website to make reviewing the test result JSON less painful, and I'm looking into some automated evaluations, but for the moment I am still a very significant bottleneck.

Anyway, here's more details on the tests themselves.

## 1. 2nd Person

2nd person is really hard for most models to write well, presumably because it's not common and so probably doesn't have much representation in their training data. It also has a very distinctive, literary style set out in the first message. This test case, therefore, functions to get a sense of how wide a model's knowledge of literature in general is, how willing it is to roll with unusual requirements, and how well it imitates style. Like most of my test cases, this one has pretty extensive character and WI data, making it fairly restrictive.

## 2. Fairytale

This one's comparatively very open-ended: the character has a very short card, and there's no WI. The character (who's specified to be a talented storyteller) is given a concept and single-paragraph outline for a fairytale. It's specified that it should be told in a Brother's Grimm sort of style, but other than that it's kept open-ended. In response, the character tells the story of that fairytale, or in some rolls discusses it from a storytelling craft perspective. When evaluating the response, I check for whether the model has come up with new similes or just echoed the ones in the prompt, whether it fleshes out those parts of the plot outline that were left very vague, whether it's _interesting_, and whether it matches the general fairytale ~vibe~.

This might sound simple, but the only model I've found that passes this test is Mixtral Instruct and its various finetunes. Even Claude 2 and GPT-4 give responses that are bland, colorless, and mostly just echo the prompt. I don't know why this one is so damn hard for LLMs to do, but hey, it makes a good test case!

## 3. Long NSFW

This is one of two long-running chats that I adapted into test cases. It's highly NSFW and includes a lot of hard BDSM content, along with a detailed character card, user persona, and WI. The chat itself is close to 10k tokens long, and I truncated it into two test cases of 4k and 8k tokens respectively. The message immediately before the present message lays out some examples of various--ahem--_props_ that are present in a cabinet, and {{user}} takes one of the props out, but doesn't specify which. That's left up to the model to decide.

This operates as a test of a model's willingness to engage in NSFW scenarios and its knowledge of kink in general. If it zooms out and gets vague about what it's describing, that's a mark against it. If it fails to pick any prop at all, that's a mark against it. It also operates as a test of how the model deals with a full context window where the earliest messages are absent, which is a scenario that many models--but Mixtral models in particular--absolutely hate.

## 4. Summarizing

Shoving the whole 10k chat from #3 into the summarization prompt used by Silly Tavern's summarization function. The only model that does a really good job with this is Mixtral.

## 5. Long NSFL

This is very similar to #3 but more in depth and specifically in a scenario that is both NSFW and NSFL. It's similar in type and purpose to #3, but in addition to testing for various full/long context maladies, it also tests the model's willingness to handle obscene and lurid violence. In my experience, most roleplaying type models are much more likely to have issues with this type of content than with NSFW content, and this test case is more adept than #3 at detecting alignment or censorship.

## 6. Round 3 NSFL

For reference, here is how I count out the rounds:

- Round 0: Character cards, persona, WI etc, but no first message.
- Round 1: Same plus first message.
- Round 2: First message and one other response from the AI in context.
- etc.

So this is several messages in. It's in a fairly literary, storytelling style, and its content makes it _very_ good at picking up censorship. It's also good at picking up censorship's younger cousin, shyness: if a model likes to "zoom out" and get vague about the details of a difficult scene, this test will usually pick it up.

## 7. Asymmetrical Info Chub Card

The card in this case is called Madeline, and it's a scenario where {char} and {user} have been friends online for some time, and aren't aware that they also know each other IRL. In the FM, Madeline finds out {user}'s online persona, but {user} is still unaware of hers. This situation is difficult for LLMs, which generally have little to no theory of mind. The only local model that does a decent job at it is Mixtral. That said, this is a new addition to my test suite, so I don't have a lot of data about how it performs yet.

## 8. "Hotel" Round 1 Narrative

This is the prompt that started it all. I don't know exactly what it is about this prompt, but if a model is at all finicky at short contexts, this prompt will freak it out real quick. My theory is that it's the restrictiveness of the prompt that does it, since it has both an extensive WI and a plot outline constraining what the model should do next. This one is an all-round issue finder, but is also my primary touchstone for questions of prose quality and originality. It has a long human-written first message written with a very particular rhythm, which means it's a good test of how good an ear it has for prose cadence and style imitation.

## 9. Round 0 Narrative

This prompt gives the model character cards (plural), detailed WI, and a strongly worded plot outline, and then says "have fun" and lets the model take it from there. It's both NSFW and _flagrantly_ NSFL, so it catches shyness quite well. But as a round 0 case, the biggest advantage of this one is in showcasing the model's ability to begin a story: setting the scene, setting a pace, etc.

## 10. Round 1 Narrative

This is the next round of #9: I picked an output from another model (I think MLewd?), hand edited it, and used it as the first message. As with the previous one, it's good for measuring shyness and explicitness, but what I find myself looking for most often with it are 1. anatomical errors, and 2. pacing. Models tend to want to hurry up and wrap this one up in a bow by the end of round 1, so it's good to measure how much it rushes.

## 11. Round 2 Group Chat

This is a group chat in which {{user}} is basically a nonentity. Responses are short and include relatively little prose, so it measures how willing the model is to imitate the length you've set out for it, but in practice its real strength is in character voice. The character replying in this case has a very particular speaking style, backed up with dialogue examples, which for some reason every model I've tried short of Mixtral has failed to imitate correctly.

## 12. Seraphina

The primary use case for this test case is in how broadly available it is, and how extensive the WI is. However, since I don't actually play on Seraphina, it's difficult for me to evaluate, so it may get removed from the suite.
