---
title: "Tone Patterns in Binumarien Noun Stems (Kainantu, Trans-New Guinea)"
summary: Poster Presentation at Phonetics and Phonology in Europe 2025, Palma, Spain.
tags:
- Low-resource Language
- Tone and Intonation
- Acoustics
date: "2025-06-01T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Poster presented at Phonetics and Phonology in Europe, Palma, Spain. 25-27 June 2025.
  focal_point: Smart

links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/ChenziAmy
#- icon: github
#  icon_pack: fab
#  name: Code
#  url: https://github.com/uoy-research/pasr-output/tree/main/icphs_23_voicequality
#- icon: download
#  icon_pack: fa
#  name: PDF
#  url: https://www.isca-archive.org/interspeech_2024/xu24j_interspeech.pdf
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: example
---

This study examines the tonal patterns of disyllabic noun stems in Binumarien, 
employing fieldwork data and phonetic-acoustic analysis. 
It also establishes a pipeline for developing and applying language technologies 
to a low-resource language, benefiting future research.
Binumarien (endonym: Afaqinna Ufa) is a Kainantu language (Trans-New Guinea) spoken 
in the Eastern Highlands Province of Papua New Guinea [1], with around 1200 speakers. 
Most children grow up with Binumarien as their first language, along with Tok Pisin, 
the lingua franca, and regional languages.

In this study, speech data were collected between February and April 2023 in Binumarien village, 
from two male L1 speakers who were born and raised there. The speakers were recorded 
using a Tascam DR-10 audio recorder with a clip-on Rohde miniature microphone (Mono), 
and a Marantz PMD 661 solid state audio recorder (Stereo). The recorders ran simultaneously at 48 kHz, 24 bps. 
The Tascam recordings in WAV format were used here. The participants were asked 
to pronounce each noun stem four times: twice in isolation and twice with a suffix. 
The nouns were extracted from texts and a word list collected during a field trip in 2018-19 
for a master’s thesis [3] and from a literacy booklet [4]. 
In addition to recording the pronunciation of each word, speakers were asked to whistle the tone of each word, 
for an impressionistic identification of tone. When the interviewer was in doubt, participants were asked to group words based on their tone patterns.
All the annotated intervals in the recordings, totalling 1.9 hours, were used to train a preliminary acoustic model with the Montreal Forced Aligner [5], which facilitates word- and segment-level time alignment between audio and text. Upon inspection, about 15% of the word-level alignments of nouns were manually corrected. Then sound intervals of disyllabic noun stems in citation form were extracted, within which f0 measurements every 10 milliseconds in voiced regions were obtained using the Parselmouth library [6, 7] (floor: 75Hz, ceiling: 300Hz). The f0 measurements in Hz were normalised to semitones relative to the speaker mean, and the corresponding time values in seconds were linearly scaled to the range [0,1], to enable the comparison of the contour shapes across speakers and word items. 

Impressionistically, we find four tones on syllables on the surface: high (H), low (L), falling (F), and rising (R) (see poster for illustrations). 
F occurs only on long vowels and diphthongs, while R appears on these as well as on stem-final short vowels and occasionally between a L and a H. 
Whether F and R should phonologically be seen as separate tonal units or as combinations of underlying L and H is to be investigated.

Acoustically, the f0 contours of 50 disyllabic noun stems (coded in different colours) are shown in the poster, 
grouped in eight preliminary clusters. The eight clusters were based on the visual inspection of the surface f0 contours. 
Variations within each cluster can be partially attributed to the differences in syllable structure among these words. 
Some clusters may appear similar but serve to differentiate word meanings. For example, the f0 contours of the minimal pair of aandau 
are illustrated in the poster, where the two f0 patterns, LH and LF, are affiliated with different word meanings (‘white hair’ and ‘animal’).
This empirical study contributes to our knowledge of the Binumarien tonal system. 
We are collecting more data on a variety of speech materials from additional speakers to generalise our findings and gain deeper insights into the tonal system, 
distinct from many Southeast Asian varieties. 

### References

[1] Pawley, A., & H. Hammarström. 2017. The Trans New Guinea family. In Bill Palmer (Ed.), The languages and linguistics of the New Guinea area: A comprehensive guide. Berlin: De Gruyter, 21–196.

[2] Oatridge, D. & J. Oatridge. 1965. Phonemes of Binumarien. In Frantz, Frantz, Oatridge, Oatridge, Loving, Swick, Pence, Staalsen, Boxwell & Boxwell (Eds.), Papers in New Guinea Linguistics. Canberra: Australian National University, 13-22.

[3] van Dasselaar, R. 2019. Topics in the Grammar of Binumarien: Tone and Switch-reference in a Kainantu Language of Papua New Guinea. Master’s thesis, Leiden University.

[4] Aadoo. 1973. Oosana Oosana Aandau Ufa - Animals and Birds. Summer Institute of Linguistics.

[5] McAuliffe, M., Socolof, M., Mihuc, S., Wagner, M., & Sonderegger, M. 2017. Montreal Forced Aligner: Trainable Text-Speech Alignment Using Kaldi. Interspeech.

[6] Jadoul, Y., Thompson, B., & de Boer, B. 2018. Introducing Parselmouth: A Python interface to Praat. Journal of Phonetics, 71, 1-15. https://doi.org/10.1016/j.wocn.2018.07.001

[7] Boersma, P., & Weenink, D. 2021. Praat: doing phonetics by computer [Computer program]. Version 6.1.38, retrieved 2 January 2021 from http://www.praat.org/
