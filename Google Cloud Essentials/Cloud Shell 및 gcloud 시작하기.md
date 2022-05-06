# Cloud Shell 및 gcloud 시작하기

## Overview
- gcloud 도구로 Cloud Shell을 통해 Google Cloud에 호스팅된 컴퓨팅 리소스에 연결

<br>

## 작업1 : 환경 구성하기
기본 Region 및 Zone 설정을 확인하기 위해 다음 명령어를 실행한다.    
만일 다음 명령어에 대한 응답이 `unset`이면 기본 Zone이나 Region이 설정되지 않은 것이다.
```bash
gcloud config get-value compute/zone
gcloud config get-value compute/region
```

<br>

### 기본 Region 및 Zone 식별하기
1. project ID를 텍스트 편집기에서 복사한다
2. Cloud Shell에서 다음과 같은 `gcloud` 명령어를 실행한다. 이 때 `<your_project_ID>`에는 1에서 복사한 project ID로 바꾼다
```bash
gcloud compute project-info describe --project <your_project_ID>
```

<br>

### 환경 변수 설정
환경 변수를 설정해두면 API 또는 실행 파일이 포함된 스크립트를 작성할 때 시간을 절약하는 데 도움이 된다.
1. project ID를 저장할 환경 변수 생성
```bash
export PROJECT_ID=<your_project_ID>
```

2. zone을 저장할 환경 변수 생성
```bash
export ZONE=<your_zone>
```

3. 변수가 적절하게 설정되었는지 확인
```bash
echo $PROJECT_ID
echo $ZONE
```

<br>

### gcloud 도구로 가상머신 만들기
1. VM을 만들기 위해 다음 명령어를 실행하고 `ZONE`을 이전 명령어의 zone에 해당하는 값으로 변경
```bash
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone $ZONE
```
![스크린샷 2022-05-06 오후 3 16 13](https://user-images.githubusercontent.com/81629116/167079737-2937fb5f-4eb2-4964-87e7-3443d8e52646.png)

- 명령어 세부 정보   
`gcloud compute` : Compute Engine API보다 간단한 형식으로 Compute Engine 리소스 관리   
`instances create` : 새 인스턴스 생성   
`gcelab2` : VM의 이름   
`--machine-type` : 머신 유형을 n1-standard-2 로 지정   
`--zone` : VM이 저장되는 위치 지정. 생략할 경우 gcloud 도구가 기본 속성을 기준으로 개발자가 원하는 zone을 추론할 수 있다   

<br>

### gcloud 명령어 살펴보기
- 참고할 수 있는 간단한 사용 가이드라인 제공
```bash
gcloud -h
```

- 길고 상세한 도움말 표시
```bash
gcloud help config
```

- 환경에서 구성 목록 조회
```bash
gcloud config list
```

- 모든 속성과 각 설정 조회
```bash
gcloud config list --all
```

- 구성요소 나열
```bash
gcloud components list
```

<br>
<br>

## 작업2 : 새 구성요소 설치하기
`gcloud` 도구에서 더 쉽게 작업할 수 있도록 도와주는 gcloud 구성요소를 설치해보자

### 자동완성 모드 
`gcloud interactive`는 명령어 및 플래그를 자동으로 추천해주고 명령어 입력 시 창의 하단에 inline 도움말 스니펫을 표시한다.
드롭다운 메뉴를 사용해서 명령어 및 하위 명령어 이름과 같은 정적 정보와 플래그 이름 및 열거형 플래그 값을 자동으로 완성할 수 있다.

1. 베타 구성요소 설치
```bash
sudo apt-get install google-cloud-sdk
```

2. `gcloud interactive` 모드를 사용 설정
```bash
gcloud beta interactive
```
대화형 모드를 사용하는 경우 Tab 키를 눌러 파일 경로 및 리소스 인수를 입력한다.   

명령어 목록은 Cloud Shell 창 아래에 표시된다. F2 키를 누르면 활성 도움말 섹션을 ON/OFF 설정할 수 있다.

<br>
<br>

## 작업3 : SSH를 사용하여 VM 인스턴스에 연결
`gcloud compute`를 사용하면 인스턴스에 쉽게 연결할 수 있다. `gcloud compute ssh` 명령어는 SSH에 래퍼 기능을 제공하여 인증 및 인스턴스 이름과 IP 주소의 매핑을 처리하게 한다.

1. SSH를 사용하여 VM 인스턴스에 연결하기 위해 다음 명령어 실행
```bash
gcloud compute ssh gcelab2 --zone $ZONE
```
2. 계속하기 위해 Y 입력
3. 암호 비워두려면 Enter 입력
4. 이후 아무것도 할 필요가 없으므로 SSH 연결을 끊고 원격 셸 종료하기 위해 `exit` 명령어 실행

<br>
<br>

## 작업4 : 홈 디렉터리 사용하기
Cloud Shell 홈 디렉터리의 콘텐츠는 가상머신을 종료하다가 다시 시작하더라도 모든 Cloud Shell 세션의 프로젝트에서 그대로 유지된다

1. 현재 작업 디렉터리를 변경
```bash
cd $HOME
```
2. vi 편집기를 사용하여 `.bashrc` 파일 구성
```bash
vi ./.bashrc
```

3. ESC > `:wq` 명령어로 편집기 종료

<br>
<br>

## Reference
- [Google Cloud Jam](https://www.cloudskillsboost.google/)
