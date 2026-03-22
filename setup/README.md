# ROS2 Jazzy 팀 개발환경

Ubuntu Noble 24.04 + ROS2 Jazzy 기반 팀 공통 Docker 개발환경입니다.

## 파일 구조

```
.
├── Dockerfile               # 팀 공통 ROS2 환경 정의
├── docker-compose.yml       # 컨테이너 실행 설정
├── .devcontainer/
│   └── devcontainer.json    # VSCode Dev Container 설정
├── .dockerignore            # 빌드 제외 파일 목록
└── src/                     # ROS2 패키지 소스코드 (여기에 작업)
```

## 처음 세팅하는 경우

### 사전 준비
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) 설치
- [VSCode](https://code.visualstudio.com/) + [Dev Containers 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 설치

### 방법 A: VSCode Dev Container (권장)

```bash
git clone https://github.com/팀/프로젝트명
code 프로젝트명
# VSCode 우하단 팝업 → "Reopen in Container" 클릭
# 또는 F1 → "Dev Containers: Reopen in Container"
```

### 방법 B: docker compose

```bash
git clone https://github.com/팀/프로젝트명
cd 프로젝트명

# X11 접근 허용 (GUI 앱 실행 위해 필요, Linux/macOS)
xhost +local:docker

# 이미지 빌드 + 컨테이너 실행
docker compose up -d

# 컨테이너 안으로 진입
docker exec -it pinky_dev bash
```

## 자주 쓰는 명령어

```bash
# 컨테이너 안에서 워크스페이스 빌드
cd /ros2_ws && colcon build --symlink-install

# 환경 source
source install/setup.bash

# 컨테이너 종료
docker compose down

# 이미지 재빌드 (Dockerfile 수정 후)
docker compose up --build
```

## ROS_DOMAIN_ID

팀 전체가 `42`로 통일되어 있습니다.
같은 네트워크에서 여러 로봇/PC 간 ROS2 통신이 가능합니다.

## Dockerfile 수정이 필요할 때

새 ROS2 패키지 추가가 필요하면 `Dockerfile`의 `apt-get install` 부분에 추가 후
팀원들에게 공유하면 `docker compose up --build`로 반영됩니다.
