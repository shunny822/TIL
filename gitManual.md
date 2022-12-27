# CLI(Command Line Interface)
## 의미
- 터미널을 통해 사용자와 컴퓨터가 상호작용하는 방식
- 명령 기반의 인터페이스로 그래픽 기반 인터페이스인 GUI와 비교하여 생각!

## 기초 파일시스템 명령어
- pwd(print working directory) : 현재 디렉토리 출력 
  cf) . : 현재 디렉토리, .. : 상위 디렉토리
- cd _디렉토리이름_ : 디렉토리 이동
- ls(list) : 해당 파일의 목록
- mkdir(make directory) : 디렉토리 생성
- touch _파일명_ : 파일 생성
- rm _파일명_ : 파일 삭제
- rm -r _폴더명_ : 폴더 삭제

<br>

# Git 버전관리 기초
## 버전관리란
- 현재 만들고 있는 파일, 프로그램 등을 각 버전별로 저장하고 필요에 따라 과거 모습 그대로 복원 가능
- 버전 간의 차이와 수정한 이유를 메세지로 남길 수 있음

## Git의 버전관리
- 데이터를 파일 시스템의 스냅샷으로 관리하고 매우 크기가 작음
- 파일에 변경사항이 없으면 성능을 위해 저장하지 않음

## 분산버전관리
- 로컬에서도 버전을 기록하고 관리 가능
- 원격 저장소를 활용하여 협업

## 기초 흐름
### git init
- 원하는 폴더에 git 저장소(repository) 생성
- .git 폴더가 생성됨
- 저장소가 생성된 폴더는 git bash에서 (master)표시 보임

### git config
- 커밋하기 위해 사용자 정보 입력
- git config --global user.name "최수현"
- git config --global user.email "sw@naver.com"
- 설정확인 : git config --global -l(엘)

### git add _file_
- working directory상의 변경 내용을 staging area에 추가
- untracked / modified → staged

### git commit -m '_커밋메세지_'
- staged 상태의 file들을 버전으로 기록
- 커밋 메세지는 변경사항을 명확히 나타내도록 작성
- SHA-1 해시를 사용하여 40자 길이의 체크섬을 생성, 고유한 커밋을 표기

### git log
- 현재 저장소에 기록된 커밋 조회

### git status
- 커밋 전 파일의 상태를 알 수 있음
- Tracked : 버전으로 관리되고 있는 파일 상태
  - Unmodified : Nothing to commit, working tree clean
  - Modified : Changes not staged for commit
  - Staged : Changes to be committed
- Untracked : 버전으로 관리된 적 없는 파일 상태
 -Untracked files
