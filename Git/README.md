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

git 브랜치 이름 변경   
(git branch -m master main을 입력하는 것과 master 브랜치에서 git branch -M main 을 입력한 것의 결과는 동일합니다)
```
git branch (-m | -M) [<oldbranch>] <newbranch>
```

git init 기본 브랜치를 master에서 main으로 변경합니다
```
git config --global init.defaultBranch main
```

## REF
* [docs](https://git-scm.com/docs)