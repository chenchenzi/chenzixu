---
title: "New Update: A Gentle Guide to Montreal Forced Aligner"
linktitle: "6. New Update: A Gentle Guide to Montreal Forced Aligner"
toc: true
type: docs
date: "2020-04-19T00:00:00+01:00"
draft: false
menu:
  forcedalignment:
    parent: Forced Alignment
    weight: 6

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 6
---

It has been a few years since my previous tutorial on the Montreal Forced Aligner (MFA). MFA is continuously evolving and becoming increasingly powerful. In this new tutorial, I would like to introduce a **more sophisticated general workflow** for producing time-aligned phone boundaries for languages that have a **pretrained acoustic model and pretrained dictionary**, especially if you are working with large datasets. 

*The following workflow has been tested on M-chip Macs and Linux (montreal-forced-aligner v3.0.0+).

- **(Advanced) MFA Forced Alignment Workflow**
    - [6.1. Organisation of working directory and sanity check.](#61-organisation-of-working-directory-and-sanity-check)
    - [6.2. Setting up the Montreal Forced Aligner.](#62-setting-up-the-montreal-forced-aligner)
      - [Install MFA and activate conda environment.](#install-mfa-and-activate-conda-environment)
      - [Download pretrained models.](#download-pretrained-models)
    - [6.3. Generating input TextGrids from transcripts.](#63-generating-input-textgrids-from-transcripts)
    - [6.4. Data validation and solutions for OOVS](#64-data-validation-and-solutions-for-oovs)
      - [Obtaining the Out-Of-Vocabulary items(OOVS) list](#obtaining-the-out-of-vocabulary-itemsoovs-list)
      - [Backtracking easy-to-fix typos](#backtracking-easy-to-fix-typos)
      - [Generating pronunciation dictionary for remaining OOVs](#generating-pronunciation-dictionary-for-remaining-oovs)
    - [6.5. MFA alignment.](#65-mfa-alignment)
      - [Initial run](#initial-run)
      - [Second run with updated dictionary](#second-run-with-updated-dictionary)
      - [Third run with a larger beam parameter](#third-run-with-a-larger-beam-parameter)
      - [Fourth run with adapted acoustic model to new data](#fourth-run-with-adapted-acoustic-model-to-new-data)


## 6.1. Organisation of working directory and sanity check

An example structure of the working directory is as follows.

   ```
   project/
   ├── corpus
   │   ├── recording1.wav
   │   ├── recording1.TextGrid
   │   ├── recording2.wav
   │   ├── recording2.TextGrid
   │   └── ...
   ├── txts
   ├── output   
   └── text2tg.py
   ```

All `.wav` audio files of a corpus are in the `/corpus/` directory, and all transcript files are in the `/txts/` directory. The generated input TextGrid files can be added to the `/corpus/` directory.

## 6.2. Setting up the Montreal Forced Aligner

#### Install MFA and activate conda environment

   ```bash
   conda config --add channels conda-forge #enable the conda-forge channel by default 
   conda install montreal-forced-aligner
   
   conda activate aligner # create a new environment for forced alignment
   ```
   
For Conda installation, check [here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html). 
Feel free to check out the [official MFA installation guide](https://montreal-forced-aligner.readthedocs.io/en/latest/installation.html).

To **update** your MFA:
   ```bash
   conda update -c conda-forge montreal-forced-aligner kalpy kaldi=*=cpu* --update-deps
   ```

#### Download pretrained models

   ```bash
   mfa model download acoustic mandarin_mfa
   mfa model download dictionary mandarin_china_mfa
   mfa model download g2p mandarin_china_mfa # if needed
   ```

There are a few different [pretrained dictionaries](https://mfa-models.readthedocs.io/en/latest/dictionary/Mandarin/index.html), which can be used for Standard Mandarin, Beijing Mandarin, and Taiwan Mandarin, please check them out.

The Montreal Forced Aligner has many pretrained models for a number of languages. You can check them out [here](https://mfa-models.readthedocs.io/en/latest/).

## 6.3. Generating input TextGrids from transcripts

In many scenarios, the transcript file for each audio recording is in a plain text `.txt` format with a matching filename as the audio. It is recommended to convert transcripts in plain text file to input `.TextGrid` files (all texts in one tier), **with the tier names being the speaker ID**. MFA automatically includes speaker adaptation. This can be achieved using a customised Python script, which depends on the specific format of the transcript file.

For example, in my ASR tutorial that works on datasets from Common Voice, I created a `cv15_totgs.py` [script](https://chenzixu.rbind.io/resources/3asr/sr4/#432-transcripts-preparation-initial-textgrids) that generates input TextGrids for forced alignment.


   > :clipboard:  **Tips for Input Transcript Text** 
   >
   > - Unknown sounds, laughter, and coughing can be represented as {LG}
   > - Non-speech vocalizations that are similar to silence such as breathing and exhalation can be represented as {SL}
   > - `sil` and `spn` are two special phones for non-speech annotations recognised by pre-trained MFA dictionaries.
   > ```
   > {LG}   spn
   > {SL}   sil
   >```
   > - I tend to remove all the punctuations in Chinese transcripts.
   > - Adding a space between Chinese characters can enable syllable-level alignment.


## 6.4. Data validation and solutions for OOVS

Validate the prepared data and pretrained models before the actual alignment. This command `mfa validate` parses the corpus and dictionary, generates a nice report summarising information about your corpus, and logs potential issues including the out-of-vocabulary items (OOVs), i.e. any words in transcriptions that are not in the dictionary.

   ```bash
   cd (your project path)
   mfa validate corpus/ mandarin_china_mfa mandarin_mfa
   ```

#### Obtaining the Out-Of-Vocabulary items(OOVS) list

In the latest version of MFA, you can find a list of the OOVs and which utterances they appear in your MFA folder. Most likely on a Mac, you can find them in your `/Documents/MFA/(your project)/oovs_found_xxx.txt`. It will be empty if there is no OOV.

#### Backtracking easy-to-fix typos

Sometimes missing words in the dictionary results from spelling mistakes. Using the information in `/Documents/MFA/(your project)/oovs_utterances.txt` from the validation output, we can backtrace the words with typos and fix them in the transcript texts.

#### Generating pronunciation dictionary for remaining OOVs

For remaining OOVs that are saved as a `oovs.txt`, we can generate a dictionary entry for each OOV using the pre-trained Grapheme-to-Phoneme (G2P) model.

   ```bash
   cd Users/chenzi/Documents/MFA/project # your project directory inside MFA directory
   
   mfa g2p [OPTIONS] INPUT_PATH G2P_MODEL_PATH OUTPUT_PATH
   
   mfa g2p oovs.txt mandarin_china_mfa oovs.dict

   # add probabilities to a dictionary (**optional**)
   mfa train_dictionary [OPTIONS] CORPUS_DIRECTORY DICTIONARY_PATH
                     ACOUSTIC_MODEL_PATH OUTPUT_DIRECTORY
                     
   mfa train_dictionary --clean ~/project/corpus/ oovs.dict mandarin_mfa ~/project/
   
   #combine pretrained dictionary and dictionary for OOVs
   cat oovs.dict ~/Documents/MFA/pretrained_models/dictionary/mandarin_china_mfa.dict > ~/project/mandarin_new.dict
   ```

## 6.5. MFA alignment.

When there is no other issues after the validation, we can now start forced alignment. You can try to adjust the parameters and compare the output TextGrids.

#### Initial run

   ```bash
   mfa align corpus/ mandarin_china_mfa mandarin_mfa output/tgs0/
   ```

#### Second run with updated dictionary
   
   ```bash
   mfa align corpus/ mandarin_new.dict mandarin_mfa output/tgs1/
   ```

#### Third run with a larger beam parameter (retry beam = 100)

When you have very long transcript for each audio file, I would suggest that you use a larger beam size in decoding, which originally defaults to 10. This of course will increase the alignment time.

   ```bash
   mfa align --clean corpus/ mandarin_new.dict mandarin_mfa output/tgs2/ --beam 100
   ```

#### Fourth run with adapted acoustic model to new data

You can also fine-tune the pretrained acoustic model to see if it gives better alignment results.

   ```bash
   mfa adapt --clean corpus/ mandarin_new.dict mandarin_mfa output/model/ output/tgs3/ --beam 100
   ```





