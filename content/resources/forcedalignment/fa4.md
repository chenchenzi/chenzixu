---
title: Use Montreal Forced Aligner
linktitle: 4. Use Montreal Forced Aligner
toc: true
type: docs
date: "2020-04-19T00:00:00+01:00"
draft: false
menu:
  forcedalignment:
    parent: Forced Alignment
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

## 4.1 Installation

The Montreal Forced Aligner (MFA) can be downloaded from [here](https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner/releases). They have a detailed [documentation](https://montreal-forced-aligner.readthedocs.io/en/latest/#), so I will only mention it briefly.

{{% alert note %}}
The stable version 1.0.1 does **NOT** work for my Mac(10.14.6), but Version 1.1.0 Beta 2 works well.
{{% /alert %}}

After you download the .zip archive and unzip it, you can open a terminal window and navigate to the MFA directory. You can then create an **input folder** where you will put all your `.wav` files and their corresponding `.txt` files and an **output folder** for the time-aligned `.Textgrid` files to be created.

{{% alert warning %}}
Never use your `/Desktop` or other important directories as the output directory! It is recommended that you create a new directory for it because MFA **DELETEs EVERYTHING** in the output directory. (What a lesson for me! My desktop was cleared in a second Oooops >.<)
{{% /alert %}}

The Montreal Forced Aligner has pretrained acoustic models and pretrained grapheme-to-phoneme (G2P) models for a number of languages. You can check them out [here](https://montreal-forced-aligner.readthedocs.io/en/latest/pretrained_models.html).

Download the pre-trained acoustic model and the G2P model if available and put them in `/MFA/pretrained_models/`.

{{% alert note %}}
**NO** need to unzip the model files.
{{% /alert %}}

## 4.2 Pronunciation Dictionary


The pronunciation dictionary here refers to a text file in which each line consisting of an orthographic transcription followed by the phonetic transcription. The phones in the dictionary must match the ones in the acoustic models and the orthography should match that in the transcripts.

If there is a pre-trained G2P model, we can generate a customised pronunciation dictionary from our transcripts. A Mandarin dictionary example:

```
bin/mfa_generate_dictionary pretrained_models/mandarin_character_g2p.zip input/ mandarin_dict.txt
```


## 4.3 Running Montreal Forced Aligner

When you have prepared the following, you're ready to go!
- [x] All `.wav` files are in 16KHz, 16-bit, mono channel
- [x] Each `.wav` file has a `.txt` transcript file with a matching filename, and they are put in the `/input/` folder
- [x] You have generated a pronunciation dictionary from all the transcripts
- [x] You have created an empty `/output/` folder
- [x] You downloaded or trained an acoustic model (in `.zip`) and put it in the `/pretrained_models/` folder.

Continuing from the previous example:

```
bin/mfa_align input/ mandarin_dict.txt pretrained_models/mandarin.zip output/
```

You can also use `mandarin` above without the `.zip` extension when you have downloaded the pre-trained model.

MFA can also train your own acoustic models and align using only the data set.
```
bin/mfa_train_and_align input/ mandarin_dict.txt output/
```
Use the flag `-o PATH` to save the acoustic model for future use.

I assume a large dataset would be helpful. I tried this option but my own dataset is not big enough so the alignment tended to derail.