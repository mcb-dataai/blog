# Franz 님의 데브노트

### TODO
 - "How to markdown"

 ### HOW TO Git Bash 101

 1. git bash 설치

 2. git bash -> 저장할 폴더 지정
```bash
mkdir [folderName]
cd [folderName]
```

3. User Name/Eamil 등록
```bash
git config --global user.name "Your Name"
git config --global user.email "Your Mail@example.com"
-- 등록 확인
cat ~/.gitconfig 
```

4. 깃허브 주소 클론 (URL)
```bash
git clone https://github.com/mcb-dataai/blog
```

5. 해당 저장소로 이동 및 원격저장소 Push&Pull 주소 확인
```bash
cd blog
git remote -v
```

6. 내가 사용할 브랜치 등록 or 변경
```bash
--등록
git branch [등록할 브랜치명]
--브랜치 변경(CheckOut)
git checkout [변경 브랜치명]
```
7. 내 로컬 브랜치 원격으로 등록
```bash
--push to remote
git push origin [My Branch]
--branch 연동
git branch --set-upstream-to origin/[My Branch]
```

