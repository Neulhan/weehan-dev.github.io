# Weehan tech blog

## ruby 개발 환경 필요

각 운영체제에 맞게 루비 개발 환경이 설치 되어 있어야 한다.

## Author에 본인 추가하기
아래와 같은 형식으로 추가한다.
```
// _data/authors.yml

<본인 Github display name>:
  name        : "<본인 Github display name>"
  bio         : "<본인 개발 분야 / 학과 학번>"
  avatar      : "<본인 github 이미지 주소>"
  links:
    - label: "<본인 이메일 주소>"
      icon: "fas fa-fw fa-envelope-square"
      url: "<mailto:본인 이메일 주소>"
    - label: "<@본인 Github display name>"
      icon: "fab fa-fw fa-github"
      url: "<본인 github 링크>"
```
글을 작성할 때 아래 설정을 markdown 파일 맨 첫 번째에 추가한다.
```
---
author: <본인 Github display name>
---
```

## 로컬 환경 구축하기
```
mkdir blog
cd blog
git clone https://github.com/weehan-dev/weehan-dev.github.io.git .
bundle
bundle exec jekyll serve --drafts # localhost:4000에서 확인
```

`_drafts` 폴더 아래에 아래 파일을 만들어서 마크다운 문서를 작성한 다음(이때는 날짜 정보가 포함될 필요 없다.), 작성이 완료 되면 `_posts`로 날짜 정보를 포함해서 아래와 같은 형식으로 포스팅을 한다.
```
* _drafts에서
title.md

* _posts에서
yyyy-mm-dd-title.md
```

## 참고 링크
- [minimal-mistake 문서](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
- [jekyil 문서](https://jekyllrb-ko.github.io/docs/home/)
- [블로그 정리 글](https://devinlife.com/howto/)
- [마크 다운 정리 글](https://weehan-dev.github.io/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC/)