# Week 13 - Docker 실습

웹서비스설계 13주차 Docker 이미지와 컨테이너 실행·격리·버전 변경 실습

## 1. 실습 목적

- Docker 이미지 빌드 및 컨테이너 실행
- 호스트와 컨테이너의 포트 매핑 이해
- 컨테이너 간 파일 시스템 격리 확인
- 컨테이너 재시작과 삭제 후 재생성의 차이 확인
- 이미지 버전 변경 및 기존 컨테이너와의 비교

## 2. 실행 환경

- OS: Windows (amd64)
- Container OS: Linux (amd64)
- Docker Desktop: 4.100.1
- Docker Engine: 29.9.3
- Base Image: nginx:1.28-alpine

## 3. 프로젝트 구조

```
project/
├── Dockerfile
├── .dockerignore
└── index.html
```

## 4. 실행 방법

Docker Desktop을 실행한 후 프로젝트 폴더에서 다음 명령을 실행한다.

```bash
docker build -t wsd13ko:v2 .
docker run -d --name wsd13ko-v2 -p 18383:80 wsd13ko:v2
```

브라우저 접속 주소: http://127.0.0.1:18383

실행 결과: Release v2 is online.

## 5. 버전 변경 이력

| 버전 | 변경 내용 | 실습 이미지 ID |
|---|---|---|
| v1 | Release v1 is online. | 35e60c69e0ad |
| v2 | Release v2 is online. | ceaeff6fbb05 |

index.html의 제목 문구를 v1에서 v2로 변경하였다. 로컬 파일을 수정하는 것만으로는 기존 컨테이너에 변경 사항이 반영되지 않았으며, 새로운 이미지를 빌드하고 컨테이너를 실행하여 변경 사항을 확인하였다.

v1과 v2의 소스코드 차이는 Git 커밋 이력을 통해 확인할 수 있다.

## 6. 실습 결과

- 이미지 빌드 및 컨테이너 실행 성공
- 호스트 포트 18381, 18382, 18383과 컨테이너 내부 포트 80의 연결 확인
- visit=1, visit=2 요청의 HTTP 200 응답 확인
- 동일한 이미지로 생성한 A와 B의 컨테이너 ID가 서로 다름을 확인
- A에서 생성한 파일이 B에서는 조회되지 않음을 확인
- 컨테이너 재시작 시 ID와 내부 파일 유지 확인
- 컨테이너 삭제 후 재생성 시 ID 변경 및 내부 파일 소실 확인
- v1과 v2 이미지 및 웹페이지 비교 완료
- 실습 컨테이너 3개와 이미지 태그 2개 삭제 완료

## 7. 자원 정리

```bash
docker stop wsd13ko-v2
docker rm wsd13ko-v2
docker image rm wsd13ko:v2
```

위 명령은 README의 실행 방법에 따라 생성한 자원을 정리하는 명령이다.

실습에서는 볼륨 및 바인드 마운트를 사용하지 않았으며, Docker 자원을 삭제한 이후에도 로컬 프로젝트 파일과 Git 커밋 이력이 유지됨을 확인하였다.