---
title: "The processing of neutral tone in self-supervised learning speech models"
summary: Poster Presentation at UK and Ireland Speech 2025, York, Cambridge.
tags:
- Self-supervised Speech Models
- Wav2Vec2
- Acoustics
- Neutral Tone
date: "2024-07-01T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Poster presented at UK and Ireland Speech 2025, York, Cambridge. 16-17 June 2024.
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

The present study explores how self-supervised learning (SSL) speech models 
represent Mandarin neutral tone, in comparison to
the four lexical tones. In Standard Mandarin, neutral tone displays greater variability 
in their pitch realisation than the canonical four
citation tones and is largely influenced by its neighbouring tonal
contexts. In the phonological literature, neutral tone has been analysed as the fifth lexical tone, 
a tone sandhi phenomenon, or a product of tonal neutralisation resulting from the interaction of stress [1].
While some scholars have proposed a phonological specification for
neutral tone (e.g. [2, 3]), others have argued for its underspecified
representation. Acoustic evidence suggests that neutral tone may
often manifest post-lexical boundary tonal features, distinguishing
it from the four lexical tones [4].

The main analysis was performed on two publicly available pre-trained models: 
wav2vec2-large-xlsr-53 [5] and wav2vec2-large-xlsr-53-chinese [6]. 
The base multilingual XLSR model was trained
on unlabelled speech data from multiple datasets including Multilingual LibriSpeech, 
Common Voice, and Babel, encompassing 53
languages. The latter model was the same base model finetuned for
Mandarin Chinese. Both models consist of a feature extraction network of 
7 convolutional neural network (CNN) layers and a context
network of 24 transformer layers. During pre-training, the CNN
block output is quantised into codewords, and these discrete representations 
over 25 ms windows allow us to probe potential abstract
representations of neutral tone at the segmental level. The study
used 6393 disyllabic Mandarin words (3337 words with two full
tones, 3056 words with a neutral tone) mined from recordings in
the Beijing Mandarin subset of the KeSpeech corpus [7], filtered
to include only speakers from Beijing to minimise the influence of
regional accent on the use of neutral tone. The identification of
neutral-toned words was based on a list of words with obligatory
neutral tone, as defined by the Standard Mandarin (Putonghua) Proficiency Test, 
as well as words with a grammatical particle such as
de, the modifier marker. It is worth noting that automatic tonal transcriptions of 
Chinese characters were often inaccurate for neutral
tone and were therefore not used.

The study applies the W2V models to all words in the dataset
and extracted the feature encoder outputs (CNN) and the outputs of
every Transformer layer. Then a layer-specific Multi-Layer Perceptron (MLP) classifiers were trained to predict the tone categories,
to understand the tonal information captured in the layers of the
speech models. Classifier probes were evaluated using Accuracies,
F1 scores, and Matthews correlation coefficients. In addition, codevectors 
for all frames of each word were generated to test where
there are sets of codevectors that differentiate the lexical tone and
neutral tone versions of a given vowel.

The findings suggest that (1) the CNN block represents neutral
tone in a segment-specific manner, similar to English
stress [8], with different codevectors for full tone and neutral tone
versions of a given vowel. (2) The transformer layer based classifiers outperform the CNN based classifiers, driven by the enriched
context in the transformer block. (3) The classifier performs best
in the middle layers of the networks. (4) The finetuned
model improves the classifier performance in the middle and later
layers, especially the last three layers. (5) The classification of all
tones improved in the first 8 layers (0-7) in both models.

### References

[1] L. Liu, “20 shiji hanyu qingsheng yanjiu zongshu 20 ([An overview of
research on the Chinese neutral tone in the 20th century),” Yu wen yan
jiu , no. 3, pp. 43–47, 2002.

[2] M. Yip, “The tonal phonology of Chinese,” Thesis, Massachusetts In-
stitute of Technology, 1980.

[3] H. Lin, “Mandarin Neutral Tone as a Phonologically Low Tone,” Jour-
nal of Chinese Language and Computing, vol. 16, no. 2, pp. 121–134,
Jan. 2006.

[4] C. Xu and C. Zhang, “A cross-linguistic review of citation tone pro-
duction studies: Methodology and recommendations,” The Journal of
the Acoustical Society of America, vol. 156, no. 4, pp. 2538–2565, Oct.
2024.

[5] A. Conneau, A. Baevski, R. Collobert, A. Mohamed, and M. Auli, “Un-
supervised Cross-Lingual Representation Learning for Speech Recog-
nition,” in Interspeech 2021. ISCA, Aug. 2021, pp. 2426–2430.

[6] J. Grosman, “Fine-tuned XLSR-53 large model for speech recog-
nition in Chinese,” https://huggingface.co/jonatasgrosman/wav2vec2-
large-xlsr-53-chinese-zh-cn, 2021.

[7] Z. Tang, D. Wang, Y. Xu, J. Sun, X. Lei, S. Zhao, C. Wen, X. Tan,
C. Xie, S. Zhou, R. Yan, C. Lv, Y. Han, W. Zou, and X. Li, “KeSpeech:
An Open Source Speech Dataset of Mandarin and Its Eight Subdi-
alects,” in 35th Conference on Neural Information Processing Systems
Datasets and Benchmarks Track (Round 2), 2021, p. 12.

[8] M. Bentum, L. T. Bosch, and T. Lentz, “The Processing of Stress in
End-to-End Automatic Speech Recognition Models,” in Interspeech
2024. ISCA, Sep. 2024, pp. 2350–2354.