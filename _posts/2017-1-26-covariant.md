---
layout: post
title: Scala, Covariant, Contravariant
---

# Covariant, Contravariant
* function간의 subtype 관계는 어떻게 형성되는가에 관한 문제
* 이해가 안 가서 그림을 그려서 이해해보려고 했는데, 이해가 안 되도 바로 알 수 있게 한 마디로 정리해줬다.
  * function argument는 contravariant, return value는 covariant
* ![problem1]({{ site.baseurl }}/images/covariant/problem1.png)
  * ![answer1]({{ site.baseurl }}/images/covariant/answer1.png)
* ![problem2]({{ site.baseurl }}/images/covariant/problem2.png)
  * ![answer2_1]({{ site.baseurl }}/images/covariant/answer2_1.png)
  * ![answer2_2]({{ site.baseurl }}/images/covariant/answer2_2.png)
  * ![answer2_3]({{ site.baseurl }}/images/covariant/answer2_3.png)
