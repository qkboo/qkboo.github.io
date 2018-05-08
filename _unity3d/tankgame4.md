# 시작 화면 만들기 

프로젝트 탭에서 화면 저장용 폴더를 만들고 이름을 Scenes로 바꿉니다. [File ▶ Save Scene As] 메뉴 또는 [Ctrl + Shift + S] 키를 눌러 현재의 화면을 MainGame으로 저장한다.

## 시작 화면

현재 MainGame 씬을 Save Scens As로 GameStart 씬 파일로 저장한다.
여기저기 폭파 불꽃도 배치하고, 적탱크는 분해해서 흩어놓으면 

카메라는 아군의 포신을 추적하도록 설정하고 옵션을 다음과 같이 설정합니다.
 - Distance      2.5
 - Height          1

3D Text를 2개 만들고 인스펙터에서 Text에 각각 ‘Start!’, ‘Press Any Key’로 써 놓는다.

### 승리 화면

현재 GameStart 씬을 Save Scens As로 GameStart 씬 파일로 저장한다.



### Scene 등록
 
화면(Scene)을 저장한다고 자동으로 불러주는 것은 아니므로 이 화면을 어플리케이션에 등록합니다. [File ▶ Build Settings] 메뉴를 클릭하면 Build Settings 창이 나타납니다.
 
프로젝트 탭의 Scenes 폴더를 열고 저장되어 있는 두 개의 화면을 이곳에 끌어다 두면 창이 Application에 등록될 것입니다. 등록한 창은 0부터 시작하는 레벨 번호를 부여됩니다. 이 레벨 번호나 화면 이름으로 해당 화면을 불러올 수 있습니다.  
레벨 0, 즉 맨 위에 놓인 씬 화면이 시작시 호출되어 화면에 보이게 된다.

위의 창을 닫고 게임을 시작한 후 아무 키나 누르면 메인 게임 화면으로 전환될 것입니다.



## 화면 호출

```
if (coll.gameObject.tag == "WALL") {
        Destroy(coll.gameObject);                                 // 장애물 제거
    } else if (coll.gameObject.tag == "ENEMY") {
        jsScore.hit++;                                                  // 점수 증가
        if (jsScore.hit > 5) {                                          // 5점 이상이면
            Destroy(coll.transform.root.gameObject);          // 적탱크 파괴
            Application.LoadLevel("WinGame");        // 승리 화면으로 분기
        }
    } else if (coll.gameObject.tag == "TANK") {
        jsScore.lose++;                                                // 실점 증가
        if (jsScore.lose > 5) {                                       // 5점 이상이면
            Application.LoadLevel("LoseGame");     // 패배 화면으로 분기
        }
    }
```



## 이 프로젝트의 문제점
 
이 프로젝트는 다음과 같은 문제가 미해결인 상태로 남아 있습니다.
 
    ① 포탄이 장애물이나 탱크에 명중하지 않은 경우에는 메모리에서 제거되지 않는다.
    ② 게임 도중에 [Esc] 키 등으로 게임을 끝내는 기능이 없다.
    ③ 게임의 옵션 설정(사운드 On/Off 등) 기능이 없다.


## 참조
 - [저녘놀:탱크 게임3](http://foxmann.blog.me/90139096997)
 - [저녘놀:탱크 게임4](http://foxmann.blog.me/90139552882)
