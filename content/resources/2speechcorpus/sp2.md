---
title: Create query scripts
linktitle: 2. Creating query scripts
toc: true
type: docs
date: "2021-12-15T00:00:00+01:00"
draft: false
menu:
  speechcorpus:
    parent: Speech Corpus Query
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## Perl snippet 
An ideal way to organise our speech corpus directory is demonstrated below. The plain transcript (`.txt`) and time-aligned transcription file (`.TextGrid`) share the same filename as the corresponding audio file (`.wav`). Designing a consistent, anonymous, interpretable, and information-dense filename system is always a good practice here.

```
speech_corpus
├── metadata.txt
├── audios
│   ├── b01_1_101q.wav
│   ├── b01_2_101q.wav
│   └── b01_3_101q.wav
├── textgrids
│   ├── b01_1_101q.TextGrid
│   ├── b01_1_102a.TextGrid
│   └── b01_1_103a.TextGrid
└── transcripts
    ├── b01_1_101q.txt
    ├── b01_2_101q.txt
    └── b01_3_101q.txt
```

The first step that enables us to search intended speech sequences from the corpus is to create a large text file assembling all time-aligned transcripts so that we have access to two key information for all audio files: temporal information and the corresponding symbol for the speech unit (it can be a segment, a syllable, or a word given the granularity of the segmentation.)

## Python query script

Usually a TextGrid file, as shown below, is not in its best format to work with. In the previous [tutorial](https://chenzixu.rbind.io/resources/1forcedalignment/fa5/), I provided Python scripts that convert a `.TextGrid` file into a plain text file with tabular format data. Again, they are available at my Github [repository](https://github.com/chenchenzi/textgrid2table). The `README.md` will take you from there.

If you would like to convert multiple `.TextGrid` files all at once, you can consider a for loop in your command line and then concatenate individual `.txt` files into a large text file.

```

```

Alternatively, I created another Python script `tg2csv.py` that loops round the `\textgrid\` directory and create one large text file in the output. It is also available at my Github [repository](https://github.com/chenchenzi/textgrid2table).

```



```

The key to creating this assembling text file in the tabular format is **forward thinking**. What information will you need in the next steps? In order to cut speech segments out from a audio file, we will need the filename (path) of the audio file, and the times of the speech segments. The correspondence between the filenames of an audio file and a transcript does us a favor in accessing the path of the audio file when we have the transcript file. Therefore, a column of filename should be in the tabular data, and we might also want to remove the file extension, which would make it easier to work with different file extensions.

### Demo task
Suppose that I am interested in some Mandarin syllables and my `.TextGrid` files are time-aligned at both the segmental(tier name: phoneme) and syllabic level(tier name: word). I hope to convert all the `word` tiers into tabular format. 

`tg2csv.py` should be placed in the project directory, i.e. `/speech_corpus` in above example. It takes three arguments: 1) the name of the directory where we put the `.TextGrid` files, in this case, `textgrids`; 2) the name of the tier that we are interested in, `word`; 3) the name of the desired output file. Let's call it `words.csv`.

In the terminal or your Unix Shell, we can do:
```
$ python tg2csv.py textgrids word words.csv
```
The final tabular output of `tg2csv.py` is demonstrated below. You may delete the first row `0,1,2,3,4` in the output file, which is the unspecified column names.

```
妈,0.0125,0.3325,0.3200,b01_1_101q
妈,0.3325,0.5125,0.1800,b01_1_101q
们,0.5125,0.7925,0.2800,b01_1_101q
正,0.7925,1.1125,0.3200,b01_1_101q
看,1.1125,1.6725,0.5600,b01_1_101q
```

The `$` isn't part of the command. It indicates that this is a Shell script in the Terminal.

## 1.2 SoX
Alternatively, you can also use `SoX` (Sound eXchange) commands in the Terminal. SoX is a collection of handy sound processing utilities. It is also required by P2FA. You can download it [here](http://sox.sourceforge.net/). To reformat the input.wav, we can use the following command in the Terminal having installed SoX.
```
$ sox input.wav -r 16k -b 16 -c 1 output.wav
```
The `$` isn't part of the command. It indicates that this is a Shell script in the Terminal. The flags here: `-r` , `-b`, `-c` define the sampling rate, precision, and number of channel of the output.wav respectively.