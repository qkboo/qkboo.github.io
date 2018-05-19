---
title: "Navigation"
permalink: /docs/navigation/
excerpt: "핵심 navigation과 breadcrumb links를 어떻게 다루는지에 대한 단계를 설명하고 있다. Instructions on how to customize the main navigation and enabling breadcrumb links."
notes:
  - "한글 추가, qkboo"
last_modified_at: 2018-03-20T15:59:40-04:00
toc: true
---

## Masthead

masthead 링크는 *priority plus* 디자인 패턴을 사용한다. 여러 네비게이션 아이템은 수평 영역을 표시하도록 토글 된다.<br>The masthead links use a "priority plus" design pattern. Meaning, show as many navigation items that will fit horizontally with a toggle to reveal the rest.

이런 링크는 제목과 주소를 `_data/navigation.yml` 안의 `main` 키 아래 추가해 준다<br>To define these links add titles and URLs under the `main` key in `_data/navigation.yml`:

```yaml
main:
  - title: "Quick-Start Guide"
    url: /docs/quick-start-guide/
  - title: "Posts"
    url: /year-archive/
  - title: "Categories"
    url: /categories/
  - title: "Tags"
    url: /tags/
  - title: "Pages"
    url: /page-archive/
  - title: "Collections"
    url: /collection-archive/
  - title: "External Link"
    url: https://google.com
```

응답형 masthead 는 아래같이 표현된다. <br>Which will give you a responsive masthead similar to this:

![priority plus masthead animation]({{ "/assets/images/mm-priority-plus-masthead.gif" | relative_url }})

선택적으로 `main` 안의 타이틀 마다 `description` 을 넣을 수 있어서, 화면에서는 툴팁 같이 표시된다. <br>Optionally, you can add a `description` key per title in the `main` key. This `description` will show up like a tooltip, when the user hovers over the link on a desktop browser.

**ProTip:** 가장 중요한 링크를 첫번째 두면 메뉴가 토글되도 보인다. <br>Put the most important links first so they're always visible and not hidden behind the **menu toggle**.
{: .notice--info}

## Breadcrumbs (Beta)

**breadcrumb** 링크를 활성화 하면 방문자가 사이트들 더 깊숙히 들여다 볼 수 있다.

Enable breadcrumb links to help visitors better navigate deep sites. Because of the fragile method of implementing them they don't always produce accurate links reliably. For best results:

1. 분류를 기준으로 **permallink** 구조를 사용. `permalink: /:categories/:title/`
2. 각 분류에 대한 페이지를 수동으로 생성하거나 [jekyll-archives](https://github.com/jekyll/jekyll-archives) 플러그인을 사용해 자동으로 생성한다. 만약 이들 페이지가 없다면 **breadcrumb** 는 깨지곳에 연결한다. If these pages don't exist breadcrumb links to them will be broken.

![breadcrumb navigation example]({{ "/assets/images/mm-breadcrumbs-example.jpg" | relative_url }})

```yaml
breadcrumbs: true  # disabled by default
```

Breadcrumb는 텍스트와 구분자로 시작한다. `_data/ui-text.yml` 에서 변경가능.

```yaml
breadcrumb_home_label : "Home"
breadcrumb_separator  : "/"
```

Breadcrumb 아래 같이 구성하면 `Start > Blog > My Awesome Post`  같이 보여진다.<br>
For breadcrumbs that resemble something like `Start > Blog > My Awesome Post` you'd apply these settings:

```yaml
breadcrumb_home_label : "Start"
breadcrumb_separator  : ">"
```

## Custom Sidebar Navigation Menu

사이드바는 [**sidebars** documentation]({{ "/docs/layouts/#custom-sidebar-navigation-menu" | relative_url }}) 문서에서 확인할 수 있다. 