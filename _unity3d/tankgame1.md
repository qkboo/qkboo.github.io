# Tank Game

탱크

## 탱크 구성

먼저 바닥을 만들고 그 위에 탱크 본체를 구성한다.

### 본체와 바닥

새 Plane 객체를 추가하고 이름을 **바닥** 이라고 한다. 인스펙터 윈도우에서 위치를 (0, -0.5, 0), 크기 조절을 위해서 Scale을 (3. 1. 3) 정도 한다.

그리고 각 본체, 포탑, 포신을 조립한다.

| 오브젝트 | 이 름 | 속 성  |
| ---------- | ------- | ----------------------------------- |
| Cube     | 본체 | Position:(0, 0, 0) <br/> Scale(1.5, 1, 4)|
| Sphere   | 포탑 | Position:(0, 0.75, 0.6) <br/> Scale(1.5, 1, 4)|
| Cylinder | 포신 | Position:(0. 1.1, 2.6) <br/> Rotation:(80, 0, 0) <br/> Scale:(0.3, 1.5, 0.3)|

- 탱크 본체의 중심이 게임의 원점(0, 0, 0)이 됩니다. 

그리고 계층 윈도우에 탱크/본체/바퀴,바퀴,본체2, 탱크/포탑/포신,포타2 같이 객체를 배치한다.

그리고 Assets 폴더에 자바스크립트 폴더로 jsScript 폴더를 만든다.

#### 탱크 스크립트

jsTank.js 파일을 하나 만들고 다음 같이 입력한다.

```js
#pragma strict

var speed = 5; // transform speed (meter/s)
var rotSpeed = 80; // Rotation speed (degree/s)

var turret: GameObject; // Turret

function Start () {
}

function Update () {
    var amtToMove = speed * Time.deltaTime; // 이동 거리
    var amtToRot = rotSpeed * Time.deltaTime; // 회전 각도
    
    var front = Input.GetAxis("Vertical"); // 전후진
    var ang = Input.GetAxis("Horizontal"); // 좌우 회전
        
    transform.Translate(Vector3.forward * amtToMove ); // 전.후진
    transform.Rotate(Vector3(0, ang * amtToRot, 0)); // 회전
}
```


 - static: static으로 선언한 변수는 다른 스크립트에서 참조가 가능한 전역변수입니다. 다른 스크립트에서 이 변수를 참조할 경우에는 jsScore.hit와 같은 형식으로 변수 앞에 파일명을 붙입니다.  

 - **transform** 객체: 씬 안의 모든 게임 객체는 *transform* 객체를 내장하고 있다. 이것은 게임 객체의 위치, 회전 그리고 배율에 대한 속성을 가지고 있다.

이 스크립트를 계층 윈도우의 '탱크' 객체에 드래그해서 끌어 놓고, 포탑을 탱크 인스펙터 윈도우의 'Turret' 항목에 드래그로 끌어 놓는다.

![](/images/unity3d/tank1-jsscript.png){: width="800"}

여기까지 하고 씬을 저장한다.

#### 포탑 회전하기

포탑을 Q, E 키로 회전 시키자.

*Edit ▶ Project Settings ▶ Input* 메뉴를 눌러 키 설정 상태를 인스펙터에 표시 하고, 속성을 다음과 같이 설정한다. 

| 항 목 | 값 |
| --------------------- | ----------------------------------------- |
| Name | MyTank |
| Alt Negative Button | q |
| Alt Positive Button | e |
| Gravity | 3 |
| Dead | 0.001 |
| Sensitivity | 3 |
| Snap | check |
| Type | Key or Mouse button |

Alt Negative Button 속성의 키가 

그리고 Update()에 다음 포탑 관련 코드르 추가 한다.

```js
//포탑회전
var ang2 = Input.GetAxis("MyTank"); // 탱크 방향 벡터
turret.transform.Rotate(Vector3.up * ang2 * amtToRot); 
```

#### 포탄 만들기

포탄을 만들고 발사할 수 있게 해보자. 

| 항 목 | 값 |
| --------------------- | ----------------------------------------- |
| Sphere | 포탄 | Scale x, y, z <br/> Rigidbody |  (0.2, 0.2, 0.5) |

포탄 객체에 Rigidbody를 추가해 준다.

그리고 포탄을 반복해서 발사하므로 반복적으로 사용할 수 잇게 프리팹으로 만든다. 프로젝트 탭에서 [Create] 버튼을 눌러 Prefab을 하나 만듭니다. New Prefab이 만들어지면 이름을 pfBullet으로 이름을 바꿉니다. 이 프리팹에 앞에서 만든 포탄을 끌어다 두면 하이어라키 탭의 포탄이 프리팹으로 등록이 됩니다. 하이어라키 탭에 표시된 **포탄** 이라는 글자가 *파란색*으로 바뀔 것입니다.

![](/images/unity3d/tank1-prefab.png){: width="800"}

프리팹은 코드에서 불러다 사용할 수 있고, 계층 윈도우에 '포탄'을 지워도 무방하다. 그리고 다음 같이 프리팹으로 만든 포탄을 스크립트의 Bullet 에 드래그해서 연결해 준다.

![](/images/unity3d/tank1-prefab-bullet.png){: width="800"}


이렇게 구성한 프리팹 호출은 다음 형식으로 포탑의 위치를 통해 추쿨한다.

> Instantiate(<프리팹>, <현재위치>, <현재각도>);

다음은 코드이다.

```javascript
#pragma strict

var speed = 5; // transform speed (meter/s)
var rotSpeed = 80; // Rotation speed (degree/s)

var turret: GameObject; // Turret

//포탄발사
var power = 2000; // speed of bullet
var bullet: Transform; // bullet

function Start () {
}

function Update () {
    var amtToMove = speed * Time.deltaTime; // 이동 거리
    var amtToRot = rotSpeed * Time.deltaTime; // 회전 각도
    
    var front = Input.GetAxis("Vertical"); // 전후진
    var ang = Input.GetAxis("Horizontal"); // 좌우 회전
  //포탑회전
    var ang2 = Input.GetAxis("MyTank"); // 탱크 방향 벡터
        
    transform.Translate(Vector3.forward * amtToMove ); // 전.후진
    transform.Rotate(Vector3(0, ang * amtToRot, 0)); // 회전

    //포탑회전
    turret.transform.Rotate(Vector3.up * ang2 * amtToRot); 

    //포탄발사
    if(Input.GetButtonDown("Fire2")) {
        var myBullet = Instantiate( bullet, turret.transform.position, turret.transform.rotation);
        myBullet.rigidbody.AddForce(turret.transform.forward * power );
    }

}
```

① 변수에 프리팹을 등록한다.
② Fire1은 [Ctrl] 키이며 그 아래에 있는 문장은 다음과 같은 의미이다.


프리팹을 변수로 등록할 때에는 Transform으로 선언합니다. GameObject로 선언해도 되지만 사용법이 조금 다릅니다. 자세한 것은 차차 배울 것이므로 Transform으로 선언하는 것으로 알고 있으면 됩니다. 

현재 포탄이 발사되기는 하는데 포신에서 발사되는 것이 아니라 포탑에서 발사되어서 아주 어색합니다. 가끔씩 포탄이 뒤쪽으로 발사되기도 하구요. 포탄이 뒤로 발사되는 것은 포탄이 발사되면서 포탑과 충돌을 일으켜 반대편으로 튕겨 나오기 때문입니다. 


#### 로컬 좌표와 글로벌 좌표

글로벌 좌표는 게임 화면 전체에 대한 좌표이며, 로컬 좌표는 현재 오브젝트를 기준으로 하는 좌표입니다. 포탄은 탱크 본체가 아니라 포탑이 향하고 있는 방향으로 발사되어야 합니다. 그렇기 때문에 Instantiate에서 좌표를 설정할 때 포탑의 위치(position)와 포탑의 방향(rotation)을 참조하고 있습니다. 


#### Spawn Point 설정

스판 포인트(Spawn Point)는 캐릭터가 등장하는 위치를 의미하는 용어입니다. 탱크에도 포구 끝에 스판 포인트를 설정하고, 포탄이 스판 포인트에서 발사되게 해야 한다.

    ① 빈 오브젝트를 하나 만들고 이름을 spawnPoint로 바꾼다.
    ② 위의 오브젝트를 탱크의 포신 앞에 배치한다.
    ③ 계층 윈도우의 spawnPoint를 탱크의 포탑 아래로 이동해서 하위 아이템으로 만든다.
스판 포인트를 설치할 때 포신과 방향이 일치하도록 해야 합니다. 이때 포탄의 길이를 고려해서 포신보다 조금 떨어진 곳에 배치합니다. 

그리고 포탄 발사하는 코드를 다음 같이 수정한다:

```javascript
//포탄발사
if(Input.GetButtonDown("Fire1")) {
    var spPoint = GameObject.Find("spawnPoint"); // spawnPoint 정보 읽기 
var myBullet = Instantiate(bullet, spPoint.transform.position, spPoint.transform.rotation);
    myBullet.GetComponent.<Rigidbody>().AddForce(turret.transform.forward * power );
}
```
  -  프로그램은 spawnPoint를 찾아서 그 정보를 읽고 포탄의 초기 위치와 각도 등을 설정해서 발사하는 것입니다. 이제 프로그램을 실행하면 포탄이 포구 끝에서 제대로 발사되는 모습을 볼 수 있습니다. 
  -  



## 참조
 - [저녘놀:탱크 게임1](http://foxmann.blog.me/90137976278)
