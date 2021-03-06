---
layout: post
title: "내가 Sass를 선택한 이유"
date: 2014-04-13 19:15:27 +0900
comments: true
categories: css Sass less stylus
---

css preprocessor라는 녀석들이 있다.  
css의 태생적 한계를 극복하기 위해 나온 것으로 대표적으론 [Sass](http://sass-lang.com/), [less](http://lesscss.org/), [stylus](http://learnboost.github.io/stylus/)가 있다.

css는 쉽고 간단하지만 갈수록 요구사항과 스펙이 복잡해지고 있고 그에따라 유지보수도 힘들어 지고 있다.  
Sass등은 변수, 함수, 확장/상속 등의 기능을 추가해 이를 돕는다. 좀 더 잘 알고 싶다면 [Sass의 guide페이지](http://sass-lang.com/guide)를 살펴보자. 다른 녀석들도 문법은 다르지만 큰 방향성은 비슷하다. 

그 중에서도 나는 Sass를 가장 선호하는데, 이번 포스팅에서는 그에 대해 풀어보려 한다.

<!-- more -->


다른 두 녀석을 먼저 간단하게 살펴보고 Sass에 대해 이야기 할 것이다.  
이 포스트는 상호간 문법적 차이나 사용법을 자세히 설명하지는 않는다. 그동안의 경험으로 알게된 특성을 간략히 정리해서 공유하고 의견을 나누는 것이 목적이다. 좀 더 친절하고 자세한 설명을 원한다면 포스트 최하단의 링크들을 참고해보자.


## less

* js로 구동된다 (처음엔 Sass처럼 ruby로 구현되었었다고 한다)
* 브라우저단에서도 동작한다 
    * 환경을 갖추기 쉽기 때문에 처음 preprocessor를 사용하는 사람들은 이 이유 하나로 less를 택한다.  하지만 조금만 알아보면 제대로 사용할 수 있는 수준이 아니란걸 알게된다. 그냥 소스파일 하나면 작동하겠지만..
* 제어문이 특이하다
    * 물론 이는 나의 주관적인 평가다. [Mixin Guard](http://lesscss.org/features/#mixin-guards-feature), [Loops](http://lesscss.org/features/#loops-feature)를 보고 알아서 판단하자
    * mixin(function)의 경우 class의 형식을 빌어 사용되고 있다. 나는 이것도 좀 혼란스러운 것 같은데..
* css3 작성을 돕는 [LESS Hat](http://lesshat.madebysource.com/) 라이브러리가 있다
* github repo 기준으로, 현재 제일 큰 커뮤니티를 갖고 있는 것으로 보인다


## stylus

* 3대 css preprocessor 중 막내
* js(node.js)로 구동된다 
* 문법이 굉장히 유연하다
    * css의 문법 자체를 비틀어 버렸다
    * 물론 기존에 css를 작성하던 방식 그대로도 가능하다
    * 너무 관대한 문법이 독이 될 수 있다. xhtml 이전 시절을 떠올려 보면..
    * 어느부분이 css고 어느부분이 stylus인지 구분이 모호하다. 초보자가 함부로 쓰기 위험할 수 있다
* css3 지원을 도와주는 [Nib](https://github.com/visionmedia/nib)이라는 라이브러리가 있다
* 기능이 막강하다
    * Sass의 그것과 비슷하거나 더 낫다
* 반면 성숙도는 제일 낮다
    * 가끔 당황스러운 버그들을 만나게 된다다
    * sourcemap이나 ide의 지원등을 보면 sass, less가 적용되고 그 후에나 stylus 지원을 기대할 수 있


## 그렇다면, Sass는?

* 역사가 제일 오래되었다
    * 그만큼 성숙도가 높고, 커뮤니티도 크다
* sass/scss라는 두가지 문법이 있다
    * sass는 stylus처럼 css의 문법 자체를 바꾼다
    * scss는 권장되는 방식으로, css나 less의 문법과 닮아있다
* 기능이 막강하다
    * stylus의 그것과 다른점은, Sass 문법은 css property와 시각적으로 좀 더 명확히 구분된다는 점이다
    * [placeholder selector](http://sass-lang.com/documentation/file.Sass_REFERENCE.html#placeholder_selectors_)
    * [parent selector](http://sass-lang.com/documentation/file.Sass_REFERENCE.html#parent-selector)의 강력함
* ecosystem이 제일 발달되어 있다
    * [Compass](http://compass-style.org/)라는 강력한 라이브러리가 css3 지원, 이미지 스프라이트 자동화 등 막강한 기능을 더해준다
    * 그외에도 그리드 시스템, 미디어쿼리 제어, 스타일링용 라이브러리 등이 다양하게 준비되어 있다
* 문제는, ruby 환경으로 구동된다
    * 그래서 느리다(고는 하는데 체감해보진 못했다)
    * 다행인것은 Sass가 C로 포팅([Libsass](http://libsass.org/))이 되었고 그 덕분에 이제 sass는 환경의 제약에서 자유로워졌다는 것이다

내가 생각하는 Sass의 유일한 단점은 ruby로 만들어졌다는 것이다. 애초에 ruby 개발환경에 Sass를 추가하는것은 간단하지만 java, python등 다른 개발환경에 오로지 Sass만을 위해서 ruby를 추가하는 것은 꽤나 큰 부담으로 다가온다. 

물론 별도로 ruby를 설치하지 않고도 Sass+Compass를 사용 가능하게 해주는 [Prepros](http://alphapixels.com/prepros) 같은 훌륭한 앱도 존재하지만, 제대로 사용하고 개발환경에 녹여내려면 cli로 Sass를 다루어야 하는데 환경구축이나 학습비용을 무시할 수는 없다.

다행스럽게도 Sass가 c로 포팅되어 ruby 없이도 Sass를 사용할 수 있게 되었지만 다음과 같은 문제가 있다.

### Libsass를 사용할 때의 문제

#### 최신 변경사항 반영에 시간이 걸린다

기본적으로 ruby기반의 Sass가 개발된 후에 Libsass로 포팅이 되는 식이라 최신의 변경사항이 바로 반영되지 못하는 문제가 있다. 
오늘(2014/04/13) 기준으로 보면, Sass는 3.3 버전이 되었고 부모선택자의 확장된 사용 등 유용한 기능들을 포함하고 있는데 Libsass의 경우는 3.2의 기능들도 아직 100% 완벽하게 구현되지 않았다. 

#### Compass를 사용할 수 없다

Sass는 C로 포팅이 되어 다양한 환경에서 사용할 수 있게 되었지만 Compass는 그렇지 못하다. Compass의 유용한 기능들은 그를 대체할 수 있는 방법을 찾아야 한다.

내가 찾아본 대안으로는 [grunt](http://gruntjs.com) 환경을 구성하고 css3 지원에는 [Autoprefix](https://github.com/nDmitry/grunt-autoprefixer)를, 이미지 스프라이트 자동화에는 [Sprite Smith](https://github.com/Ensighten/grunt-spritesmith)를 사용하면 될 것 같다. 


## 정리

나는 Sass를 선택해서 2년이 넘도록 개인프로젝트와 회사 업무 모두에 사용하고 있다.

'내가 Sass를 선택한 이유'는 결국 다음과 같이 간단하다.  
**less**의 경우 문법과 빈약한 기능이 맘에 안들었고, **stylus**는 혼자서 빠르게 작업할 때는 잘 사용하지만 협업시에는 문법과 ide 지원때문에 별로 호응을 얻지 못했다. 너무 문법이 관대한 점이 초보자가 사용하기엔 올바른 css 작성습관을 헤칠 수 있다고 판단되어 협업시에도 권장하지 않는다. **Sass**는 ruby 환경셋팅을 처음 할땐 괴로웠지만 친숙한 문법과 강력한 기능, 그리고 Compass의 지원으로 사용하지 않을 이유가 없었다. 앞으로도 계속 사용하고 주변인에게 꾸준히 전파할 것이다.

Libsass와 그의 Node.js 바인딩인 [node-sass](https://github.com/andrew/node-sass)가 있기 때문에, 앞으로 Ruby를 사용하지 않는 환경에서는 다음과 같이 구성하려고 한다.

* ruby Sass -> **node-sass**
* Compass CSS3 helper -> **Autoprefix** (oldIE 대응은 별도 mixin 제작)
* Compass Image-sprite helper -> **Sprite Smith**
* 그리고 이들을 엮어줄 **Grunt**

이렇게 하면 이제 Node.js만 갖추면 작업자 모두 빠르고 손쉽게 개발환경을 구축할 수 있을것 같다. 
Libsass가 ruby Sass의 최신사항 반영에 시간이 좀 걸린다고 해도 이는 결국 시간이 해결해 줄테고.. 뭐 활발히 업데이트 되고 있으니 기대해봐도 좋을 것 같다.

아직도 어떤녀석을 선택할지 망설여지는가? 이 글보다 좀 더 친절하고 자세한 <s>영어로 된</s> 자료들이 많이 있으니 천천히 살펴보고 자신의 상황에 알맞는 녀석을 골라보자. 

* [CSS Preprocessors. Comparing SASS, LESS and Stylus](http://www.slideshare.net/patricka1/css-preprocessors-sass-less-and-stylus)
* [Sass vs. LESS vs. Stylus: Preprocessor Shootout](http://code.tutsplus.com/tutorials/sass-vs-less-vs-stylus-a-preprocessor-shootout--net-24320)
* [LESS VS SASS VS STYLUS](http://www.scottlogic.com/blog/2013/03/08/less-vs-sass-vs-stylus.html)

----
다음번엔 프론트엔드 최적화에 이제는 필수가 되어버린 Grunt와 위에 설명한 Sprite smith를 이용해 이미지 스프라이트 자동화를 하는 방법에 대해 포스팅 할 것이다 :)