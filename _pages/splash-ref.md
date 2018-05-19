---
title: "References Page"
layout: splash
permalink: /references/
notes:
  - "docs 를 포함하는 참고문서들의 집합"
date: 2018-04-30T09:00:00-04:00
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/unsplash-image-2.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: "개발에 참고 및 학습 목적으로 Linux, Raspberry Pi, Arduino, Android, Firebase, Python, Bigdata R 등에 관련한 블로그 포스팅을 좀더 정리하고, 완료된 문서를 나열하고 있다."

feature_miniamlmistake:
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "Unsplash image 2"
    title: "Jekyll/Minimal-mistakes 한글 요약 문서"
    excerpt: 'Jekyll에 minimal-mistakes 테마에 대한 문서를 필요에 따라 한글 요약 문서이다.  `type="left"`'
    url: "/docs"
    btn_label: "Let's go Minimal-mistakes"
    btn_class: "btn--primary"

feature_kotlin:
  - image_path: /assets/images/feature-kotlin-android-600px.png
    alt: "Kotlin language"
    title: "Kotlin Language"
    excerpt: "**Kotlin** 에 대한 개발 문서를 보유하고 있다."
    url: "/kotlin"
    btn_label: "Read More"
    btn_class: "btn--primary"

feature_linux:
  - image_path: /assets/images/feature-linux-600px.png
    alt: "Linux documents."
    title: "Linux Documents"
    excerpt: "**Linux** 이용을 위해 정리한 문서 모듬"
    url: "/linux/"
    btn_label: "Go to Linux collections"
    btn_class: "btn--primary"

feature_row:
  - image_path: /assets/images/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    title: "Placeholder 1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Placeholder 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: /assets/images/unsplash-gallery-image-3-th.jpg
    title: "Placeholder 3"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
feature_row4:
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Placeholder Image Center Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Centered with `type="center"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row id="feature_kotlin" type="left" %}

{% include feature_row id="feature_linux" type="left" %}

{% include feature_row id="feature_miniamlmistake" type="left" %}
