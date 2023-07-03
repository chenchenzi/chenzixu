---
title: Apply Large Pre-trained Models
linktitle: 1. Applying large pre-trained models
toc: true
type: docs
date: "2023-6-20T00:00:00+01:00"
draft: false
menu:
  speechrecognition:
    parent: Speech Recognition
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

<br>

In this chapter, I demonstrate how to utilise two sets of open-source state-of-the-art pre-trained ASR models which were trained on very large dataset to transcribe English speech: (1) OpenAI's Whisper; (2) Facebook's Wav2Vec2. The models come in various sizes and accuracy. The larger models are usually much slower but generate more accurate transcripts.

You will benefit from this tutorial if (1) you are working on one of the major languages with pre-trained ASR models and (2) you want to transcribe speech recordings of the language. Although having some basic knowledge of Unix Shell and Python would enhance your understanding of the tutorial, don't worry if you're not familiar with them. Feel free to follow along regardless!

**Demo task**

Suppose that I have some speech data and they are in the directory `/speech_data/audios/` as follows. Our goal is to create the corresponding transcript text files in `.txt` or `TextGrid` format, as shown in the directory `/speech_data/texts/`.

The sample audio recordings used here come from the [Northwestern ALLSSTAR Corpus](https://speechbox.linguistics.northwestern.edu/#!/home). A million thanks go to the creators of the ASR models and the publicly accessible data!

```
speech_data
├── metadata.txt
├── audios
│   ├── ALL_057_M_ENG_ENG_NWS_01_16kHz.wav
│   ├── ALL_058_M_ENG_ENG_NWS_01_16kHz.wav
│   └── ALL_059_M_ENG_ENG_NWS_01_16kHz.wav
└── texts
    ├── ALL_057_M_ENG_ENG_NWS_01_16kHz.txt
    ├── ALL_058_M_ENG_ENG_NWS_01_16kHz.txt
    └── ALL_059_M_ENG_ENG_NWS_01_16kHz.txt
```
{{% alert note %}}
In phonetic research we prefer uncompressed audio formats such as WAV.
{{% /alert %}}

We will be using Python and related packages. Setting up an environment to manage these packages is a good starting point.

<br>

## 1.1 Setting up an environment

I recommend [Anaconda](https://www.anaconda.com/download) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html) (the small, bootstrap version of Anaconda), a free all-in-one installer and environment management system. You can install it through the Installer downloaded from the official website.

In your Terminal or command line Shell, you can create an environment named (`--name` or `-n`) "asr", then activate the environment:
```{bash}
(base) chenzi@joashitakosMBP3 ~ % conda create --name asr
(base) chenzi@joashitakosMBP3 ~ % conda activate asr
(asr) chenzi@joashitakosMBP3 ~ % 
```
You can see the environment changed from `(base)` to `(asr)`. Python is usually automatically installed. Now we can install other packages we need:

```{bash}
~ % conda install -c pytorch pytorch
~ % conda install -c conda-forge transformers
~ % conda install -c conda-forge pydub
~ % conda install -c conda-forge pyaudio
~ % pip install miniaudio
~ % pip install SpeechRecognition
~ % conda install -c conda-forge ffmpeg
```

We will use ASR models shared on [Hugging Face](https://huggingface.co/models?pipeline_tag=automatic-speech-recognition&sort=trending). It might be useful for you to create your own account and get a User Access Token so that you can download private repositories if needed (optional step). 

```{bash}
~ % pip install huggingface_hub
# You already have it if you installed transformers or datasets
~ % huggingface-cli login
# $Huggingface_token will be needed here
```

<br>

## 1.2 Whisper by OpenAI
[Whisper](https://openai.com/research/whisper) is a powerful general-purpose speech recognition model developed by OpenAI. It is a **multitasking** model that can perform **multilingual** speech recognition, speech translation, and language identification.

There are five freely available model sizes (`tiny`, `base`, `small`, `medium`, and `large`), offering speed and accuracy tradeoffs. Please  visit [this Github repo](https://github.com/openai/whisper) for more details.
To setup, you can use one of the following commands in your Terminal:
```
~ % pip install -U openai-whisper

# alternatively
~ % pip install git+https://github.com/openai/whisper.git
```

Having installed *Whisper*, you can already use it through command lines. For example:

```
~ % whisper audio.wav --model medium # Choose medium-sized model
~ % whisper spanish.wav --language Spanish # Choose Spanish language
~ % whisper spanish.wav --language Spanish --task translate # Translate Spanish language
```

Alternatively, you can create a python script as follows, or run these lines in a Jupyter/Colab Notebook.

```{python}
import whisper

model = whisper.load_model("base")
transcription = model.transcribe("audio.wav")

# Print the whole chunk of the transcript
print(transcription["text"])

# Print the transcript in sentence sizes with time stamps
result = []
    
for i in range(len(transcription["segments"])):
  result.append("[{0} --> {1}]{2}".format(
    transcription["segments"][i]["start"],
    transcription["segments"][i]["end"],
    transcription["segments"][i]["text"]))

results ="\n".join(result)
print(results)
```

The two types of output are as follows for a sample audio:

```
The North Wind and the Sun were disputing which was the stronger when a traveler came along wrapped in a warm cloak. They agreed that the one who first succeeded in making the traveler take off his cloak should be considered stronger than the other. Then the North Wind blew as hard as he could, but the more he blew, the more closely did the traveler fold his cloak around him, and at last the North Wind gave up the attempt. Then the Sun shined out warmly, and immediately the traveler took off his cloak, and so the North Wind was obliged to confess that the Sun was the stronger of the two.

```

```
[0.0 -> 4.64] The North Wind and the Sun were disputing which was the stronger when a traveler came along
[4.64 -> 6.76] wrapped in a warm cloak.
[6.76 -> 11.28] They agreed that the one who first succeeded in making the traveler take off his cloak
[11.28 -> 13.8] should be considered stronger than the other.
[13.8 -> 17.92] Then the North Wind blew as hard as he could, but the more he blew, the more closely did
[17.92 -> 23.36] the traveler fold his cloak around him, and at last the North Wind gave up the attempt.
[23.36 -> 28.88] Then the Sun shined out warmly, and immediately the traveler took off his cloak, and so the
[28.88 -> 32.12] North Wind was obliged to confess that the Sun was the stronger of the two.

```

<br>

## 1.3 Wav2Vec2.0 by Facebook

The [Wav2Vec2 model](https://huggingface.co/facebook/wav2vec2-base-960h) trained on 960 hours of Librispeech (with 16kHz sampling rate) is available at Hugging Face. You can also check out their [paper](https://arxiv.org/abs/2006.11477).

{{% alert warning %}}
When using the model, make sure that your speech input is sampled at 16kHz.
{{% /alert %}}

The following Python script generate the transcript for `audio.wav`.
```{python}
import torch
from transformers import Wav2Vec2ForCTC, Wav2Vec2Processor
import speech_recognition as sr
import io
from pydub import AudioSegment

tokenizer = Wav2Vec2Processor.from_pretrained("facebook/wav2vec2-base-960h")
model = Wav2Vec2ForCTC.from_pretrained("facebook/wav2vec2-base-960h")
r = sr.Recognizer()

audio=sr.AudioFile("audio.wav")

with audio as source:
    try:
        audio=r.record(source)
        data=io.BytesIO(audio.get_wav_data())
        clip = AudioSegment.from_file(data) #numpy array
        x = torch.FloatTensor(clip.get_array_of_samples()) # tensor
        inputs = tokenizer(x, sampling_rate=16000,
                           return_tensors='pt', padding='longest').input_values
        logits = model(inputs).logits
        tokens = torch.argmax(logits, axis=-1) # get the distributions of each time step
        text = tokenizer.batch_decode(tokens) #tokens into a string
        print(str(text).lower())
    except Exception as e:
        print("Exception: "+str(e))

```

The output of above script is as follows:
```
the north wind and the sun were disputing which was the stronger when a traveller came along wrapped in a warm cloak they agreed that the one who first succeeded and making the traveller take off his cloak should be considered stronger than the other then the north wind blew as hard as he could but the more he blew the more closely did the traveller fold his cloak around him and at last the north wind gave up the attempt then the sun shined out warmly and immediately the traveler took off his cloak and so the north wind was obliged to confess that the sun was the stronger of the two
```

## Many Other Models

In a similar way, you can have access to many other ASR models on the Hugging Face platform including [Facebook's Hubert](https://huggingface.co/facebook/hubert-large-ls960-ft) and [Fine-tuned XLSR-53 large model](https://huggingface.co/jonatasgrosman/wav2vec2-large-xlsr-53-english). You can follow the instructions detailed in the website to try it out.

More coming soon...