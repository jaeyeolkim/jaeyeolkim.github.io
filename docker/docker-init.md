# docker hub 에 push 하기
1. docker commit ${id} centos:7-git
2. docker login
3. docker tag centos:7-git jaeyeolkim/centos:7-git
4. docker push jaeyeolkim/centos:7-git
