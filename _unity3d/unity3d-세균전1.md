---
title: Unity3D 세균전 게임 만들기(1)
date: 2017-06-05 09:10:00 +0900
layout: post
tags: unity3d
categories: 
 - unity3d
---

이 게임은 게임코디 강좌를 따라한 것이다.
 - 세균전 게임 강좌 [^1]
 - 저자의 소스 [^2]

아래 같이 구성되고 타이틀 화면에서 클릭 혹은 터치로 게임 화면으로 전환 한다.

![](http://www.gamecodi.com/board/data/mutiupload/GAMECODI_HDD/54c1a11899b914284c5bf363d2e90e41.png){:width="600px"}


## 세균전게임 프로젝트

새로운 프로젝트를 시작해 

![](/images/unity3d/landwar-project-new.png){:width="450px"}

[그림. 새 프로젝트 ]

프로젝트 윈도우에서 이미지 등을 넣을 *Resources* 폴더, 화면 씬을 담을 *Scenes* 폴더를 그리소 소스 파일을 담을 *Scripts* 폴더를 만든다. 그리고 Hierachy 윈도우에서 빈 게임 객체를 두 개 만들고 각각의 이름을 *MainTitle*, *BattleRoom* 등으로 바꿔 준다.

![](/images/unity3d/landwar-project-new-assets.png){:width="450px"}

[그림. Assets 폴더와 새 게임 객체 ]

*Resources* 폴더 밑에 *images* 폴더를 만들고 다음 배경 이미지, 캐릭터 이미지... 들을 복사한다.


### MainTitle.cs

그리고 Scripts 폴더에 새 소스 파일 MainTitle.cs 파일을 만든다. 유니티 기본 클래스 파일은 아래 같은 내용을 생성한다.

*MainTitle.cs*

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MainTitle : MonoBehaviour {
    // Use this for initialization
    void Start () {
    }

    // Update is called once per frame
    void Update () {
    }
}
```

씬 스크립트의 *Start()* 는 씬이 랜더링 되기 전에 실행된다. *Update()* 함수는 매 프레임 마다 호출된다. MainTitle.cs 소스를 BattleRoom 객체에 연결한다.

![](/images/unity3d/landwar-project-battleroom.png){:width="350px"}

[그림. MainTitle.cs를 MainTitle 객체에 Script 연결]


### 타이틀 화면

이 소스가 활성화 되어서 마우스 버튼이 눌리면 battleroom 오브젝트를 활성화 시키고, 현재 오브젝트를 비활성화 시켜서 room화면으로 전환시킨다.

*MainTitle.cs*

```csharp
public class CMainTitle : MonoBehaviour {
    Texture bg;
    GameObject battleroom;

    void Start () {
      this.bg = Resources.Load("images/title_blue") as Texture;
      this.battleroom = GameObject.Find("BattleRoom");
      this.battleroom.SetActive(false);
    }

    void OnGUI() {
      GUI.DrawTexture( new Rect(0, 0, Screen.width, Screen.height), this.bg);
    }
}
```

Start() 함수에서 배경을 그리기 위해 [Resources.Load()](https://docs.unity3d.com/ScriptReference/Resources.Load.html) 함수로 *Assets/Resources/images* 폴더 밑의 이미지 이름으로 가져온다. 
> 리소스파일은 반드시 *Assets/Resources* 폴더 하위에 존재해야 한다.

*BattleRoom* 객체를 불러오기 위해 [GameObject.Find(name)](https://docs.unity3d.com/ScriptReference/GameObject.Find.html) 함수에 객체의 이름을 전달해 씬에 가져온다. 가져온 battleRoom 객체는 우선은 비활성화 한다.

#### BattleRoom 객체

화면에 마우스를 클릭하면 게임 객체 를 활성화 하기 위해 Update() 에서 활성화 한다. 이렇게 객체가 활성화 되면 객체에 연결한 스크립트의 Start()함수가 호출되면서 작동이 시작된다. 비활성화 되면 스크립트 역시 작동을 멈춘다. 

*MainTitle.cs*

```csharp
public class MainTitle : MonoBehaviour {
    void Update () {
      if( Input.GetMouseButtonDown(0)) {
        this.battleroom.SetActive(true);
        this.gameObject.SetActive(false);
      }
    }
}
```


#### MainTitle.cs 소스

*MainTitle.cs*

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MainTitle : MonoBehaviour {

    Texture bg;
    GameObject battleRoom;

    // Use this for initialization
    void Start () {
      //Resources.Load() 시 파일은 반드시 'Assets/Resources' 폴더에 위치한다.
      this.bg = Resources.Load("title_blue") as Texture;
      // gameobject for Scene
      this.battleRoom = GameObject.Find("BattleRoom");
      this.battleRoom.SetActiveRecursively(false);
    }
    
    // Update is called once per frame
    void Update () {

      if( Input.GetMouseButtonDown(Input.KeyCode.Mouse0)) {
        this.battleroom.SetActiveRecursively(true);  
        gameObject.SetActiveRecursively(false);
      }

    }

    void OnGUI() {
      // 게임 배경을 그린다.
      GUI.DrawTexture( new Rect(0,0,Screen.width, Screen.height, this.bg);
    }
}
```

 OnGUI함수는 유니티 엔진 내부에서 호출되는 함수입니다. 매 프레임(혹은 더 자주) 호출되는 함수이며 모든 draw코드가 들어가게 됩니다. 성능을 위해서는 OnGUI함수는 되도록 쓰지 않는 것이 좋습니다
 얼마 되지 않는 코드로도 DrawCall이 쭉쭉 올라가는 모습을 볼 수 있기 때문이죠
    // 이 강좌에서는 게임 로직에 집중하기 위하여 그냥 순수한 유니티의 기능 그대로를 사용해씁니다.    


실행해서 배경이 그려지고, 버튼 클릭하면 빈 화면이 보이는지 확인한다.


### BattleRoom.cs 소스

BattleRoom 객체에서 사용할 CBattleRoom.cs 소스를 작성한다.

```csharp
public class BattleRoom : MonoBehaviour {
  Texture bg, graycell, focus_cell, blank_skin, blank_image;
  Texture game_board, background, button_playagain;

  List<Texture> img_players;
  GameObject battleRoom;
  List<short> table_board;
  List<short> available_attack_cells;
  List<short> board;
  List<CPlayer> players;  //게임 참가자 
```

#### CPlayer 클래스

```csharp
// CPlayer.cs
public class CPlayer
{
  void Start()
  { }

  public void move()
  { }
}
```


#### 리소스 읽어 들이기

CMainTitle에 Awake()에서 필요한 자원을 읽어 들인다. Awake() 함수는 해당 스크립트가 작동될 때 제일 먼저 호출되는것을 보장하고, Start() 함수는 랜더링 직전에 호출됩니다. Awak함수와, Start함수는 최초 한번만 호출됩니다.

```csharp
    void Awake() {
      this.table_board = new List<short>(); // board list

      this.available_attack_cells = new List<short>();  
      this.graycell = Resources.Load("images/graycell") as Texture;  
      this.focus_cell = Resources.Load("images/border") as Texture;  
        
      this.blank_skin = Resources.Load("blank_skin") as GUISkin;  
      this.blank_image = Resources.Load("images/blank") as Texture;  
      this.game_board = Resources.Load("images/gameboard") as Texture;  
      this.background = Resources.Load("images/gameboard_bg") as Texture;  
      this.img_players = new List<Texture>();  
      this.img_players.Add(Resources.Load("images/blue") as Texture);  
      this.img_players.Add(Resources.Load("images/red") as Texture);  
        
      this.button_playagain = Resources.Load("images/playagain") as Texture;  
      this.board = new List<short>();  

    // CPlayer        
      this.players = new List<CPlayer>();  
      for (byte i=0; i<2; ++i)  
      {  
          GameObject obj = new GameObject(string.Format("player{0}", i));  
          CPlayer player = obj.AddComponent<CPlayer>();  
          player.initialize(i);  
          this.players.Add(player);  
      }  
        
      reset ();  
    }

```



게임을 재시작하여 변수값을 초기상태로 만들어야 하는 등의 코드는 clear나 reset같은 함수들을 만들어서
로직 흐름에 맞게 호출해주는 방식을 써야 합니다.

#### reset()

```
private void reset()
{
  this.players.ForEach(obj => obj.clear()); //clear() 함수로 전달
  this.players[0].add(6);
  this.players[0].add(42);
  
  this.players[1].add(0);
  this.players[1].add(48);
  this.players[1].change_to_agent();
  
  this.board.Clear();
  this.table_board.Clear();
  for(int i = 0 ; i < COL_COUNT * COL_COUNT; ++i)
  {
    this.board.Add(short.MaxValue);
    this.table_board.Add((short)i);
  }
  
  this.players.ForEach(obj => {
    obj.cell_indexes.ForEach(cell => {
      this.board[cell] = obj.player_index;
    });
  });
  
  this.current_player_index = 0;
  this.step = 0;
}

```

##### 익명함수 (람다 식)

```
(T x) => y  익명 함수(람다 식)
```

> https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/statements-expressions-operators/operators


##### C#에서 제네릭과 foreach

https://msdn.microsoft.com/ko-kr/library/bwabdf9z(v=vs.110).aspx


#### OnGUI()

성능을 위해서는 OnGUI함수는 되도록 쓰지 않는것이 좋습니다. 얼마 되지 않는 코드로도 DrawCall이 쭉쭉 올라가는 모습을 볼 수 있기 때문이죠.

```csharp
float ration = 1.0f;
void OnGUI() {

  // 화면의 가로 비율
  ratio = Screen.width / 800.0f;

  GUI.skin = this.blank_skin;

  // 게임 그린다.
  draw_board();

  // 재시작 버튼
  if( GUI.Button( new Rect(10, 10, 80 * ratio, 80 * ratio), this.button_playagain)) {
    MonoBehaviour.StopAllCoroutines();
    reset();
  }
}
```



## 참고

[^1]: 게임 코디: 세균전 게임1 [http://bit.ly/2s1H9bB](http://bit.ly/2s1H9bB)
[^2]: 게임 코디: 세균전 게임 소스 [https://github.com/karais89/landwar](https://github.com/karais89/landwar)


