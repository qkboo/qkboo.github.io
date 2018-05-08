# 지구-달과 태양

새로운 프로젝트를 만들고 유니티를 하나씩 사용해 보자,,,


> 이 이후 프로젝트 부터는 **[여기]() 에서 설명한데로 외부에디터인 Sublime Text**에서 사용한다.

## 회전 시키기

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RotateConroller : MonoBehaviour {

  public int rotateSpeed = 1;

  private Transform tr = null;

  // Use this for initialization
  void Start () {
     tr = this.gameObject.GetComponent<Transform> (); 
  }
  
  // Update is called once per frame
  void Update () {
    tr.Rotate(Vector3.up, rotateSpeed);
  }
}
```


**public** 으로 공개된 변수는 inspector window에서 해당 스크립 콤포넌트에서 접근할 수 있다. 그래서 시작할 때 해당 객체를 선택하고 run 하면 아래 그림 같이 스피드를 조정해 가며 확인할 수 있다.

![](/images/unity3d/unity-3d-texture2.png){:width="800px" height="600px"}

그리고 초당 회전 각도를 이용할 수 있다. 예를 들어 1초에 360도 돌면 1초에 1회전 하는 것이므로 측정하기 이해하기 쉽다.
아래 같이 회전시 각도에 따른 제한을 주려면 *Time.deltaTime* 을 준다.

```
    tr.Rotate(Vector3.up, rotateSpeed * Time.deltaTime);
```

여기에 스피어를 하나 추가하고 큐브는 rotateSpeed 를 90, 스피어는 rotateSpeed를 360 으로 지정하고 실행하면 서로 다른 속도로 회전하는 것을 확인 할 수 있다.

계층 윈도우에서 Sphere를 Cube 밑으로 Parent해서 실행해 보면 큐브를 중심으로 스피어가 함게 회전하면서 자신도 자전을 하는 모습을 확인할 수 있다.


## Skybox

화면의 하늘을 Skybox로 채워보자

### Material 이용해 만들기
Assets 에서 마우스 오른쪽에서 Create -> Material 을 만든다. 새로 만든 material의 이름을 skybox로 바꾸고, skybox 머터리얼의 Inspector window에서 Shader를 skybox로 해준다.

![](/images/unity3d/unity-3d-skybox1.png){: width="800"}

스카이박스 머터리얼을 드래그해서 Scene 화면에 놓는다.


### Skybox 추가하기

Terrain을 선택하고 Component -> Rendering -> Skybox 로 기본 스카이박스를 추가할 수 있다.

http://alolomersedess.tistory.com/entry/Unity3d-50-SkyBox를-적용하는-메뉴-위치가-사라졌네요-Render-Settings


### 스카이박스 만들기

http://alolomersedess.tistory.com/entry/Unity3d-50-SkyBox를-적용하는-메뉴-위치가-사라졌네요-Render-Settings


#### https://docs.unity3d.com/kr/current/Manual/HOWTO-UseSkybox.html
스카이박스의 6면 각각에 대응하는 6개의 텍스처를 만들고 프로젝트의 Assets 폴더에 둔다. 각 텍스처의 Wrap mode를 Repeat에서 Clamp로 변경해야 테두리 색상이 일치한다.

https://docs.unity3d.com/kr/current/uploads/Main/SkyboxWrapmode.png





## Build

Build는 작성한 씬을 실행 가능한 바이너리 파일로 만들어 준다. 여러 플랫폼을 지원하고 있다.


## 참조
 - [Using sublime Text as Script Editor](http://wiki.unity3d.com/index.php/Using_Sublime_Text_as_a_script_editor)