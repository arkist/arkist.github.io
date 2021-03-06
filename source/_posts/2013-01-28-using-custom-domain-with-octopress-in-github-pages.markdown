---
layout: post
title: "Github Pages+Octopress에서 커스텀 도메인 사용시 주의사항"
date: 2013-01-28 11:57
comments: true
categories: octopress
---

근래 2~3일간 블로그 접속이 잘 안되었을 것이다.
옥토프레스를 좀 건드려보다가 도메인 셋팅파일이 자꾸 날아가는 바람에 그렇게 되었다.
나와 같은 환경에서 커스텀 도메인을 사용시 문제점과 필요한 셋팅에 대해 남겨본다.

<!--more-->

----

__[2013-07-11]__ 이 문제는 정식으로 보고되어 해결된 것으로 보인다. [정식사이트의 Github Pages를 통한 배포](http://octopress.org/docs/deploying/github/)에서 __Custom Domains__부분을 참고하자.

----

> _Github Pages에 커스텀 도메인을 연결하는 법은 이 포스트에서 다루지 않는다. 관련해서는 [GitHub의 페이지 기능 이용하기 - dogfeet](http://dogfeet.github.com/articles/2012/github-pages.html)를 참고하자._

여기서는 Github Pages+Octopress 환경에서 커스텀 도메인을 사용하는 법에 대해서만 간단히 다룬다.

----

현재 이 블로그는 [Octopress](//octopress.org)를 이용해 정적 페이지를 생성하고, [Github Pages에 배포](//octopress.org/docs/deploying/github/)하는 방식으로 운영하고 있다. Github Pages는 `arkist.github.com` 같은 도메인 대신에 `windtale.net` 같은 [자신의 고유 도메인을 사용할 수 있는 기능](https://help.github.com/articles/setting-up-a-custom-domain-with-pages)을 제공한다. 도메인쪽에는 DNS를 설정해주고 실제 운영되는 계정에는 해당 도메인이 적힌 CNAME 파일만 하나 있으면 된다.

``` bash CNAME - windtale.net https://github.com/arkist/arkist.github.com/blob/master/CNAME
windtale.net
```

그런데 옥토프레스를 사용하면서 문제가 되는게.. 포스트를 작성하거나 뭔가 수정을 하면서 `rake generate`, `rake preview`, `rake watch` 명령을 사용하게 되는데, 이때 __public 폴더의 내용을 통째로 갱신하면서 CNAME 파일을 삭제__해버리는 것이다. 그래서 옥토프레스의 Rakefile에 다음과 같이 추가해 주었다.

``` ruby Rakefile
task :deploy do
  # Check if preview posts exist, which should not be published
  if File.exists?(".preview-mode")
    puts "## Found posts in preview mode, regenerating files ..."
    File.delete(".preview-mode")
    Rake::Task[:generate].execute
  end

  puts "## Generating CNAME, README"
  system "echo windtale.net > #{public_dir}/CNAME"
  system "echo Windtale.net Blog! > #{public_dir}/README"
  puts "## Done!"

  Rake::Task[:copydot].invoke(source_dir, public_dir)
  Rake::Task["#{deploy_default}"].execute
end
```

위 소스상에서 9-12번째 라인이 추가한 부분다.
deploy 명령을 실행하면 `deploy`를 구성하기전에 먼저 `public` 디렉토리에 `CNAME` 파일을 생성해 준다.
그동안 파일생성 시점을 착각하고 generate쪽에 추가해뒀더니, preview시에 CNAME 파일이 삭제되는데도 모르고 지나쳤었다.
<del>요번에 블로그 접속이 안되던게 그것 때문이었다-_-)</del>

