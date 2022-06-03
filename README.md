# git Tutorial
git을 배우자


<br/>


## 개념정리

### origin
원격 저장소(remote)의 이름 default로 origin이라는 원격 저장소 이름으로 생성된다.
### origin/HEAD
**HEAD**는 커밋을 가르키는 참조 포인터 개념이다. 새로운 커밋은 부모 커밋을 기반으로 새로운 커밋을 생성하는 것이고 HEAD는 부모 커밋을 가리키고 있다. (처음 깃을 설치하고 처음 커밋할 땐 HEAD 없음)
origin/HEAD는 특정 리모트 브랜치의 현재 생성된 커밋을 가리키고 있다.
**git branch -r** 입력시 origin/HEAD -> origin/develop 등으로 나타나는 것을 확인할 수 있다. 이는 원격 저장소에 있는 develop이라는 브랜치를 origin/HEAD가 가리키고 있다는 뜻이다.
### upstream
로컬브랜치와 연결된 원격저장소
```
git push --set-upstream origin feature_branch
git push -u origin feature_branch
```
위 명령어로 push를 하게 되면 origin과 해당 feature_branch가 upstream, downstream 관계가 형성되어 다음 부턴 git push만 입력하여도 된다.
<br/> 


## 기본 명령어
- git init

- git remote add origin (git repo)

- git fetch

  특정 브랜치에 병합 없이 **FETCH_HEAD**라는 브랜치로 최신 커밋 정보를 가져오기만 한다
  ```
  git fetch origin {branch_name}

  // 커밋 비교하고 병합할지 말지 결정
  git checkout FETCH_HEAD 
  git diff HEAD origin/{branch_name}

  // 병합시
  git checkout {branch_name}
  git merge FETCH_HEAD or git merge origin/{branch_name}
  ```


<br/>

##  TIL about git
### git clone SSH로 받아보기
[SSH 등록 참고](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

<br/>

### 리모트 또는 로컬 브랜치명이 자꾸 대문자로 변환되는 상황
원인은 아마 처음에 대문자로 저장을 해서 (예로 Feature/fix_event_listener) 그럴 가능성이 높다

임시 방편으로 리모트 브랜치 생성하여 push 할 떄는
```
git push origin {local_branch_name}:{remote_branch_name} 
git push origin Feature/fix_event_listener:feature/fix_event_listener 
```
으로 리모트 브랜치라도 소문자로 변환 가능하다.

근본적인 원인 해결법은 ([참고](https://stackoverflow.com/questions/15371866/why-is-git-capitalizing-my-branch-name-prefix)) 이렇게 해야할듯

나중에 해결 후 다시 정리


<br/>

### 작업 도중 완료 되지 않았는데 다른 브랜치에 작업 요청이 들어온 상황
```
git stash
```
현재 작업중이던 unstaged 상태의 파일들을 잠시 숨겨놓을 수 있다.

git stash list : stash된 목록

git stash pop : stash list의 맨 위(stash@{0}) 다시 꺼내고 stash list에서 삭제


<br/>

### 다른 작업자 또는 다른 브랜치에서 작업한 커밋을 가지고 와야될 때
```
git cherry-pick
```
<br/>

### develop 브랜치에서 checkout 한지 너무 오래되었을 때
내 브랜치를 develop 브랜치의 commit들을 다시 merge해서 작업하자


<br/>

### 방금 한 commit에 stage할 파일을 빼먹었을 때
```
git reset --soft HEAD~1
```
방금한 커밋 1개를 다시 stage 상태로 돌릴 수 있다

단, **git reset --hard HEAD~1** 을 하게 되면 방금 한 commit과 함께 기존의 unstaged된 수정파일이 사라지게 되므로 사용에 주의해야한다. 

#### reset과 revert의 차이점 ([참고](https://velog.io/@sonypark/git-reset-vs-git-revert-%EC%B0%A8%EC%9D%B4))

<br/>

### 이미 리모트에 푸시한 브랜치를 rebase 해야할 때
rebase 후 --force로 푸시할 수 있다.
```
git checkout develop
git pull origin develop

git checkout {feature_branch_name}
git rebase develop

git push --force origin {feature_branch_name}
```
- rebase 후 git pull에서 오류 발생하는 이유
```
rebase 전
A - B - C - F (master)
         \
          D - E (feature, origin/feature)
          

rebase 후          
A - B - C - F (master)
         \   \
          \   D' - E' (feature)
           \
             D - E (origin/feature)
             
             
rebase 후 git pull(fetch + merge) 하는 경우             
A - B - C - F (master)
         \   \
          \   D' - E'- M (feature)
           \         /
             D - E - (origin/feature)
```
git pull 하는 과정에서 E, E'가 M commit을 생성하기 위해 non-fast-forward merge하는 과정에서
`Updates were rejected because the tip of your current branch is behind` 오류가 발생하게 된다.
push -f 를 이용하여 강제로 push 해서 오류를 해결할 수 있다.

