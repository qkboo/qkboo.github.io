# 공 굴려 보기

앞서 만든 지형에에서 Sphere 를 물리적인 힘으로 굴려 보자. 굴리는 방법은 화살표 키로 입력을 하면 해당 강도 만큼 공이 바닥면에서 구르게 된다.

바닥에 공이 있고 Rigidbody의 물리적 특성으로 움직이게 해보자.

## Rigid body

Rigid body(강체(剛體)란 물리학에서 형태가 고정되어 변하지 않는 물체를 가리킨다. 


### 환경 구성하기

바닥을 만들어야 게임 객체들이 배치되고 움직일 수 있다. 바닥을 만들기 위해 새 plane 추가하고 position을 (0,0,0)으로 한다.
그리고 Sphere 객체를 추가하고 위치를 (0,0.5,0) 으로 해주면 바닥면에 닿게 배치가 된다.

#### Rididbody 추가하기

Rigidbody는 Components 메뉴 혹은 Inspector window에서 게임객체에 추가할 수 있다.

- 게임 객체를 선택하고 **Compnent -> Physics -> Gigidbody** 를 추가한다.
- Inspector Window에서 **Add Components** 에서 추가한다.

![](unity3d/hello-project-cube-rigidbody.png)

Rigidbody에서 중력관련한 선택 사항은,

 - Use Gravity: 중력 사용
 - Is Kinematic: 객체가 스크립트로만 동작한다.

이제 Rigidbod를 가진 객체에 방향키에 따라 움직임을 주려면 스크립트를 이용한다.


#### Rigibody 이용 움직이기

다음 같이 void FixedUpdate() 메서드에 입력을 받아 Vector3 로 구성한 좌표 방향으로 Rigdbody.addForce() 를 주면 물리적 힘이 가해진다.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TestController : MonoBehaviour {

  private Rigidbody rigiBody;

  // Use this for initialization
  void Start () {
    rigiBody = GetComponent<Rigibody>();
  }
  // 
  void FixedUpdate() {
    float moveHorizontal = Input.GetAxis("Horizontal")
    float moveVertical = Input.GetAxis("Vertical");

    Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);

    this.rigiBody.AddForce(movement);
  }
}
```

실행을 하고 방향키로 이리저리 공을 굴려보자.

공은 바닥면을 넘어서면 아래로 떨어진다. 이것은 공에 Gravity 중력이 작용해서 바닥이 있을 때 까지 떨어지는 것이다.


### 캡슢

캡슐 객체를 추가하고 위치를 (0,1,0)으로 하면 바닥면 놓여 진다.


## 참조
 - [Using sublime Text as Script Editor](http://wiki.unity3d.com/index.php/Using_Sublime_Text_as_a_script_editor)