# 원격저장소

## 원격저장소 생성
- GitHub에서 `Create a new repository`를 통해 생성

<br>

## 원격저장소 설정
```bash
git remote add origin <url>
```
: 주소는 GitHub에서 확인 가능

```bash
git remote -v
```
: 지정된 원격저장소 확인, 
cf) v : verbose


```bash
git remote rm origin
```
: 원격저장소 삭제

<br>

## push
```bash
git push origin master(branch이름)
```
: 원격저장소로 로컬저장소의 커밋된 변경사항을 올림(인증 필요)

<br>

## pull
```bash
git clone <url>
```
: pull하기 위해서는 먼저 clone을 통해 원격저장소를 복제해와야함

```bash
git pull origin master(branch이름)
```
: 원격 저장소로부터 변경된 내용을 받아와 이력을 병합

<br>

## gitignore
- 버전관리와 상관없이 외부에 공개하지 않을 파일을 git이 무시하도록 하는 것
- git 저장소에 .gitignore 파일을 생성 후 버전관리를 하지 않을 파일을 기입(적용되면 회색으로 바뀜)
- 이미 커밋된 파일을 삭제해야 .gitignore 적용 가능
- [gitignore.io](https://gitignore.io)  : 일반적으로 ignore하는 파일 알 수 있는 사이트
