---
layout: post
title: "Octopress - 글목록에서 Disqus 댓글 카운트가 항상 '0'일 때"
date: 2013-01-29 15:36
comments: true
categories: octopress
---

요며칠 옥토프레스 자잘한 문제들을 수정하고 있다.  
이번엔 blog, archives 메뉴에서 댓글이 '0'으로 나오는 문제를 해결.

<!--more-->

## Disqus

옥토프레스는 정적페이지 기반이기 때문에, 댓글은 외부 시스템을 끌어다 사용한다. 흔히 사용하는게 [Disqus](//disqus.com). 이 블로그에도 달려있는 녀석으로 소셜 댓글 시스템이라고 해야하나.. 자세한 사항은 홈페이지를 참고해보자. <del>비슷한 국내서비스로는 [LiveRe](http://livere.com)라는 녀석이 있는데 얜 내 취향이 아니다.</del>

여튼, 이 녀석이 달려있는데 글 하단에 달린 녀석은 잘 작동하는데 글목록 등 메뉴에서 댓글 카운트를 가져오지 못하고 항상 '0'으로 표시를 하는 것이다. 아마 Disqus가 2012년판으로 업그레이드 되면서 내부동작방식이 뭔가 수정이 된 모양이었다. 해결법을 열심히 뒤지기 시작.. 옥토프레스의 깃헙 이슈페이지에서 [2.1버전에서 해당 문제가 해결 되었다는 글](https://github.com/imathis/octopress/pull/277)을 발견했다. 해결이 안된것 같은데..? [해당 이슈가 어떤식으로 소스에 반영되었는지](https://github.com/imathis/octopress/pull/277/files) 코드를 살펴 보았다.  
아하, `source/_includes/article.html` 파일이 문제였구먼?

## 써드파티 테마 이슈

만약 옥토프레스의 기본 테마가 아닌 다른 개발자의 써드파티 테마를 사용하고 있다면 동일한 문제를 겪을 수 있다. `source/_includes` 디렉토리는 개별 테마에서 다루기 때문에, 메인소스와는 별개로 추가/수정된 내용을 반영해주어야 한다. 옥토프레스의 업데이트 절차에서 자동으로 해당 내용을 반영해줄 수 있을지 모르지만, `.themes` 디렉토리에서 관리되는 써드파티 테마의 경우 해당 수정사항을 반영해주지 못하기 때문에 테마를 재적용하거나 변경시 추가/수정된 내용이 누락될 수 있다. css를 수정하는 수준이 아닌 html 구조 자체를 변경하기 위해서는 조금이라도 로직이 섞인 템플릿 파일을 다루게 되고, 그렇게 원 저작자의 손을 떠난 소스는 어떻게 될지 알 수가 없다. 역시 유지보수는 어렵다.

## 그래서 결론

동일한 문제가 발생한 분들은,  
`source/_includes/article.html`, `source/_includes/archive_post.html` 파일의 `span.comments>a`에 `data-disqus-identifier` 속성이 누락되진 않았는지 확인해보시면 됩니다. 만약 해당 속성이 누락되었다면 [해결법](https://github.com/imathis/octopress/pull/277/files)을 참고해서 수정하세요.  
끝.

