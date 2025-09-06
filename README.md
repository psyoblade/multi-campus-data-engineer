# 설치 가이드
> Docker Desktop, WSL 및 Terminal (Ubuntu 22.04 LTS, ubuntu 계정) 환경은 모두 구성이 완료되었다고 가정하고 실행합니다

## 설치 환경 확인
* Docker Desktop CE 버전 설치
  - 라이센스 문제가 있다면 `Rancher Desktop` 구성도 괜찮음
* Ubuntu-22.04 LTS 설치 후, 관리자 계정으로 ubuntu 계정으로 생성
  - `wsl -l` 실행 시에 `Ubuntu-22.04` 임을 확인
* Windows Store Terminal 앱 설치
  - 터미널 접속 시에 현재 경로(`pwd`)가 `/home/ubuntu` 임을 확인


## 1. 관리자 최초 1회 이미지 다운로드
> 관리자 노드 혹은 첫 번째 학생 컴퓨터에서 1차 다운로드 후 캐시된 이미지를 USB 등으로 옮겨서 구성하는 방식

### 1-1. `Terminal` 프로그램을 통해서 도커 컨테이너 이미지 다운로드
```bash
# Terminal 프로그램을 통해서 Ubuntu-22.04 접속
mkdir -p /home/ubuntu/work
cd /home/ubuntu/work

# 다운로드 스크립트를 깃헙을 통해 내려받음
git clone https://github.com/psyoblade/multi-campus-data-engineer.git
cd multi-campus-data-engineer

# 도커 이미지 다운로드 프로그램 실행
# cache 경로가 자동으로 생성되며 네트워크 상황에 따라 30분 ~ 1시간 소요
chmod +x pull-images.sh
./pull-images.sh
```

### 1-2. `PowerShell` 프로그램을 통해서 다운로드 된 이미지를 외부 저장소(E:\)에 복사
```shell
# 캐시 저장경로를 생성
New-Item -ItemType Directory -Force -Path "E:\cache"

# 다운로드가 모두 완료되면 cache (도커 이미지) 파일을 USB 등으로 복사
Copy-Item "\\wsl$\Ubuntu-22.04\home\ubuntu\work\multi-campus-data-engineer\cache\*" "E:\cache\" -Recurse -Force
Copy-Item "\\wsl$\Ubuntu-22.04\home\ubuntu\work\multi-campus-data-engineer\load-offline.sh" "E:\" -Force
```


## 2. 학생 캐시 이미지 복사 및 이미지 저장

### 2-1. `PowerShell` 프로그램을 통해서 USB (E:\cache) 이미지 파일을 로컬에 복사
```bash
# 학생 컴퓨터의 워크스페이스 경로에 설치 경로를 생성
New-Item -ItemType Directory -Force -Path "\\wsl$\Ubuntu-22.04\home\ubuntu\work\setup\cache"

# 캐시 파일을 외부 저장장치로부터 복사
Copy-Item "E:\cache\*" "\\wsl$\Ubuntu-22.04\home\ubuntu\work\setup\cache\" -Recurse -Force
Copy-Item "E:\load-offline.sh" "\\wsl$\Ubuntu-22.04\home\ubuntu\work\setup\" -Force
```

### 2-2. `Terminal` 프로그램을 통해서 도커 컨테이너 이미지 로컬에 저장
```bash
# 설치 경로로 이동하여 설치
cd "/home/ubuntu/work/setup"
chmod u+x ./load-offline.sh
./load-offline.sh

# 설치된 이미지 확인
docker images
```