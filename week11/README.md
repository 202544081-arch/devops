
# 11주차 - 쿠버네티스 설정과 상태 점검

## 오늘 학습한 내용

- ConfigMap과 Secret의 개념과 차이
- 환경변수를 이용한 설정값 주입
- Redis 연결 및 설정 관리
- requests와 limits를 이용한 자원 관리
- Liveness Probe와 Readiness Probe
- Kubernetes 트러블슈팅

## 새롭게 배운 명령어

```bash
# ConfigMap과 Secret 확인
kubectl get cm,secret

# 리소스 적용
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml

# Pod 상태 확인
kubectl get pods

# Pod 상세 정보 확인
kubectl describe pod <Pod이름>

# 로그 확인
kubectl logs <Pod이름>

# 이전 컨테이너 로그 확인
kubectl logs <Pod이름> --previous

# Deployment 재시작
kubectl rollout restart deployment guestbook
```

## 실습 내용

### 1. ConfigMap과 Secret
- ConfigMap과 Secret 생성
- 환경변수로 설정값 주입
- Secret의 Base64 인코딩 및 디코딩

### 2. Redis 연결 및 설정 변경
- Redis Deployment와 Service 생성
- Guestbook과 Redis 연결
- ConfigMap 변경 후 설정 적용 확인

### 3. Probe 실습
- Liveness Probe와 Readiness Probe 설정
- Redis 연결 중단으로 Readiness 실패 확인
- 잘못된 경로 설정으로 Liveness 실패 확인

### 4. 트러블슈팅
- Pod 상태 확인
- 상세 정보와 Events 확인
- 로그를 이용해 오류 원인 파악

## 핵심 정리

- ConfigMap은 일반 설정값, Secret은 민감한 정보를 관리한다.
- Secret의 Base64는 암호화가 아닌 인코딩이다.
- Liveness Probe는 컨테이너 재시작 여부를 확인한다.
- Readiness Probe는 요청을 받을 준비가 되었는지 확인한다.
- requests는 필요한 자원, limits는 최대 사용 자원을 설정한다.
- 문제 발생 시 get → describe → logs 순서로 확인한다.

