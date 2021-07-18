# git Tutorial
git을 배우자
## 개념정리

### origin
원격 저장소(remote)의 이름 default로 origin이라는 원격 저장소 이름으로 생성된다.
### origin/HEAD
**HEAD**는 커밋을 가르키는 참조 포인터 개념이다. 새로운 커밋은 부모 커밋을 기반으로 새로운 커밋을 생성하는 것이고 HEAD는 부모 커밋을 가리키고 있다. (처음 깃을 설치하고 처음 커밋할 땐 HEAD 없음)
origin/HEAD는 특정 리모트 브랜치의 현재 생성된 커밋을 가리키고 있다.
**git branch -r** 입력시 origin/HEAD -> origin/develop 등으로 나타나는 것을 확인할 수 있다. 이는 원격 저장소에 있는 develop이라는 브랜치를 origin/HEAD가 가리키고 있다는 뜻이다.

## 당황당스러운 상황들 정리
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

### 작업 도중 완료 되지 않았는데 다른 브랜치에 작업 요청이 들어온 상황
git stash로 현재 작업중이던 unstaged 상태의 파일들을 잠시 숨겨놓을 수 있다.

git stash list : stash된 목록

git stash pop : 숨겨놓은 파일 다시 꺼내고 stash list에서 삭제

### 다른 작업자 또는 다른 브랜치에서 작업한 커밋을 가지고 와야될 때
git cherry-pick
