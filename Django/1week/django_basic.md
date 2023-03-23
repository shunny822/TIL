# Django의 이해

## Framework

> 웹 애플리케이션을 빠르게 개발할 수 있도록 도와주는 도구(기본 구조, 규칙, 라이브러리 등)

- 기본적인 구조와 규칙을 제공하기 때문에 필수적인 개발에만 집중할 수 있음

- 여러 라이브러리를 제공해 개발 속도를 빠르게 할 수 있음

- 유지보수와 확장에 용이해 소프트웨어의 품질을 높임

<br>

## django

> Python기반의 대표적인 웹 프레임워크

### django 가상환경 및 프로젝트 생성

: 우리가 평소 사용하는 것은 global 환경이기 때문에 다른 여러 라이브러리나 패키지의 영향을 받을 수 있음. 따라서 가상환경을 만들어 독립적인 공간 확보!

1. 가상환경(venv) 생성
    ```bash
    $ python -m venv venv
    ```
    - 뒤의 venv는 가상환경의 이름, 'venv'로 하는 것이 암묵적 약속


2. 가상환경 활성화
    ```bash
    $ source venv/Scripts/activate
    ```


3. django 설치
    ```bash
    $ pip install django==3.2.18
    ```
    - LTS버전 사용

    - 버전 명시하지 않으면 4.0버전이 설치되니 주의


4. 패키지 의존성 관리
    - 의존성 파일 생성(패키지 설치 시마다 진행)
      ```bash
      $ pip freeze > requirments.txt
      ```
      - 현재 환경에 설치된 패키지 목록을 requirements 포맷으로 출력

      - 패키지는 대소문자를 구별하지 않는 정렬 형식으로 나열됨

    - 생성된 의존성 파일을 이용해 패키지 목록 설치
      ```bash
      $ pip install -r requirments.txt
      ```
      - `requirements.txt`에 있는 내용을 읽어 자동으로 패키지를 설지해줌으로써 해당 프로젝트가 어떤 버전의 패키지를 썼는지 기억하지 않아도 개발환경을 설정할 수 있음
      
      - github에서 프로젝트를 받게되는 사람도 해당 파일이 있으면 가상환경 설정 후 바로 설치가 가능

      - python 버전은 READEME에 별도로 명시하는 것이 좋음

5. .gitignore 파일 생성
    - https://www.toptal.com/developers/gitignore/ 활용(gitignore 설정 서비스)


6. git 저장소 생성


7. django project 생성
    ```bash
    $ django-admin startproject [projectName] .
    ```
    - 현재 위치에 [projectName]이라는 프로젝트 생성


8. django 서버 실행
    ```bash
    $ python manage.py runserver
    ```
    - manage.py와 동일한 경로에서 명령어 진행

9. 기타
    - 서버 비활성화
      ```bash
      $ deactivate
      ```
      또는 'ctrl + c'
    
    - pip를 통해 현재 가상환경인지 확인
      ```bash
      $ pip list
      ```

### VScode에서 python Interpreter 설정

1. ctrl + shift + p

2. 입력창에 'Python: Select Interpreter' 검색 후 클릭

3. 현재 파일의 가상환경 선택

<br>

## 참고

### 가상환경을 사용하는 이유
- 의존성 관리 : 라이브러리 및 패키지를 각 프로젝트마다 독립적으로 사용

- 팀 프로젝트 협업 : 모든 팀원이 동일한 환경과 의존성 위에서 작업하여 버전 간 충돌 방지


### LTS(Long-Term Support)
- 프레임워크나 라이브러리 등의 소프트웨어에서 장기간 지원되는 안정적인 버전을 의미

- 기업이나 대규모 프로젝트에서는 소프트웨어 업그레이드에 많은 비용과 시간이 필요하기 때문에 안정적이고 장기간 지원되는 버전이 필요
