---
title: Audio data augmentation
linktitle: Audio data augmentation
toc: true
type: docs
date: "2023-9-10T00:00:00+01:00"
draft: false
menu:
  audioprocessing:
    parent: Audio Processing
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

<br>

Audio data augmentation is a technique that allows us to enhance the diversity of our audio dataset and increase the number of samples for training an machine learning model, which can often boost model performance. In this tutorial, we will delve into various techniques and strategies to generate new and varied samples from existing audio data. 


<br>

## 1 The benefits of audio data augmentation



<br>

## 2 The golden rule of data augmentation



<br>

## 3 Audio data augmentation techniques

## 4 Audio data augmentation datasets

## 5 Offline audio data augmentation demo

```python
'''
Offline Audio Augmentation for training
By Chenzi Xu
'''

import glob, numpy, os, random, soundfile, torch
from scipy import signal

class augment_loader(object):
	def __init__(self, train_path, musan_path, rir_path):
		self.train_path = train_path
		
        # Load and configure augmentation files
		self.noisetypes = ['noise','speech','music']
		self.noisesnr = {'noise':[0,15],'speech':[13,20],'music':[5,15]}
		self.numnoise = {'noise':[1,1], 'speech':[3,8], 'music':[1,1]}
		self.noiselist = {}
  
        # musan and rir audio files
		augment_files   = glob.glob(os.path.join(musan_path,'*/*/*/*.wav'))
		for file in augment_files:
			if file.split('/')[-4] not in self.noiselist:
				self.noiselist[file.split('/')[-4]] = []
			self.noiselist[file.split('/')[-4]].append(file)
		self.rir_files  = glob.glob(os.path.join(rir_path,'*/*/*.wav'))
  
		# Load data & labels
		self.data_list  = []
		for index, file in enumerate(os.listdir(train_path)):
			self.data_list.append(file)

	def __getitem__(self, index):
		# Read the utterance and randomly select the segment
		audio, sr = soundfile.read(self.data_list[index])		
		basename = os.path.split(self.data_list[index])[1][-4]
		
		augmented_audios ={               
            "original": audio,
            "reverberation": self.add_rev(audio),
            "babble": self.add_noise(audio, 'speech'),
            "music": self.add_noise(audio, 'music'),
            "noise": self.add_noise(audio, 'noise'),
            "television_noise": self.add_noise(self.add_noise(audio, 'speech'), 'music')
        }
		for aug_type, aug_audio in augmented_audios.items():
			filename = f"{basename}_{aug_type}.wav"
			soundfile.write(filename, aug_audio[0], sr)  

	def __len__(self):
		return len(self.data_list)

	def add_rev(self, audio):
		rir_file    = random.choice(self.rir_files)
		rir, sr     = soundfile.read(rir_file)
		rir         = numpy.expand_dims(rir.astype(numpy.float),0)
		rir         = rir / numpy.sqrt(numpy.sum(rir**2))
		return signal.convolve(audio, rir, mode='full')[:len(audio)]

	def add_noise(self, audio, noisecat):
		clean_db    = 10 * numpy.log10(numpy.mean(audio ** 2)+1e-4) 
		numnoise    = self.numnoise[noisecat]
		noiselist   = random.sample(self.noiselist[noisecat], random.randint(numnoise[0],numnoise[1]))
		noises = []
		for noise in noiselist:
			noiseaudio, sr = soundfile.read(noise)
			length = len(audio)
			if noiseaudio.shape[0] <= length:
				shortage = length - noiseaudio.shape[0]
				noiseaudio = numpy.pad(noiseaudio, (0, shortage), 'wrap')
			start_frame = numpy.int64(random.random()*(noiseaudio.shape[0]-length))
			noiseaudio = noiseaudio[start_frame:start_frame + length]
			noiseaudio = numpy.stack([noiseaudio],axis=0)
			noise_db = 10 * numpy.log10(numpy.mean(noiseaudio ** 2)+1e-4) 
			noisesnr   = random.uniform(self.noisesnr[noisecat][0],self.noisesnr[noisecat][1])
			noises.append(numpy.sqrt(10 ** ((clean_db - noise_db - noisesnr) / 10)) * noiseaudio)
		noise = numpy.sum(numpy.concatenate(noises,axis=0),axis=0,keepdims=True)
		return noise + audio
```

