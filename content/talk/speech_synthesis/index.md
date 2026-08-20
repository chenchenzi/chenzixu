---
title: Speech Synthesis and Recognition
event: HG4052 · Semester 1, AY 2026/27
event_url: 

location: TR+69, Nanyang Technological University, Singapore
address: 
  street: 
  city: 
  region: 
  postcode: ''
  country: 

summary: HG4052 introduces the linguistic, acoustic, and computational foundations of speech recognition and synthesis for linguistics students, from the speech chain and DTW/HMMs to CTC, Whisper, and neural TTS, with hands-on Praat and Colab practicals and a focus on Singapore's multilingual speech environment.
abstract: "This course introduces the linguistic, acoustic, and computational foundations of automatic speech recognition (ASR) and text-to-speech (TTS) synthesis. The course is designed for linguistics students: technical content is mostly taught conceptually and visually, with the mathematical foundations built in Weeks 1–2 and a deliberate emphasis on phonetic intuition over mathematical derivation. Students move from the speech chain and acoustic phonetics, through digital signal representations and classical statistical systems (dynamic time warping, HMMs, n-grams, unit selection), to modern neural architectures (CTC, attention, Whisper, neural vocoders, voice cloning), treated as systems whose inputs, outputs, and failure modes can be analysed phonetically. Particular attention is paid to Singapore's multilingual speech environment: Singlish, accent variation, Mandarin–English code-switching, and the IMDA National Speech Corpus. Hands-on work uses Praat and scaffolded Google Colab notebooks (librosa, Whisper, neural TTS models). Ethical issues are engaged where they arise — recognition bias, voice cloning and consent, low-resource languages — and the course closes with a guest panel on careers in speech technology and a poster fair."

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: "2026-08-10T00:00:00Z"
date_end: "2026-11-13T00:00:00Z"
all_day: true

# Schedule page publish date (NOT talk date).
publishDate: "2026-08-13T00:00:00Z"

authors: []
tags: [Phonetics, ASR, TTS, speech technology, academic]

# Is this a featured talk? (true/false)
featured: false

image:
  caption: ''
  focal_point: 

links:
- icon: desktop
  icon_pack: fas
  name: Week 2 slides
  url: https://chenzixu.rbind.io/slides/speech-synthesis/week02.html
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: "example"

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
#projects:
#- internal-project

# Enable math on this page?
math: false
---

*Fridays 09:30 – 13:20, TR+69 · Prerequisite: HG2003 Phonetics & Phonology. No programming experience required — all Python work runs in scaffolded Google Colab notebooks.*

## Weekly slides

<style>
.hg-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(215px,1fr));gap:0.9rem;margin:1.2rem 0 2rem;}
.hg-card{display:flex;flex-direction:column;gap:0.3rem;padding:0.95rem 1.05rem;border:1px solid #eadfd2;border-radius:10px;background:#fffdf9;text-decoration:none!important;color:inherit;box-shadow:0 1px 2px rgba(60,40,20,0.04);position:relative;overflow:hidden;}
a.hg-card{transition:transform .12s ease,box-shadow .12s ease,border-color .12s ease;}
a.hg-card:hover{transform:translateY(-2px);box-shadow:0 6px 16px rgba(60,40,20,0.10);border-color:rgb(247,91,0);}
.hg-thumb{position:absolute;inset:0;background-size:cover;background-position:left center;opacity:0;transform:scale(1.06);transform-origin:left center;transition:opacity .2s ease,transform .35s ease;pointer-events:none;}
a.hg-card:hover .hg-thumb,a.hg-card:focus-visible .hg-thumb{opacity:1;transform:scale(1);}
.hg-card .hg-week{font-size:0.72rem;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:rgb(247,91,0);}
.hg-card .hg-dates{font-size:0.72rem;color:#8a8177;letter-spacing:0.02em;}
.hg-card .hg-topic{font-size:0.88rem;line-height:1.35;color:#3d3831;flex-grow:1;}
.hg-card .hg-go{font-size:0.78rem;font-weight:600;color:rgb(247,91,0);margin-top:0.15rem;}
.hg-soon{background:#faf7f2;border-style:dashed;opacity:0.72;}
.hg-soon .hg-week{color:#a89e92;}
.hg-recess{grid-column:1/-1;display:flex;align-items:center;gap:0.6rem;padding:0.35rem 0.2rem;font-size:0.75rem;letter-spacing:0.1em;text-transform:uppercase;color:#a89e92;}
.hg-recess:before,.hg-recess:after{content:"";flex-grow:1;border-top:1px dashed #ddd2c4;}
</style>

<!-- To publish a week: change its <div class="hg-card hg-soon"> to
     <a class="hg-card" href="https://chenzixu.rbind.io/slides/speech-synthesis/weekNN.html">,
     add <span class="hg-go">View slides →</span> before the closing tag, and for the
     hover preview drop a thumb-weekNN.jpg (~900px wide title-slide capture) in this
     folder and add <span class="hg-thumb" style="background-image:url('thumb-weekNN.jpg')"></span>
     as the card's first child. -->
<div class="hg-grid">
  <a class="hg-card" href="https://chenzixu.rbind.io/slides/speech-synthesis/week01.html">
    <span class="hg-thumb" style="background-image:url('thumb-week01.jpg')"></span>
    <span class="hg-week">Week 1</span>
    <span class="hg-dates">10 – 14 Aug</span>
    <span class="hg-topic">Inside the black box: what ASR and TTS do · Math primer I: vectors &amp; similarity</span>
    <span class="hg-go">View slides →</span>
  </a>
  <a class="hg-card" href="https://chenzixu.rbind.io/slides/speech-synthesis/week02.html">
    <span class="hg-thumb" style="background-image:url('thumb-week02.jpg')"></span>
    <span class="hg-week">Week 2</span>
    <span class="hg-dates">17 – 21 Aug</span>
    <span class="hg-topic">Math primer II: probability, Bayes&rsquo; rule, logs &amp; surprisal</span>
    <span class="hg-go">View slides →</span>
  </a>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 3</span>
    <span class="hg-dates">24 – 28 Aug</span>
    <span class="hg-topic">Acoustics primer: sampling, aliasing, the Fourier idea, spectrograms, source–filter</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 4</span>
    <span class="hg-dates">31 Aug – 4 Sep</span>
    <span class="hg-topic">From waveform to features: mel spectrograms, MFCCs, F0</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 5</span>
    <span class="hg-dates">7 – 11 Sep</span>
    <span class="hg-topic">ASR I: variability, template matching, dynamic time warping, word error rate</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 6</span>
    <span class="hg-dates">14 – 18 Sep</span>
    <span class="hg-topic">ASR II: HMMs, Viterbi, lexicons &amp; n-gram language models · Singlish case study</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 7</span>
    <span class="hg-dates">21 – 25 Sep</span>
    <span class="hg-topic">Neural networks primer: from neurons to vowel classifiers</span>
  </div>
  <div class="hg-recess">Recess week · 28 Sep – 4 Oct</div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 8</span>
    <span class="hg-dates">5 – 9 Oct</span>
    <span class="hg-topic">End-to-end neural ASR: CTC, attention, wav2vec 2.0, Whisper</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 9</span>
    <span class="hg-dates">12 – 16 Oct</span>
    <span class="hg-topic">Evaluation, bias &amp; forced alignment · speech corpora and ethics</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 10</span>
    <span class="hg-dates">19 – 23 Oct</span>
    <span class="hg-topic">TTS I (classical): text normalisation, G2P, formant / unit-selection / articulatory synthesis</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 11</span>
    <span class="hg-dates">26 – 30 Oct</span>
    <span class="hg-topic">TTS II (neural): acoustic models, vocoders, end-to-end voices, speaker embeddings</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 12</span>
    <span class="hg-dates">2 – 6 Nov</span>
    <span class="hg-topic">Prosody, voice cloning &amp; the ethics of synthetic voices</span>
  </div>
  <div class="hg-card hg-soon">
    <span class="hg-week">Week 13</span>
    <span class="hg-dates">9 – 13 Nov</span>
    <span class="hg-topic">Frontiers: spoken dialogue, speech LLMs, speech-to-speech translation · careers panel &amp; poster fair</span>
  </div>
</div>
