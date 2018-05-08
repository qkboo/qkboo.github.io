# 목표물 맞추기

장애물을 만들고 포탄을 쏘아 맞추게 할 수 있다. 장애물은 프리랩으로 만들고 배치한 후에 크기 등을 조절해서 사용한다.

## Collider

### 포탄의 Rigdbody 속성

포탄 프리랩의 속성에서 Spherer Collider의 *isTrigger* 속성을 체크하면 ,  Trigger 속성이 있는 오브젝트는 충돌 시 물체가 여러 개 있을 때 모두 관통해서 지나간다. 
그리고 Gigidbody의 Collision Detection 속성을 체크하면,,,

![](/images/unity3d/tank1-prefab-bullet-trigg.png){: width="400"}

### Mesh Collider

객체가 충돌했다는 것은 이벤트로 감지할 수 있는데 이런 이벤트는 어느 한쪽이라도 Rigidboxy 콤포넌트가 있어야 한다. 그리고 물체가 충돌하는 것을 탐지하려면 양쪽 객체에 모두 콜라이더 컴포넌트가 있어야 한다. 바닥에 사용한 Plane 객체는 Mesh Collider를 가지고 있다. 만약 이 것을 제거하고, Rigidbody에 Gravity 속성을 가진 객체들은 중력의 영향을 받아 모두 바닥을 뚫고 내려가 버린다.

![](/images/unity3d/mesh-collider-rigid-gravity.png){: width="400"}

[그림. Mesh Collider 속성과 Rigidboxy의 Gravity 속성]

#### Rigidbody 추가

탱크 객체에 Rigidboxy를 추가하면 기본으로 Gravity 속성이 켜지고, 장애물에 부딫히면 충돌로 서서히 방향을 바꾸는 모습을 확인할 수 있다. 그리고 Gravity 속성 때문에 바닥면을 지나치면서 아래로 떨어져 버린다. Gravity 속성을 끄면 충돌후 Gravity가 없어서 공중으로 붕 뜨는 현상을 확인해 볼 수 있다.

#### 이벤트 추가

오브젝트가 충돌할 때 발생하는 이벤트는 다음과 같은 것이 있습니다(캐릭터 컨트롤러를 사용할 때 발생하는 이벤트는 제외합니다).
 
```
OnCollisionEnter(Collision)     // 충돌 후 반사 등의 수학적인 계산을 수행한다
OnTriggerEnter(Collider)        // 충돌 후 수학적인 계산을 수행하지 않는다
```
 -  충돌 객체 어느 하나가 Trigger 속성이 있으면 OnTriggerEnter()가 호출.

충돌의 처리는 장애물에서 해도 되고, 포탄에서 해도 됩니다. 그런데 포탄은 장애물 뿐만 아니라 장차 적군 탱크가 나타나면 적군 탱크와의 충돌도 생길 것이므로 충돌의 판정은 포탄에서 처리허는 것으로 하겠습니다. 
파일명을 jsBullet으로 입력하고 다음과 같이 작성

```js
#pragma strict

var snd: AudioClip;

function OnTriggerEnter(coll: Collider) {
    AudioSource.PlayClipAtPoint(snd, transform.position);
    Destroy(coll.gameObject); // remove a obstacle
    Destroy(gameObject); // remove this bullet
}
```

포탄 프리팹에 연결한다.

사운드를 출력하는 PlayClipAtPoint() 함수는 AudioClip과 함께 transform.position을 인수로 받습니다. 여기에서의 transform은 포탄 자신입니다. 실제 사운드 파일을 포탄 프리팹 인스팩터 속성의 snd 속성에 연결해 준다.

![](/images/unity3d/tank1-prefab-bullet-snd.png){: width="400"}


>  사운드에 입체감을 주려면 오디오 클립을 재생할 때 현재의 포탄 위치가 관찰자로부터 얼마나 떨어져 있는지를 알아야 합니다.   거리에 따라 음량 등을 다르게 처리하려면 도플러 효과 등의 물리학 지식이 필요하지만 그런 것은 모두 유니티가 처리해 주므로 우리는 신경쓸 필요가 없습니다.
>  니티는 방향에 따라 좌우의 볼륨을 조절함으로써 소리에 입체감을 부여합니다.


#### Tag 설정

포탄의 충돌 이벤트는 장애물, 바닥 그리고 다른 객체를 가리지 않고 충돌 한 후에 물체를 Destroy() 함수로 지우게 되어 있다. 이것을 장애물이라는 Tag로 지정한 물체만 가능하게 변경해야 한다

![](/images/unity3d/tank1-wall-tag.png){: width="400"}

장애물을 프리팹으로 만들었으므로 화면의 장애물을 확인해 보면 모든 장애물의 태그가 WALL로 설정된다. 이제 이 WALL 만 충돌시 제거하도록 한다.

```js
#pragma strict

var snd: AudioClip;

function OnTriggerEnter(coll: Collider) {
    AudioSource.PlayClipAtPoint(snd, transform.position);
  if( coll.gameObject.tag == "WALL") {
    Destroy(coll.gameObject); // remove a obstacle
    Destroy(gameObject); // remove this bullet
  }
}
```


### Particle 

파티클(Particle)은 화염, 연기, 눈, 비, 안개 등 입자가 아주 작은 물질들을 표현하기 위한 기술로, 여기서 포탄이 충돌하면 터지는 효과를 발휘할 수 있다.

#### 유니티 내장 파티클 이용하기

프로젝트 윈도우에서 Import 로 내장한 Particle을 불려온다. Import Package -> Particle Systems 에서 

import한 파티클에서 Prefabs에 정의되어 있는 효과중 괜잖은 것을 사용하기 위해서,  프리팹을 하나 만들고 이름을 pfExposion으로 설정한 후 방금 수정한 Fire1을 여기에 등록하고, 화면에 있는 파티클은 필요가 없으므로 삭제합니다. 이제 jsBullet을 수정합니다.





## 참조
 - [저녘놀:탱크 게임2](http://foxmann.blog.me/90138221206)
