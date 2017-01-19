---
layout: post
title: Hadoop Streaming
---

# Troubleshooting

* performance problem
  * 별로 큰 문제가 아닌데도 성능이 이상하게 느린 경우, 환경 설정 문제일 수 있음
  * 문제
    * 12억 entry의 query count(disk에서 32GB)에서 bigram count를 구하는데, 몇 시간씩 소요
    * cloudera manager의 애플리케이션 탭에서 워크로드 요약도 볼 수 없었음
  * 원인; 실행하는 서버의 HADOOP_CONF_DIR에 yarn-site.xml이 없었음
    * 이로 인해 yarn resource manager에 적절하게 할당이 되지 않았고, cloudera manager의 애플리케이션 탭에서 워크로드 요약도 볼 수 없었음
    * history도 볼 수 없고, debugging하기도 어려워짐
  * 해결; 실행하는 서버에 환경 설정 문제가 있어, (yarn-site.xml을 정상적으로 참조할 수 있는) 다른 서버에서 실행
    * resource manager에도 잘 등록되었고, (별다른 tuning 없이) 한 번 실행하는 데 대략 15m 정도 소요
    * ![yarn application]({{ site.baseurl }}/images/yarn_application.png)
  * [7 Tips for Improving MapReduce Performance](http://blog.cloudera.com/blog/2009/12/7-tips-for-improving-mapreduce-performance/)
