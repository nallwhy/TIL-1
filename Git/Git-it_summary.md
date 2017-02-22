
# Git-it 간략 요약

```
// 버전 확인
$ git --version

// 이름/이메일 설정
$ git config --global user.name "hee"
$ git config --global user.email "hee940727@gmail.com"

// github 유저이름 설정
$ git config --global user.username "heelog"
 
// 변경사항 상태 확인
$ git status
 
// 변경사항 보기
$ git diff
 
// 변경사항 추가
$ git add file.txt

// 짧은 메시지를 포함한 변경사항 커밋
$ git commit -m "input message"

// 리모트 연결 추가
$ git remote add origin https://github.com/heelog/hello-world.git

// 리모트에 url 설정
$ git remote set-url origin https://github.com/heelog/hello-world.git

// 변경 푸시
$ git push origin master

// 리모트 주소 보기
$ git remote -v

// 클론 (repository 다운로드)
$ git clone https://github.com/heelog/patchwork.git

// 원본 repo에 연결
$ git remote add upstream https://github.com/heelog/patchwork.git

// 새 브랜치 생성
$ git branch branchname

// 브랜치 이동
$ git checkout branchname

// 브랜치 생성 후 그 브랜치로 이동
$ git checkout -b branchname

// 브랜치 리스트 보기
$ git branch

// 브랜치 이름 변경
$ git branch -m newname

// 리모트 브랜치로부터 변경 풀해오기
$ git pull remotename branchname

// 풀해오기 전에 리모트의 변경들 보기
$ git fetch --dry-run

// 현재 브랜치에 어떤 브랜치 머지하기
$ git merge branchname

// 브랜치 삭제
$ git branch -d branchname

// 리모트 브랜치 삭제
$ git push remotename --delete branchname

// 리모트 브랜치 풀
$ git pull remotename branchname
```
