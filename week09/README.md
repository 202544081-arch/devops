
# 9주차 - 쿠버네티스

## 오늘 학습한 내용

- 쿠버네티스(Kubernetes)의 개념과 필요성
- Docker Compose와 쿠버네티스의 차이
- 컨테이너 오케스트레이션의 개념
- 명령형(Imperative)과 선언형(Declarative)의 차이
- 쿠버네티스 클러스터의 구조
- Control Plane, Node, Pod의 역할
- kubectl 명령어 사용 방법
- Deployment와 자가 치유(Self-healing)

## 새롭게 배운 명령어

### 1. 설치 및 클러스터 실행

```bash
# kubectl 설치 확인
kubectl version --client

# Minikube 실행
minikube start --driver=docker

# Minikube 상태 확인
minikube status

# 클러스터 노드 확인
kubectl get nodes

# 현재 사용 중인 클러스터 확인
kubectl config current-context
```

### 2. Pod 생성 및 확인

```bash
# nginx Pod 생성
kubectl run mynginx --image=nginx

# Pod 목록 확인
kubectl get pods

# Pod 상태 실시간 확인
kubectl get pods -w

# Pod 상세 정보 확인
kubectl describe pod mynginx

# Pod 로그 확인
kubectl logs mynginx

# Pod 내부 접속
kubectl exec -it mynginx -- bash

# Pod 삭제
kubectl delete pod mynginx
```

### 3. 포트 포워딩

```bash
# 내 컴퓨터 8080 포트와 Pod의 80 포트 연결
kubectl port-forward mynginx 8080:80
```

- 브라우저에서 http://localhost:8080 접속
- Ctrl + C로 포트 포워딩 종료

### 4. Deployment와 자가 치유

```bash
# Deployment 생성 및 Pod 3개 실행
kubectl create deployment mynginx --image=nginx --replicas=3

# Deployment 확인
kubectl get deployments

# ReplicaSet 확인
kubectl get replicasets

# Deployment, ReplicaSet, Pod 한 번에 확인
kubectl get deploy,rs,pods

# Pod 복제본 개수 변경
kubectl scale deployment mynginx --replicas=5

# Deployment 삭제
kubectl delete deployment mynginx
```

### 5. 내 이미지 배포하기

```bash
# 이전 실습 Deployment 삭제
kubectl delete deployment mynginx

# GitHub Container Registry에 올린 이미지 실행
kubectl create deployment guestbook --image=ghcr.io/내아이디/guestbook:v1

# Pod 상태 확인
kubectl get pods -w

# Deployment 확인
kubectl get deployments

# Pod 로그 확인
kubectl logs -l app=guestbook

# 브라우저 접속을 위한 포트 포워딩
kubectl port-forward deployment/guestbook 8080:5000
```

## 실습 내용

### 1. 쿠버네티스 설치

- kubectl 설치 및 버전 확인
- Minikube 또는 kind를 이용한 클러스터 생성
- kubectl get nodes 명령어로 노드 상태 확인
- 노드 상태가 Ready인지 확인

### 2. 첫 번째 Pod 생성

- nginx 이미지를 이용해 mynginx Pod 생성
- kubectl get pods로 실행 상태 확인
- describe와 logs를 이용한 상세 정보 및 로그 확인
- exec 명령어로 컨테이너 내부 접속
- port-forward로 브라우저에서 nginx 접속

### 3. Deployment와 자가 치유 실습

- kubectl run으로 생성한 Pod 삭제
- Pod 삭제 후 자동으로 복구되지 않는 것 확인
- Deployment를 이용해 nginx Pod 3개 생성
- Pod 하나를 삭제한 뒤 새로운 Pod가 생성되는 것 확인
- kubectl scale로 Pod 개수 변경

### 4. 나만의 방명록 배포

- GHCR에 업로드한 guestbook:v1 이미지 사용
- 쿠버네티스에서 guestbook Deployment 생성
- Pod 상태와 로그 확인
- port-forward를 이용해 브라우저에서 방명록 실행

## 핵심 정리

- 쿠버네티스는 컨테이너의 배포, 확장, 관리를 자동화하는 오케스트레이션 도구이다.
- kubectl은 쿠버네티스 클러스터에 명령을 전달하는 도구이다.
- Pod는 쿠버네티스에서 컨테이너가 실행되는 기본 단위이다.
- Deployment는 원하는 Pod 개수를 유지하고 관리한다.
- 선언형 방식은 원하는 상태를 지정하면 쿠버네티스가 현재 상태를 지속적으로 맞추는 방식이다.
- 자가 치유 기능으로 Pod가 삭제되면 Deployment가 새로운 Pod를 생성한다.
- Pod가 새로 만들어지면 내부의 임시 데이터는 사라질 수 있으므로 별도의 저장 공간이 필요하다.
- 문제가 발생하면 kubectl describe와 kubectl logs로 원인을 확인한다.
