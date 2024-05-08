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
      - [Re-running the validation, and get an updated list of OOVS](#re-running-the-validation-and-get-an-updated-list-of-oovs)
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

In many scenarios, the transcript file for each audio recording is in a plain text `.txt` format with a matching filename as the audio. It is recommended to convert transcripts in plain text file to input `.TextGrid` files (all texts in one tier), with the tier names being the speaker ID. MFA automatically includes speaker adaptation. This can be achieved using a customised Python script `text2tg.py`, which depends on the format of the transcript file.

Here I have attached an example 

   ```python
   python text2tg.py <directory_name>
   ```

   > :clipboard:  **USAGE of df2tg2.py**\* 
   >
   > *Name*: dataframe(df) to(2) textgrid(tg); \*last digit 2 means version 2.
   >
   > *Function*: Consider the original transcripts as dataframe and transform them into sentence-level aligned input textgrid files for forced alignment. 
   > - Words tagged with (LAND-xx)\*, (LGH), (GRB), (OTHER), ??, are transformed into {LG}, unknown sound label recognised by MFA. 
   > - Filler words such as (eh) and (mmm) have brackets removed and labels matching with the dictionary.
   > - A few transcript files in 1_see_20/ corpus such as MDCIUKENG57571602, MDCIUKENG13215701, and MDCIUKENG94352301 had label sets\[colloquial\]\[/colloquial\], which wouldn't be recognised by MFA and were thus removed \*. 
   > - Punctuations such as '~', '!!', and '_' were removed or turned into space. Replacing `_` with space can be very helpful in breaking phrases into individual words, but sometimes `_` breaks acronym words into individual letters, e.g., `M_A_C` --> `M A C`, which may not be the best. 
   > - File MDCIUKENG86722001.txt has a unique label \[silence\]\*, which was transformed into {SL}, together with other labels including (BACKGROUND), (BRT), (NOISE).
   > - Special characters in other languages including í, à, ü(LANG-xx) \* need to be taken care of.
   >t
   > *Packages needed*: `Praatio`, `librosa`, `pandas`, `os`, `sys`, `re`
   >
   > *Usage example*:
   > In terminal, write:
   > ```
   > python df2tg2.py 1_see_200 > tgslog_see200.txt
   > ```
   > where `1_see_20` is the directory of Southeastern English 20 hours corpus.


## 6.4. Data validation and solutions for OOVS

   ```bash
   mfa validate corpus/ mandarin_china_mfa mandarin_mfa
   ```

#### Obtaining the Out-Of-Vocabulary items(OOVS) list

   ```bash
   cat oovs_*.txt >> oovs.txt
   cat oovs.txt|sort|uniq >oovs_v1.txt
   ```
Scanning the list, we can identify three categories of words that can be better processed in Step 3:
 - Common words with consistent typos, i.e. `c-`; 
 - Common words marked with random labels such as `[colloquial]`, `[silence]`,`(silence)`, etc. These are not annotated in `HO_Collection_TranscriptionNotes.txt`;
 - Words that marked with foreign language or with foreign accent marker.

For the latter two categories, we can go back to Step 3 and update the script `df2tg2.py` (already updated above; **\*** indicates the added or updated transformation rules). Words with a `(lang-xx)` marker can be turned into a `{LG}` , the unknown words marker, using regular expression. Random labels are either deleted or turned into `{SL}`, the silence marker.
   

#### Re-running the validation, and get an updated list of OOVS
   ```bash
   mfa validate corpus/ english_uk_mfa english_mfa 
   #change sub-corpus here or use viking script

   cat oovs_*.txt >> oovs.txt
   cat oovs.txt|sort|uniq >oovs_v2.txt
   ```

#### Generating pronunciation dictionary for remaining OOVs

   ```bash
   mfa g2p english_uk_mfa oovs_v2.txt oovs.dict

   # add probabilities to a disctionary (optional)
   mfa train_dictionary --clean corpus/ oovs.dict english_mfa output/
   cat oovs.dict english_uk_mfa.dict > english_uk_up.dict
   ```

## 6.5. MFA alignment.

#### Initial run

   ```bash
   mfa align corpus/ english_uk_mfa english_mfa output/tgs0/
   ```

#### Second run with updated dictionary
   
   ```bash
   mfa align corpus/ english_uk_up.dict english_mfa output/tgs0/
   ```

#### Third run with a larger beam parameter (retry beam = 400)

   ```bash
   mfa align --clean corpus/ english_uk_mfa english_mfa output/tgs2/ --beam 400
   ```


#### Fourth run with adapted acoustic model to new data

   ```bash
   mfa adapt --clean corpus/ english_uk_up.dict english_mfa output/model/ output/tgs3/ --beam 400
   ```





