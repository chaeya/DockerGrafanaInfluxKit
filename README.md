# Grafana with InfluxDB for Docker
특정한 데이터에 대한 모니터링이 필요한 사용자가 쉽게 사용할 수 있도록 도커를 이용하여 Grafana 와 InfluxDB 를 바로 사용할 수 있도록 패키징하였습니다.
최신버전의 InfluxDB 와 Grafana 와 필요한 플러그인들이 함께 설치되므로 바로 사용할 수 있습니다.

## InfluxDB
InfluxDB 는 시계열 데이터베이스 입니다.
시계열 데이터베이스란 시간을 기반으로 지표를 쌓을 수 있게 해주는 데이터베이스로서 time 컬럼이 기본으로 탑재되는 데이터베이스를 의미합니다.
InfluxDB 는 HTTP 기반의 REST API를 제공하기 때문에 쉽게 지표를 쌓을 수 있습니다.

## Grafana
Grafana 는 시각화 도구입니다. 
쌓여있는 데이터를 효과적으로 시각화 할 수 있도록 해주는 프로그램으로서 모니터링을 위한 대시보드를 만드는 데 많이 사용되고 있습니다.

## nmon2influxdb
nmon 의 결과 파일을 influxDB 로 임포트 할 수 있습니다.
관련문서는 [https://nmon2influxdb.org/] 를 참고하세요

## History
* 2018-05-27 v2.0 - nmon2influxdb 를 이용한 대시보드 추가
* 2018-05-20 v1.5 - nmon2influxdb 추가
* 2018-05-10 v1.0 - Grafana with InfluxDB Using docker-compose


# Requirement
- Ubuntu 16.04 에서 테스트 되었습니다.
- 시스템에 docker 가 필요합니다. 없는 경우 [https://docs.docker.com/install/linux/docker-ce/ubuntu/] 를 참고하세요.
- docker-compose 가 필요니다. 없는 경우 [https://docs.docker.com/compose/install/#install-compose] 를 참고하세요.
- port 3000/tcp, 8086/tcp 를 사용할 수 있어야 합니다. 
- nmon2influxdb를 사용하기 위해서는 nmon 이 설치되어 있어야 합니다. 


# Demo
- Demo site : [http://demo.invesume.com:3000]
- Login Account : admin / admin

# Install and Run

## 소스코드 다운로드
```
$ git clone https://github.com/chaeya/DockerGrafanaInfluxKit.git
```

## 빌드
```
$ cd DockerGrafanaInfluxKit
$ docker-compose build
```

## 서비스 구동
```
$ docker-compose up -d
```
구동 후 grafana 플러그인을 다운로드 받는 과정이 있어서 시간이 걸립니다.
바로 접속하지 말고 1~2분 정도 기다려야 정상적으로 구동 됩니다.

## 접속

[http://localhost:3000](http://localhost:3000) 으로 접속한 후 admin/admin 으로 로그인 합니다.

## 데이터 보관
docker 를 재구동해도 데이터가 로컬 서버에 남아 있도록 분석한 데이터는 서버에 보관됩니다.
데이터가 저장되는 경로는 서버에서 아래의 명령어를 실행하면 알 수 있습니다.
```
$ docker volume inspect dockergrafanainfluxkit_grafana_data | grep Mountpoint
$ docker volume inspect dockergrafanainfluxkit_influxdb_data  | grep Mountpoint
```

# nmon 으로 리눅스 모니터링 하기

다운로드 받은 nmon2influxdb 를 전체 시스템에서 사용할 수 있도록 /usr/local/bin 으로 복사하고 실행권한을 부여합니다.
```
$ sudo cp nmon2influxdb/nmon2influxdb /usr/local/bin
$ sudo chmod +x /usr/local/bin/nmon2influxdb
```

nmon 으로 리눅스 시스템 모니터링 데이터를 수집해서 grafana 대시보드로 보기 위해서, 매일 10초 간격으로 nmon 이 모니터링 데이터를 수집할 수 있도록 crontab 에 다음과 같이 설정해 줍니다.
```
0 0 * * * /usr/local/bin/nmon2influxdb import <nmon 결과값을 저장할 경로>/*.nmon && /usr/bin/nmon -f -m <nmon 결과값을 저장할 경로> -s 10 -c 43195
```

grafana 에서 보여 줄 대시보드를 생성해줍니다.
```
nmon2influxdb dashboard -f <nmon 결과 파일>
```

grafana 에 대시보드 업로드
```
nmon2influxdb dashboard <nmon 결과 파일>
```

## nmon Dashboard 사용
- 대시보드를 별도로 만들기 어려운 사용자들을 위해서 미리 준비된 대시보드를 제공합니다.
- grafana 로그인 후 왼쪽의 Create > Import 메뉴를 이용하면 다운받은 소스코드 중 nmon2influxdb/nmonDashboard.json 파일을 grafana 에서 불러와서 사용할 수 있습니다.

![import dashboard](https://github.com/chaeya/DockerGrafanaInfluxKit/blob/master/nmon2influxdb/import.png)


## 모니터링 화면 예

![Memory](https://github.com/chaeya/DockerGrafanaInfluxKit/blob/master/nmon2influxdb/mem.png)
![Network](https://github.com/chaeya/DockerGrafanaInfluxKit/blob/master/nmon2influxdb/network.png)
![Disk](https://github.com/chaeya/DockerGrafanaInfluxKit/blob/master/nmon2influxdb/du.png)




## Link to the related article: 
https://www.blazemeter.com/blog/how-to-create-a-lightweight-performance-monitoring-solution-with-docker-grafana-and-influxdb
