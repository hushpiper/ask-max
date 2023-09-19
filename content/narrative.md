---
title: "A Guide to Using Local Language Models for Writing and Roleplaying with Google Colab and SillyTavern"
date: 2023-08-24T10:49:28-06:00
draft: false
---

### Introduction

Large language models are powerful tools that are useful for more than just cheating on school papers and ruining your legal career! My first impression on using ChatGPT was that it's useless for creative writing and porn, but I was determined to find a way that would make it work, and I found one that's working well for me.

**Note: this is _even more messy_ than most sections of this site, and a great deal of it is unfinished. It was also made pre-Llama 2, so the settings suggestions are a little eh: for that, go to the [Presets](/presets) page. Hopefully this will be helpful and get updated quicklyish!**

Another guide that does similar things, but using koboldcpp instead of Colab, can be found [here](https://rentry.org/llama_v2_sillytavern).

## Setting Shit Up

If your computer can play modern games, it can probably run a local model, but setup is a pain if you're not sure it's right for you yet. If you're writing on a potato, you probably can't run one on your computer at all. Due to that, I think people should first try running it on Google's hardware instead.

### Setting Up Google Colab

Google Colab is a platform for machine learning researchers to do their research without needing to buy expensive hardware on dinky little university budgets. It's also very popular with machine learning hobbyists, because it has a free tier that allows you to use the hardware on an as-available basis for a nebulous number of hours per week. It uses what are called "notebooks", a.k.a. Jupyter Notebooks, which provide easy interfaces to run Python code. Several notebooks are available that will set up a language model for you to use. Here's a step-by-step guide to get you started:

* Create a Google account (if you don't have one already).
* Go [here](https://colab.research.google.com/drive/1Zi6-G_jdrGADnvMJ285CikFyF4taYx4o?usp=sharing).
* From the dropdown select a model. If you don't know which to do, do TheBloke/MythoMax-L2-13B-GPTQ. (If it's not in the list, just type it in.)
* Click the the little play buttons on the left like it says. Make sure to press play on the audio player.
* It will spit out a ton of text. When it says Restart Runtime, do NOT restart the runtime. {{< figure src="/narrative/restart_runtime.png" alt="Don't restart the runtime" >}}
* If you see something like this, go back to the play button that you clicked (the one underneath the audio player) and hit it twice: once to stop, and once to start it again. {{< figure src="/narrative/start_cloudflared.png" alt="Couldn't start cloudflared" >}}
* At the end, it should give you something like this: {{< figure src="/narrative/finished.png" alt="The end result." >}}
* Your URLs will be different, but there are two that you should copy and save for later. In the case from the screenshots, you want:
    * https://tackle-force-sierra-orbit.trycloudflare.com/api
    * wss://value-emphasis-belize-wife.trycloudflare.com/api/v1/stream (right above the first one)

If you wanna get more into the guts of the thing, or read more info about how it all works, try [this](https://colab.research.google.com/drive/1CThnhkW5a16_SFOBykCScRwIbUv-8TJq?usp=sharing) Colab notebook, by the same author as the one above.

### Installing SillyTavern

SillyTavern is a powerful node.js software that enriches your writing and roleplaying experience. [Here's](https://docs.sillytavern.app/installation/windows/) how to set it up. (Better guide coming soon, sorry, there's a lot to go through here. In the meantime, the one above does have screenshots etc.)

### SillyTavern Configuration

TBD - settings preset, connection, instruct mode, adding a character card, adding personas

### Optional: SillyTavern Extras

TBD

## High-Speed No-Nonsense SillyTavern Orientation

TBD

### Understanding Plist and Character Cards

A plist is a text prompt that is formatted like this, used to encode information about characters in very few tokens:
```
[Character: traits; Character's clothes: traits; Character's body: traits; Genre: genre; Tags: tags; Scenario: scenario]
```

Here is an example, from [this guide](https://rentry.co/kingbri-chara-guide):
```
[Manami's persona: extroverted, tomboy, athletic, intelligent, caring, kind, sweet, honest, happy, sensitive, selfless, enthusiastic, silly, curious, dreamer, inferiority complex, doubts her intelligence, makes shallow friendships, respects few friends, loves chatting, likes anime and manga, likes video games, likes swimming, likes the beach, close friends with {{user}}, classmates with {{user}}; Manami's clothes: mint-green blouse, denim shorts, flats; Manami's body: young woman, fair-skinned, light blue hair, short hair, messy hair, blue eyes, magenta nail polish; Genre: slice of life; Tags: city, park, quantum physics, exam, university; Scenario: {{char}} wants {{user}}'s help with studying for their next quantum physics exam. Eventually they finish studying and hang out together.]
```

Character cards store specially formatted (json) text data embedded in pictures, which you can import into SillyTavern. They have several fields: description and first message, and (under the book icon) personality summary, scenario, and dialogue examples. Except for the last one, all the fields can have text data in any format. Though originally intended to encode character data, they're often used for scenarios instead, such as this [Fallout RPG Card](https://chub.ai/characters/mrnobody99/fallout-new-mexico).

#### Using Lorebooks/World Info

Lorebooks/World Info are basically dictionaries that store bits of text you want the AI to know about. They will either trigger on keywords that you set (such as the name of a character or spell), or they can be set to be always on. When triggered, the content of the entry will be sent to the AI as part of the prompt. They can be made recursive, so entries mentioning another entry's keyword will trigger that entry. Lorebooks are connected to character cards and can be embedded into them. Character cards that describe scenarios may keep their actual character data in a connected lorebook, which is also encoded in the card.

#### Settings Presets

Settings are the leftmost button at the top. Settings make a huuuge difference in the way the AI behaves, often making it feel like a totally different model. Don't be overwhelmed by the weird settings names, I mostly don't know what they mean either, but that's why Hashem gave us presets! I switch between NovelAI Ouroboros (very verbose, flowery), Storywriter (quite terse and straightforward), and Best Guess (in between). For manual tweaking, temperature = more randomness = more creativity, longer outputs etc., but less coherent the further up you go. And repetition penalty = shorter sentences, less likely to go off in loops, but might also end up being too constrained or terse.

#### Author's Notes

Author's Notes are freeform data you can add to the prompt that will be treated as extremely important by the AI. Characters have their own notes as well. They can be in any format, but I find that plist works well. Here's what I'm using for my current kink RP:

```
[Genre: erotica, drama; Style: terse, harsh, poetic, explicit; Scenario: {{user}} and {{char}} have met in {{char}}'s private suite to have sex.; System note: The sex will be kinky, violent, and rough. Strangling, blood play, ropes and restraints, hair pulling, huge dildos and fisting, slapping, etc. {{char}} should be detailed and creative, and not limited to usual kink scenarios. {{char}} well not skip or gloss over any actions, and {{char}} will never speak for {{user}} or describe {{user}}'s viewpoint.]
```

Then I add extra information as needed as the RP goes on, such as this:

```
[System note: your next message must be from {{char}}'s point of view as he begins to eat her out. Describe how she tastes and how he loves it and missed it. He will reminisce about the first time he did this to her, when she was younger, and how he convinced her to let him do it.]
```

#### Instruct Mode

TBA

## How TF does this work?

SillyTavern does a lot of magic behind the scenes to make RP chats work in a (mostly) seamless way. In order to figure out how to utilize it most effectively, you're gonna need to look behind the curtain at how it actually does the things it does.

#### Understanding Context Window and Tokens

Frontends like ChatGPT and SillyTavern make it look like the AI remembers what you said previously, but it doesn't: the whole chat history (or as much of it as will fit) is sent to the AI as the prompt every time you ask it to generate a reply, and it has no memory of anything outside of that, other than the data it was trained on. The size of the prompt that it's able to process at once is the "context window." If your character card + author's notes + chat history etc comes to exceed the context window, some ofthe chat history wil get truncated, leading to what looks a lot like short term memory loss. Most local language models have a small context window of 2048 to 4096 tokens (token = about 3/4 of a word), so it's important to be clever and economical with your tokens when it comes to characters, scenarios, etc., so you can fit as much of the chat history in as possible.

#### Anatomy of a Prompt

Here's an example of what's ACTUALLY happening behind the curtain when you send a message to an AI in SillyTavern (ew length ew het, I'll get a better example soon):

```
{
  prompt: `[Clara's persona: nicknamed "little bird", ghost of a rich Jazz Age debutante, James March's old flame, murdered by James March, sadist, masochist, kinky, confident, vivacious, sensual;\n` +
    "Clara's clothes: expensive low-waisted dress, side lace bra, cloche hat, rolled stockings;\n" +
    "Clara's body: beautiful, delicate build, deceptively strong, petite, hourglass figure, full breasts, tight pussy, auburn hair, hazel eyes;]\n" +
    "[James March's persona: the ghost of a Jazz Age millionaire serial killer, 35 years old, died in 1930, scars on his back, charismatic, theatrical, easily bored, hates god, hedonist, loyal, intelligent, sociable, engineer, architecture geek, amoral, aroused by torture and murder, manic, vigorous, cheerful, sadist, masochist, kinky, bisexual; James March's clothing: pinstripe suit, derby hat, collared shirt, suspenders; James March's body: medium height, lean build, solid and indistinguishable from a living human, black eyes, dark slicked back hair, pencil mustache, scars on his back]\n" +
    "James is the charismatic ghost of a Roaring 20's-era robber baron, sadist, and serial killer. He's been dead for over 60 years. He is highly educated, intelligent, and constantly understimulated. He is a self-made man who comes from a poor background with an abusive father. He has a vigorous, almost manic manner. He derives an almost orgasmic excitement and sense of satiation from murder.\n" +
    '\n' +
    "Summary: The text describes James March and Clara having sex in James's private suite. James is seducing Clara, who does not remember him but he remembers her. He wants to eat her out and strap her into torture devices, and asks her if she trusts him. She does, and he takes out a knife and begins to cut her underwear, revealing her bare pussy. He kisses her forcefully, pushing his tongue past her lips, and then begins to lick her pussy.\n" +
    "Clara: She shivers, her hands digging into the arms of her chair. She realizes she's panting as though she'd been running, for nothing but the adrenaline of that knife lightly skimming her skin.\n" +
    '\n' +
    'When he pulls away the sodden underwear, her pussy feels cool, vulnerable, and exposed. Every nerve ending in her tight sex tingles with painful want.\n' +
    'Clara tries to press her hips against his pain, but he draws back, drawing out a frustrated groan from her.\n' +
    '\n' +
    '"Not unless you do as I say," he says smoothly. "Whatever I say."\n' +
    'Clara: Her breath comes in gasps as he rests his hand on her wet cunt, fingers spread wide and ready to touch her again--but only if she relents and releases control.\n' +
    '\n' +
    "Clara hesitates, unsure if she can utter the words but knowing also they are the only way she'll ever find peace until she gives in.\n" +
    '\n' +
    '"Fine," she whispers shakily, the muscles of her belly fluttering with nervous anticipation.\n' +
    '\n' +
    'She feels him smile, knowing he can smell the fear and want radiating from her flesh.\n' +
    "James kisses her forcefully, pushing his tongue past her lips until she half-hysterically responds. It's an awful caricature of desire, more suited to horror films than lust--but it's effective nonetheless. His excitement flares, and James knows there are many, many ways he could torture her before finally bringing her to sexual release.\n" +
    '\n' +
    `"You won't regret this."\n` +
    '\n' +
    'Then he drops to his knees before her, pressing his face between her thighs.\n' +
    `Clara: "Oh please, no, don't--" but Clara's protest is silenced by the sensation of his cool tongue lapping at her exposed flesh.\n` +
    '\n' +
    'Clara feels herself buckle and fall back bonelessly in the armchair. Her pussy feels swollen and tight inside, suddenly needful for something more violent, more intense--anything to extinguish this fire his lips are working to kindle upon her sensitive skin. Her teeth grind together as he brushes two fingers beneath her skirts and finds her clit, pressing gently on it.\n' +
    '\n' +
    "The fire blooms wildly and unbearably inside her, her body trembling with pent-up need. There's nothing in her mind now but pure, feral want, an animalistic hunger.\n" +
    'James eagerly presses his lips and tongue to her folds, savoring the way her inner thighs, bracketing his head, twitch each time his thumb passes over her clitoris. She tastes just like he remembers: salt and mead and something uniquely Clara, some heady concoction that he lapped up freely when they were younger and more foolish.\n' +
    '\n' +
    'He licks gently, tracing lazy paths along her tight slit. Her body arches beneath him pleadingly, as if begging for mercy from a force far greater than anything either can acknowledge.\n' +
    '\n' +
    'He glances up to see her looking down at him, her face flushed and her lips parted. Then he thrusts his tongue inside of her, groaning a little at the way she responds to the push, her legs shaking and her hands scrambling for purchase around the armchair supports.\n' +
    '\n' +
    "[Genre: erotica; Style: terse, harsh, poetic, explicit; System note: Clara and James have met in James's private suite in the Hotel Cortez to have sex. The sex will be kinky, violent, and rough. Strangling, blood play, ropes and restraints, hair pulling, huge dildos and fisting, slapping, etc. James March should be detailed and creative, and not limited to usual kink scenarios. James March well not skip or gloss over any actions, and James March will never speak for Clara or describe her viewpoint. James is aware that Clara does not yet remember him, their relationship, or that she is a ghost. He will not inform her of it. He finds the courtship more interesting. To Clara, James is just someone she had sex with once, but she doesn't remember details.]\n" +
    '[System note: write James fingering Clara.]\n' +
    "Clara: Clara squirms and whimpers as his cool, clever tongue works her over. Had anyone ever done anything so intimate for her before? Her husband hadn't, that much was certain. Yet it feels so familiar that she can almost remember the room she was in--but she can't place it, not with James doing his best to drive her out of her mind with pleasure.\n",
  max_new_tokens: 200,
  do_sample: true,
  temperature: 0.72,
  top_p: 0.73,
  typical_p: 1,
  repetition_penalty: 1.1,
  encoder_repetition_penalty: 1,
  top_k: 0,
  min_length: 0,
  no_repeat_ngram_size: 0,
  num_beams: 1,
  penalty_alpha: 0,
  length_penalty: 1,
  early_stopping: false,
  seed: -1,
  add_bos_token: true,
  stopping_strings: [ '\nYou:', '\nClara:' ],
  truncation_length: 2048,
  ban_eos_token: false,
  skip_special_tokens: true,
  top_a: 0,
  tfs: 1,
  epsilon_cutoff: 0,
  eta_cutoff: 0
}
```

Breaking it down:

[Clara's persona: ] etc is the plist I created for Clara, the character I was playing for this chat. [James March's persona: ] etc is the plist from the description field of the character card I was chatting with, and "James is the charismatic ghost..." is the personality summary from that card. "Summary:" etc is the summary of the chat up to that point that was created by the SillyTavern Extras summary module (as a way of making up for the early parts of the chat that don't fit in the context window).

"Clara: She shivers... painful want." is the earliest message in the chat that made it into the context window, and everything from there to the next "Clara" is the AI's reply. It continues like that on down to "her hands scrambling for purchase around the armchair supports".

After that we see [Genre: erotica; Style: terse... she doesn't remember details.] and, on the next line, [System note: write James fingering Clara.]. Those are the Author's Note. It can be set to appear basically anywhere in the prompt, but in this case I set it to put that right before the most recent message in the chat, so that the AI will consider it especially important.

Finally, "Clara: Clara squirms and whimpers" is the most recent reply from me (the one that prompted the prompt to the AI).

After that, there are a ton of settings that you don't need to know about.

**That info is everything the AI knows. 100%. All of its "memory" is there, all of its context is there. Anything that isn't in it doesn't exist to the AI.**

#### Tense???

idk
