# Branch
> ## 독립적인 작업흐름을 만들고 관리
<br>

## 명령어
- 브랜치 생성
```bash
git branch <branch_name>
```
- 브랜치 이동
```bash
git checkout <branch_name>
```
- 브랜치 생성 + 이동
```bash
git checkout -b <branch_name>
```
- 브랜치 목록 보기
```bash
git branch
```
- 브랜치 삭제
```bash
git branch -d <branch_name>
```
<br>

## Merge
> ### 각 branch에서 작업한 이력 병합
<br>

### 1. fast forward
기존 master branch에 변경사항 없어 단순히 앞으로 이동
```bash
(master) $ git checkout -b feature/test

(feature/test) $ touch a.txt
(feature/test) $ git add a.txt
(feature/test) $ git commit -m '~'

(feature/test) $ git checkout master
(master) $ git merge feature/test
#주가 되는 master branch로 와서 feature/test branch를 병합
#병합 후 feature/test branch 삭제
```

### 2. merge commit
기존 master branch에 변경사항이 있어 병합 시 커밋 발생

master branch와 feature/test branch를 각각 commit 후 merge

### ※ merge conflict
각각의 branch에서 작업한 파일이 겹칠 경우 merge 시 conflict 발생
- conflict 발생한 파일을 원하는대로 수정하고 git commit
- 자동으로 작성된 커밋 메시지를 확인
- `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료( w : write, q : quit )
```bash
git log --oneline --gragh
```


<br>

## Git flow
> ### Git branch의 활용 전략

### 대표 전략
- master(main) : 배포 가능한 상태의 코드
- develop(main) : feature branch로 나뉘거나 발생된 버그 수정, 개발 이후 release branch로 분기
- feature branches(supporting) : 기능별 개발 브랜치, 기능이 반영되거나 드랍되는 경우 브랜치 삭제
- release branches(supporting) : 개발완로 이후 QA/Test 등을 통해 얻어진 다음 배포 전 minor bug fix 등 반영
- hotfixes(supporting) : 긴급하게 반영해야하는 bug fix, 현재 버전 유지보수를 위한 것

#### cf) 커밋메세지 작성법
1. 제목과 본문 한 줄 띄우기
2. 제목은 영문 50자 이내
3. 제목 대문자로 시작
4. 제목에 `.`금지
5. 제목은 `명령문`
6. 본문은 영문 72자 마다 줄바꾸기
7. 본문에 `Why`에 대한 내용 쓰기

### GitHub Flow Model
#### 1. Shared Repository Model
: repository owner가 상대를 collaborator로 등록하고 저장소를 공유하는 방식(push 권한 부여)

#### 2. Fork & Pull Model
: collaborator 등록 없이 contributor가 repository owner에게 Pull Request를 보내어 오픈소스에 참여하는 방식(push 권한 X)

repository를 fork하여 내 저장소로 복제하여 작업 → 복제한 저장소에 커밋 후 PR 보내기 → owner가 merge 고려