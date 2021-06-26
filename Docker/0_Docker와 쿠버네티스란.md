

### Docker란

- **컨테이너 기반의 오픈소스 가상화 플랫폼.** 

- 이미지를 컨테이너에 띄우고 실행하는 기술.

- 소프트웨어를 **컨테이너(Container)**라는 표준화된 유닛으로 패키징하고, 이 컨테이너에는 라이브러리, 시스템도구, 코드, 런타임 등 소프트웨어를 실행하는 데 필요한 모든 것들이 포함된다.

  

  #### **컨테이너(container)란?**

  - **애플리케이션, 애플리케이션을 구동하는 환경을 격리한 공간.**

  - 기존 `VirtualBox`같은 *가상머신*은 host OS 위에 guest OS 전체를 가상화하여 사용하는 방식이다. 여러가지 OS를 사용할 수 있다는 장점이 있지만 무겁고 느리다는 단점이 있다.

  - 이후에 이를 개선하기 위해 *CPU 가상화기술(HVM)을 이용한 KVM(Kernel-based VM)과 반가상화(Paravirtualization)방식*의 `Xen`이 등장했다. 전체 OS를 가상화하지 않기 때문에 이전에 비해 성능이 향상되었다.

  - 어쨌든 추가적인 OS를 가상화하는 방식은 성능문제가 있었다. 이를 개선하기 위해 프로세스를 격리하는 방식이 등장하였다.

    ![img](https://media.vlpt.us/images/ckstn0777/post/79726ae8-55ec-4f5e-88a3-ef976bc97c43/image.png)

    - **가상화**란?
      - 물리적인 HW 장치를 논리적인 객체로 추상화하는 것. 즉, 하나의 HW를 여러개처럼 동작시키거나 반대로 여러개의 장치를 묶어 하나의 장치인 것 처럼 사용자에게 제공하는 것. 

![img](https://blog.kakaocdn.net/dn/bOopWd/btqyAcmVho9/kZW4AzFrGuuIX4Mjb6Smtk/img.png)



#### 이미지(image)

- 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있는 것. 
- immutable하다.





### 쿠버네티스란?

- 구글에서 만든 컨테이너 오케스트레이션 툴

- **도커를 기반으로 컨테이너를 관리하는 서비스**. (컨테이너 스케줄링 문제로 컨테이너 관리 도구가 주목을 받음.) 
- 여러 host(= node in k8s)를 묶어 클러스터를 구성하고, 컨테이너를 적절한 위치에 배포하고, 컨테이너가 죽으면 자동으로 복구하며, 필요에 따라 컨테이너를 매끄럽게 추가,복제,업데이트,롤백 등을 할 수 있다.
  - **쿠버네티스 클러스터**란?
    - **컨테이너화된 애플리케이션을 실행하기 위한 일련의 노드 머신**. 쿠버네티스를 실행중이라면 클러스터를 실행하고 있는 것이다.

![img](https://raw.githubusercontent.com/1ambda/1ambda.github.io/master/assets/images/infra-kubernetes/intro/physical-layout.png)

하나의 k8s클러스터는 하나의 master와 여러개의 node로 구성되어 있다. 개발자는 `kubectl`을 이용하여 master에 명령을 내리고, node를 관리한다. 사용자는 node에 접속해 서비스를 이용한다.





----

### 참고자료

- https://aws.amazon.com/ko/docker/

- [도커 관련 문서를 모아놓은 깃 레파지토리](https://github.com/remotty/documents.docker.co.kr)

- https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html

- [도커와 쿠버네티스 간단 비교](https://wooono.tistory.com/109)

- [가상화란 무엇인가](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)

- [Window에서 도커 설치하기](https://goddaehee.tistory.com/251)

- https://boying-blog.tistory.com/29

- [쿠버네티스 소개](https://www.popit.kr/kubernetes-introduction/)

- https://www.redhat.com/ko/topics/containers/what-is-a-kubernetes-cluster