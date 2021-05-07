### 깃 개념 정리
깃을 알고 있다고 생각하고 쓰면서, 평소와 다른 문제가 일어나면서 깃에 대한 개념이 부족하다고 생각되어 찾아 본 내용들을 정리 해놓았습니다.  
자세한 내용은 블로그 보다는 [Git book](https://git-scm.com/book/ko/v2)정독하는 것을 추천드립니다.

### 기본 사용 플로우 
리모트에 있는 소스코드를 받거나(clone), 새로운 레파지토리를 만듭니다. 변경사항을 저장(commit)하고, 리모트에 반영(push)할 수 있습니다.  
이미 존재하는 소스코에서 독립적으로 작업을 진행해야 하는 경우에는 새로운 브랜치(branch)를 생성해 작업을 해야합니다. 새로운 브랜치를 생성하는 방법은 깃 플로우를 검색해보셔야 합니다.  
작업이 완료 된 후에, 갈라져 나온 origin branch에 merge되거나 rebase를 한다.  

### Merge and Rebase
mrege는 기존 존재하던 브랜치에 병합(merge)하는 기능이다. 기존 브랜치에 작업 내용이 없다면, fast-forward merge를 이용해 병합을 하거나
기존의 브랜치에도 작업 내용이 있다면, 3-way-merge 방식으로 각각의 브랜치의 작업 내용을 남겨놓고 하나의 브랜치로 병합한다.
>master 에서 slave를 merge해주면, master브랜치로 slave가 병합된다.   

rebase는 커밋 내역을 재정렬 해서 하나의 브랜치로 합쳐준다. 결과적으로는 한 갈래의 브랜치를 만들어준다. rebase해준 최신의 커밋으로 정렬되고 checkout 되어있던 브런치의 커밋이 먼저 정렬된다.

>slave 에서 master를 rebase해주면, master브랜치로 slave가 병합된다.

### Stash
아직 마무리 하지 않은 작업이 있지만, 다른 작업을 해야해서 변경사항을 저장해야 할 때는 어떻게 해야할까? 커밋해서 저장하기에는 내키지 않는다. 이럴 때 쓰는 명령어가 Stash 이다. 아직 마무리하지 않은 작업을 스택에 잠시 저장할 수 있도록 하는 명령어이다.  

다음과 같은 파일을 Stash할 수 있다.
- Modified이면서 Tracked 상태인 파일
- Staging Area에 있는 파일(Staged 상태의 파일)

작업중인 상태에서 tash
다른 작업 후 stash apply
적용 후 stash drop

### Reset / Revert
무언가를 과거의 기록으로 돌리기 위해 Reset과 Revert를 사용한다.

reset은 세가지 옵션이 있다. hard, soft, mixed

- hard : 돌아가고자 하는 커밋 이후의 내역은 모두 삭제하고 돌아간다.
  - $ git reset --hard [커밋]
- soft : 돌아가고자 하는 커밋 이후의 내역은 모두 삭제되지만, 바로 다시 커밋할 수 있는 상태(staged)로 돌아간다.
  - $ git reset --sorf  [커밋]
- mixed : 돌아가고자 하는 커밋 이후의 내역이 모두 삭제되고, 파일들이 unstaged로 돌아간다.
  - $ git reset --mixed [커밋]

revert는 원하는 시점으로 상태를 돌려준다. 기존의 이력은 남아있는 상태로 돌려준다.
$ git revert [커밋] 

reset은 로컬 저장소의 커밋이력만 컨트롤 할 수 있다. 그래서 reset으로 돌리더라도 remote와 싱크가 맞지 않아 push 할 수 없다. 그러므로 이미 push를 한 상태라면 revert로 돌려 remote에 다시 push하자.

### Cherry pick
브랜치에서 작업한 내용 중 중간에 잘못된 수정사항이 있어서 전체를 Push할 수 없을 때가 있었다. reset으로 잘못 된 수정사항까지 돌린 후 다시작업하기에도 막막할 때 사용할 수 있다.
새로운 브랜치를 따서 다른 브랜치에 있는 커밋 내용만 가져올 수 있다. 명령어 그대로 체리 픽, 나에게 필요한 것 만 취한다.
사용방법은 `$ git cherry-pick [커밋]`.
잘 활용하면, 열심히 작업한 브랜치의 내용을 다 버릴 필요없이 살릴 수 있다.

참고 : https://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html
