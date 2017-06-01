---
layout: post
title: Change IP address for cloudera manager
---

* 상황; server 이전으로 인해 cloudera manager로 관리하는 hadoop server들의 ip address가 변경
* server 이전하기 전 처리 내역; cloudera manager에서 cluster및 cloudera management service 정지 후

  ```
  $ sudo service cloudera-scm-server stop
  $ sudo service cloudera-scm-server-db stop
  (cluster의 모든 server) $ sudo service cloudera-scm-agent stop
  $ ./zeppelin-daemon.sh stop
  ```
* 문제 발생; server 이전 후 위의 작업을 반대 순서로 start를 했으나 cloudera manager에서 cluster와 cloudera management service를 시작할 수 없음
  * 원인; 바뀐 ip address를 제대로 알지 못하기 때문
  * cloudera manager에서 각 server ip를 어떻게 가지고 있는지는 메뉴바 상단의 ‘호스트' 부분을 누르면 됨(당연히 아는 건데 이렇게 문제가 생기면 기억이 잘 나질 않음)
    ![_config.yml]({{ site.baseurl }}/images/20170601_host.png)
* 해결; 바뀐 server ip address를 다음과 같이 반영
  * /etc/hosts를 수정하고, 모든 server에 복사
  * 모든 cloudera-scm-agent, cloudera-scm-server 중지
  * (cluster의 모든 server) $ sudo vi /etc/cloudera-scm-agent/config.ini의 server_host 수정
    ![_config.yml]({{ site.baseurl }}/images/20170601_config.png)
  * 모든 cloudera-scm-agent, cloudera-scm-server 시작
  * (필요한 경우) $ sudo service postgresql initdb
  * $ sudo service postgresql restart
  * cloudera manager에 접속해 다시 메뉴바 상단의 ‘호스트' 부분에서 변경한 ip가 적용되었는지 확인
  * cloudera management service와 cluster 시작
  * cluster에서 클라이언트 구성 배포
    ![_config.yml]({{ site.baseurl }}/images/20170601_distribution.png)
  * cluster 재시작
* 참고
  * https://community.cloudera.com/t5/Cloudera-Manager-Installation/CDH-5-1-host-IP-address-change/td-p/20034
  * http://shulhi.com/change-ip-address-for-existing-nodes-in-cdh-5-3/
  * https://www.cloudera.com/documentation/enterprise/5-8-x/topics/cm_ag_mgmt_service.html#xd_583c10bfdbd326ba-3ca24a24-13d80143249--7f1a__section_df1_4ft_4n
  * https://community.cloudera.com/t5/Cloudera-Manager-Installation/cloudera-scm-server-is-dead-and-pid-file-exists/td-p/22701
