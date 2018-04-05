
초보자를 위한 위니티 5 입문

 - 캐릭터를 다루는 방법 
 - 터레인으로 스테이지 (땅, 산)를 만드는 방법,텍스처 사용법
 - 스테이지에 캐릭터 배치하기
 - 에셋 임포트 , 에셋 스토어에서 리소스 얻기
 - 파티클 사용법 

## 캐릭터 움직이기

이 프로젝트에서는 외부 자원을 가져와 새 프로젝트에서 사용해 보겠다.

### 새 프로젝트

새 프로젝트를 생성하고 Assets 폴더에 Scenes, Sprites, Scripts, PhysicsMaterials 폴더를 만든다.


http://dw.hanbit.co.kr/exam/2250/ 접속해서 에셋 패키지 다운로드한다. 다운 받은 Unity5Assets_kr.zip 에서 Chapter4 Terrain을 원하는 폴더에 저장한다.

패키지 불러오기는 *Assets > Import Package > Custom Package* 메뉴에서 패키지 선택해서 열고 불러들일 파일 선택해 Import 클릭한다.

![](unity3d/import-custom-package.png)


### 스테이지 만들기

프로젝트에 터레인 추가 Hierarchy 뷰에서  *Create > 3D Object > Terrain* 

일반적으로 오브젝트의 크기를 변경할 때 Transform의 Scale 값 변경하지만 터레인의 크기는 인스펙터 터레인 인스펙터 뷰에서 Terrain Settings 를 실행해 표시되는 Resolution 속성의 값으로 변경한다.
 - Terrain Width – X 축의 길이 : 100
 - Terrain Length – z 축의 길이 : 100

그리고 카메라의 위치를 카메라의 위치 (Position) 와 Rotation (기울기) 값 변경
 - Poisition: (53, 26.5, -4.07)
 - Rotation: (41.7, 0, 0)

#### 터레인 텍스쳐 입히기

앞서 추가한 패키지에서 잔디와 흙 텍스처를 터레인의 바닥면 질감으로 변경해 보자. 

그러기 위해서 터레인 인스펙터의 Paint 속성에서 Edit Texture를 실행해 잔디 텍스처를 추가해 준다.

![](unity3d/terrain-add-texture.png)

터레인 인스펙터에서 흑 텍스처를 선택하고 브러시 크기를 조절한 다음에 씬 화면에 색 칠하듯이 적당한 칠을 해보자.

![](unity3d/terrain-paint-texture.png)

#### 산 모양

터레인 인스펙터 뷰에서 Raise / Lower terrain을 선택하고 씬 화면에서 위 아래로 올리면 산 모양으로 텍스쳐가 입혀 진다.


#### 캐릭터

애니메이터로 제공된 캐릭터를 끌어다 Hierachy 뷰에 추가한다.


카메라 범위를 벗어나도 캐릭터가 보이도록 Assets 메뉴 *Import Package > Cameras > Import* 로 표준 에셋을 가져온다.
그리고 Assets / Standard Assets / Cameras / Prepabs / FreeLookCameraRig 를 Hierachy 뷰에 끌어 놓는다.

FreeLookCameraRig 는 Player 라는 tag를 쫓아 다닌다. 그래서 캐릭터를 선택하고 Tag를 Player로 해준다.


### Asset Store 

Windows / Asset Store 
