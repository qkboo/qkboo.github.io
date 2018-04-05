## Theme - minimal mistake

### Full text search

#### Lunr

#### Algoria

https://github.com/algolia/jekyll-algolia

https://community.algolia.com/jekyll-algolia/getting-started.html

`_config.xml` 에서 

[algoia](https://www.algolia.com) 는 



### Collection 다루기

[Working with Collections](https://mmistakes.github.io/minimal-mistakes/docs/collections/) 를 참고.

**1**: `_config.yml` 의 **collections:** 섹션에 컬렉션을 넣는다.

```yaml
collections:
  portfolio:
    output: true
    permalink: /:collection/:path/
```

콜렉션 폴더는 Jekyll 루트 폴더에  `_` 가 붙은 `_portfolio` 폴더가 된다. 그리고  `_config.yml` 의 defaults 섹션에 레이아웃 관련 속성을 넣을 수 있다.

```yaml
# Defaults
defaults:
  # _portfolio
  - scope:
      path: ""
      type: portfolio
    values:
      layout: single
      author_profile: false
      share: true
```

