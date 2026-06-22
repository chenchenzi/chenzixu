---
title: "Voice quality in connected speech: annotating creaky, breathy, and whispery phonation"
summary: Work-in-progress presented at the CorpusPhon 2 workshop, LabPhon 2026 — a frame-ready annotation protocol for creaky, breathy, and whispery phonation in connected forensic-style speech.
tags:
- Forensic Phonetics
- Voice quality
- Phonation
- Corpus phonetics
date: "2026-06-22T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Annotating creaky, breathy, and whispery phonation in connected speech
  focal_point: Smart

links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/ChenziAmy
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

Voice quality (VQ) plays an important role in phonetic description and forensic speaker comparison, yet automatic approaches to non-modal phonation often transfer poorly to natural connected-speech corpora. A recent systematic review reports that acoustic studies of breathy and whispery voice in vocally healthy speakers rely heavily on sustained vowels and that whispery voice remains comparatively under-specified methodologically [1]. We present a work-in-progress annotation protocol designed to produce frame-ready supervision for creaky, breathy, and whispery phonation in connected forensic-style speech. The data comprise 62 speakers of Standard Southern British English from two style-controlled Cambridge corpora: 31 male speakers from DyViS and 31 female speakers from DIVERSE [2,3]. DyViS was designed to examine variability in forensic speaker comparison across speaking styles and telephone transmission conditions, while DIVERSE extends this design to female speakers in a mock forensic recording scenario [2,3]. We target approximately 120–150 annotated segments per speaker. The annotation protocol is perceptually guided by Vocal Profile Analysis (VPA), adapting its structured listening approach to frame-level corpus annotation [4]. Three trained phoneticians annotate a shared pilot subset, followed by calibration and adjudication sessions that refine criteria for voicing boundaries, overlap, and recurrent perceptual confusions before the main annotation phase. This workflow reflects evidence that reliable perceptual voice-quality annotation requires explicit training and calibration procedures [4].

Methodologically, the protocol separates voicing from voice quality. Annotators first label voiced versus unvoiced intervals, then annotate creaky, breathy, and whispery on separate tiers within voiced speech. This design addresses a known challenge in irregular-phonation detection: aperiodicity from unvoiced consonants and background noise can produce false positives unless voiced regions are explicitly identified [5]. Within voiced intervals, overlap across VQ categories is permitted when perceptually warranted rather than forcing a single categorical label, reflecting evidence that phonation states may co-occur or transition gradually [1,4]. Interval annotations are subsequently converted to fixed-hop frame targets for modeling. Two training representations are derived: (1) high-confidence consensus labels for categorical model training and evaluation, and (2) agreement-based soft targets that preserve overlapping or mixed phonation. This interval-to-frame pipeline produces a reusable corpus-phonetic resource and provides training data compatible with frame-based phonation detection methods [6].

#### References

1. Patman, C., Foulkes, P., & McDougall, K. 2025. Acoustic methods for analysing breathy and whispery voices: a systematic review. *Phonetica*.
2. Nolan, F., McDougall, K., de Jong, G., & Hudson, T. 2009. The DyViS database: style-controlled recordings for forensic phonetics. University of Cambridge Phonetics Laboratory.
3. University of Cambridge Phonetics Laboratory. DIVERSE: Database of Individual Variation in English by Recording style and SEx.
4. San Segundo, E., Foulkes, P., French, P., Harrison, P., Hughes, V., & Kavanagh, C. 2019. The use of the Vocal Profile Analysis for speaker characterization: methodological proposals. *Journal of the International Phonetic Association*.
5. Ishi, C. T., Sakakibara, K., Ishiguro, H., & Hagita, N. 2008. A method for automatic detection of vocal fry. *IEEE Transactions on Audio, Speech, and Language Processing*.
6. Murton, O., Hillenbrand, J., & Houde, R. 2019. Identifying a creak probability threshold for an irregular pitch period detection algorithm. *Journal of the Acoustical Society of America*.
