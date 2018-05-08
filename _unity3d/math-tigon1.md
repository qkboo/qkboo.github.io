---
title: Unity3D 삼각함수-1
date: 2017-06-07 09:10:00 +0900
layout: post
tags: unity3d, math
categories: 
 - unity3d
---

## 강체 (Rigid body) 회전

캡슐을 배치하고 마우스로 클릭하면 해당 방향으로 캡슐을 회전하게 해보자. 그리고 회전한 위치에서 새로운 객체를 튀어 나오게 표현해 보자.

### 새 프로젝트 만들기

> 프로젝트는 **Hello-Math** 프로젝트에 구성했다.

Inspector에서 MainCamera의 Projection 속성을 *Orthographic* 으로 지정한다.

MainTitle 씬이 다른 씬의 진입구 역할로 버튼을 배치해서 씬을 불러오게 한다.

#### MainTitle 씬 만들기

화면에 3D Object에서 Capsule을 추가한다. 게임이 시작하고 마우스 버튼을 클릭한 방향으로 캡슐의 끝이 방향을 바꾸도록 해보자.

MainTitle 씬은 앱의 진입구 역할로 다양한 씬을 불러오는 구조로 되어 있다. 먼저 GameObject 에서 Canvas를 추가한다. 그러면 2D 캔버스가 추가되고 이곳에 버튼, 텍스트 등을 넣고 정면을 바라보는 메뉴 같은 화면을 꾸밀 수 있다.

![](/images/unity3d/math-new-canvas2.png){: witdh="600px"}

2D 모습을 잘 비추려면 Camera의 설정에서 Project 콤포넌트를 Orthographic으로 해준다.

#### 씬 초기화

씬 초기화를 통해서 화면의 비율을 맞추자.

*SceneLoader.cs*

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;
using System.Collections;

public class SceneLoader : MonoBehaviour {

    void Awake () {
        float ratio = Mathf.Clamp((float)1024 / Screen.height, 0, 1);
        Screen.SetResolution((int)(Screen.width * ratio), (int)(Screen.height * ratio), true, 60);
    }
    public void Load(string name) {
        StartCoroutine(DoLoad(name));
    }

    IEnumerator DoLoad(string name) {
        AsyncOperation async = SceneManager.LoadSceneAsync(name);
        yield return async;
    }
}

```

#### MainTitle에 버튼 기능 적용하기

먼저 Maintitle에서 Button을 클릭하면 새로 만드는 Trigon1 씬을 부르게 하려고 한다. 그래서 씬을 읽는 스큽트 *SceneLoader.cs* 소스를 Maintitle 씬의 Main Camera에 끌어 놓는다.

Hierachy 윈도우에서 Button->Text를 선택해 버튼의 텍스트를 바꿔 주고, Button을 선택해서 *On Click()* 에 이벤트 함수를 지정한다. 함수는 *SceneLoader.cs*에 정의한 DoLoad() 함수를 선택한다. 이 함수는 전달한 씬 이름의 씬을 가져온다. *On Click()*에 전달하는 이름으로 'Trigon1' 이라고 하자.


#### Trigon1 씬 만들기

파일 메뉴에서 새로운 씬 만들기를 시작하고, Trigon1 이라고 저장한다.

![](/images/unity3d/math-new-trion1-scene.png){: witdh="600px"}

Canva를 추가하고, Button도 배치한다.
그리고 3D Game Object에서 Capsule을 추가한다. 씬 뷰에서는 게임 뷰 같이 보이지 않으므로 주의한다. Capsule의 위치를 (0,0,0) 으로 해주고 버튼과 적절히 보이게 배치한다.


Return 버튼을 누르면 메인 화면으로 돌아가게 하기 위해 *SceneLoader.cs* 소스를 Button에 드래그해주고,  *On Click()* 에 이벤트 함수를 *SceneLoader.cs*에 정의한 DoLoad() 함수를 선택한다. 이 함수는 전달한 씬 이름의 씬을 가져온다. *On Click()*에 전달하는 이름으로 'MainTitle' 이라고 하자.


#### Build Settings 

여러 씬이 있을 때 시작하는 씬은 빌드 설정에서 첫번째 씬이 된다. [File ▶ Build Settings] 메뉴를 클릭하면 Build Settings 창이 나타납니다.
 
프로젝트 탭의 Scenes 폴더를 열고 저장되어 있는 두 개의 화면을 이곳에 끌어다 두면 창이 Application에 등록될 것입니다. 등록한 창은 0부터 시작하는 레벨 번호를 부여됩니다. 이 레벨 번호나 화면 이름으로 해당 화면을 불러올 수 있습니다.  
레벨 0, 즉 맨 위에 놓인 씬 화면이 시작시 호출되어 화면에 보이게 된다.

위의 창을 닫고 게임을 시작한 후 아무 키나 누르면 메인 게임 화면으로 전환될 것입니다.


### Tigon1 문제


![](/images/unity3d/math-new-trion1-p1.png){: witdh="600px"}


#### Trigon1.cs

먼저 캡슐 객체를 찾는다.

```csharp
public class Trigon1 : MonoBehaviour {

    private GameObject capsule; // 캡슐   
    private float targetAngle = 0f;
    private rotSpeed = 4f;

    void Start () {
        capsule = GameObject.Find("Capsule");
    }
}
```

#### Update() 메소드

**Application.targetFrameRate** 에 지정한 프레임 수 만큼 호출 된다 (기본 30). 1회의 Update() 호출을 Frame이라 하고 1초당 프레임 수를 fps 하고 한다. 예를 들어 50fps는 1/50초 마다 호출된다는 의미.

> Update 내부 로직이 시간을 지연한다면 예정된 1프레임에 끝나지 않을 수 있다. 혹은 받ㄴ드시 일정한 간격으로 꼭 호출하고자 하면 **FixedUpdate()** 메서드를 사용한다.

마우스 포인터가 게임 객체가 아닌 곳에 있을 경우와 마우스 왼쪽 버튼이 눌린 경우에만 동작하게 한다.

```csharp
if (Input.GetMouseButtonDown(0) && !EventSystem.current.IsPointerOverGameObject()) { ... }
```

마우스 클릭 위치는 **Vector3** 객체로 (x,y,z) 좌표가 표현되어 있다.
 - 마우스 클릭 좌표: Input.mousePosition.x, Input.mousePosition.y 

클릭한 방향의 각도를 마우스 좌표를 받아 계산하는 *GetRotationAngleByTargetPosition()* 메서드로 얻는다:

```csharp
targetAngle = GetRotationAngleByTargetPosition(Input.mousePosition);
```


디버그 로그로 출력한다.

```csharp
Debug.Log (string.Format("mousePosition ({0:f}, {1:f})", Input.mousePosition.x, Input.mousePosition.y));
```


#### GetRotationAngleByTargetPosition() 메서드

1) 캡슐의 월드 좌표에서 스크린 좌표로 변환한다.

```csharp
Vector3 selfScreenPoint = Camera.main.WorldToScreenPoint(capsule.transform.position);
```

2) 객체 위치와 클릭한 위치의 차이

```csharp
Vector3 diff = mousePosition - selfScreenPoint;
```

3) 아크텐젠트로 각도를 계산.

Mathf.Atan2() 메서드를 사용해 계산하여 라디안 값을 Mathf.Rad2Deg 상수로 각도로 변환한다.

```
float angle = Mathf.Atan2(diff.y, diff.x) * Mathf.Rad2Deg;
loat finalAngle = angle - 90f;
```


#### 실제 객체의 방향을 바꾸자.

마우스 클릭한 방향 각으로 객체 방향을 회전 시키기 위해서 오일러 각 (Eeuler angles)[^1]을 사용한다. 여기서 2차원 Canvas가 x, y 를 전면으로 보이므로 캡슐의 회전은 z 축을 중심으로 회전해야 한다 그래서  Mathf.LerpAngle() 함수에 *capsule.transform.eulerAngles.z* 의 값을 선형보간 (Linear interpolation) 기법으로 클릭한 위치인 *targetAngle* 접근해 가면 된다. 이 회전 방향 좌표 (x,y,z) Vector3를 프레임마다 유니티에서 강체를 위한 회전 속성으로 `Transform.eulerAngles` 에 대입하면 부드럽게 회전하는 모습을 볼 수 있다.

```csharp
capsule.transform.eulerAngles
            = new Vector3(0, 0, Mathf.LerpAngle(capsule.transform.eulerAngles.z, targetAngle, Time.deltaTime * capsuleRotationSpeed));
```

> Time.deltaTime: 마지막 프레임의 초단위 실행 시간. 60fps라면 1/60.


#### Euler Angles [^1][^2]

강체의 방향을 3차원 공간 좌표계의 회전으로 이해하는 것이다.  3차원 공간 좌표계를 (x, y, z)라고 하고, 이를 회전시킨 좌표계를 (X,Y,Z)라고 하면, 강체의 방향은 다음의 세 각도로 표시될 수 있다.
 - (1) α\alpha (또는 ψ\psi ): z-축(파란색)을 회전축으로 하여 회전된 x-y 좌표축의 각도
 - (2) β\beta(또는 θ\theta ): 회전된 x-축(즉, N-축, 녹색)을 회전축으로 하여 회전된 z-y 좌표축의 각도
 - (3) γ\gamma (또는 ϕ\phi ): 위에서 회전된 z-축(즉, Z축, 빨간색)을 회전축으로 하여 회전된 x-y 좌표축의 각도



#### 실행

Trigon1.cs 소스를 Trigon1 씬의 Canvas 객체에 드래그 해서 게임 시작후 마우스 이벤트를 받도록 한다.
그리고 (Ctrl+P, Cmd+P)로 시작해서 화면을 클릭할 때 마다 캡슐이 클릭한 방향으로 고개를 트는 것을 볼 수 있다.

#### 구체 나타나게 하고 튀게 하기

화면에 클릭한 위치에 구체 (Sphere)가 나타나고, 화면 중앙을 향해 움직이게 해보자.

```
private GameObject sphere;
public float sphereMagnitudeX = 2.0f;
public float sphereMagnitudeY = 3.0f;
public float sphereFrequency = 1.0f;
```

Update 메소드에서 sphere 객체가 생성되고 null 이 아니면, 캡슐의 방향 회전이 이뤄진 후에 구체가 위치하는 좌표를 다음 같이 배치할 수 있다:

```csharp
if (sphere != null) {
    sphere.transform.position = new Vector3( sphere.transform.position.x + (capsule.transform.position.x 
        - sphere.transform.position.x) * Time.deltaTime * sphereMagnitudeX,
        Mathf.Abs(Mathf.Sin ((Time.time - buttonDownTime) * (Mathf.PI * 2) * sphereFrequency) * sphereMagnitudeY),
        0
    );
}
```
 - 구체의 x 좌표를 서서히 캡슐의 x 좌표에 가깝게 한다.
 - 

## 참조

[^1]: 오일러 각 https://ko.wikipedia.org/wiki/오일러_각
[^2]: 역학-오일러 각 설명 http://blog.naver.com/hongjeonga/150096103061

