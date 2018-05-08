# TankGame: Part3

카메라를 주인공의 1인칭 시점으로 적용해 게임 객체를 따라 가면서 ..

## 카메라 워킹

FPS 게임은 주인공이 바라보는 시각에서 진행한다. 게임 캐릭터와 카메라를 동기화해서 다룬다. 유니티에서 제공하는 패키지를 사용해 카메라와 캐릭터를 동기화 시킬 수 있다.

프로젝트 윈도우 Assets에서 Import packages->Environments의 Unitity 스크립을 가져온다.

하위 폴더에 Camera Scripts가 있습니다. SmoothFollow를 찾아서 하이어라키 탭에 있는 Main Camera에 끌어다 놓습니다. 다음에는 카메라가 따라다닐 오브젝트(탱크)를 선택해서 인스펙터의 Target란에 끌어다 놓습니다. 그 아래에 있는 Distance와 Height는 카메라의 거리와 높이이므로 5와 1.5 정도로 입력합니다.  


![](/images/unity3d/tank1-camera-script.png){: width="400"}

오브젝트를 이동하거나 회전할 때 카메라가 출렁거리는 느낌과 따라오는 속도는 인스펙터의 Damping(완충 작용)으로 되어 있는 부분의 숫자를 조절합니다. 이 값이 클수록 출렁이는 속도가 줄어듭니다.

## 포탄 발사 화염 만들기

파티클을 프리팹으로 만들고 포탄을 발사할 때 화몀을 보여주게 한다. 포탄 발사 위치인 스판포인트에 표시하면 된다.

### 파티클을 하나 만든다.

계층 윈도우에서 마우스 우클릭으로 'Create ParticleSystem' 으로 새 파티클을 추가한다. 그리고 기본 속성을 다음 같이 지운다.


![](/images/unity3d/tank1-prefab-bullet-firework.png){: width="400"}


메뉴에서 Component->Effects->Legacy Particles 에 우리가 사용할 수 있는 컴포넌트, Particle Animator, Particle Renderer, Ellipsoid Particle Emitter 세 개를 모두 파티클에 추가합니다. 추가한 파티클의 속성을 다음과 같이 설정하세요.

 

## Enemy Tank

게임에서 어려운 부분은 바로 적군을 이동시키는 부분입니다. 적군이 너무 멍청해도 안되고, 죽자살자 아군만 쫓아다녀도 곤란합니다. 적군 탱크는 항상 아군을 주시하고 있다 아군 탱크가 사정거리 안에 들어오면 가차 없이 포를 발사하도록 할 것입니다. 

### 탱크 추가하기

기존 탱크를 복제해서 'Enemy'로 만든다. 
 적군 탱크의 포신 앞에 설치되어 있는 spawnPoint를 spawnPoint2로 이름을 바꿉니다. 

적군 탱크에 연결된 스크립트를 삭제합니다. 적군 탱크를 선택한 후 인스펙터의 Js Tank (Script) 항목을 마우스 오른쪽 버튼으로 클릭하고 Remove Component로 삭제합니다.

 적군 탱크에 색깔을 입힙니다. 매트리얼을 하나 만들어 색을 지정하고, 그것을 적군 탱크의 본체와 포탑, 포신에 각각 적용합니다.

메인 카메라를 클릭한 후 인스펙터에서 Target으로 표시된 맨 오른쪽에 있는 ‘◉’ 아이콘을 클릭하면 다음과 같은 Select Transform 창이 나타나므로 여기에서 None를 지정합니다.

### 목표물 주시

브젝트가 특정한 오브젝트를 주시하도록 설정할 때에는 *LookAt()* 함수를 사용합니다.
 
> transform.LookAt(Transform);


```js
#pragma strict

//private로 선언한 변수는 인스펙터에 표시되지 않는 지역변수
private var power = 1200;         // 포탄 발사 속도 -- ①
var bullet : Transform;           // 포탄 -- ②
var target : Transform;           // LookAt() 목표 -- ③
var spPoint : Transform;          // spawnPoint -- ④
var explosion: Transform;         // 포구 앞의 화염 -- ⑤
var snd : AudioClip;              // 발사음 -- ⑥
 
function Update () {
    transform.LookAt(target);     // 아군 방향으로 회전
}

```

jsEnemy.js 파일을 Enemy 객채에 드래그하고, 스크립트의 Bullet, Target, Sp point, Explosion 에 해당하는 객체를 연결해 준다.

![](/images/unity3d/tank1-enemy-script.png){: width="400"}



### 적군 발포

적군이 아군을 탐색해서 아군이 20유닛 이내로 들어오면 발포하도록 하겠습니다.

*jsEnemy2.js*
```js
function Update () {
    transform.LookAt(target);     // 아군 방향으로 회전
    var hit : RaycastHit;         // 탐색 결과 저장
    var fwd = Vector3.forward;    // 포탑의 전방
 
    if (Physics.Raycast(spPoint.transform.position, fwd, hit, 20) == false) return;        // 탐색 실패
 
    // 포탄 발사
    Instantiate(explosion, spPoint.transform.position, Quaternion.identity);             // 포구 앞의 화염
    var obj = Instantiate(bullet, spPoint.transform.position, Quaternion.identity);      // 포탄
    obj.GetComponent.<Rigidbody>().AddForce(fwd * power);
    AudioSource.PlayClipAtPoint(snd, spPoint.transform.position);                       // 포탄 발사음
}
```


 의도와는 전혀 다른 상황이 발생했습니다. 다음과 같은 문제가 있네요.
 
① 적군이 탐색하는 방향이 자신의 정면이 아니라 화면상의 정면(z축)이다.
② 적군의 포탄이 기관총 모드로 발사된다.
 
#### Quaternion.identity

Quaternion 은 회전을 표현한다. Quaternion.identity 는 월드 혹은 패어런트에 정확히 배열한 결과를 알려준다.


### 글로벌 좌표

유니티에서 사용하는 좌표는 Local 좌표와 Global 좌표가 있습니다. 
유니티는 오브젝트가 이동하거나 회전할 때 Local 좌표를 사용합니다. 즉, 오브젝트를 기준으로 전후좌우를 결정합니다. 
Raycast() 함수가 글로벌 좌표를 사용하는데 있습니다. 그래서 포탑이 향하는 로컬 좌표는 글로벌 좌표로 바꿔야 합니다. 로컬 좌표를 글로벌 좌표로 변환하기 위해 다음과 같은 함수를 제공합니다.
 
> transform.TransformDirection(Vector3)

사정권 안에 왔을 때 사격하고, 폭발, 포탄 한 발 발사가 아직 안되고 있다.


### 상대 조준

적군 탱크가 무조건 공격 모드가 되는 것은 탱크가 사격을 할 때 날아가는 포탄도 탐지되기 때문입니다. 그래서 아군 탱크에 표식을 달아줄 필요가 있습니다. 인스펙터에서 TANK 태그를 만들고 **본체, 포탑, 포신**에 모두 태그를 할당합니다.
적군 탱크가 탐색한 목표가 아군이 아니면 발포하지 않도록 처리합니다.

```
if (hit.collider.gameObject.tag != "TANK") return;
```


### Debug 출력

유니티는 프로그램의 디버깅을 위해서 몇 가지 함수를 제공합니다.  
 
        Debug.Log(<디버그 정보>);                                콘솔 창에 문자열 출력
        print(<디버그 정보>);                                         위의 Debug.Log()와 같다
        Debug.DrawRay(<위치>, <길이>, <컬러>);          화면에 직선 표시
        Debug.DrawLine(<시작점>, <끝점>, <컬러>);      화면에 직선 표시
 

*jsEnemy4.js*

```js
function Update () {
    transform.LookAt(target);     // 아군 방향으로 회전
    var hit : RaycastHit;         // 탐색 결과 저장
    //var fwd = Vector3.forward;    // 포탑의 전방:로컬 좌표 
    var fwd = transform.TransformDirection(Vector3.forward);
 
    // 거리 표시
    Debug.DrawRay(spPoint.transform.position, fwd * 20, Color.red); 
    if (Physics.Raycast(spPoint.transform.position, fwd, hit, 20) == false) return;        // 탐색 실패
    // 탐색한 오브젝트 이름 표시
    Debug.Log(hit.collider.gameObject.name);
    // 아군 여부에 따라 발포
    if ( hit.collider.gameObject.tag != "TANK") return;
 
    // 포탄 발사
    Instantiate(explosion, spPoint.transform.position, Quaternion.identity);             // 포구 앞의 화염
    var obj = Instantiate(bullet, spPoint.transform.position, Quaternion.identity);      // 포탄
    obj.GetComponent.<Rigidbody>().AddForce(fwd * power);
    AudioSource.PlayClipAtPoint(snd, spPoint.transform.position);                       // 포탄 발사음
}

```

게임을 실행하면 다음 그림과 같이 DrawRay()가 표시되는 화면을 볼 수 있습니다. DrawRay()는 디자인 탭에만 표시되고, 실제의 게임 화면에는 나타나지 않습니다. 

### 사격 속도 제한

직전 프레임과의 경과 시간이 Time.deltaTime으로 구해진다고 했으므로 이것을 이용합니다.

> **private var ftime : float = 0.0**;           // 사격 제한 시간 -- ①
> function Update () {
>     **ftime += Time.deltaTime;**
>     
>     if (Physics.Raycast(spPoint.transform.position, fwd, hit, 20) == false) return;
    **if (hit.collider.gameObject.tag != "TANK" || ftime < 2) return;** // 시간 체크
> }


*jsEnemy5.js*

```js
private var ftime : float = 0.0;    // 사격제한 시간

function Update () {
    transform.LookAt(target);     // 아군 방향으로 회전
    var hit : RaycastHit;         // 탐색 결과 저장
    ftime += Time.deltaTime;  // 경과 시간 누적 -- ②
     //var fwd = Vector3.forward;    // 포탑의 전방:로컬 좌표 
    var fwd = transform.TransformDirection(Vector3.forward);
 
    // 거리 표시
    Debug.DrawRay(spPoint.transform.position, fwd * 20, Color.red); 
    if (Physics.Raycast(spPoint.transform.position, fwd, hit, 20) == false) return;        // 탐색 실패

    // 탐색한 오브젝트 이름 표시
    Debug.Log(hit.collider.gameObject.name);
    // 아군 여부에 따라 발포
    if (hit.collider.gameObject.tag != "TANK" || ftime < 2) return; // 시간 체크 -- ③
 
    // 포탄 발사
    Instantiate(explosion, spPoint.transform.position, Quaternion.identity);             // 포구 앞의 화염
    var obj = Instantiate(bullet, spPoint.transform.position, Quaternion.identity);      // 포탄
    obj.GetComponent.<Rigidbody>().AddForce(fwd * power);
    AudioSource.PlayClipAtPoint(snd, spPoint.transform.position);                       // 포탄 발사음
    ftime = 0; 
}
```



① ftime을 실수형으로 선언한다. 정수형으로 선언하면 Time.deltaTime의 소수 부분이 잘려져서 누적되지 않는다.   
② 매 프레임마다 경과 시간을 누적한다.
③ 목표물을 탐색한 후 경과 시간이 2초 미만이면 사격을 하지 못한다.
④ 포탄을 발사한 후에는 경과 시간을 0으로 클리어한다.


### 적 포탄의 방향

아군 탱크는 프리팹으로 만든 포탄이 포탑의 회전 방향으로 회전하는 상태로 발사되서 포탑 쪽에서 보면 포탄이 직선으로 발사되는 것으로 보이지만, 현재 적군 탱크는 탱크 방향, 포탑의 방향과 관계 없이 포탄이 돌아간 형태로 
발사된다.

이것은 적군의 포탄 발사시 spPoint 로 지정한 스판포인트 객체를 사용하고 있다. 


그래서 다음 같이 아군의 스판포인트를 가져오는 코드를 이용해서 변경한다.

*jsEnemy6.js*
 
> function Update () {
>   ...
    //var myBullet = Instantiate(bullet, spPoint.transform.position, Quaternion.identity);      // 포탄
    var myBullet = Instantiate(bullet, spPoint.transform.position, spPoint.transform.rotation);      // 포탄
>   ,,,
> }


### 적 탱크 움직이기

적 탱크가 아군을 겨냥하면 포탑이 아군 탱크로 향하게 해보자. *RotateAround()* 함수는 지정한 좌표를 중심점으로 하는 원을 그리면서 이동하는 기능으로 적군을 이동시키는데 사용할 수 있습니다. 다음과 같이 사용합니다.

> transform.RotateAround(<중심점>, <회전축>, <회전각도/sec>);
>  - 글로벌 좌표를 사용하며, 컴퍼스로 원을 그리는 동작과 같이 자신이 있는 위치와 중심점과의 거리를 반지름으로 하는 원을 그리면서 이동을 한다.

jsEnemy는 적군 탱크의 포탑을 제어하는 스크립트이므로 다른 스크립트를 작성한다.

*jsEnemyMove.js*

```js
#pragma strict

var rotAng = 15; // 초당 회전 각도

function Update() 
{
  var amtToRot = rotAng * Time.deltaTime;
  // Vector3.zero는 Vector3(0, 0, 0)과 
  transform.RotateAround( Vector3.zero, Vector3.up, amtToRot);
}
```

이 스크립트를 적군 탱크에 연결하고 아군 탱크 움직임에 따라 적군 탱크가 위치를 조정하는 것을 확인한다.

### 적군 탱크와 포격전

Main Camera를 선택해 인스펙터 에서 'Smooth Follow' 스크립트의 Target을 아군 탱크로 지정한다. 그리고 포탄에 사용하는 *jsBullet* 스크립트에서는 장애물만 명중하고 폭발하도록 되어 있어서, 아군 탱크 그리고 적군 탱크에 모두 포탄을 발사해 폭발 시키기 위해서 *적군 탱크의 본체, 포탑, 포신 모두 **ENEMY** 태그*를 붙입니다.


> Instantiate( explosion, coll.transform.position, Quaternion.identity);
> 을 아래 같이 변경해서 포탄을 기준으로 
> Instantiate( explosion, transform.position, Quaternion.identity);
> 


*jsBullet.js*

```
#pragma strict

var snd: AudioClip;
var explosion: Transform;

function OnTriggerEnter(coll: Collider) {
  //explosion
  Instantiate( explosion, transform.position, Quaternion.identity);
  AudioSource.PlayClipAtPoint(snd, transform.position);
  Destroy(gameObject); // remove this bullet
  
  if( coll.gameObject.tag == "WALL") {
    Destroy(coll.gameObject); // remove a obstacle
  } else if( coll.gameObject.tag == "ENEMY") {

  } else if( coll.gameObject.tag == "TANK") {
    
  }
}
```


### 점수 표시

OnGUI()는 화면에 메시지나 이미지, 버튼 등을 출력하기 위해 사용하는 함수입니다. OnGUI()에서 작업한 내용은 항상 게임 화면에 표시됩니다. 
GUI(Graphical User Interface)는 유니티가 제공하는 가상의 Window이며 메시지나 이미지, 버튼 등을 표시할 수 있다. GUI는 게임 화면을 2D 화면과 같은 개념으로 취급하는데 화면의 왼쪽 위를 (0, 0)으로 하는 절대 좌표를 다루고 **Rect(<좌측상단 좌표>, <폭과 높이>)**  에 좌표를 담는다.

*jsScore.js*

```js
#pragma strict
static var hit = 0;    // 명중시킨 횟수
static var lose = 0;   // 피격당한 횟수
 
// 화면 출력
function OnGUI() {
    GUI.Label(Rect(10, 10, 120, 20), "명중 : " + hit);
    GUI.Label(Rect(10, 30, 120, 20), "피격 : " + lose);
}
```
 - GUI.Label: 텍스트 라벨 출력

점수를 표시하는 jsScore 스크립트를 작성해 아군 탱크에 연결한다.

줌수 계산은 포탄이 대상을 맞추는 jsBullet.js 파일의 Update() 에서 다룬다.

*jsBullet.js*

```js
  if( coll.gameObject.tag == "WALL") {
    Destroy(coll.gameObject); // remove a obstacle
  } 
  else if( coll.gameObject.tag == "ENEMY") {
    jsScore.hit++;  //점수 증가
    if( jsScore.hit > 5) {
      Destroy(coll.GameObject); // 적군 탱크 폭파
      // 승리 화면으로 간다.
    }

  } 
  else if( coll.gameObject.tag == "TANK") {
    jsScore.lose++; // 실점
    if( jsScore.lose > 5) {
      // 패배 화면으로 분기
    }  
  }
```

적군 탱크를 지울 때 *coll.gameObject* 객체를 사용하면 충돌을 일으키는 부분(오브젝트)만 제거하기 때문에 적군 탱크 전부가 제거 안된다. 그래서 Destroy()문을 다음과 같이 수정하면 탱크 전체가 제거될 것입니다.

```js
 Destroy(coll.transform.root.gameObject);         // 적군 탱크 파괴
```

Collider 는 계층구조일 때

<충돌체>.transform.parent            충돌체의 부모 오브젝트
<충돌체>.transform.root                충돌체의 최상위 오브젝트



## 참조
 - [저녘놀:탱크 게임2](http://foxmann.blog.me/90138221206)
 - [저녘놀:탱크 게임3](http://foxmann.blog.me/90139096997)
