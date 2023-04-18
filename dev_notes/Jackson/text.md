# Bash 기초 사용법
## 이미지 첨부하는 방법
```
![사진명](경로 || url)
```
![me](https://github.com/mcb-dataai/blog/blob/dev_notes/jackson/dev_notes/Jackson/img/mcb_logo.jpg)\
주의 : 경로나 주소에 사진이 있어야한다.


## 로컬을 원격에 업데이트 하는 방법

### git add
작업 디렉토리 상의 변경 내용을 스테이징 영역에 추가하기 위해서 사용하는 명령어
- 사용법

작업 디렉토리의 변경내용 일부만 스테이징 영역에 넘기는 방법
```bash
$ git add 파일 || 디렉토리 경로
``` 

현재 디렉토리의 모든 변경 내용을 스테이징 영역으로 넘기는 방법, '.'을 인자로 사용한다.
```bash
$ git add . 
```

작업 디렉토리내의 모든 변경 내용을 스테이징 영역으로 넘기는 방법 -A 옵션을 사용한다.
```bash
$ git add -A 
```


### git commit
파일 및 폴더의 추가/변경 사항을 저장소에 기록하기 위한 명령어
- 사용법

반드시 -m "~~" 을 통해 커밋 메세지를 작성한다.
```bash
$ git commit -m "커밋 메세지"
```


### git push
로컬 브랜치(local branch)를 원격 저장소(remote repository)로 푸시할 때 사용하는 기본 명령어
- 사용법

```bash
$ git push 
```


### git pull
원격에서 수정한 파일을 로컬에 업데이트하는 명령어
- 사용법

```bash
$ git pull
```

## .gitconfig에서 사용자 계정 맟 alias 설정등을 조절하는 방법
- python의 사용자 정의 함수 처럼 명령어를 설정하는 방법

vim으로 편집기에 들어간다.
```bash
$ vim ~/.gitconfig
```
원하는 명령어 이름과 동작을 작성한다.
```bash
$ st = status
```
이방법으로 아래의 코드를 통해 add, commit, push를 한번에 하는 명령어를 작성 할 수 있다.
```bash
$ cmp = "!f() { git add -A && git commit -m \"$@\" && git push; }; f"
```
주의사항 : cmp를 실행 시키기 위해 git cmp를작성하고 뒤에 "커밋 메세지"를 작성해야한다.

## 작업한 명령어 저장하기
vim을 통해 작업한 내용을 저장하기 위해서는 esc를 누른후 ```:wq!``` 를 입력해 저장한다.
