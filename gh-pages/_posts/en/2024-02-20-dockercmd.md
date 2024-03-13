---
layout: post
title:  "Docker CMDs"
categories: ['Error Resolution']
lang: en
tags: docker
sub-title: "Notes of Docker Commands to deal with images and containers"
---


#### Search Image

```
docker search (OPTIONS) [IMAGE_NAME]
```

```--automated=false```  Show only Automated Build

```--no-trunc=false```  Do not truncate information

```-s=n``` ```--stars=n```  Show images which have stars over ```n```

e.g. docker search --stars=100 mysql



#### Download Image

```
docker pull (OPTIONS) [IMAGE_NAME](:TAG)
```

```-a```  Download all versions of the image

```TAG_NAME```  The version tag intended to download, or the lastest version

e.g. docker pull ubuntu:22.04



#### Check the Image list

```
docker images (OPTIONS) (REPOSITORY)
```

```-a``` ```--all```  Show all images

```--digests```  Show digest

```-q``` ```--quiet```  List up images by only those IDs

*Check the container list  ```docker ps```  



#### Check the details of image

```
docker image inspect [IMAGE_ID]
```

Check with the ID of the image. It's okay only with the part of the ID. 


#### Delete Image

```
docker rmi (OPTION) [IMAGE_NAME](:TAG)
```

```-f```  실행 중인 container의 image를 강제 삭제, 그러나 실질적으로 untagging만 되고 image와 container 모두 제대로 삭제되지 않으니 사용 X

* ```[IMAGE_NAME_1] [IMAGE_NAME_2] ... ```  image 여러 개 한 번에 삭제 가능 

* **특정 image의 실행 중인 container를 모두 종료시킨 후 image 삭제**

  ```
  docker rm -f $(docker ps -a --filter ancestor=[IMAGE_NAME])
  docker rmi [IMAGE_NAME]
  ```

  ```$(CMD)```



#### Image 저장/로드

```
docker save -o [DIRECTORY] [IMAGE_NAME]
```

```-o```  image를 저장할 디렉토리(output)를 지정

```
docker load -i [DIRECTORY] 
```

```-i```  불러올 image의 디렉토리(input)를 지정, 해당 디렉토리 내 image가 로드됨



#### Image 태그 지정

```
docker tag [IMAGE_NAME]:[TAG] [NEW_NAME]:[NEW_TAG]
```

존재하는 image를 복사하여 새로운 이름과 tag으로 참조 가능하게 함

e.g. docker tag ubuntu:22.04 abcd:0.1



#### Container list 확인

``` 
docker ps (OPTION)
```

실행 중인 container들 확인

```-a```  실행 여부와 상관 없이 (종료된 것까지) 모든 container 확인



#### Container 세부 정보 확인

```
docker inspect [CONTAINER_NAME]
```



#### Image에서 Container 실행

```
docker run (OPTIONS) [IMAGE_NAME] 
```

```--name [CONTAINER_NAME]```  container 이름 설정

```--rm```  ```run```명령어 수행 후 container 삭제. container 일회성 사용 

```-it```  container에 터미널 입력을 계속해서 전달

*  ```-i```  container 접속하지 않은 상태에서도 stdin 활성화

*  ```-t```  pseudo-TTY 할당(TTY모드 사용), 쉘에 명령어 작성

```-d```  container 백그라운드 실행. 옵션 입력 시 실행된 container id가 출력됨

```-e```  container에 환경변수 추가. 추가하고 싶은 환경변수만큼 사용해야 한다. 

* ```
  docker run -e APP_ENV=production APP2_ENV=dev ubuntu:22.04 env
  ```

```-p [HOST_PORT]:[CONTAINER_PORT]```  호스트에 연결된 container의 특정 포트를 호스트의 포트와 binding하기 위해 사용. 보통 웹서버의 포트를 외부로 노출하기 위해 사용

```-w [DIR]```  container의 working directory 변경

```-v [HOST_DIR]:[CONTAINER_DIR]```   호스트의 특정 디렉토리를 container에 mount

* e.g. 현재 작업 디렉토리를 container에 mount 하기

  ``````
  docker run -v `pwd`:/opt ubuntu:22.04
  ``````

```-u [USERID]```  특정 user id로 container에 접속. image build 시 계정 추가해야 함



#### 실행 중인 Container에 명령어 입력

```
docker exec [CONTAINER_ID or NAME] [CMD]
```

실행한 container에서 입력한 cmd 실행

```-it```  container 환경에서 shell 실행, 





run과의 차이점: run은 image에서 container를 실행하는 명령어, exec는 이미 실행 중인 container에만 적용 가능하며, container 환경을 디버깅하는 목적. run으로 실행된 container는 stdin/out 에러 확인 가능하나, exec로 실행된 프로세스는 container가 아님, 로그 확인이나 프로세스 완료 여부를 알기 어렵다

