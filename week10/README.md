
# 10주차 - 쿠버네티스 배포와 서비스 / 왜 굳이

## 오늘 학습한 내용

### Part 1. 쿠버네티스 배포와 서비스

- Deployment, ReplicaSet, Pod의 계층 관계
- YAML을 이용한 선언형 배포
- 레이블(Label)과 셀렉터(Selector)의 역할
- Service가 필요한 이유와 동작 방식
- ClusterIP, NodePort, LoadBalancer의 차이
- port, targetPort, nodePort의 차이
- 스케일링과 로드밸런싱
- 롤링 업데이트와 롤백

### Part 2. 왜 굳이 이 도구들을 사용할까?

- WSL과 Ubuntu를 사용하는 이유
- Docker를 이용한 실행 환경 통일
- Git과 GitHub를 이용한 코드 관리와 공유
- GUI와 CLI의 차이
- Docker Compose의 필요성
- Kubernetes를 이용한 컨테이너 운영 관리
- 상황에 맞는 개발 및 배포 도구 선택

## 새롭게 배운 명령어

### 1. 클러스터 실행 및 확인

```bash
# Minikube 실행
minikube start --driver=docker

# 노드 확인
kubectl get nodes

# 현재 컨텍스트 확인
kubectl config current-context

# Minikube 컨텍스트 사용
kubectl config use-context minikube
```

### 2. YAML 파일을 이용한 배포

```bash
# YAML 파일 문법 및 규칙 검사
kubectl apply -f k8s/deployment.yaml --dry-run=server

# Deployment 배포
kubectl apply -f k8s/deployment.yaml

# Deployment 확인
kubectl get deployments

# ReplicaSet 확인
kubectl get replicasets

# Pod 확인
kubectl get pods

# 전체 리소스 확인
kubectl get all

# Service 확인
kubectl get services
```

### 3. Pod와 레이블 확인

```bash
# Pod의 레이블 확인
kubectl get pods --show-labels

# 특정 레이블을 가진 Pod 조회
kubectl get pods -l app=guestbook

# Pod 하나 삭제
kubectl delete pod <Pod이름>

# 레이블을 기준으로 Pod 삭제
kubectl delete pods -l app=guestbook

# Pod 상태 실시간 확인
kubectl get pods -w
```

### 4. Service 배포 및 접속

```bash
# Service 배포
kubectl apply -f k8s/service.yaml

# Service 확인
kubectl get svc

# Service 상세 정보 확인
kubectl describe svc guestbook

# Minikube 서비스 접속
minikube service guestbook

# 포트 포워딩
kubectl port-forward svc/guestbook 8080:80
```

### 5. 스케일링

```bash
# Pod 복제본을 4개로 늘리기
kubectl scale deployment guestbook --replicas=4

# Pod 확인
kubectl get pods

# Pod 복제본을 2개로 줄이기
kubectl scale deployment guestbook --replicas=2
```

### 6. 롤링 업데이트 및 롤백

```bash
# 새 이미지로 변경
kubectl set image deployment/guestbook web=ghcr.io/내아이디/guestbook:v2

# 업데이트 진행 상태 확인
kubectl rollout status deployment/guestbook

# 업데이트 기록 확인
kubectl rollout history deployment/guestbook

# 이전 버전으로 롤백
kubectl rollout undo deployment/guestbook

# 롤백 상태 확인
kubectl rollout status deployment/guestbook
```

## 실습 내용

### 1. Deployment 작성 및 배포

- guestbook 디렉터리에 k8s 폴더 생성
- deployment.yaml 파일 작성
- Deployment의 replicas를 2개로 설정
- selector.matchLabels와 template.metadata.labels 연결
- kubectl apply를 이용해 YAML 배포
- Deployment, ReplicaSet, Pod 상태 확인

### 2. 자가 치유 확인

- Deployment가 관리하는 Pod 삭제
- 새로운 Pod가 자동으로 생성되는지 확인
- 레이블을 이용하여 Pod를 선택하고 삭제
- 원하는 Pod 개수를 유지하는 원리 이해

### 3. Service 작성 및 접속

- service.yaml 파일 작성
- NodePort 타입의 Service 생성
- app=guestbook 레이블을 가진 Pod와 연결
- port: 80, targetPort: 5000, nodePort: 30080 설정
- minikube service 명령어로 브라우저 접속
- describe svc 명령어로 Endpoints 확인

### 4. 스케일링과 로드밸런싱

- Deployment의 Pod 개수를 2개에서 4개로 변경
- Service가 여러 Pod에 요청을 분배하는 과정 확인
- 브라우저 새로고침으로 서로 다른 Pod 응답 확인
- Pod 개수를 다시 2개로 축소

### 5. 롤링 업데이트

- guestbook의 index.html 수정
- v2 이미지 빌드 및 업로드
- Deployment의 이미지를 v1에서 v2로 변경
- 기존 Pod와 새 Pod가 순차적으로 교체되는 과정 확인
- 서비스가 계속 응답하는지 확인

### 6. 롤백

- Deployment의 업데이트 이력 확인
- kubectl rollout undo로 이전 버전 복구
- 이전 ReplicaSet이 다시 활성화되는 과정 확인
- 브라우저에서 이전 버전으로 돌아왔는지 확인

## 핵심 정리

### 쿠버네티스 배포와 서비스

- Deployment는 애플리케이션의 원하는 상태와 배포 버전을 관리한다.
- ReplicaSet은 원하는 Pod 개수를 유지한다.
- Pod는 컨테이너가 실제 실행되는 단위이다.
- Service는 Pod의 IP가 바뀌어도 안정적으로 접근할 수 있는 창구이다.
- Label은 리소스를 구분하는 이름표이고 Selector는 해당 레이블을 가진 리소스를 선택한다.
- NodePort는 노드의 포트를 통해 외부에서 접근할 수 있게 한다.
- 스케일링은 Pod의 복제본 개수를 조절하는 작업이다.
- 롤링 업데이트는 기존 Pod를 순차적으로 새 버전으로 교체하는 방법이다.
- 롤백은 문제가 발생했을 때 이전 배포 버전으로 되돌리는 작업이다.

### 왜 굳이 이 도구들을 사용할까?

- WSL은 Windows에서 Linux를 실행할 수 있는 환경이다.
- Ubuntu는 Linux 배포판 중 하나이다.
- Docker는 프로그램의 실행 환경을 이미지로 만들어 일관되게 재현하도록 돕는다.
- Docker Compose는 여러 컨테이너와 네트워크, 환경 변수, 볼륨을 하나의 파일로 관리한다.
- Kubernetes는 컨테이너의 복제본 수 유지와 장애 발생 시 재생성을 관리한다.
- Git은 코드의 변경 이력을 관리하고 GitHub는 저장소를 온라인에 보관하고 공유하는 서비스이다.
- CLI는 명령어를 조합하여 반복 작업을 자동화하기 쉽고 원격 서버 관리에도 유용하다.
- 도구를 무조건 많이 사용하는 것보다 해결하려는 문제에 맞는 도구를 선택하는 것이 중요하다.

