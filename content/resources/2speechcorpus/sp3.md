---
title: Create audio-trimming scripts
linktitle: 3. Creating trim scripts
toc: true
type: docs
date: "2021-12-15T00:00:00+01:00"
draft: false
menu:
  speechcorpus:
    parent: Speech Corpus Query
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

Now that we have picked out the relevant syllables from the assembled text file, the third step is to create a trim script to cut the audio files by employing the information in the text file.

## SoX for audio trimming
Here we make use of `SoX` (Sound eXchange) to achieve the audio extraction. SoX is a collection of handy sound processing utilities. You can download it [here](http://sox.sourceforge.net/). 

The syntax for trimming audio file is:
```
$ sox input.wav out.wav trim starting_time(s) =ending_time(s)
```
Given the usage, we will create a audio trimming script that extracts all the targeted phrases in one go. Thus we need to arrange the information according to the sox syntax. This can be done in the Terminal or Unix shell.

### Demo task
Suppose that I hope to find all Mandarin phrases in which the second syllable is the functional particle "*de*" 的, which is commonly used as a possessive modifiers or nominaliser, in the corpus.

```
cat de_phrases.txt | awk '{print "sox " $5 ".wav ../xde_pm/" $5 $1 $6 "s" $7 "e" $8 ".wav trim " $2 " =" $8}' > de_trim.txt
```

```
sox b08_1_114a.wav ../xde_pm/b08_1_114a大的s0.1225e0.2060.wav trim 0.0125 =0.2060
sox b02_1_119q.wav ../xde_pm/b02_1_119q搭的s0.3725e0.4638.wav trim 0.2351 =0.4638
sox b03_3_118a.wav ../xde_pm/b03_3_118a回的s0.2397e0.3125.wav trim 0.1015 =0.3125
```