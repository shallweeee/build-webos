# 도커에서 webOS 빌드

도커에서 webOS를 빌드하기 위한 환경 생성 및 webOS 빌드.

<mark>주의</mark>:  
처음부터 빌드하면 상당히 많은 시간과 컴퓨팅 자원이 필요.  
빌드가 필요 없으면 [공식 이미지](https://github.com/webosose/build-webos/releases) 사용을 권장.

## 파일

- scripts/Dockerfile  
  도커 이미지 생성용 도커파일. ubuntu 22.04 기준.

- scripts/docksh  
  docker 명령은 build-webos:<prerequisites.sh 버전>의 도커 이미지 생성,  
  그 외는 도커상에서 명령어 실행 (source oe-init-build-env 포함).  
  아래 사용 예제 참고.

## 라즈베리파이 4용 이미지 생성

```bash
$ git clone https://github.com/shallweeee/build-webos.git
$ cd build-webos
# 도커 이미지 생성
$ ./scripts/docksh docker
# RPI4 64비트용으로 설정 (옵션 값은 다음 항목 참고)
$ ./scripts/docksh ./mcf -b 5 -p 4 raspberrypi4-64
# webos-image 빌드
$ ./scripts/docksh bitbake webos-image
```

## 병렬 옵션 찾기

CPU 가 여러 개인 환경에서 다음 옵션을 조절하여 빌드 속도를 높일 수 있다.  
(물론 premirror 나 sstate-cache 사용이 더 빠름)

| mcf 옵션 | Yocto 변수        | 설명                                                        |
| :------: | ----------------- | ----------------------------------------------------------- |
|    -b    | BB_NUMBER_THREADS | 동시에 처리하는 BitBake 태스크 개수                         |
|    -p    | PARALLEL_MAKE     | 한 태스크에서 동시에 컴파일하는 파일 개수 (make 의 -j 옵션) |

CPU 개수(`grep processor /proc/cpuinfo | wc -l`)가 8일 때,  
[webOS OSE 빌드 가이드](https://www.webosose.org/docs/guides/setup/building-webos-ose/#appendix-a-how-to-find-the-optimum-parallelism-values)에서는 절반인 -b4 -p4 를,  
[Yocto 문서](https://docs.yoctoproject.org/ref-manual/variables.html)에서는 같은 값인 -b8 -p8 를 권장.  
테스트 환경에서는 swap 발생 상태를 기준으로 -b5 -p4 로 설정.  
각자의 환경에 맞춰 설정.

## 참고

- [원본 README](README-webos.md)

- [Building webOS OSE](https://www.webosose.org/docs/guides/setup/building-webos-ose/)
