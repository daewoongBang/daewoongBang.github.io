---
title: Gatsby로 Blog 만들기
date: '2020-01-15 20:40:00'
template: 'post'
draft: false
slug: 'gatsby-blog'
category: 'Blog'
tags:
  - 'Gatsby'
  - 'Blog'
description: 'Gatsby와 Github Page를 활용한 Blog 포스팅'
---

### Gatsby로 블로그 포스팅 시작하기.

<br>

빠른 시작을 위해 Starter Library를 사용하였다.  
많은 고민 끝에 고른 테마는 gatsby-starter-lumen

**Gatsby CLI 설치**

```
npm install -g gatsby-cli
혹은
yarn global add gatsby-cli
```

**Starter Library를 이용하여 site 생성**

```
gatsby new [폴더명] [라이브러리 링크 url]
gatsby new gatsby-starter-lumen https://github.com/alxshelepenok/gatsby-starter-lumen
```

**로컬에서 구동**

```
cd gatsby-starter-lumen
gatsby develop
```

Gatsby 에 대한 정보는 아래 링크에서 자세히 볼 수 있다.  
<https://www.gatsbyjs.org/docs/>

master 브랜치는 build 결과물을 올리고
development 브랜치에는 개발 중인 전체 코드를 올린다.  
development 브랜치와 master 브랜치

package.json  
gh-pages -d public -b master

yarn run deploy

disqus 기능 활성화
