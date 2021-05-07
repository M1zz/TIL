
git의 upstream과  origin

### upstream과 downstream
사전적인 의미를 파악해보면, `upstream`이 상류, `downstream`이 하류입니다.  
물이 상류에서 하류로 흐르듯이 pull 하는 주최가 `downstream` 당하는 쪽이 `upstream` 입니다.
내 레포에서 다른 레포를 풀 해서 작업하고 있다면 다른 레포가 업스트림이 되겠군요!

```
otherRepository(upstream) -> (git pull) myRepository(downstream)
``` 

*이미지 출처 : https://aktiasolutions.com/upstream-kanban-business-agility/*

### upstream과 origin
그렇다면 업스트림과 오리진은 어떤 차이가 있는지 정리 하겠습니다.  
보통은 다른 레포를 포크해서 내 레포를 만들어 놓고 내 레포를 클론, 풀 해서 작업합니다.  
이때 가장 근본이 되는 레포를 upstream 이라고 부릅니다.  
내가 fork해서 가져온 레포를 origin이라고 부릅니다.  
그러므로 내가 로컬에 클론 해 놓고 작업할 때, origin/master에서 가져올지, upstream/master에서 가져와야 할지 
생각해서 가져와야겠죠?

``` 
otherRepository(upstream) -> (fork) myRepository(origin)  
otherRepository(upstream) -> (git pull) myRepository(local)
myRepository(origin)  -> (git pull) myRepository(local)
``` 
