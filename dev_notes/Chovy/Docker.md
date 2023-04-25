# Docker

도커는 리눅스의 응용 프로그램들을 프로세스 격리 기술들을 사용해 컨테이너로 실행하고 관리하는 오픈 소스 프로젝트이다.

[https://docs.docker.com/desktop/windows/wsl/](https://docs.docker.com/desktop/windows/wsl/)

Docker WSL2를 사용 가능한 exe 설치

1. 위 링크에서 매뉴얼이 있으니 그대로 설치

1. 설치, 셋팅 완료 후 리눅스 환경에서 설치 확인

       # docker ps -a

1. docker test
#docker run —name nginx-test -d -p 8080:80 nginx:latest
# docker ps

인터넷 주소창에 [localhost:8080](http://localhost:8080) 검색 

도커설치 완료