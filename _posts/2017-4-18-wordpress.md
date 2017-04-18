---
layout: post
title: Review of wordpress book
---

# [만들면서 배우는 워드프레스](http://www.hanbit.co.kr/store/books/look.php?p_code=B5608198861)
* 한빛 미디어에서 리뷰를 제안해서 읽어보게 되었다. 워드프레스는 관심만 있었지 실제로 써보지는 못했는데, 좋은 기회가 될 거 같아 바로 하겠다고 말씀을 드려서 책을 받게 되었다.
* 책을 처음 받고 우선 대략 훑어보니, 개발자용 책은 아니고, 무료나 가능한 적은 비용으로 자신의 홈페이지를 운영할 사람들을 대상으로 한 책이었다. 호스팅부터 시작해서 설치, 사용 방법 및 필요한 플러그인에 대해 가능하면 자세하게 설명한 걸로 보였다. 나는 무료 호스팅 서비스를 사용할 계획이 없어서, 어떤 방식으로 설치를 해볼까 하다가 docker를 사용해 설치하면 간단하게 테스트할 수 있을 거 같아, 이 방법을 쓰기로 결정했다.
* 검색을 하면 다음과 같은 link를 쉽게 찾을 수 있고, 해당 설명대로 `docker-compose.yml`만 만들고, `docker compose up -d` 명령을 주면 아래와 같이 docker가 구동이 된다.
  * [Quickstart: Compose and WordPress](https://docs.docker.com/compose/wordpress/)

    ```
    $ docker ps -a
    CONTAINER ID  IMAGE             COMMAND                 CREATED            STATUS      PORTS                 NAMES
    c891a1843480  wordpress:latest  "docker-entrypoint..."  24 seconds ago Up  22 seconds  0.0.0.0:8000->80/tcp  wordpress_wordpress_1
    df4563e5da51  mysql:5.7         "docker-entrypoint..."  26 seconds ago Up  24 seconds  3306/tcp              wordpress_db_1
    ```
  * 그 뒤에 127.0.0.1:port#로 접속하면 이렇게 설치 화면을 만나서 시작을 할 수 있다.
    * ![docker compose install]({{ site.baseurl }}/images/0417_docker_compose_install.png)
* 그 뒤는 전체 혹은 필요한 부분을 읽으면서 따라하기만 하면 된다. 위에서 내가 설치한 버전은 4.7이고, 책에서 사용하는 버전은 4.6이라서 약간의 차이는 있지만, 설명을 따라가기에는 아무 문제가 없다. 예를 들어 위의 docker instance에서 따라했던 작업 두 가지의 스크린샷을 보면 다음과 같다.
  * 책 90page의 첫 번째 글 작성 예제
    * ![90p first post]({{ site.baseurl }}/images/0418_90p_first_post.png)
  * 책 257page부터 나오는 iThemes Security plugin 설치 및 사용 방법
    * ![257p plugin]({{ site.baseurl }}/images/0418_257p_plugin.png)
    * ![258p plugin]({{ site.baseurl }}/images/0418_258p_plugin.png)
    * ![266p plugin]({{ site.baseurl }}/images/0418_266p_plugin.png)
* 책의 구성 자체가 웹사이트 사용에 필요한 부분을 설명하고, 실제 사용 방법을 스크린샷과 함께 설명하기 때문에, 크게 어렵지 않을 거라는 생각이 든다. 다만 일부 내용이 저자의 의도와 달리 약간 혼란을 줄 수도 있다는 생각이 들지만(예를 들어, 웹사이트 기획을 위해 xmind를 설명하는 부분은, 어떤 사람들에게는 유용할 수도 있지만, mindmap이 뭔지 모르거나, 아니면 컴퓨터 사용에 능숙하지 않다면 또 하나의 복잡한 설명이 되지 않을까 약간 우려도 됨), 전체적으로 보면 난이도가 높은 부분은 거의 없어서 워드프레스를 사용하고 싶은 초보자에게 매우 유용한 책이 될 거라고 생각한다.
