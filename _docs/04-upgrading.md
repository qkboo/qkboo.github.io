---
title: "Upgrading"
permalink: /docs/upgrading/
excerpt: "테마 업그레이드를 위한 방법과 고려점. Instructions and suggestions for upgrading the theme."
last_modified_at: 2018-03-20T15:59:07-04:00
toc: true
---

Minimal Mistakes 테마를 [Ruby Gem]({{ "/docs/quick-start-guide/#ruby-gem-method" | relative_url }}) 혹은 [remote theme]({{ "/docs/quick-start-guide/#github-pages-method" | relative_url }}) 버전을 사용중이면 어렵지 않게 업그레이드 할 수 있다. 

If you're using the [Ruby Gem]({{ "/docs/quick-start-guide/#ruby-gem-method" | relative_url }}) or [remote theme]({{ "/docs/quick-start-guide/#github-pages-method" | relative_url }}) versions of Minimal Mistakes, upgrading is fairly painless.

현재 버전을 확인해 보려면 사이트 소스 모든 `.html` 파일,  `_site/assets/css/main.css` 혹은 `_site/assets/js/main.min.js` 를 보며 아래 내용을 확인할 수 있다:

```
<!--
  Minimal Mistakes Jekyll Theme 4.9.0 by Michael Rose
  Copyright 2013-2018 Michael Rose - mademistakes.com | @mmistakes
  Free for personal and commercial use under the MIT license
  https://github.com/mmistakes/minimal-mistakes/blob/master/LICENSE.txt
-->
```


## Ruby Gem

`Gemfile` 이 있고 Bundler를 사용하면 `bundle update` 를 실행하거나, 아니면 `gem update minimal-mistakes-jekyll`을 실행한다. 
<br>Simply run `bundle update` if you're using Bundler (have a `Gemfile`) or `gem update minimal-mistakes-jekyll` if you're not.

Bundler를 사용하면 다운그레이드 혹은 지정한 출시 ([tag](https://github.com/mmistakes/minimal-mistakes/tags)), branch, commit 로 테마를 고정할 수 있다. `Gemfile`에 `gem "minimal-mistakes-jekyll"` 대신 아래 구문으로 대체한다.<br>
When using Bundler you can downgrade or lock the theme to a specific release ([tag](https://github.com/mmistakes/minimal-mistakes/tags)), branch, or commit. Instead of `gem "minimal-mistakes-jekyll"` you'd add the following to your `Gemfile`:

```ruby
gem "minimal-mistakes-jekyll", :git => "https://github.com/mmistakes/minimal-mistakes.git", :tag => "4.9.0"
```

For more information on [installing gems from git repositories](http://bundler.io/v1.16/guides/git.html) consult Bundler's documentation.

## Remote Theme

원격테마를 사용하기 위해 `_config.yml`에 `remote_theme: "mmistakes/minimal-mistakes"` 를 지정할 때도 역시 ([tag](https://github.com/mmistakes/minimal-mistakes/tags)), branch, commit 에 대해 `@` 를 붙여 가능하다.<br>
When setting `remote_theme: "mmistakes/minimal-mistakes"` in your `_config.yml` you may also optionally specify a branch, [tag](https://github.com/mmistakes/minimal-mistakes/tags), or commit to use by appending an @ and the Git ref.

예를 들어 이전 배포본 4.8.1로 돌리려면 `mmistakes/minimal-mistakes@4.8.1` 혹은 `mmistakes/minimal-mistakes@bbf3cbc5fd64a3e1885f3f99eb90ba92af84063d` 같이 넣어준다.<br>
For example you can roll back to release 4.8.1 with `mmistakes/minimal-mistakes@4.8.1` or a specific commit with `mmistakes/minimal-mistakes@bbf3cbc5fd64a3e1885f3f99eb90ba92af84063d`).

깃헙 페이지에 테마를 업데이트하려면 푸시 후에 강제로 빌드를 해야 한다. 아래 git 명령을 참조하라.<br>
To update the theme on GitHub Pages you'll need to push up a commit to force a rebuild. An empty commit works well if you don't have anything to push at the moment:

```terminal
git commit --allow-empty -m "Force rebuild of site"
```

## Use Git

If you want to get the most out of the Jekyll + GitHub Pages workflow, then you'll need to utilize Git. To pull down theme updates you must first ensure there's an upstream remote. If you forked the theme's repo then you're likely good to go.

To double check, run `git remote -v` and verify that you can fetch from `origin https://github.com/{{ site.repository }}.git`.

To add it you can do the following:

```terminal
git remote add upstream https://github.com/{{ site.repository }}.git
```

### Pull Down Updates

Now you can pull any commits made to theme's `master` branch with:

```terminal
git pull upstream master
```

Depending on the amount of customizations you've made after forking, there's likely to be merge conflicts. Work through any conflicting files Git flags, staging the changes you wish to keep, and then commit them.

## Update Files Manually

Another way of dealing with updates is [downloading the theme](https://github.com/{{ site.repository }}/archive/master.zip) --- replacing your layouts, includes, and assets with the newer ones manually. To be sure that you don't miss any changes it's probably a good idea to review the theme's [commit history](https://github.com/{{ site.repository }}/commits/master) to see what's changed since.

Here's a quick checklist of the important folders/files you'll want to be mindful of:

| Name                   |     |
| ----                   | --- |
| `_layouts`             | Replace all. Apply edits if you customized any layouts. |
| `_includes`            | Replace all. Apply edits if you customized any includes. |
| `assets`               | Replace all. Apply edits if you customized stylesheets or scripts. |
| `_sass`                | Replace all. Apply edits if you customized Sass partials. |
| `_data/navigation.yml` | Safe to keep. Verify that there were no major structural changes or additions. |
| `_data/ui-text.yml`    | Safe to keep. Verify that there were no major structural changes or additions. |
| `_config.yml`          | Safe to keep. Verify that there were no major structural changes or additions. |

---

**Note:** If you're not seeing the latest version, be sure to flush browser and CDN caches. Depending on your hosting environment older versions of `/assets/css/main.css`, `/assets/js/main.min.js`, or `*.html` may be cached.
{: .notice--info}
