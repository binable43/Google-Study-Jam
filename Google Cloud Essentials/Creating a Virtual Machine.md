# Creating a Virtual Machine

## Overview
- Google Cloud Console과 gloud 명령줄을 사용하여 다양한 머신 유형의 가상 머신 인스턴스 생성
- 웹 서버 배포 후 가상 머신에 NGINX 웹 서버 연결
- vim, emacs, nano 등 표준 Linux 텍스트 편집기 사용에 익숙하면 도움이 됨

<br>

## 설정
- 실습 시작 후 Google Console에 로그인

<br>

### Google Cloud Shell 활성화
- Google Cloud Shell
  - 다양한 개발 도구가 탑재된 가상 머신, 5GB의 영구 홈디렉토리 제공
  - 명령줄을 통해 GCP 리소스에 액세스 가능

1. 오른쪽 상단 툴바에서 Cloud Shell 열기
2. Continue 클릭
3. 환경 프로비저닝하고 연결

<img width="828" alt="스크린샷 2022-05-06 오후 1 40 07" src="https://user-images.githubusercontent.com/81629116/167068285-bc3d3a73-856d-40a2-91c4-25db6069e965.png">

<br>

### 리전 및 영역의 이해
**리전**이란, 리소스를 실행할 수 있는 특정 지리적 위치를 의미한다. 각 리전에는 하나 이상의 영역이 있다. 예를 들어 us-central1 리전은 `us-central1-a`, `us-central1-b`, `us-central1-c`, `us-central1-f` 영역이 있는 미국 중부의 리전을 나타낸다. 

**영역별 리소스** 란, 영역 내에 상주하는 리소스를 의미한다. 가상머신 리소스와 영구 디스크는 영역에 상주한다. 영구 디스크를 가상머신 인스턴스에 연결하려면 두 리소스가 모두 같은 영역에 있어야 하며, 마찬가지로 인스턴스에 정적 IP 주소를 할당하려면 인스턴스가 정적 IP와 같은 리전에 있어야 한다


<br>
<br>

## 작업1 : cloud console에서 새 인스턴스 만들기
1. Cloud Console의 오른쪽 탐색 메뉴에서 **Compute Engine > VM Instanse** 클릭
2. **인스턴스 만들기** 를 통해 새 인스턴스 생성
3. **새 인스턴스** 에서 다양한 매개변수를 설정할 수 있다. 본 실습에서는 아래와 같이 구성   

|필드|값|추가정보|
|---|--|----|
|Name|gcelab|VM 인스턴스 이름|
|Regions|us-central1(아이오와)||
|Zone|us-central1-f||
|Series|N1|시리즈의 이름|
|Machine Type|vCPU 2개|(n1-standard-2), 2-CPU, 7.5GB RAM 인스턴스|
|부팅 디스크|Debian GNU/Linux 10GB(buster)||
|Firewalls|HTTP 트래픽 허용|나중에 설치할 웹 서버에 액세스하기 위해 해당 옵션 선택|

4. **만들기**  클릭
5. **SSH** 를 사용하여 가상머신에 연결하기 위해 머신이 표시된 행에서 **SSH** 클릭
<img width="953" alt="스크린샷 2022-05-06 오후 1 54 42" src="https://user-images.githubusercontent.com/81629116/167071829-89f03e60-c671-47d2-8213-0d1cedd6cf85.png">

<br>
<br>

## 작업2 : NGINX 웹 서버 설치

가상머신을 원하는 인스턴스에 연결하기 위해 NGINX 웹 서버를 설치해보자. NGINX 웹 서버는 전 세계적으로 가장 많이 사용되는 웹 서버 중 하나이다.

1. SSH 터미널에서 `root` 권한 획득을 위해 다음 명령어 실행
```bash
sudo su -
```

2. `root` 사용자로서 OS 업데이트
```bash
apt-get update
```

3. **NGINX** 설치
```bash
apt-get install nginx -y
```

4. NGINX가 실행 중인지 확인
```bash
ps auwx | grep nginx
```

5. 웹페이지 확인 (Cloud Console에서 머신이 표시된 행에서 **외부 IP** 링크 클릭)
<img width="556" alt="스크린샷 2022-05-06 오후 2 26 26" src="https://user-images.githubusercontent.com/81629116/167072357-847a0d4a-1ee6-4e98-853e-cf0c6337bb41.png">

<br>
<br>

## 작업3 : gcloud를 사용하여 새 인스턴스 생성

Google Console을 사용해서 가상머신 인스턴스를 만드는 대신 Google Cloud Shell에 사전설치되어 있는 명령줄 도구 `gcloud`를 사용할 수 있다. **Cloud Shell**은 필요한 모든 개발 도구(gcloud, git 등)가 로드된 Debian 기반 가상 머신으로 5GB의 영구적인 홈 디렉토리를 제공한다.

1. Cloud Shell에서 `gcloud`를 사용하여 명령줄에서 새 가상머신 인스턴스 생성
```bash
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone us-central1-f
```

2. 모든 기본값을 보려면 다음 명령어 실행
```bash
gcloud compute instances create --help
```

3. `help` 종료하려면 **Ctrl+C**
4. Cloud Console의 탐색 메뉴에서 **Compute Engine > VM Instance** 클릭하면 새 인스턴스 2개 확인 가능
5. SSH를 사용하여 `gcloud`를 통해 인스턴스에 연결할 수도 있음. 이 경우 영역을 추가해야 하며, 옵션을 전역으로 설정한 경우 `--zone` 플래그 생략
```bash
gcloud compute ssh gcelab2 --zone us-central1-f
```
6. **Y** 입력하여 계속 진행
7. 암호 섹션에서는 **Enter** 눌러서 공백으로 설정
8. 연결 후에는 원격 셸을 종료하여 SSH 연결 끊음 (`exit`)

<br>
<br>

## Reference
- [Google Cloud Jam](https://www.cloudskillsboost.google/)
