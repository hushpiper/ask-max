+++
title = "Clarity Alpha"
date = "2023-09-19T11:01:56-06:00"
author = "hushpiper"
authorTwitter = "" #do not include @
cover = ""
tags = ["context templates", "clarity context", "parroting"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "orange" #color from the theme settings
+++

Parroting is my white whale.

(Okay, one of them.)

It's a tough issue to address, since there are so many possible causes. My theory, which I've gone into some detail about on [the symptoms page](/symptoms/#parroting), is that parroting happens when the model doesn't know what else to do: either it's confused or it's short on possible responses, so it falls back to the "safe" option of just repeating the prompt. There are, therefore, only really 1 or 2 causes -- and confusion is a major one.

My experience with doing Ask Max prompts has taught me that when you're doing tricky prompting, it is extremely important to make sure that the model knows what is what, to avoid confusion and prevent the model from giving responses that you don't want. For Ask Max, I usually do something like:

```md
The beginning and end of THING will be marked by ---.

---
THING
---

Do STUFF with THING.
```

Since SillyTavern implemented the powerful-but-confusing context templates system, it's opened up new possibilities for more precise ways to address tricky prompts -- and make no mistake, the prompts that we routinely make through SillyTavern are tricky. My own attempts to create a context template originated with a conversation about how to prevent the model from mistaking example dialogues for things that were said, or should happen, in the chat itself; this would make example dialogues more accessible to more people, as they are currently finicky enough that many don't bother. Going with the theory that the key is to make it thoroughly clear to the model what is and is not an example dialogue, I created [a context template](/presets/clarity-context-alpha.json) and [an accompanying instruct template](/presets/clarity-instruct-alpha.json) to try to address that issue -- and, by the same token, reduce parroting.

I consider these templates to be in alpha at best, and given my many projects I am short on time to fully test them, so I'd appreciate testing! My conclusion so far is that it's very effective in reducing most parroting, but tends to exacerbate issues with impersonation, for whatever reason.

## Methodology

The basic idea of the whole thing is to encase the pre-chat information (world info, character cards, persona, etc) with `BEGIN CONTEXT` and `END CONTEXT`, which echoes Airoboros's closed-context question answering system prompt:

```md
USER: BEGININPUT
BEGINCONTEXT
date: 2021-01-01
url: https://web.site/123
ENDCONTEXT
In a shocking turn of events, blueberries are now green, but will be sticking with the same name.
ENDINPUT
BEGININSTRUCTION
What color are bluberries?  Source?
ENDINSTRUCTION
 ASSISTANT:
```

Airoboros is a major component of all our current leading RP models, so those phrases are things that the model should understand and respond to. Then, inside of the context block, the string `NEWEXAMPLE` begins each new example dialogue.

To test this, I decided to take a test prompt involving around 1000 tokens of example dialogue plus world info (ensuring that there's a large chunk of context filled), and put it all in Oobabooga's notebook mode to give me a better window to see what's happening. Then I set my sampler preset to Deterministic and started slowly nudging encoder repetition penalty up until it started to parrot, and compared my results to doing the same thing with the Roleplay context preset. What I found is that Roleplay tends to parrot quite readily, and does so much earlier than my current preset (which I have named Clarity, because I am very creative). I take this to be an encouraging result.

## Improvements

There's lots of room for potential improvements here:

- If we're mimicking the closed-context question answering prompt, wouldn't `BEGIN INPUT` be more appropriate than `BEGIN CONTEXT`?
- If we do that, should we add a `BEGIN CONTEXT` section? What should go in it? In my experience, Airo tends to bitch if you leave out the context section.
- I added the space in `BEGINCONTEXT` etc for clarity, but is that really appropriate, or does it just confuse the model by creating more distance between this and the closed-context prompt?
- On the other hand, closed-context is a System Prompt, and we're not, actually, replacing the System Prompt with this. In fact, we're mimicking the Alpaca prompt by putting `### Instruction:` right after the user's System Prompt. Is that even a good idea or will the two clash? Should we scrap Alpaca and attempt to go all CC all the way, using the user's System Prompt as the `BEGININSTRUCTION` section?
- As far as I can tell, example dialogue goes at the end of the Story String. Therefore, could we put `BEGIN EXAMPLE DIALOGUE` at the bottom of the Story String, and set Chat Start to `END EXAMPLE DIALOGUE\nEND CONTEXT`, to be even more clear about where the examples happen? I would usually find that preferable to the current approach.
- I've put 0 thought into what the optimal order of prompt sections is, so, like, I should do that maybe.

What is best? Only time and testing will tell!
