# Weehan tech blog

## ruby 개발 환경 필요

각 운영체제에 맞게 루비 개발 환경이 설치 되어 있어야 한다.

윈도우는 루비를 아래 링크에서 설치한다.

[Ruby+Devkit 2.6.5-1 (x64)](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.6.5-1/rubyinstaller-devkit-2.6.5-1-x64.exe)

## 로컬 환경 구축하기

CSS를 수정할 일이 있다든지, Draft를 작성하려고 한다든지 등은 로컬 환경에서 먼저 구현하고 테스트한다.

```
mkdir weehan-blog
cd weehan-blog
git clone https://github.com/weehan-dev/weehan-dev.github.io.git .
bundle
bundle exec jekyll serve --drafts # localhost:4000에서 확인
```

`_drafts` 폴더 아래에 아래 파일을 만들어서 마크다운 문서를 작성한 다음(이때는 날짜 정보가 포함될 필요 없다.), 작성이 완료 되면 `_posts`로 날짜 정보를 포함해서 아래와 같은 형식으로 포스팅을 한다.
```
* _drafts에서
draft.md

* _posts에서
yyyy-mm-dd-title.md
```

윈도우에서 draft는 한글 이름으로 작성하면 열어볼 수가 없다. 항상 draft 수가 많지는 않을테니, 영어로 작성해서 확인하고 나중에 업로드 할 때 파일명을 위 양식대로 작성해서 올리면 된다. (올릴 때는 한글이어도 괜찮다.)

## Author와 staff에 본인 추가하기
아래와 같은 형식으로 추가한다. 특히 `Github ID` 값과 `Github display name` 값 주의

```
// _data/authors.yml

<본인 Github ID>:
  name        : "<본인 Github display name>"
  position    : "<본인 위한 기수, 역할>"
  bio         : "<본인 bio>"
  avatar      : "<본인 github 이미지 주소>"
  links:
    - label: "<본인 이메일 주소>"
      icon: "fas fa-fw fa-envelope-square"
      url: "mailto:<본인 이메일 주소>"
    - label: "@<본인 Github display name>"
      icon: "fab fa-fw fa-github"
      url: "<본인 github 링크>"
```

```
// _staff/<본인 Github ID>.md

---
author: <본인 Github ID>
title: <본인 Github ID>
---
```

Staff에 잘 나타나는지 확인.

글을 작성할 때는 아래 설정을 포스팅 하는 markdown 파일 맨 첫 번째에 추가한다.

```
---
author: <본인 Github ID>

categories:
    - <포스트가 해당되는 기존 카테고리>

tags:
    - <포스트가 해당되는 기존 or 새로운 태그>
    - <포스트가 해당되는 기존 or 새로운 태그>
    ...
---
```

## 태그와 카테고리
태그는 포스트의 관련된 주제를 모두 포함한다. 태그를 넣는 것은 되도록이면 원래 사용되었던 것을 쓰면 좋지만, 기본적으로 쓰는 사람이 임의로 설정할 수 있다.

카테고리는 포스트가 주로 포함된 기술과 관련지어 넣는다. 카테고리는 기존에 생성되었던 카테고리 안에 넣어야 하고 만약 현재 적절한 카테고리가 없다면, 앞으로 계속 관련 카테고리 포스트가 생길 것 같은 경우에만 개발팀에게 알린 후 추가 하도록 한다.


## 참고 링크
- [minimal-mistake 문서](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
- [jekyil 문서](https://jekyllrb-ko.github.io/docs/home/)
- [블로그 minimal-mistakes 정리 글](https://devinlife.com/howto/)
- [블로그 jekyll 커스텀 정리 글](http://jihyeleee.com/blog/third-designer-can-make-jekyll-blog/)
- [마크 다운 정리 글](https://weehan-dev.github.io/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC/)