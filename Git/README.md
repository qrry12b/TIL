# Git

빈 Git 저장소를 생성하거나 기존 저장소를 다시 초기화 합니다
```
git init
```

작업 트리의 상태를 표시합니다. (new file, modified, untracked 등..)
```
git status
```

브랜치 만들기
```
git branch <branchname>
```

브랜치 전환하기
```
git checkout <branchname>
```

브랜치 만들면서 전환하기
```
git checkout -b <branchname>
```

마지막 커밋의 작성자 변경 (계정이 복수 있을 경우)
```
git commit --amend --author="user_name <user_email>"
```

다른 브랜치의 커밋 병합하기
```
git merge <branch_name>
```

다른 브랜치를 하나의 커밋으로 병합하기
```
git merge --squash <branch_name>
```

git 브랜치 이름 변경   
(git branch -m master main을 입력하는 것과 master 브랜치에서 git branch -M main 을 입력한 것의 결과는 동일합니다)
```
git branch (-m | -M) [<oldbranch>] <newbranch>
```

git init 기본 브랜치를 master에서 main으로 변경합니다
```
git config --global init.defaultBranch main
```

커밋 author 설정하기
```
git config user.name "user_name"
git config user.email user_email
```

커밋 전역 author 설정하기
```
git config --global user.name "user_name"
git config --global user.email user_email
```

커밋 히스토리 조회
```
git log
```

## REF
* [docs](https://git-scm.com/docs)