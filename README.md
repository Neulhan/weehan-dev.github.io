# Weehan tech blog

## ruby 개발 환경 필요

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
bundle exec jekyll serve # localhost:4000에서 확인
```

_drafts 폴더 아래에 아래 형식의 파일 이름으로 파일을 만들고 Markdown 문법에 맞춰서 글을 작성하면 된다.
```
yyyy-mm-dd-title.md
```

