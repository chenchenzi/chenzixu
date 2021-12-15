---
title: Assemble time-aligned transcription files
linktitle: 1. Assembling time-aligned transcripts
toc: true
type: docs
date: "2021-12-15T00:00:00+01:00"
draft: false
menu:
  speechcorpus:
    parent: Speech Corpus Query
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

## Structure of a speech corpus
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

The first step that enables us to search intended speech sequences from the corpus is to create a large text file assembling all time-aligned transcripts so that we have access to three key information for all audio files: 1) temporal information; 2) the symbol for the speech unit (it can be a segment, a syllable, or a word given the granularity of the segmentation); 3) the filename/path.

## Assemble time-aligned transcripts

Usually a TextGrid file, as shown below, is not in its best format to work with. In the previous [tutorial](https://chenzixu.rbind.io/resources/1forcedalignment/fa5/), I provided Python scripts that convert a `.TextGrid` file into a plain text file with tabular format data. Again, they are available at my Github [repository](https://github.com/chenchenzi/textgrid2table). The `README.md` will take you from there.

If you would like to convert multiple `.TextGrid` files all at once, you can consider a for loop in your command line and then concatenate individual `.txt` files into a large text file.

```
File type = "ooTextFile"
Object class = "TextGrid"

xmin = 0.0125 
xmax = 1.8725 
tiers? <exists> 
size = 2 
item []: 
    item [1]:
        class = "IntervalTier" 
        name = "phone" 
        xmin = 0.0125 
        xmax = 1.8725 
        intervals: size = 26 
        intervals [1]:
            xmin = 0.0125 
            xmax = 0.0925 
            text = "t" 
```

**Alternatively**, I created another Python script `tg2csv.py` that loops round the `\textgrid\` directory and create one large text file in the output. It is also available at my Github [repository](https://github.com/chenchenzi/textgrid2table). You can download the script.

The key to creating this assembling text file in the tabular format is **forward thinking**. *What information will you need in the next steps?* In order to cut speech segments out from a audio file, we will need the filename (path) of the audio file, and the times of the targeted speech segments. 
The correspondence between the filenames of an audio file and a transcript does us a favor in accessing the path of the audio file when we have the transcript file. Therefore, a column of filename should be in the tabular data, and we might also want to remove the file extension, which would make it easier to work with different file extensions later.

### Demo task
Suppose that I am interested in some Mandarin syllables and my `.TextGrid` files are time-aligned at both the segmental (tier name: phoneme) and syllabic level (tier name: word). I hope to convert all the `word` tiers into tabular format. 

`tg2csv.py` should be placed in the project directory, i.e. `/speech_corpus` in above example. It takes three arguments: 1) the name of the directory where we put the `.TextGrid` files, in this case, `textgrids`; 2) the name of the tier that we are interested in, `word`; 3) the name of the desired output file. Let's call it `words.csv`.

In the Terminal or your Unix Shell, we can do:
```
python tg2csv.py textgrids word words.csv
```
The final tabular output of `tg2csv.py` is demonstrated below. You may delete the first row `0,1,2,3,4` in the output file, which is the unspecified column names.

```
妈,0.0125,0.3325,0.3200,b01_1_101q
妈,0.3325,0.5125,0.1800,b01_1_101q
们,0.5125,0.7925,0.2800,b01_1_101q
正,0.7925,1.1125,0.3200,b01_1_101q
看,1.1125,1.6725,0.5600,b01_1_101q
...
```

Now that we have some organised information about all syllables in our own Mandarin corpus.

