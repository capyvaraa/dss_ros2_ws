# DSS ROS2 Bridge - 교육 내용 정리

**날짜:** 2026.01.26

---

## 1. Linux 명령어

### WSL 환경 진입
```bash
# WSL Ubuntu 22.04 환경 진입
wsl -d Ubuntu-22.04-DSS

# 홈 디렉토리로 이동
cd ~

# ROS2 워크스페이스로 이동
cd ros2_ws

# 화면 지우기
clear
```

### 필수 소프트웨어 설치
```bash
# gedit 텍스트 에디터 설치
sudo apt install gedit
```

---

## 2. ROS 2 관련 명령어

### 시각화 및 빌드
```bash
# RViz2 시각화 도구 실행
rviz2

# 워크스페이스 빌드
colcon build

# 특정 패키지만 빌드
colcon build --packages-select dss_ros2_bridge
```

### 토픽 및 노드 확인
```bash
# 활성화된 모든 토픽 목록 확인
ros2 topic list

# 활성화된 모든 노드 목록 확인
ros2 node list

# 토픽 내용 확인
ros2 topic echo /dss/sensor/imu
ros2 topic echo /dss/sensor/gps/fix

# 토픽 주기 확인
ros2 topic hz /dss/sensor/lidar3d
```

### Launch 파일 사용
```bash
# DSS ROS2 Bridge 실행
source install/setup.bash
ros2 launch dss_ros2_bridge launch.py

# Bag Replay Tool 실행
ros2 launch bag_replay_tool_ros2 bag_replay_tool.launch.py
```

### ROS2 dss_bridge 사용법
- 모든 센서 브릿지 노드 자동 실행
- NATS 서버 (172.17.208.1:4222)와 자동 연결
- 5개 노드 동시 실행:
  - DSSToROSImageNode (카메라)
  - DSSToROSPointCloudNode (LiDAR)
  - DSSToROSIMUNode (IMU)
  - DSSToROSGpsNode (GPS)
  - DSSToROSClockNode (시뮬레이션 시간)

### ROS2 bag_replay_tool 사용법
- GUI 기반 ROS2 bag 파일 재생 도구
- 재생 속도 조절 가능
- 루프 재생 옵션
- 시뮬레이션 클록 동기화 지원

---

## 3. GitHub 사용법

### 초기 설정
```bash
# Git CLI 설치 (이미 설치됨)
git --version

# GitHub CLI 설치 (이미 설치됨)
gh --version

# 사용자 정보 설정
git config --global user.name "capyvaraa"
git config --global user.email "pitagi999@gmail.com"

# GitHub CLI 로그인
gh auth login
```

### 기본 Git 명령어
```bash
# 변경 사항 스테이징
git add .

# 커밋 생성
git commit -m "commit message"

# 원격 저장소로 푸시
git push

# 브랜치 생성 및 변경
git branch -m master main

# 커밋 메시지 수정
git commit --amend -m "new message"
```

### GitHub 저장소
- **Repository:** https://github.com/capyvaraa/dss_ros2_ws
- **Default Branch:** main
- **.gitignore:** build/, install/, log/, rosbag/ 제외

---

## 4. VSCode 사용법

### 오늘 한 작업 (2026.01.28)

#### 1. Build 문제 해결
```bash
# CMakeLists.txt 수정
- Protobuf 3.15.0 경로 설정
- NATS 라이브러리 경로 설정
- nlohmann-json 개발 패키지 설치

# Build 명령
colcon build --packages-select dss_ros2_bridge
```

#### 2. 의존성 설치
- **Protobuf 3.15.0:** 소스에서 빌드 및 설치
- **NATS C 1.4.4:** GitHub에서 클론 후 CMake로 빌드
- **nlohmann-json3-dev:** apt로 설치
- **ldconfig:** 라이브러리 캐시 업데이트

#### 3. Launch 및 테스트
```bash
# 설정 로드
source install/setup.bash

# Bridge 실행
ros2 launch dss_ros2_bridge launch.py

# Bag Replay Tool 실행
ros2 launch bag_replay_tool_ros2 bag_replay_tool.launch.py
```

### GitHub Copilot 활용
- **Chat AI Agent 사용:** 코드 작성 및 디버깅 지원
- **Debugging:** 복잡한 문제 해결을 위해 Copilot에 질문
- **Code Explanation:** 코드 이해를 위한 설명 요청

### VSCode 단축키
```bash
# Command Palette 열기
Ctrl + Shift + P

# 터미널 열기
Ctrl + `

# 파일 검색
Ctrl + P

# 전체 텍스트 검색
Ctrl + Shift + F

# 파일 저장
Ctrl + S

# 파일 편집 취소
Ctrl + Z
```

### VSCode에서 파일 편집
- TOPICS.md, CMakeLists.txt, EDUCATION_NOTES.md 수정
- Markdown 형식 문서 작성
- 터미널에서 git 명령 실행

---

## 5. 설치된 주요 라이브러리

### 시스템 라이브러리
- **Protobuf 3.15.0** - Protocol Buffer 컴파일러
- **NATS C 1.4.4** - NATS 메시징 클라이언트
- **nlohmann-json3** - JSON 처리 라이브러리

### ROS2 패키지
- **dss_ros2_bridge** - DSS to ROS2 센서 브릿지
- **bag_replay_tool_ros2** - ROS2 Bag 재생 도구

---

## 6. 주요 학습 포인트

### ROS2 아키텍처
```
DSS (Unreal) ──── NATS Server ──── dss_ros2_bridge ──── ROS2 Topics
```

### CMake 설정
- Protobuf 3.15.0 수동 설정
- NATS 라이브러리 경로 설정
- C++17 표준 사용

### Build 트러블슈팅
1. Protobuf 버전 문제 해결
2. NATS 라이브러리 설치 및 ldconfig 실행
3. nlohmann-json 개발 패키지 설치
4. 라이브러리 경로 업데이트

---

**마지막 업데이트:** 2026.01.28
