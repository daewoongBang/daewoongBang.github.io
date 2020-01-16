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

### Gatsby와 Github Page로 블로그 포스팅 시작하기.

<br>

어떤 곳에서 블로그를 시작할까 고민하던 중 정적 사이트 생성기(Static Site Generator)라는 것을 알게되었는데, 그 중 많이 알려진 것이 Jekyll과 Gatsby였다.  
Jekyll은 Ruby 기반이고 Gatsby는 React 기반인데 개인적으로는 React가 익숙하여 Gatsby로 블로그를 시작하기로 결정했다.  
빠른 시작을 위해 **Starter Library**를 사용하였고 많은 고민 끝에 고른 테마는 **gatsby-starter-lumen**.  
사용법과 같은 자세한 정보는 <https://www.gatsbyjs.org/> 에서 볼 수 있다. (Docs와 Tutorial, 테마 등)

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
gatsby develop 혹은 yarn run develop
```

서버가 구동이 되면  
http://localhost:8000/ 해당 주소에서 페이지를 확인해볼 수 있다.

**Github Page와 연동**

- Github에서 username.github.io로 repository 생성.
- 개발 전체 코드는 `development branch`에 push한다.  
  (`master branch`에는 build 결과물이 올라간다.)

```
git init
git checkout -b development
git remote add origin https://github.com/daewoongBang/daewoongBang.github.io.git
git add .
git commit -m "init setting"
git push origin development
```

- package.json 에서 script 속성의 `deploy` value를 변경한다.  
  yarn run clean && gatsby build && `gh-pages -d public -b master`  
  (master branch에 build 결과물이 올라가도록 변경)

- deploy를 실행하여 배포한다.

```
yarn run deploy
```

정상적으로 배포가 되면 Github에서 생성한 repository 이름으로  
아래와 같은 url형태로 접속이 가능하다.  
<https://daewoongbang.github.io/>

- 정상적으로 페이지가 뜨는 것이 확인이 되면,  
  이제부턴 자기 것으로 커스터마이징하거나 기본 메타정보를 변경하면 된다.

기본적으로 `config.json`에서 author 정보, 사이트명, disqus 기능 활성화, 페이지당 포스트 수,  
google Analytics, side menu 등을 수정할 수 있다.  
`google-site-verification` 은 `Layout.js`에서 `<Helmet>` 태그 안에 `meta` 태그를 추가해주면 사이트 소유권 확인이 가능하다.

---
