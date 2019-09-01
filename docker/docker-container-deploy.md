# Docker Study
>[Slipp](https://www.slipp.net/wiki/pages/viewpage.action?pageId=41582977)에서 Docker&Kubernates를 스터디 하며 정리
>참고한 책 : [도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문](http://aladin.co.kr/shop/wproduct.aspx?ItemId=186771560)

## 1. 컨테이너 실전 구축 및 배포
### 1.1 컨테이너와 애플리케이션의 비중
* 도커의 공식문서에는 `컨테이너는 하나의 관심사에만 집중해야한다`라는 입장이 있다. 이는 1개의 컨테이너가 1개의 프로세스라는 말은 아니므로 역할을 적절히 분배하여 컨테이너를 복제하거나 역할을 합쳐 하나의 컨테이너로 구성하여 효율적인 운영을 해야한다.
### 1.2 도커의 이식성
* 커널 및 아키텍처 차이
    도커의 컨테이너형 가상화 기술은 호스트 운영체제와 커널 리소스를 공유한다. 때문에 CPU 아키텍처, 운영체제 등에 따라서 빌드한 이미지가 실행되지 않을 수 있다.
> 한번 만들어진 이미지에 대한 설명이다. 또한 도커는 지원 플랫폼을 현재 넓혀가고 있다.
* 라이브러리와 동적 링크 문제
    프로그램을 빌드하여 의존성있는 모든 라이브러리를 한번에 묶어 패키징하면 용량이 커지지만 어느 환경에서나 필요한 라이브러리가 모두 있기때문에 문제되지 않는다.
    프로그램이 커지기 떄문에 실행되는 곳에 라이브러리가 없이 동적으로 링크하면 제대로 실행되지 않을 수 있는 문제가 발생하기 때문에 이를 고려해야한다.
* 도커에 잘 맞는 애플리케이션을 위해 애플리케이션 실행 시 재사용성과 유연성을 가질 수 있도록 환경변수 등을 통해 제어하도록 하고 컨테이너 실행 시 주입할 수 있는 방식으로 작성한다
    - 실행 시 인자를 사용
    - 설정 파일 사용
    - 애플리케이션 동작을 환경변수로 제어
    - 설정 파일에 환경 변수 포함
### 1.3 퍼시스턴트 데이터를 다루는 방법
* 데이터 볼륨
    도커 내부 디렉터리를 호스트 디렉터리와 공유하고 재사용 할 수 있도록 한다.    
    ```
    docker container run -v 호스트_디렉터리:컨테이너_디렉터리 리포지터리
    ```
    호스트의 위치에 종속적이기 때문에 권한 문제 혹은 데이터가 다른 사용자에 의해 조작될 수 있는 문제가 발생할 수 있다.
* 데이터 볼륨 컨테이너
    데이터를 가지는 컨테이너를 볼륨으로 만들어 다른 컨테이너에 공유하는 방식으로 컨테이너 간에 데이터를 공유한다.
    `호스트의 스토리지에 데이터 볼륨 컨테이너가 공유한 디렉터리 역시 저장된다.` 하지만 /var/lib/docker/volumes/ 아래에 위치하여 영향을 최소화 하고 데이터 볼륨 컨테이너가 직접 볼륨을 다뤄주므로 호스트 디렉터리에 대한 별도 관리가 필요 없다.
    > 애플리케이션과 데이터 결합이 느슨해지므로 데이터 볼륨 컨테이너만 교체하여 활용할 수 있다.
* 데이터 익스포트 및 복원
    데이터 볼륨은 같은 도커 호스트로 범위가 제한되기 때문에 다른 도커 호스트로 컨테이너를 이전하는 경우 데이터를 익스 포트하여 호스트로 꺼네야 한다.
    > 여러 호스트에 걸친 데이터 이식 면에서는 개선의 여지가 있다고 함
    
