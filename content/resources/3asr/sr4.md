---
title: "ASR from Scratch II: Training models of Hong Kong Cantonese with MFA implementation"
linktitle: "4. ASR from Scratch II: Training models of Hong Kong Cantonese with MFA implementation"
toc: true
type: docs
date: "2023-6-20T00:00:00+01:00"
draft: false
menu:
  speechrecognition:
    parent: Speech Recognition
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

<br> 

In the previous extensive chapter *ASR from Scratch I*, I have demonstrated how to train acoustic models of Hong Kong Cantonese using (source) Kaldi ASR. This chapter achieves the same goal with the help of the Montreal Forced Aligner (MFA), which is also based on Kaldi but a more streamlined process.

Some pre-trained acoustic models of Hong Kong Cantonese are now available in my Github {{< icon name="github" pack="fab" >}}[HKCantonese_models](https://github.com/chenchenzi/HKCantonese_models).

**Table of Contents**
- [4.1 MFA Installation](#41-mfa-installation)
- [4.2 The Common Voice Dataset](#42-the-common-voice-dataset)
- [4.3 Data Preprocessing](#43-data-preprocessing)
  - [4.3.1 Audio preprocessing: `.mp3` to `.wav`](#431-audio-preprocessing-mp3-to-wav)
  - [4.3.2 Transcripts preparation: initial TextGrids](#432-transcripts-preparation-initial-textgrids)
  - [4.3.3 The dictionary `lexicon.txt`: Cantonese G2P](#433-the-dictionary-lexicontxt-cantonese-g2p)
- [4.4 Training acoustic models using MFA](#44-training-acoustic-models-using-mfa)

<br> 

## 4.1 MFA Installation

MFA is built on Conda Forge, so we will install MFA via Conda. Install [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html) or [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/) first if you haven't done it.

Then in your Unix Shell, or Terminal on a Mac, create a new environment and install MFA:
```bash
conda create -n aligner -c conda-forge montreal-forced-aligner
conda activate aligner
```
For more information, see the official installation [guide](https://montreal-forced-aligner.readthedocs.io/en/latest/installation.html).

<br> 

## 4.2 The Common Voice Dataset

We will be using the same dataset as that in [**§ 3.3** in *ASR from Scratch I*](https://chenzixu.rbind.io/resources/3asr/sr3/#33-the-common-voice-dataset), the latest `Chinese (Hong Kong)` subset of the publicly available Common Voice corpus `Common Voice Corpus 15.0` updated on 9/14/2023.
You can download the dataset [here](https://commonvoice.mozilla.org/en/datasets) and unzip it to your working directory. The downloaded corpus has the following structure:

```
├── cv-corpus-15.0-2023-09-08
... └── zh-HK
        ├── clip_durations.tsv
        ├── train.tsv
        ├── validated.tsv
        ├── dev.tsv
        ├── test.tsv
        ├── Invalidated.tsv
        ├── other.tsv
        ├── reported.tsv
        ├── times.txt
        └── clips
            ├── common_voice_zh-HK_20096730.mp3
            ├── common_voice_zh-HK_20096731.mp3
            ├── common_voice_zh-HK_20096732.mp3
            ├── ...
            └── ... #118736 items in total
```

<br> 

## 4.3 Data Preprocessing

### 4.3.1 Audio preprocessing: `.mp3` to `.wav`

While the compressed format `.mp3` is storage-friendly, we should use `.wav` for acoustic modeling and training.
Inside the Common Voice directory `cv-corpus-15.0-2023-09-08/zh-HK/`, I created a new directory `clips_wavs/` for converted `.wav` files.

I wrote a python script `mp3towav.py`, located in the same directory as the Common Voice corpus directory `cv-corpus-15.0-2023-09-08/zh-HK/`, to convert the audio format from `.mp3` to `.wav` with 16K sampling rate, using `sox`. The Python package `subprocess` enables running the external `sox` command in parallel.

```python
# my3towav.py
# Created by Chenzi Xu on 30/09/2023

import re
import os
from tqdm import tqdm
import subprocess
from tqdm.contrib.concurrent import process_map

path = 'cv-corpus-15.0-2023-09-08/zh-HK/clips'
output = 'cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs'

file_pairs = [(file,re.search(r'(.*?)\.mp3',file).group(1)+'.wav') for file in tqdm(os.listdir(path))]

def convert_and_resample(item):
    command = ['sox', os.path.join(path,item[0]),'-r','16000',os.path.join(output,item[1])]
    subprocess.run(command)

if __name__ == '__main__':
    wavs = process_map(convert_and_resample, file_pairs, max_workers=4, chunksize=1)
```
{{% alert note %}}
To use the Python scripts in this tutorial, make sure to modify the path variables so that they match the file structure on your machine.
{{% /alert %}}

In this tutorial, we will be using the `train` subset of this Common Voice Hong Kong Cantonese corpus, consisting of 8426 audio recordings of short utterances (fewer than 30 syllables). I created another new directory `train_wavs/` inside the Common Voice corpus directory `cv-corpus-15.0-2023-09-08/zh-HK/`. The following Python script `cv15_select_wavs` selects recordings that belong to the `train` subset and saves them in a new directory `train_wavs/`.

```python
# cv15_select_wavs.py
# Created by Chenzi Xu on 30/09/2023

import pandas as pd
import subprocess
import os

dir = 'cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs'
train_dir = 'cv-corpus-15.0-2023-09-08/zh-HK/train_wavs'

cv_tsv = pd.read_csv('cv-corpus-15.0-2023-09-08/zh-HK/train.tsv', sep='\t', header=0)

def move(item):
    item = item[:-4] + '.wav'
    command = ['mv', os.path.join(dir, item), os.path.join(train_dir, item)]
    subprocess.run(command)

cv_tsv['path'].apply(move)
```

I have also set up a working directory for this mini project `~/Work/mfa-canto`, and moved the `train_wavs/` directory there to house the corpus data files for training acoustic models.

### 4.3.2 Transcripts preparation: initial TextGrids

The following Python script `cv15_totgs.py` prepares the initial transcript files for training acoustic models in MFA. It creates a corresponding `.TextGrid` file for each audio recording, and writes in the transcription in a processed format: ❶ all punctuation marks are removed; ❷ Chinese characters / morphemes or English words are separated with a space. Furthermore, the tier name of transcriptions is indicated by `client_id` so that transcripts belonging to the same speaker has the same tier name.

```python
# cv15_totgs.py
# Created by Chenzi Xu on 30/09/2023

import pandas as pd
import re
from praatio import textgrid

dir = '/Users/cx936/Work/mfa-canto/train_wavs/'
cv_tsv = pd.read_csv('cv-corpus-15.0-2023-09-08/zh-HK/train.tsv', sep='\t', header=0)

cv_tsv = cv_tsv[['client_id', 'path', 'sentence']]
# remove punctuation
cv_tsv['sentence']=cv_tsv['sentence'].apply(lambda x:re.sub(r'[^\u4e00-\u9FFFa-zA-Z0-9 ]', '', x))
# add space between Chinese characters
cv_tsv['sentence']=cv_tsv['sentence'].apply(lambda x: re.sub(r'([\u4e00-\u9fff])', r'\1 ', x).strip())
# add space after an English word followed by a Chinese character
cv_tsv['sentence']=cv_tsv['sentence'].apply(lambda x: re.sub(r'([a-zA-Z0-9_]+)([\u4e00-\u9fff])', r'\1 \2', x))
dur = pd.read_csv('cv-corpus-15.0-2023-09-08/zh-HK/clip_durations.tsv', sep='\t', header=0)

df = pd.merge(cv_tsv, dur, left_on='path', right_on='clip')

for index, row in df.iterrows():
    try:
        tg_path = dir + row['path'][:-4] + '.TextGrid'
        entry = (0, row['duration[ms]']/1000, row['sentence'])
        #print(entry)
        wordTier = textgrid.IntervalTier(row['client_id'], [entry], 0, row['duration[ms]']/1000)
        tg = textgrid.Textgrid()
        tg.addTier(wordTier)
        tg.save(tg_path, format="short_textgrid", includeBlankSpaces=True)
    except Exception as e:
        print("Failed to write file",e)
```

### 4.3.3 The dictionary `lexicon.txt`: Cantonese G2P

We will need a Cantonese pronunciation dictionary `lexicon.txt` of the words/characters, in fact, **only** the words, present in the training corpus. This will ensure that we do not train extraneous phones. If we want to use IPA symbols for acoustic models, we should transcribe the words/characters in IPA in this dictionary. 

We first get all the transcripts from the `train.tsv` file:

```python
# cv15_getscript.py
# Created by Chenzi Xu on 30/09/2023

import pandas as pd
import re

dir = '/Users/cx936/Work/mfa-canto/train_wavs/'
cv_tsv = pd.read_csv('cv-corpus-15.0-2023-09-08/zh-HK/train.tsv', sep='\t', header=0)

cv_tsv = cv_tsv[['sentence']]
# remove punctuation
cv_tsv['sentence']=cv_tsv['sentence'].apply(lambda x:re.sub(r'[^\u4e00-\u9FFFa-zA-Z0-9 ]', '', x))
# add space between Chinese characters
cv_tsv['sentence']=cv_tsv['sentence'].apply(lambda x: re.sub(r'([\u4e00-\u9fff])', r'\1 ', x).strip())
# add space after an English word followed by a Chinese character
cv_tsv['sentence']=cv_tsv['sentence'].apply(lambda x: re.sub(r'([a-zA-Z0-9_]+)([\u4e00-\u9fff])', r'\1 \2', x))

cv_tsv.to_csv('transcripts.txt', index=False, header=False)
```

We find the list of unique words/characters in the training corpus:
```
cut -f 2 transcripts.txt | sed 's/ /\n/g' | sort -u > words.txt
```

We can then download an open Cantonese dictionary from {{< icon name="github" pack="fab" >}} [CharsiuG2P](https://raw.githubusercontent.com/lingjzhu/CharsiuG2P/main/dicts/yue.tsv) and utilise the multilingual [CharsiuG2P](https://github.com/lingjzhu/CharsiuG2P) tool with a pre-trained Cantonese model for grapheme-to-phoneme conversion.

Generally for a dictionary file, we want ❶ each phone to be separated by a space. ❷ The tone label in `yue.tsv` is always put at the end of an IPA token, which gives an impression of tone being a linearly arranged segment. Tone, however, is suprasegmental. We might want to exclude the tone labels here. ❸ We can have multiple pronunciation entries for a word, which are usually put in different rows. ❹ We need to add the pseudo-word entries following the [MFA non-speech annotation convention](https://montreal-forced-aligner.readthedocs.io/en/latest/user_guide/dictionary.html#non-speech-annotations).
such as `{LG}` and `{SL}`. `{LG} spn` is used to model unknown words or sounds including coughing and laughter, `{SL} sil` is used to model silence, or non-speech vocalizations that are similar to silence like breathing or exhalation.

Therefore, we need to revise the format of a downloaded open dictionary. The following python script `canto_g2p.py` creates a `lexicon.txt` file using `CharsiuG2P` and their open dictionary. Then we manually added the pseudo-word entries.

```python
# canto_g2p.py
# Created by Chenzi Xu on 30/09/2023

from transformers import T5ForConditionalGeneration, AutoTokenizer
from tqdm import tqdm
import pandas as pd
from lingpy import *

# load G2P models
model = T5ForConditionalGeneration.from_pretrained('charsiu/g2p_multilingual_byT5_small_100')
tokenizer = AutoTokenizer.from_pretrained('google/byt5-small')
model.eval()

# load pronunciation dictionary
pron = {l.split('\t')[0]:l.split('\t')[1].strip() for l in open('yue.tsv','r',encoding="utf-8").readlines()}

with open('lexicon.txt','w', encoding='utf-8') as output:
    
    rows=[]
    with open('words.txt','r',encoding='utf-8') as f:
        for line in tqdm(f):
            w = line.strip()
            word_pron = ''
            if w in pron:
                word_pron+=pron[w]
            else:
                out = tokenizer(['<yue>: '+w],padding=True,add_special_tokens=False,return_tensors='pt')
                preds = model.generate(**out,num_beams=1,max_length=50)
                phones = tokenizer.batch_decode(preds.tolist(),skip_special_tokens=True)
                word_pron+=phones[0]
            
            rows.append([w,word_pron])
    
    lexicon = pd.DataFrame(rows, columns=['word', 'ipa'])
    lexicon['ipa'] = lexicon['ipa'].str.split(',')
    lexicon = lexicon.explode('ipa')
    
    #remove IPA tones and tokenize IPA-encoded strings
    lexicon['ipa'] = lexicon['ipa'].str.replace(r'[\u02E5-\u02E9]+', '', regex=True)
    lexicon['ipa'] = lexicon['ipa'].apply(lambda x: ' '.join(map(str, ipa2tokens(x))))

    #remove duplicated rows if any
    lexicon.drop_duplicates(inplace=True)
    lexicon.to_csv(output,sep='\t', index=False, header=False)
```

The final dictionary is as follows: 
```
{LG}	spn
{SL}	sil
A	a:
Annual	a: nn ʊ ŋ
Anson	a: n s ɔ: n
B	b i:
Browser	pʰ r ɔ: w s ɐ
...
一	j ɐ t
丁	t s a: ŋ
丁	t ɪ ŋ
丁	t s ɐ ŋ
...
```

We can then move this `lexicon.txt` file to our MFA project directory at `~/Work/mfa-canto/`. 

Now the working directory for this MFA project has the following structure:

```
.
├── alignment/
├── lexicon.txt # pronunciation dictionary
└── train_wavs # audio data and transcripts
    ├── common_voice_zh-HK_20099684.TextGrid
    ├── common_voice_zh-HK_20099684.wav
    ├── common_voice_zh-HK_20099796.TextGrid
    ├── common_voice_zh-HK_20099796.wav
    ├── common_voice_zh-HK_20099797.TextGrid
    ├── common_voice_zh-HK_20099797.wav
    ├── ...
```

<br>

## 4.4 Training acoustic models using MFA

Before we start, use the `mfa validate` command to look through the training corpus, `train_wavs/` in our case, and to make sure that the dataset is in the proper format for MFA.

```
mfa validate ~/Work/mfa-canto/train_wavs ~/Work/mfa-canto/lexicon.txt
```
The output of this command reports on aspects of the training corpus including the number of speakers, the number of utterances, the total duration, the missing transcriptions or audio files if any, the Out of Vocabulary (oov) items if any, etc. You can see the first 22 INFO lines printed in the Unix Shell below:

```bash
 INFO     Setting up corpus information...                                  
 INFO     Found 288 speakers across 8426 files, average number of utterances per speaker: 29.256944444444443      
 INFO     Jobs already initialized.                                         
 INFO     Text already normalized.                                          
 INFO     Features already generated.                                       
 INFO     Corpus                                                            
 INFO     8426 sound files                                                  
 INFO     8426 text files                                                   
 INFO     288 speakers                                                      
 INFO     8426 utterances                                                   
 INFO     34249.574 seconds total duration                                  
 INFO     Sound file read errors                                            
 INFO     There were no issues reading sound files.                         
 INFO     Feature generation                                                
 INFO     There were no utterances missing features.                        
 INFO     Files without transcriptions                                      
 INFO     There were no sound files missing transcriptions.                 
 INFO     Transcriptions without sound files                                
 INFO     There were no transcription files missing sound files.            
 INFO     Dictionary                                                        
 INFO     Out of vocabulary words                                           
 INFO     There were no missing words from the dictionary. If you plan on using the a model trained on this       
          dataset to align other datasets in the future, it is recommended that there be at least some missing    
          words.        
 ...
```
The above output indicates that we passed our data validation. If there are any missing files or the number of speakers is incorrect, you will need to fix the problems and run `mfa validate` again. Oscillate between these two steps until you have validated your data.

The MFA command for training a new acoustic model is `mfa train`, which takes three arguments:
```
mfa train [OPTIONS] <corpus_directory> <dictionary_path> <output_model_path>
```
For more details, see the official [guide](https://montreal-forced-aligner.readthedocs.io/en/latest/user_guide/workflows/train_acoustic_model.html#mfa-train).

I have added an optional argument `--output_directory` to put the output TextGrids for our training data.

```
mfa train ~/Work/mfa-canto/train_wavs ~/Work/mfa-canto/lexicon.txt ~/Work/mfa-canto/new_acoustic_model.zip --output_directory ~/Work/mfa-canto/alignment
```

If you see the following few lines at the end of the Shell output, congratulations 🎉 on completing training the acoustic model.

```
 ...
 INFO     Training model...                                                 
 INFO     Completed training in 27031.43283891678 seconds!                  
 INFO     Saved model to /Users/cx936/Work/mfa-canto/new_acoustic_model.zip 
 WARNING  Alignment analysis not available without using postgresql         
 INFO     Exporting sat_3_ali TextGrids to /Users/cx936/Work/mfa-canto/alignment...                               
 100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╸ 8,401/8,426  [ 0:00:30 < 0:00:01 , 293 it/s ]
 INFO     Finished exporting TextGrids to /Users/cx936/Work/mfa-canto/alignment!                                  
 INFO     Done! Everything took 27140.052 seconds
```

An example of the TextGrid output is as follows:
{{< figure library="true" src="can-fa.png" title="Time-aligned phones for `common_voice_zh-HK_20099684.wav`" style="width: 10%">}}

The `train` subset of the corpus is not very big, with total duration of 9.5 hours, but the time alignment is in fact already looking very good with 9 hours of training data. We can use the whole validated subset of the HK Cantonese Common Voice corpus to train a better model, in the same workflow.