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


# Requirement
- Ubuntu 16.04 에서 테스트 되었습니다.
- 시스템에 docker 가 설치되어 있어야 합니다. 없는 경우 링크[https://docs.docker.com/install/linux/docker-ce/ubuntu/]를 참고하세요.
- docker-compose 가 설치되어 있어야 합니다. 없는 경우 링크[https://docs.docker.com/compose/install/#install-compose]를 참고하세요.


# Install and Run

## 소스코드 다운로드
git clone https://github.com/chaeya/DockerGrafanaInfluxKit.git

## 빌드
cd DockerGrafanaInfluxKit
docker-compose build

## 서비스 구동
docker-compose up -d

## 접속

http:<설치한 서버 IP>:3000 으로 접속한 후 admin/admin 으로 로그인 합니다.


## Link to the related article: 
https://www.blazemeter.com/blog/how-to-create-a-lightweight-performance-monitoring-solution-with-docker-grafana-and-influxdb
