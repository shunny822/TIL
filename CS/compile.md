# Compiling

## 컴파일의 과정

> 전처리 - 컴파일 - 어셈블 - 링크

### 전처리, Precompile

- C언어의 `#include`와 같이 실질적인 컴파일이 이루어지기 전 전처리 과정을 지시

- `#include`는 전처리기에게 stdio.h와 같은 내용을 담는 새로운 파일을 생성하여 컴파일할 파일에 포함


### 컴파일, Compile

- 컴파일러를 통해 C코드를 저수준 프로그래밍 언어인 어셈블리어로 컴파일 시행

- 기계어 : 컴퓨터의 CPU가 읽어 실행할 수 있는 0과 1로 이루어진 명령어의 조합

- 어셈블리 : 이러한 기계어를 사람이 조금 더 알아보기 쉽고 컴퓨터를 제어하기 쉽게 만든 언어


### 어셈블, Assemble

- 어셈블러를 통해 어셈블리 코드를 기계어인 오브젝트 코드로 변환


### 링크, Link

- 프로그램이 여러 개의 파일로 이루어진 경우 하나의 오브젝트 파일로 합치는 과정

- 링커를 통해 하나의 파일로 합쳐짐