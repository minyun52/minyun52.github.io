# pull request 개념

pull request란 깃 레포지토리에 있는 브랜치에 푸시한 변경 사항들을 다른 사람에게 알리는 것이다. 소스 코드를 수정하고 pull request를 보내면 master 브랜치를 담당하는 사람이 해당 pull request를 확인하고 허락 여부를 판단한다. 그렇기 때문에 브랜치를 merge하기 전에 변경 사항을 다른 사람들과 논의 및 검토를 할 수 있다는 장점이 있다.
예시를 살펴보면, 순서는 
1. 소스 코드 수정을 원하는 레파지토리를 본인 계정으로 fork 시킨다.
![이미지1](/img/1.png)

정가영 연구원의 otes 레포지토리에서 fork를 클릭했다.
![이미지2](/img/2.png)

otes 레포지토리를 내 저장소로 가지고 왔다. 이미 otes라는 레포지토리가 있어서 otes-1이라고 붙였다.

2. fork받아온 레포지토리를 clone 하고 소스를 수정한다.
![이미지3](/img/3.png)

인텔리제이로 이동해서 해당 레포지토리를 clone한다. 그리고 min이라는 브랜치를 만들고 간단한 소스 수정을 진행한다. 최상위 init.jsp에서 pr test라는 주석을 달아주었다.
![이미지4](/img/4.png)

3. commit과 push를 진행한다. 

변경 사항을 나의 레포지토리에 commit과 push를 진행한다.
![이미지5](/img/5.png)

4. 깃허브에서 pull request를 작성한다.

![이미지6](/img/6.png)

pull request의 내용을 입력하고 등록을 해주면 master를 담당하는 사람이 소스 리뷰를 진행하고 pull request를 수락 여부를 결정할 수 있다.
