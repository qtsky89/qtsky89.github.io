---
title: Build tech blog with Jekyll Chirpy template
date: 2024-09-07 17:00:00 +0900
categories: [blog]
tags: [blog, jekyll, chirpy, static site generator]
author: wonhee
toc: true
comments: true
---

## Summary

해당 글에서는 기술 블로그를 구축 하기 위해 필자가 겼었던 다양한 옵션들과 장단점을 소개하고,

왜 최종적으로 Jekyll + Chirpy template 조합을 선택했는지 설명 한다.

## My Criteria

기술 블로그를 위해 고르려고 했던 플랫폼 및 템플릿의 기준은 아래와 같았다.

1. Markdown 이 native 로 지원 되어야 한다.
2. 디자인이 이뻐야 한다.
3. Table Of Content 가 오른쪽에 표시 되고, 스크롤시에 색깔 등으로 표시가 되면 좋겠다.
4. 댓글 기능이 native 로 지원되면 좋겠다.
5. 한번만 지불하는 것이라면, 유료도 상관 없다.
6. 지원되는 blog 용 custom template 이 많아야 한다.
7. 지속적으로 관리가 되고 있는 template 이어야 한다.
8. Migration 이 용이 해야 한다.
9. 글에 tag 항목이 있고, 각 tag 를 눌렀을때 해당 항목으로 글들이 소팅되서 보여야 한다.

## Tech Blog Platform Options

아래는 techblog 를 구축하기 위해 시도해 보았던, 다양한 플랫폼들에 대한 경험과, 

내 기준에 비춰 보았을때의 장단점을 정리해 보았다.

### WordPress

도메인을 제공해 주는 모델 (https://wordpress.com ) 이 있고, 직접 서버에 설치해서 구축할 수도 있다. 

사실 AWS 에서 운영하고 싶은 도메인도 이미 사두기도 했고, ( wonhee.net )

AWS lightsail 으로 이미 최적화 환경세팅이 끝난 WordPress 를 손쉽게 띄울 수도 있었다.

지원되는 플러그인도 상당히 많고, 쓰기 편하지만 문서 파일이 DB 에 저장되어 Migration 이 불편한 부분이 있고,

단순하지만 강력한 Markdown 기반의 static site generator 가 기술 블로그와는 더 맞겠다는 생각이 들어서

customize 도중에 접었다.

![alt text](/assets/img/image2.png)

#### Pros
- 도메인 제공 모델로 구축할 수 있음.
- 서버 운영에 지식이 있다면, AWS, Azure, GCP 등의 cluster provider 를 사용해서 운영할 수 있음.

#### Cons
- Markdown 기반이 아님.
- 기술 블로그로 쓰기에는 무거움.
- 문서가 DB 에 저장되어, Migration 에 불편함.

#### Ref
- [https://wordpress.org](https://wordpress.org)
- [https://wordpress.com](https://wordpress.com)
- [https://lightsail.aws.amazon.com](https://lightsail.aws.amazon.com)

### VitePress

회사에서 문서 생성기로 잘 사용하고 있는 솔루션 이다.

vue 에서 VuePress 를 제치고, 모든 공식 문서에 사용되는 솔루션 이라 믿을수 있을 뿐만 아니라

디자인이 깔끔하고, 계속 개발이 되고 있어 좋아 보였다.

다만, 문서 사이트에 특화 되어 있다 보니, 블로그 용으로 쓰기에는, 나름의 커스터마이즈가 필요해 보였다.

블로그용으로 커스터마이징을 한 몇몇 솔루션이 오픈소스로 공개 되어 있었지만, 

디자인등 내 기대를 충족하기에는 부족해 보였다.

![alt text](/assets/img/image3.png)

![alt text](/assets/img/image6.png)

#### Pros
- 현재도 꾸준히 개발중임.
- 디자인이 깔끔하게 잘나옴.
- Vue 를 안다면 커스터마이즈 하기 편함.

#### Cons
- 문서 사이트 제작에 최적회 되어 있어, 블로그용으로는 적합하지 않음.

#### Ref
- [https://vitepress.dev](https://vitepress.dev)

### Hugo

hugo 는 회사에서 문서 생성기로 잘 사용하고 있었다.

빌드 속도도 빠르고, 문서 반영도 바로바로 된다.

blog 용 template 도 다수 있지만, 

jekyll 과 비교 했을때 선택할만한 마땅한 template 이 존재 하지 않았다.

![alt text](/assets/img/image4.png)

#### Pros
- 빌드 속도가 빠름.
- 문서 반영이 바로바로 되서, 작업하기 편함.

#### Cons
- template 의 개수가 jekyll 과 비교했을때 현저히 적음.

#### Ref
- [https://gohugo.io](https://gohugo.io)
- [https://github.com/QIN2DIM/awesome-hugo-themes](https://github.com/QIN2DIM/awesome-hugo-themes)

### Jekyll (+ chirpy theme)

github pages 에서 공식적으로 지원을 해주는 플랫폼이라 그런지,

[https://github.com/topics/jekyll-theme](https://github.com/topics/jekyll-theme) 페이지에서, 쓸만한 template 들이 다수 보였다.

예전에 jekyll 을 비롯한 플랫폼을 github pages 에 배포하려면 복잡한 환경설정이 많았는데,

요즘은 github action 으로 손쉽게 배포할 수 있도록 제공을 해주고 있어, 환경 세팅 부담이 적어서 좋았다.

![alt text](/assets/img/image7.png)

여러 템플릿 중에 위에 언급되어 있는 [My Criteria](http://localhost:8282/posts/JekyllChirpyTemplate/#my-criteria) 을 전부 만족 시키는 

[https://github.com/cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)

템플릿으로 최종 선택을 했다.

디자인도 마음에 들고, 빠르고 마음에 아주 쏙 든다.

![alt text](/assets/img/image5.png)

#### Pros
- jekyll 을 배포해주는 github action 을 github 에서 example 로 제공해줌 가져다 쓰기만 하면 됨.
- [My Criteria](http://localhost:8282/posts/JekyllChirpyTemplate/#my-criteria) 에 언급한 기준을 모두 충족함.
- 블로그 관련 template 이 다양함.
- 디자인이 이쁨.

#### Cons
- ruby 기반 플랫폼이라, customize 하기 위해서는 ruby 기반 생태계를 어느정도 알아야함.

#### Ref
- [https://jekyllrb.com](https://jekyllrb.com)
- [https://github.com/topics/jekyll-theme](https://github.com/topics/jekyll-theme)

## Conclusion

최종적으로 Jekyll + chirpy theme 을 선택 했지만,

최근에는 React 기반 플랫폼인 Gatsby 이 잘나가는 것 같았다.

우선은 현재 상태로 운영 해보다가, 추후 변화가 필요할때, Gatsby 쪽도 살펴 봐아 겠다.