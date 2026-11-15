
# 7주차

## 오늘 학습한 내용

- Docker Compose의 개념과 필요성
- 여러 컨테이너를 함께 실행하고 관리하는 방법
- compose.yaml 파일의 구조와 작성 방법
- 컨테이너 간 네트워크 통신
- Docker Volume을 이용한 데이터 영속성
- 환경 변수를 이용한 컨테이너 설정

## 새롭게 배운 명령어

```bash
# Docker Compose 설정 확인
docker compose config -q

# 컨테이너 실행
docker compose up -d

# 실행 중인 컨테이너 확인
docker compose ps

# 특정 서비스 로그 확인
docker compose logs web

# 실시간 로그 확인
docker compose logs -f web

# 특정 컨테이너 재시작
docker compose restart web

# 컨테이너와 네트워크 삭제
docker compose down
```

## 실습 내용

### 1. Docker Compose 작성

- compose.yaml 파일 생성
- Flask 웹 애플리케이션과 Redis 서비스 정의
- web과 redis 컨테이너 연결

### 2. 컨테이너 네트워크

- Compose가 생성한 가상 네트워크 확인
- 서비스 이름(redis)을 이용한 컨테이너 간 통신
- IP 주소 대신 서비스 이름으로 접근하는 원리 이해

### 3. Volume 실습

- Redis 데이터를 Volume에 저장
- 웹 컨테이너 재시작 후 방명록 데이터 유지 확인
- docker compose down 이후에도 데이터가 유지되는지 확인

### 4. Scale 실습

- 웹 서비스를 여러 개로 확장하는 방법 학습
- 서비스의 복제본을 늘리고 관리하는 방법 이해

## 핵심 정리

- Docker Compose는 여러 컨테이너의 구성과 실행을 관리하는 도구이다.
- compose.yaml에 서비스 설정을 작성한다.
- 같은 네트워크에서는 서비스 이름으로 서로 통신할 수 있다.
- Volume은 컨테이너가 삭제되어도 데이터를 유지할 수 있도록 한다.
- 컨테이너마다 역할을 분리하면 개별 관리와 확장이 쉬워진다.
