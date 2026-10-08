# 6주차

## 오늘 학습한 내용
- Dockerfile (FROM, WORKDIR, COPY, RUN, ENV, EXPOSE, CMD)
- 레이어 캐시 (자주 변경되는 내용을 뒤쪽에 배치)
- ghcr.io에 이미지 push/pull하여 공유
- 설정 우선순위: 코드 < ENV < `-e`

## 새롭게 배운 명령어
```bash
docker build -t guestbook:v1 .
docker rmi guestbook:v1
docker tag guestbook:v1 ghcr.io/아이디/guestbook:v1
docker push ghcr.io/아이디/guestbook:v1
git tag week06
```
