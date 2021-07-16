# git Tutorial
git을 배우자
## 개념정리
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

### 
