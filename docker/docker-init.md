# docker hub 에 push 하기
1. docker commit ${id} centos:7-git
2. docker login
3. docker tag centos:7-git jaeyeolkim/centos:7-git
4. docker push jaeyeolkim/centos:7-git

# step1 마무리
1. 도커 커맨드는 백그라운드에서 돌고 있는 도커 엔진에 명령을 주어 컨테이너를 실행한다.
2. 컨테이너를 실행하기 위해서는 리눅스 커널이 필요하기 때문에 macOS나 Windows에서는 가상 머신의 리눈그셍서 컨테이너를 실행한다.
3. 도커 엔진은 docker 커맨드의 요청을 받아서 원격 리포지터리에서 이미지를 다운로드하고, 컨테이너를 생성하여 실행하고, 애플리케이션의 입출력을 터미널과 연결하는 역할 등을 수행한다.
4. hello-world 이미지는 도커 허브에 등록되어 있다. 도커 허브에 등록된 이미지는 누구나 무료로 다운로드할 수 있다.
5. 컨테이너는 도커 커맨드에 따라 이미지, 실행, 정지의 세 가지 상태를 전이한다.
