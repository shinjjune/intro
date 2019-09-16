### 개요

최근 서비스 운영환경 관리에 도커(Docker)를 선택하는 것은 자연스러운 일이 되었습니다. 그만큼 도커를 사용하면 쉽고 간편하게 서비스 운영환경을 만들고 쓸 수 있습니다. 이제는 미룰 수 없게 된 도커 조금씩 정리해 봅니다.

이 글에서는 도커가 무엇인지 소개합니다. 그리고 무료 버전인 도커 커뮤니티 에디션을 우분투에 설치하는 방법을 다루겠습니다. 도커로 서비스 운영환경을 만들어가는 기초 내용은 다음 편에 소개하겠습니다.

참고로 도커를 학습하며 정리한 거라 다소 부족하거나 틀린점이 있을 수 있습니다. 그런 부분이 발견되면 댓글로 알려주시면 감사하겠습니다.

### Immutable infrastructure – 변경 불가능한 인프라 구조

도커를 언급하기에 앞서 기본 배경 지식을 학습해 보겠습니다.

보통 서비스는 특정 호스트 OS(Windows, Linux 등) 위에 서비스 운영환경이 구축되어 운영됩니다. 여기서 서비스 운영환경이란, 개발 팀이 만든 소스 코드 및 컴파일된 코드, 그 코드를 구동시키기 위한 환경을 지칭합니다(가령, Nginx, Tomcat, War 등).

과거에는(지금도 많은 서비스들은) 호스트 OS에 운영환경 일부(Nginx, Tomcat, Apache 등)를 설치하고 개발 코드를 여기에 FTP 등으로 업로드(좀 크면 CI로 자동화 정도) 해서 서비스를 운영합니다. 이런 서비스 운영환경은 OS에 의존할 수밖에 없고 개발 코드(가령, war, php 등)는 자신이 실행될 환경에 적응해야 하는 구조가 됩니다. 이를 Mutable infrastructure(변경 가능한 인프라 구조)라고 합니다. 이런 구조에서는 서비스 운영환경이 호스트 OS에 의존적이고 개발 코드와도 일치하지 않는 버전이 될 수밖에 없습니다.

Immutable infrastructure(변경 불가능한 인프라 구조)이라는 개념은 서비스 운영환경이 호스트 OS의 환경 사정에 의해 변경되거나 개발 코드와 어긋나는 것을 막기 위해 도입된 개념입니다. 이 개념에서는 서비스 운영환경을 하나의 이미지(일종의 실행 파일입니다)로 생성하면 다시는 변경하지 않습니다. 즉, 호스트 OS 환경과 상관없는 하나의 서비스 운영환경 이미지로 만들어 버전 관리한다는 의미입니다. 그렇기에 Immutable infrastructure에서는 기존 서비스 운영환경 이미지는 버리고 새 이미지를 쓰는 거죠. 호스트 OS 환경에 상관없이요.

이미지가 배포가 되어 운영(실행) 되면 이를 컨테이너(container)라고 합니다. Windows로 따지면 이미지는 exe 파일 자체이고 컨테이너는 그 exe가 실행된 프로세스라고 보면 됩니다. 이렇게 하면 우리가 만드는 서비스가 더 이상 호스트 OS에 의존하지 않게 되고 언제 어디서나 동일한 운영환경에서 실행되게 됩니다.

그럼 이 컨테이너를 실행하는 환경(플랫폼)은 도대체 뭘까요? 이는 컨테이너를 실행시킬 수 있는 호스트 OS에 설치된 가상머신(Virtual Machine, VM)처럼 보입니다. 하지만 기존의 가상머신 개념과는 좀 다른데요. 기존의 가상머신은 호스트 OS 위에서 Hypervisor를 다루는 VMWare나 VirtualBox등의 가상머신을 설치하고 또 거기에 Guest OS를 설치해서 운영환경을 구축하는 형태입니다. 안정성과 보안성 측면에서는 좋지만 태생이 많이 무겁습니다. 반면, 컨테이너는 호스트 OS 위에 프로세스 단위의 격리환경내에 설치되고 실행되어 성능 손실이 거의 없습니다.

![image](https://user-images.githubusercontent.com/47058441/64939082-006a2e80-d89b-11e9-96d0-468b7444ddca.png)

### 도커

![image](https://user-images.githubusercontent.com/47058441/64939102-0e1fb400-d89b-11e9-8af8-f4c4cb2cd90e.png)

대표적인 도커 이미지. 바다 위(호스트OS)를 고래(도커)가 컨테이너를 싣고 운항하네요.
Immutable infrastructure을 구현한 프로젝트 중 하나가 도커(Docker)입니다. 다양한 호스트 OS 위에 돌아가는 일종의 가상머신 같은 것입니다. 도커를 통해 앞서 설명한 이미지와 컨테이너를 만들고 관리할 수 있습니다. 도커에 의해 만들어진 이미지는 도커 위에서 컨테이너라는 이름으로 실행됩니다. 컨테이너는 언제든 실행, 종료, 파기될 수 있지요.

도커 공식 사이트에서는 도커를 아래처럼 소개합니다.

도커는 개발자와 시스템 관리자가 컨테이너를 사용하여 애플리케이션을 개발, 배포, 실행하기 위한 플랫폼입니다. 리눅스 컨테이너를 사용하여 응용 프로그램을 배포하는 것을 컨테이너화(Containerization)이라고 합니다. 여기서 컨테이너는 새로운 개념이 아니지만 응용 프로그램을 손쉽게 배포하는 데 사용합니다.

리눅스 컨테이너라고 불리는 단순 프로세스 격리 층은 가상머신과 다르게 성능에 거의 손실 없이 운영됩니다. 도커의 컨테이너는 이 기반으로 만들어졌기 때문에 가상머신과는 차원이 다른 정도로 빠르게 실행됩니다.

도커는 리눅스 컨테이너 개념의 장점을 최대한 살려서 서비스 운영환경을 구성한 이미지와 컨테이너를 아주 쉽게 명령어 몇 개만 알면 만들어 쓸 수 있도록 고도로 추상화한 플랫폼입니다.

더욱 자세하게 도커를 학습하고 싶다면 아래 링크의 글을 확인해 보세요.

* 공식 도커 문서 : https://docs.docker.com
* Docker란 무엇인가? http://avilos.codes/infra-management/virtualization-platform/docker/what-is-docker/
* Docker란? : https://nobase-dev.tistory.com/34

### 오래된 버전의 도커 지우기

docker, docker.io 또는 docker-engine과 같은 오래된 버전은 아래 명령으로 지웁시다. 기존 이미지, 컨테이너, 볼륨 및 네트워크를 포함한 콘텐츠들은 /var/lib/docker/ 디렉터리에 보존되기 때문에 이 명령을 안심하고 쓰셔도 됩니다.
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

### 저장소를 사용하여 도커 설치하기

Docker CE를 설치하기 전에 먼저 도커 저장소(repository)를 설정해야 합니다. 그런 후에 저장소로부터 도커를 설치하거나 업데이트할 수 있습니다.

저장소 설정
오래된 버전은 이미 삭제되어 있다고 가정하고 진행합니다.
먼저, apt 패키지를 업데이트합니다.
```
$ sudo apt-get update
```
apt가 HTTPS를 통해 저장소를 사용할 수 있도록 패키지를 설치합니다.
```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
 ```
도커의 공식 GPG 키를 추가합니다.
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88에서 마지막 8자를 검색하여 fingerprint 인식 키가 있는지 확인하십시오.
```
$ sudo apt-key fingerprint 0EBFCD88
```
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
안정화된 저장소로 설정하려면 다음 명령을 사용하세요.
제 서버의 아키텍처가 x86_64(amd64) 임을 감안해야 했습니다.
```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
위 아키텍처가 armhf, ppc65le 390x에 따라 명령에서 arch의 값을 해당 아키텍처로 할당하셔서 실행하면 되겠습니다.

도커 저장소 설정이 완료되었습니다.

Docker CE 설치하기
먼저 apt 패키지를 업데이트합니다.
```
$ sudo apt-get update
```
Docker CE 최신 버전을 설치합니다. 아니면 다음 단계로 가서 특정 버전을 설치하세요.
```
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
참고로 apt-get install 또는 apt-get update 명령으로 버전을 지정하지 않고 설치 또는 업데이트를 하면 항상 최신 버전이 설치되므로 안정성 측면에서 적합하지 않을 수도 있습니다.

만약 특정 버전으로 설치하려면 다음 두 단계로 진행하세요.

a. 저장소에 사용 가능한 버전 리스트를 봅니다.
```
$ apt-cache madison docker-ce
```
```
 docker-ce | 5:18.09.2~3-0~ubuntu-cosmic | https://download.docker.com/linux/ubuntu cosmic/stable amd64 Packages
 docker-ce | 5:18.09.2~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.1~3-0~ubuntu-cosmic | https://download.docker.com/linux/ubuntu cosmic/stable amd64 Packages
 docker-ce | 5:18.09.1~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.0~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.2~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.1~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.0~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.03.1~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
b. 두 번째 열의 버전 문자열을 사용하여 특정 버전을 설치합니다 (예 : 5 : 18.09.1 ~ 3-0 ~ ubuntu-xenial).
```
```
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```
설치된 Docker CE 버전 확인
이제 설치가 완료되었습니다. 잘 설치되었는지 docker 명령을 통해 확인할 수 있습니다. 가장 단순하게는 버전을 보면 되겠습니다.
```
$ sudo docker version
```
Client:
 Version:           18.09.2
 API version:       1.39
 Go version:        go1.10.6
 Git commit:        6247962
 Built:             Sun Feb 10 04:13:46 2019
 OS/Arch:           linux/amd64
 Experimental:      false
 
Server: Docker Engine - Community
 Engine:
  Version:          18.09.2
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.6
  Git commit:       6247962
  Built:            Sun Feb 10 03:42:13 2019
  OS/Arch:          linux/amd64
  Experimental:     false
위처럼 Client와 Server 정보가 제대로 나온다면 설치가 완료된 것입니다. 만약 아래처럼 에러 난다면 도커 소켓 연결에 문제가 있음을 뜻합니다.

```
# sudo docker version
```
Client:
 Version:           18.06.1-ce
 API version:       1.38
 Go version:        go1.10.4
 Git commit:        e68fc7a
 Built:             Wed Sep 26 01:43:33 2018
 OS/Arch:           linux/amd64
 Experimental:      false
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
그럼 아래처럼 해보세요.
```
# sudo systemctl unmask docker.service
# sudo systemctl unmask docker.socket
# sudo systemctl start docker.service
# sudo docker version
```
(정상적으로 나오는지 확인!)
축하합니다. Docker CE를 성공적으로 설치 완료했습니다.


