---
title: Audacity - Reduction Noise
date: 2018-06-18 09:00:00 +0900
layout: post
tags: [Audacity, 오다시티, Audio, Sound, Noise Reduction, 사운드, 노이즈,]
categories: Programming
---

Audicity 에서 잡음을 줄이는 작업을 정리한다.


### 탐색과 오디오 식별

https://manual.audacityteam.org/man/navigation_tips.html


### 음원의 잡음

오디오에 잡음, 크기가 작은 음원이라면 준비한 오디오에서 잡음은 없애고(줄이고), 음질은 높이는 작업을 해주어야 하고 이것은 보통 잡음 제거 단계를 거치면 된다.

음원에서 잡음을 줄이는 작읍은 크게 3 단계로 이루어 진다.

1. Noise Reduction 
2. Normalize
3. Compressor

녹음을 통해 "One", "Two", "Three" ... "Ten" 을 녹음한후에 트랙을 Wave DB로 본 화면이다.

![](/images/audacity/audacity-track-wave.png)


#### 1. Noise Reduction

트랙 구간중 무음 상태의 구간을 선택해 Noise Recution의 Noise Profile 구하기를 수행한다.

![](/images/audacity/audacity-noise-reduction1.png)

프로파일을 구한 후에 전체구간을 선택해 Noise Recution를 수행한다. 그러면 무음 구간이 프로파일로 필터한 잡음을 상쇄해서 잡음을 제거하는 것이다.

여기에 필요시 무음을 원하는 구간을 선택해 *편집->특수제거->오디오 무음 처리* 를 실행하면 선택된 구간이 완전 무음 처리된다.

![](/images/audacity/audacity-no-sound.png){: witdh="350"}

#### 2. Normalize

Nomalize는 파형의 최고 피크에 맞춰, 전체 음을 일정하게 처리해주는 필터 기술이다.[^1]
잡음 처리를 한 후에 전체 구간을 Normalize 해주면, 

![](/images/audacity/audacity-normalize1.png){: witdh="550"}

전체 음파중 최고 피크에 맞춰 일관되게 해준다.

![](/images/audacity/audacity-normalize2.png){: witdh="550"}


#### 3. Compressor

Compressor 필터는 오디오의 Dynamic Range(동적범위)를 줄여준다. 오디오에서 동적범위를 줄이면 오디오를 더 증폭 시키는 효과를 얻는 것이다.[^2]



#### 연습

위 잡음 제거 기법을 유튜브 오디오 라이브러리에서 음원을 하나 찾아 적용해 보자.

![](/images/audacity/youtube-audiolibrary-sparrow.png){: witdh="550"}

먼저 오디오를 가져와서 트랙 메뉴에서 Wave Form으로 바꿔 본다.

![](/images/audacity/audacity-sparrow1.png){: witdh="550"}

파형을 확대해서 잡은 구간을 찾아 선택한 후에 효과 -> Noise Recution을 수행해 Profile을 해준다.

![](/images/audacity/audacity-sparrow2.png){: witdh="550"}

그리고 오디오 전체를 선택해 Noise reduction profile한 값을 Noise Reduction을 수행한다.

![](/images/audacity/audacity-sparrow3.png){: witdh="550"}

이렇게 잡음 제거를 한 후에 보면 참새 소리가 명확히 들어나는 구간만 보이게 된다.

![](/images/audacity/audacity-sparrow4.png){: witdh="550"}

여기서는 이 구간만을 남기고 나머지를 제거해서 사용하겠다.

![](/images/audacity/audacity-sparrow5.png){: witdh="550"}

오디오 소리를 증폭하기 위해 Normalize 한 후에 Compressor로 최적화 해주는데, 

![](/images/audacity/audacity-sparrow6.png){: witdh="550"}





##  참고

- [How to Clean Audio using Audacity 2.1.0 May 2015](https://www.youtube.com/watch?v=iQOdCgH0qPo)
- [[트리지] LMMS x 오다시티 강좌 - 샘플링의 기초](https://www.youtube.com/watch?v=hFYmAGeragA&t=5s)



[^1]: https://manual.audacityteam.org/man/normalize.html
[^2]: https://manual.audacityteam.org/man/compressor.html


