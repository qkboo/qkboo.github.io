---
title: Sublime Text로 Unity3D Script 작성하기
date: 2017-06-02 09:10:00 +0900
layout: post
tags: unity3d, sublimetext
categories: 
 - unity3d
 - sublimetext
---

# Sublime Text로 Unity3D Script 작성하기

MonoDeveloper 에서 한글 입력이 안된다. 외부의 자주 사용하는 에디터를 대신 사용해도 좋다. 여기서 Sublime Text 3 에디터를 사용한다.

> 단축키 표시시 **Windows**와 **macOS**용 단축키를 **(ctrl+1,cmd+1)** 형식으로 표시한다.

### External Editor:: Sublime Text

Sublime Text 3를 다운로드하고 설치한다. (이하 Sublime Text 3를 **SB**로 하겠다)


#### Package manager 관련


Side Bar Enhancements - Provides extra options when right-clicking on a file or folder on the sidebar.
Sublime Linter 3 - A framework for creating linters for a variety of programming languages. An



#### Unity3D 관련 패키지 설치

패키지 관리자에서 *install package*로 unity3d 로 검색해서 Unity3D 패키지를 설치한다. 설치후 확장자에 의해서 C# or JavaScript 파일의 문법 부각 효과가 나타난다.

you can select "View" -> "Syntax" -> "Unity3D" and then either "Unity C#" or "Unity JavaScript". Y

##### Code Autocompletion

Setting C# Unity API Classes Autocompletion
Install the following Sublime Text packages by following the instructions on their respective Github README:
"Unity Completions Package Light" or "Unity Completions Package" for Unity API element autocompletion
"C# Snippets" for inserting useful C# snippets
"Unity3D Snippets and Completes" for inserting useful Unity related snippets
"CompleteSharp" for dynamic autocompletion


#### Project  생성

SB에서 프로젝트를 생성하면 잇점이 있다:
 - Context : 스크립트 폴더를 지정해 검색과 탐색이 쉬워진다.
 - Settings : filter out irrelevant files (e.g., .meta files)

SB에서 프로젝트를 폴더 단위로 관리하는게 권장된다. 그래서 Unity3D 프로젝트 폴더를 Sublime Text에서 Open으로 연다 - 이 폴더는 Assets 폴더 바깥쪽이다. 그리고 이 폴더에 프로젝트 파일을 생성하기 위해서 Project 메뉴에서 Save Project As로 *hello1_project.sublime-project* 파일[2]을 저장한다.
SB에서 *hello1_project.sublime-project* 파일을 열고 다음 같이 입력해 준다.
 - *Assets/Scripts* 폴더만 열고, .meta 와 .dll 바이너리 파일은 제외해 준다.

```
{
    "folders":
    [
        {
      "path": "Assets/Scripts",
      "file_exclude_patterns":
      [
        "*.dll",
        "*.meta"
      ]
    }
    ]
}
```




#### 스크립트 폴더

Unity3D 에서 Project 윈도우에서 Assets 폴더 위헤서 마우스 우클릭으로 폴더를 만들고 이름을 *Scripts*로 주면 SB에서 이 폴더의 소스를 쉽게 다룰 수 있다.

스크립트를 SB에서 작성하고 수정해서 Unity3D의 프로젝트 윈도우의 스크립트를 드래그해서 필요한 게임객체에 드래그해서 넣어 주면 새로운 콤포넌트로 추가된다.


### Error 

Scripts/ 밑에 있는 소스 코드를 서브라임텍스트 같은 외부 에디터로 작업하면 문법 오류 등의 에러가 유니티에서 파싱하며 에러가 있으면 상태바에 빨간색으로 에러가 발생한 라인 번호, 글자 위치가 나타난다. 이 에러를 클릭하면 콘솔에 자세한 내용이 표시된다.

![](/images/unity3d/unity3d-source-error.png)

서버라임텍스트에서 해다아 줄번호에서 오류를 수정하고 저장하면 된다. 유니티에서는 리플레시 (단축키 Ctrl+R, Cmd+R)를 실행해 주면 수정한 내용을 다시 읽어 온다.



## 참조
 - [Using sublime Text as Script Editor](http://wiki.unity3d.com/index.php/Using_Sublime_Text_as_a_script_editor)
 - [](http://sleepydev.tistory.com/4)
 - [Unity 에디터 삽질기록](http://sleepydev.tistory.com/2)


[1]: http://wiki.unity3d.com/index.php/Using_Sublime_Text_as_a_script_editor
[2]: http://docs.sublimetext.info/en/latest/reference/projects.html