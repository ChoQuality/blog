+++
title = "Hugo + Github"
description = "Github 페이지를 만들어 보자"
date = "2021-06-02"
aliases = ["hugo","github"]
author = "k-mong"
+++

github에 Hugo를 적용 하여 정적인 사이트 만든 후 기록차 아래 같이 남겨 둠.(Mac 기준)

1.  [Hugo 퀵 스타트 가이드](https://gohugo.io/getting-started/quick-start/)  에 맞춰 아래 내용만 진행.
    - Step 1: Install Hugo    
            \[brew install hugo]    
            \[hugo version] 시 문제 없다면 설치는 완료 된것  
            
    - Step 2: Create a New Site    
            \[hugo new site blog]   
            
    - Step 3: Add a Theme [Theme 종류](https://themes.gohugo.io/) 를 선택 후 아래 명령어 진행      
            \(현재 사이트 적용 테마)    
            \[cd blog]  
            \[git init]  
            \[cd themes]    
            \[git submodule add https://github.com/alexandrevicenzi/soho.git]   

2. 1번 내용의 Step 3 까지 진행이 되어 있다고 하면 아래와 같이 결과가 있을 것 이다.
    - blog 폴더 내 이동 후 [ls -al] 명령어를 통해 
      .git, .gitmodules 파일이 존재 하는지 확인(없다면 1번 사항이 제대로 안된 것이다. 다시 진행)

3. github 계정 접속 후 Repository를 아래와 같이 3개 생성한다.   
     {{< img "images/posts/gitRepo.png" >}}
    - blog Repository = brew 로 생성된 폴더 전체를 저장할 곳
    - ChoQuality.github.io Repository = 웹사이트 표출 내용 저장할 곳
    - blog-comment Repository = 각 블로그 내용별 커멘트 저장할 곳
    
4. 생성된 Repository 들을 blog와  git 명령어를 통해 연결 
    (아래 명령어 중간은 각자의 github 주소로 변경)
    - [git remote add origin https://github.com/ChoQuality/blog.git]
    - [git submodule add -b master https://github.com/ChoQuality/ChoQuality.github.io.git public]

5. 새로운 글을 등록 후 git push 해주자 
    - hugo new posts/test.md
    - hugo -D
    - cd public
    - git add .
    - git commit -m "msg"
    - git push origin master
    - cd ..
    - git add .
    - git commit -m "msg"
    - git push origin master
    - ==========================
        * 위 내용을 귀찮게 다 쓰는게 싫어서 아래 같이 해버림. [다른 사람 블로그 내용을 넣었다.](https://ryan-han.com/post/etc/creating_static_blog/)
        * [touch deploy.sh] 
      {{< gister choquality ffcb5d114671962d15f334f03fc57639 0 5 >}}

6. blog 내 comment 를 등록 [utterances](https://utteranc.es/) 기준
    - [utterances apps 등록](https://github.com/apps/utterances) 을 한다.
      등록을 하고 계정의 blog-comment의 setting 내 Integrations 를 보면 utterances가 보인다.
      {{< img "images/utterances.png" >}}      
      옆의 Configure을 눌러 repository 선택 후  blog-comment 를 등록 
    - [utterances](https://utteranc.es/) 내 configuration 내 repo를 알맞게 적어준 주고,     
         원하는 Blog Post <-> Issue Mapping을 선택
      {{< img "images/comment.png" >}}
    - Enable Utterances 아래 copy를 눌러 해당 내용을  
         layouts/partials/share.html 아래 넣어 준다.
         [soho theme 기준이다. 다른 테마 이면 알아서 알맞은 곳에 넣자]
      {{< gister choquality 48f076e391d269a752b85cf87e393102 0 8 >}}
    - 잘 표출 되는지 확인 하자.
         만약 안되는 사람의 경우  utterances.json 을 만들어 blog-comment 에 push
      {{< gister choquality a6268d1e87371ddc60554f4b67856dc9 0 100 >}}

7. Gist 등록해 보기?
    - 사실 7번은 안해도 되는 내용인데 쓰고 싶어 하는 사람이 있는거 같아서 넣어본다.
    - [Gist](https://gist.github.com/) 에 들어가서 원하는 내용 넣고 아래 Create 를 한다.
        * create 된 내용을 눌러보면 오른쪽에 embed 버튼이 있고 해당 내용을 넣으면 된다.
    {{< img "images/gist.png" >}}
    <script src="https://gist.github.com/ChoQuality/48f076e391d269a752b85cf87e393102.js"></script>
    - 만약 gist 내용 일부 표출 되는 걸 원하는 사람이 있다면, 별도의 shortcodes 내 html를 작성해야 한다.
      * layouts 폴더 아래 shortcodes 생성 후 gister.html 란 파일을 생성 [이름은 알아서]
        gister.html 내용은 아래 같이 작성 했다.
       * {{< span "[]" >}}
        우선 동작은 원하는 gist 내용을 html에 넣고 그후 아래 gistParser.js를 불러 해당 gist 내용의 일부를 잘라 표현 하게 했다.    
        [js 동작은 제대로 검증을 안함, 적당히 돌아가는듯]
       {{< gister choquality 21ab82646e4786dda4c9c9752b576002 0 2 >}}
       사용법은 다음과 같다.      
       gister[생성한 html 명] choquality[gist 주소] 21ab82646e4786dda4c9c9752b576002[gist hash] 0[min] 2[max] 

"<div> id="{{.Get 1}}" data-parse="{{.Get 2}}"><script src="https://gist.github.com/{{.Get 0}}/{{.Get 1}}.js"></script><script src="https://choquality.github.io/js/gistParser.js?id={{.Get 1}}&min={{.Get 2}}&max={{.Get 3}}"></script></div>"

