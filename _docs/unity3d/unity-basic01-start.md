---
title: Unity3D basic usage
date: 2017-06-07 09:10:00 +0900
layout: post
tags: unity3d, math
categories: 
 - unity3d
---


## 유니티 시작

New Project 로 프로젝트를 만든다.

![](unity3d/hello-project-new.png){: width="600px" height="480px"}

### 유니티 에디터 인터페이스

유니티 윈도우는 아래 같은 작은 윈도우와 뷰로 구성되어 있다. Scene View에 Game object 들이 있고 선택하면 Inspector Window에서 해당 객체의 속성을 변경할 수 있다.

1. 씬 뷰 Scene View
  - 게임이 만들어지는 화면
  - 완전히 렌더링 된 모습을 제공하는 게임 월드 에디터

2. 게임 뷰
  - 미리보기창, 플레이 모드에서 활성화

3. 계층 윈도우
 -  씬에 사용된 게임 오브젝트 리스트
 - 알파벳 순서에 따라 오브젝트 열거

4. 프로젝트 윈도우: 라이브러리 같은 역할을 하는 프로젝트에 포함된 애셋 리스트

5. 인스펙터 윈도우: 이 옵션들은 각각 한 칸당 하나의 '컴포넌트' 라고 합니다.

<!-- ![](https://docs.unity3d.com/kr/current/uploads/Main/Editor-Breakdown.png){: width="800px" height="600px"} -->
![](/images/unity3d/unity-interface2.png){: width="800px" height="600px"}

[그림. Unitiy Interface ]

이 구성은 layout 으로 지정되어 미리 배치가 되어 있다. 우측 상단의 Layout 버튼을 눌러 2 by 3, 4 split 레이아웃으로 변경해 보자. 그리고 윈도우의 탭을 드래그해서 위치를 변경하고 우측 상단의 Layout 버튼에서 새로운 레이아웃으로 저정할 수 있다.




### Scene view

새 게임객체(Game Object)를 추가하려면, GamgObject 메뉴에서 선택한다. 새 오브젝트로 큐브를  *GameObject->3D Object->Cube* 에서 추가해 보자.


![](/images/unity3d/hello-project-new.png){: width="800px" height="600px"}


#### 씬 뷰와 게임 객체

씬 뷰에 있는 게임객체를 선택하고 위치 이동, 회전, 확대 를 할 수 있다. 씬 뷰가 보이는 시점을 마우스와 키보드를 통하여 다양하게 조종할 수 있습니다. 또한 툴바의 Transform tool에 따라 씬뷰의 개체를 다루게 된다.

![](/images/unity3d/hello-project-new-object2.png)



#### 이동

왼쪽부터 핸드 툴, 이동 툴, 회전 툴, 스케일 툴 이다. 핸드 툴을 제외한 나머지 툴들은 각각의 x, y, z축을 독립적으로만 변경할 수 있다.

![](/images/unity3d/unity-transform-toolbar.png)

[그림. Camera]

- 핸드 툴: 씬 뷰 이동    ( 단축키: Q )
- 이동 툴: 선택한 오브젝트의 좌표를 이동.    ( 단축키: W )
- 회전 툴: 선택한 오브젝트의 회전한다.    ( 단축키: E )
- 스케일 툴: 선택한 오브젝트의 크기를 변경한다.    ( 단축키: R )

**핸드 툴**을 선택, 단축키 Q로 선택하고 마우스 클릭한 후에 씬 뷰를 움직이면 씬 뷰가 방향에 따라 이동한다. 

**이동 툴**, 단축키 W로 선택하면 씬 뷰에서 객체를 선택할 수 있다. 선택한 객체는 상태에서 마우스 왼쪽으로 선택하고  
 - 또한 윈도우/리눅스 Ctrl키, Mac은 Cmd키를 누르고 개체를 이동하면 Snap 단위로 이동한다.

씬 뷰에서 마우스와 키보드를 이용해 화면을 이동, 전환 할 수 있다.

- 확대/축소: 마우스 휠
- 자유회전: 마우스 오른쪽 버튼 드래그
- 가운데를 중심으로 회전: Alt키를 누른 채 마우스 왼쪽 버튼 드래그
- 이동: 핸드 툴 상태에서 마우스 왼쪽 드래그 
- 게임 객체 중심 포커스: 계층 뷰에서 물체를 선택한 상태에서 F키 또는 물체를 더블클릭

씬 기즈모는 현재 나의 시점이 어디를 바라보고 있는지를 나타내 줍니다.


#### 트랜스폼 툴

핸드 툴을 제외한 나머지 툴은 게임 객체를 x, y, z 방향으로 조작할 수 있게 x,y,z 방향 화살표 표시가 있고 이 방향으로 조작할 수 있다.

[그림. 게임 객체의 x,y,z 방향]


#### 뷰 포트 전환

http://zorotoss.com/?p=1112



### Hierachy

Hierarchy(계층도)는 현재의 씬에 있는 모든 게임 오브젝트를 포함하고 있다. 이들 중 몇몇은 3D 모델처럼 asset 파일의 instance 이며 나머지는 Prefabs의 instance 이다. 
계층도에서 오브젝트를 선택하거나 Parenting을 할 수 있습니다. 씬에서 추가되거나 제거되는 오브젝트들은 계층도에서도 마찬가지로 추가되거나 사라집니다.

계층 뷰에 등록된 오브젝트를 더블 클릭 하면 씬 뷰에서 게임 객체가 화면 중심에 선택된다. 혹은 계층 혹은 씬뷰엣 선택한 객체에서 단축키 F키를 누르면 화면에 포커스를 갖는다.


#### Parenting

유니티는 Parenting이라는 개념으로 상위구조가 하위구조(child) 게임 오브젝트를 종속시킬 수 있다. parent를 원하는 개체를 계층도에서 드래그 해 상위구조 항목에 갖다 놓으면 됩니다. 아래 그림 같이 상위-하위 개체 사이에 의존하게 된다.

![](/images/unity3d/unity-interface-parent.png)

하위구조에 속한 요소들은 상위구조가 움직임이거나 회전할 때 동일하게 영향을 받습니다. 


### Inspector view

유니티의 객체가 가진 콤포넌트를 속성을 보여준다.

유니니 객체는 기본적인 3D Object 혹은 Camera object 에 콤포넌트 요소를 담아 게임 요소를 만든다.

3D Object
  - Game object 
  - Transform component
    + 게임 객체의 Psition, Rotation, Scale에 대한 제어를 하는 스크립트
  - modeling component
    + 3D 모델링 형태에 관련한 데이터를 가지고 있는 스크립트
  - Mesh component
    + 실제 게임 객체를 화면에 랜더링 하기 위한 스크립트

Camera object
 - Game object
 - Camera component
 - Transform
 - Audio listener

인스펙터 뷰느느 게임 객체에 이런 콤퍼넌트를 대입하고 속성을 제공할 숭 있다.



###  유니티의 주요 요소

유니티에서 사용되는 게임 요소는 게임 오브젝트와 콤퍼넌트로 구성된다. 게임 오브젝트 game object는 여러개의 컴포넌트component로 이루어져 있으며 컴포넌트에는 구성요소인 변수들이 있습니다.

 - 게임 오브젝트: Scene view와 Hierach view에서 사용
 - 게임 콤포넌트: 요소의 기능을 다루는 스크립트

#### 게임 오브젝트

열려있는 씬에서 활성화된 오브젝트는 모두 게임오브젝트입니다. 프로젝트 패널의 모델이나 프리팹prefab 같은 애셋을 현재 씬에 배치(인스턴스화)하면 애셋은 게임 오브젝트가 됩니다.

파티클 시스템particle system이나 프리미티브 같은 오브젝트는 계층 패널에 있는 create버튼이나 인터페이스 상단의 **GameObject 메뉴**를 통해 생성합니다.

모든 게임 오브젝트는 하나 이상의 컴포넌트를 가지며 부모 자식 관계로서 특정 계층 구조를 만들수 있습니다.


#### 컴포넌트

컴포넌트는 오브젝트의 행동, 외형, 기능 등을 담당합니다.유니티 기본 컴포넌트도 많으며 심도있는 개발을 위해서 스크립트를 사용합니다.

스크립트는 C#, JAVA, BOO를 지원합니다. BOO는 모바일환경은 지원하지 않습니다. 스크립트 에디터는 기본적으로 모노디벨롭을 사용하며 별도로 설정을 통해서 다른 코드 편집기를 사용할 수 있습니다.

> 유니티 스크립트는 Monobehaviour라는 유니티 기본 명령을 담은 클래스가 있습니다.


#### 애셋 asset

애셋은 텍스처, 메쉬, 사운드 등 게임을 만드는데 사용되는 파일들을 뜻합니다.


#### 씬 scene

씬은 레벨 또는 게임의 한 영역 입니다. 각 씬은 별개로 구분되며 하나의 씬을 작업할 수 있습니다.
씬은 개층 패널과 씬 뷰를 통해서 작업됩니다.


#### 프리팹 prefab

게임 오브젝트를 애셋으로 저장하고 이것을 재사용하고 필요에 따라 인스턴스화 시키면 아주 효율적입니다.
재사용 할 수 있는 데이터의 틀을 만들기 위해서 오브젝트를 담아두는 빈 용기를 프리팹이라고 합니다.
생성된 인스턴스는 개별적으로 수정도 가능합니다.



### Scripting

https://code.visualstudio.com/docs/other/unity
https://code.visualstudio.com/docs?start=true

#### Code 

C#을 사용한다. 다음은 주요한 C# 요소를 구성한 코드이다.

```cs
using UnityEngine;    //--------------------------(1)

namespace MyGame {    //--------------------------(2)

  [RequireComponent(typeof(SpriteRenderer))] //---(3)

  class Alien : MonoBehaviour {  //---------------(4)
    public bool appearsPeaceful; //---------------(5)
    private int cowsAbducted;
    
    public void GreetHumans() {
      Debug.Log("Hello, humans!");
      if (appearsPeaceful == false) {
        cowsAbducted += 1;
      }
    }
  }
}
```

 - (1): `using` 키워드는 사용할 패키지를 가르킨다. *UnityEngine* 패키지는 핵심 유니티 형식을 포함하고 있다.
 - (2): namespace 에 사용자 형을 선언해서 다른 이름과 충돌을 피한다.
 - (3): *애트리뷰트*는 각괄호 사이에 위치하고 형과 메서드에 대한 추가적인 정보를 넣는다.
 - (4): *Class*는 *class*로 서언하고, 콜론 다음에 부모 클래스를 삽입하고, *MonoBehaviour* 의 하위 클래스면 스크립트 콤포넌트로 사용할 수 잇다.
 - (5): 변수는 클래스 필드로 선언된다.







## 게임 오브젝트와 엔진

유니티는 3D 기반 게임 엔진으로 여러 게임 오브젝트를 다룬다. 게임 오브젝트가 허용하는 행위는 콤포넌트로 구성된다. 

콤포넌트는 게임의 구성 블록이 되고 각 인스펙터에서 보이는 속성이 콤포넌트 속성이다.

유니티는 기본 매시 오브젝트를 제공한다. 

또한 스크립트도 콤포넌트로서 다뤄진다.

### 기본 매시 오브젝트

Scene view에 Hierachy view에서 create 버튼으로 Cube, Spine, Capsule, Cylinder, Plane을 생성해 보자. 


### 스크립트 콤포넌트

Monobihaviour 를 상속하는 스크립트는 스크립트 콤포넌트로 사용할 수 있다.



```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class AlienSpaceship : MonoBehaviour {

  public string shipName;

  // Use this for initialization
  void Start () {
  }
  // Update is called once per frame
  void Update () {
  }
}
```


스크립트는 게임 오브젝트에 추가되어 인스펙터에서 public 변수를 접근할 수 있다.

#### Component 접근

스크립트에서 게임 오브젝트의 콤포넌트는,

```cs
// gets the Animator component on this object, if it exists
var animator = GetComponent<Animator>();
```

#### 주요 메시지들.

**MonoBehaviours** 클래스는 유니티에 특별히 중요한 메서드를 가지고 있다.

```cs
public class BallController : MonoBehaviour {

    // Use this for initialization
    void Start () {
    }
    
    // Update is called once per frame
    void Update () {
    }
}
```




### 물리엔진돌

유니티에는 게임 요소의 물리적인 형상을 표현해주는 물리엔진을 제공하고 있다. 물리엔진을 이용해서 물체의 충돌, 접촉 시점 체크 등등...

게임 개체 사이의 충돌, 접촉을 위해 Collider, Trigger 라는 역할을 개념으로 제공한다.
 - Collider: 요소의 충돌에 대한 반응을 처리한다.
 - Trigger: 충돌에 대한 반환값을 처리한다. 

#### Collider 충돌체

Scene view 의 객체를 선택해 보면 외각에 초록색 선을 볼 수 있는데 이 선에 닿는 물체에 대해 충돌 처리를 하는 것을 Collider라고 한다. 선택한 오브젝트의 Inspectorview 의 속성에 Collider 속성을 볼 수 있다.

![](/images/unity3d/hello-project-sphere-collider.png)

게임 오브젝트들을 하나씩 선택해 보면 다른 collider가 붙어 있는 것을 알 수 있다. 이런 기본 collider 외에 다른 collider를 추가하려면 새로운 collider를 생성한다. Component / Physics / Collider 로 새로운 collider를 만들 수 있다.

재생 버튼으로 시작을 해보면 화면에 움직임이 없는 것을 확인할 수 있다. 이것은 게임 오브젝트는 물리적 작용을 스스로 할 수 없어서 움직임을 주는 Ridigbody를 추가해야 한다.


#### Rigid Body 리지드 바디

게임 오브젝트에 RigidiBody를 붙이는 방법은 Component / Physics / Rigidbody 콤포넌트를 추가해 준다.

![](/images/unity3d/hello-project-cube-add-rigidbody.png)

재생 버튼으로 시작을 하면 RigidBody를 추가한 오브젝트가 물리작용(Gravity)에 의해 화면 아래로 떨어지는 것을 볼 수 있다.

인스펙터뷰에서 RigidBody 속성의 *Use Gravity* 선택을 풀고 재생을 한 후에 마우스로 오브젝트를 이동해 충돌 시키면 충돌 효과가 발생하는 것을 확인해 볼 수 있다.

이렇게 유니티가 제공하는 물리엔진을 이용해 물리적인 효과를 줄 수 있다. 게임 오브젝트에 너무 많은 RigidiBody를 주면 계산 시간이 소요되어 CPU 소비가 많아지므로 중의해야 한다.





### Camera 

Hierach view에서 Main Camera를 선택하면 Scene View에 Camera가 선택되고 Camera preview 가 나타난다. 그리고 Inspector widnow의 Transform 속성에 카메라 위치, 회전, 배율을 조정할 수 있다.

![](/images/unity3d/unity-interface-camera.png){: width="800px" height="600px"}

[그림. Camera]




#### assets 폴더

https://docs.unity3d.com/Manual/SpecialFolders.html

유니티 프로젝트는 특수한 폴더를 가지고 있다. 

##### Assets

유니티 프로젝트가 사용하는 자료를 포함하는 주요 폴더.에디터 안의 내용이 이 폴더 자원과 연결된다. 그리고 대부분의 API는 자원이 모두 Assets 폴더 밑에 있다고 가정한다.

##### Editor

Editor 스크립트가 놓이는 폴더를 Editor라고 한다. 이 에디터 스크립은 실행중에 허용되지 않고 개발중에만 Editor에 기능을 추가해 준다.


### GUI

게임의 사용자 인터페이스는 다음 3가지 방법으로 만들 ㅅ 있다.

- Object GUI : 오브젝트로 GUI를 생성해서 콤포넌트의 GUI용 Trasform을 조절해 제어한다.
- OnGUI 함수: 스크립트로 GUI를 작성하고 제어한다.
- GUI plugin: 외부 플러그인으로 생성해 관리한다.


#### GUITexture 만들기

GameObject / Create Other / GUITexture 로 생성한다. 생성된 GUI는 Game View에서만 확인할 수 있다.




## 에셋 스토어 이용하기

메뉴 Window -> Asset Store 혹은 단축키 Ctrl+9 (macOS Cmd+9)으로 에셋 스토어를 열고 *Asset Store* 탭에서 Maximize 해서 전체창으로 확인하자. 
오른쪽 메뉴에서 보면 Top paid, Top Free, Top Grossing, Latest 메뉴가 있다.

### Tree 받기

검색어로 *tree* 를 입력하고 아래 옵션에서 **FREE ONLY** 로 찾아 보면 'Nature Start Kit 2' 가 있다. 다운로드가 완료되면 'Import' 버튼이 보이는데 시작한다.

## Multiple instance 

macOS에서 유니티를 여러개 실행하려면 스크립트 에디터에서 다음 같이 입력하고 Application 으로 저장한다.

```
tell application "Terminal"
    do script "open -n /Applications/Unity/Unity.app"
end tell
```




## Snap

Snap은 3D 객체 사이의 공간, 불일치, 밀착된 이동을 위해서 지정한 Snap 단위로 객체를 이동할 수 있게 해준다. 앞서 배웠듯이 씬 뷰에서 윈도우/리눅스 Ctrl키, Mac은 Cmd키를 누르고 개체를 이동하면 Snap 단위로 이동한다. 이런 단위는 Edit -> Sanp Settings 메뉴에서 변경하면 된다.

### 정점 스냅

정점 스냅은 두 오브젝트의 각 정점끼리 붙여주는 기능으로, 예를 들어 두개의 큐브가 있고 이 큐브를 이동시  정점이 기준되어 Snap으로 이동하게 하는 것이다.
움직일 큐브를 W키(변환 툴)를 눌러 선택한 후, V키를 꾹 누르시는동안 정점 스냅기능이 활성화 된다.


### 표면 스냅

표면 스냅은 오브젝트가 다른 오브젝트를 통과하지 않고 그 위에 살포시 얹는 기능입니다. 이 기능을 사용할 시, 캐릭터가 어느 지형을 갑자기 파고드는 현상을 막을 수 있습니다.
표면 스냅기능의 키는 Ctrl + Shift + 마우스 왼쪽클릭 이고, Ctrl 키와 Shift 키를 누르고 있어야 한다.


### Move To View

Move To View는 계층 뷰에서 선택한 오브젝트를 자신이 현재 보고있는 시점의 중앙으로 이동시켜줍니다.
GameObject -> Move To View 또는 Ctrl + Alt + F 로 쉽게 이동할 수 있습니다.

### Align With View

Align With View는 계층 뷰에서 선택한 오브젝트를 자신이 현재 보고있는 시점과 같은 곳으로 이동시켜줍니다.
GameObject -> Align With View 또는 Ctrl + Shift + F 로 쉽게 이동할 수 있습니다.


## 프리팹

Prefabs은 씬에서 재사용될 수 있는 게임오브젝트와 구성요소의 집합체입니다. 몇 개의 동일한 오브젝트들이 하나의 Prefab에서 만들어질 수 있습니다. (이를 instancing이라 합니다.) 나무로 예를 들어 보겠습니다. 나무 Prefab을 만든다면 몇 개의 동일한 나무들을 복사하여(instance) 당신의 씬에 넣는 것입니다. 모든 나무들은 Prefab에 링크되어 있기 때문에 Prefab에 적용되는 모든 변화들은 자동적으로 모든 복사된 나무들에게도(tree instance) 적용이 됩니다. 그렇기 때문에 매시나 재질이나 기타 다른 사항들을 변경하기 원한다면 Prefab만 한 번 바꿔주면 됩니다. 반대로 복사된 오브젝트를 수정한 뒤 메인메뉴의 GameObject->Apply Changes to Prefab을 통해서도 가능합니다. 이 작업은 asset을 설정해주고 업데이트 하는데 많은 시간을 절약할 수 있게 해줍니다.

http://kurosai.tistory.com/entry/Unity3D-프리팹

### 피라미드 프리랩 만들기

이렇게 계층 뷰에 만들어진 Pyramid를 프로젝트 뷰로 드래그해줍니다. Pyramid 프리팹이 완성되었습니다. 이제부터는 이 Pyramid 프리팹을 통하여 Pyramid를 양산할 수 있게 되었습니다. 


프로젝트 뷰에 있는 Pyramid를 드래그하여 씬 뷰로 이동시켜보십시오. Pyramid를 아까 프리팹을 설계할 때처럼 하나하나 제작하는 거와는 달리 쉽게 생성할 수 있게 되었습니다.


이렇게 생성한 프리팹은 프로젝트 뷰에서 Pyramid 오른쪽 클릭 -> Export Package를 통해 쉽게 파일로 보관할 수 있고, 나중에 사용할 수도 있습니다.

### Game Objects

이번엔 프리팹에 연결된 게임 오브젝트들을 관리하는 것을 알려드리겠습니다.
여럿 생성된 피라미드 중 하나를 선택하신 후 인스펙터 뷰를 봐주십시오.

#### 게임객체에 텍스쳐 입히기

Project 윈도우의 Assets 밑에 Picture라는 폴더를 만들고 사용할 이미지를 드래그한다. 화면의 큐브 혹은 게임 오브젝트에 원하는 사진을 드래그 하면 텍스쳐가 입혀진다.

![](/images/unity3d/unity-3d-texture1.png)



## 참조
 - [인터페이스에 대한 학습](https://docs.unity3d.com/kr/current/Manual/LearningtheInterface.html)
 - [Unity 3D의 인터페이스에 대하여](http://www.devkorea.co.kr/bbs/board.php?bo_table=m03_lecture&wr_id=192)
 - [Unity3D Asset Store 이용하기](http://rgy0409.tistory.com/295)
 - [kurosai Unity3D](http://kurosai.tistory.com/category/Game/Unity3D)
 - [Unity3D 스내핑](http://kurosai.tistory.com/entry/Unity3D-스내핑)