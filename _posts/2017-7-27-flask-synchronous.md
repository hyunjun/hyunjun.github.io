---
layout: post
title: Flask development server, single threaded, synchronous, ...
---

* 상황
  * flask API server에서 다른 server(tornado)를 호출해 결과를 만드는 API가 있는데, 결과가 비정상
  * 비정상인 이유는 중간에 timeout이 발생했기 때문
* 아주 간략한 전체 흐름
  * ![0]({{ site.baseurl }}/images/20170727_0.jpg)
  * ePattern API를 시작하면 (내부 콜은 생략하고) Extract API를 호출
  * Extract API는 library call로 sparser 호출
  * sparser가 Entity API를 호출
  * 리턴, 리턴, 리턴이 발생하면 정상 종료
* ePattern API는 requests.get으로 Extract API를 호출
  * ![1]({{ site.baseurl }}/images/20170727_1.jpg)
* sparser는 libcurl의 curl_easy_perform으로 Entity API를 호출
  * ![2]({{ site.baseurl }}/images/20170727_2.jpg)
* timeout 발생 이유
  * ![3]({{ site.baseurl }}/images/20170727_3.jpg)
  * sparser는 Entity API의 리턴을 기다리지만, 여기서 timeout이 발생
  * 왜냐하면 flask server는 ePattern에 대한 리턴값을 Extract API에게서 기다리는 중이라, 이걸 먼저 해결해야 그 다음 Entity API를 처리해줄 수 있기 때문
* 해결 방법
  1. storage API의 흐름을 asynchronous하게 (X)
    * flask development server가 single threaded + synchronous
    * 이걸 async로 만드는 플러그인이나 기타 다른 방법이 있지만, 번거롭기도 할 뿐더러 성능상 이점이 있는지, 안정적인지 알 수가 없음
  2. storage API의 Entity / ePattern을 별도 서버로 만들어서 각각 호출 (O)
    * 가능 하고 간단
    * 지금까지 한 서버에서 호출하면서 url만 다르게 하던 걸 port를 바꿔서 해야 하고, 소스 관리가 조금 귀찮아짐
  3. production을 위해 준비했던 apache server 사용 (O)
    * 원래 서비스용으로 준비했던 apache server를 이용하면, multi threaded로 동작하기때문에 위와 같은 문제가 발생하지 않을 걸로 생각했고, 실제로 해결
