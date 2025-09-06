# 설치 가이드
> Docker Desktop, WSL 및 Terminal (Ubuntu 22.04 LTS, ubuntu 계정) 환경은 모두 구성이 완료되었다고 가정하고 실행합니다

```ps
# 1) Terminal 실행 후 work 디렉토리로 이동
WORK_DIR="/home/ubuntu/work"
mkdir -p $WORK_DIR
cd $WORK_DIR

# 2) 깃헙 리포지토리 내려받기 (히스토리 최소화)
git clone --depth=1 https://github.com/psyoblade/multi-campus-data-engineer.git

# 3) 작업 폴더로 이동
cd multi-campus-data-engineer

# 4) 스크립트 실행
# ./pull-images.sh # 관리자
./load-offline.sh # 수강생
```
