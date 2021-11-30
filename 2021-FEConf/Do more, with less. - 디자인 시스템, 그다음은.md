## Do more, with less. - 디자인 시스템, 그다음은?


### 목차
1. [Toss Design System](#Toss-Design-System)
2. [Framer](#Framer)
3. [Design Syntax Tree](#Design-Syntax-Tree)
4. [Design Lint](#Design-Lint)
5. [마무리](#마무리)


### Toss Design System

이미 오래전 Toss의 `Design System`은 많은 개발자들의 관심을 받고 있었다. Toss의 앱을 보면 정말 잘만들어진 System이라 생각되었다. 직관적, 통일감, 가독성. 뭐 하나 부족한 부분이 없다고 생각했다. 그런데 단순히 잘 만들어졌구나 생각만 하고 그 안쪽은 어떻게 돌아가는지 상상을 하지 못했다. 다른 회사와 같이 하나의 규격을 가지고 디자인이 이루어지며 툴을 이용한 협업이 이루어지는 일반적인 플로우로 상상했다.

**이 영상을 보기 전까지는...**


### Framer

Toss는 피그마나 제플린이 아닌 `Framer`를 사용한다고 소개했다. 해당 툴은 다른 툴과는 다른 장점들을 설명해주었는데, 여기서 가장 눈에 띈 장점은 디자인을 React Component로 만들 수 있는 장점이 있다고 소개했다. 이 장점은 뒤에 나올 **무서운 기술**에 이용된다.

디자인이 컴포넌트로 출력이 가능하기에 개발자들은 컴포넌트를 새롭게 만들지 기존 소스를 사용할지 쉽게 알 수 있게 되었다. 덕분에 불필요한 개발을 줄일 수 있게 되었다고 한다.

세션을 듣는 입장에서 이 부분까지는 그냥 Toss의 자기자랑을 한번 더 하는 느낌을 받았다. 디자인과 개발의 조금 더 효율적인 협업을 소개하는 영상같았다.


### Design Syntax Tree

디자인을 Component로 만들 수 있으니 이 Component를 이용한 Design Tree를 구성했다. 지금까지 디자이너의 결과물을 Tree로 구성할 생각을 할 수 없었다. 디자이너마다 디자인 방식, 레이아웃 구성 방식이 너무나도 다르기에 애초에 불가능하다고 생각했다. 하지만 구축된 시스템을 활용한 디자인은 가능하다. 이 부분부터 소름이 조금씩 돋기 시작했다.


### Design Lint

이 세션의 하이라이트이다. Toss는 자기들의 기술을 충분히 자랑해도 될 정도로 대단한 것을 만들었다.

개발자나 디자이너나 결과물을 만들기 위해서 공통된 디자인이 있는지 확인을 필수로 해야한다. 이용자의 경험을 통일시켜야 하기 때문이다. 개발자는 위 협업 툴로 확인을 할 수 있지만, 디자이너는 코드를 읽는 것에 익숙하지 않기에 확인이 어렵다.

Toss는 이 문제를 해결하기 위해 Lint를 개발했다. 디자이너를 위한 `Design Lint`. 디자이너가 화면을 구성하면 시스템과 부합한 디자인인지 확인을 한다. 바로 위에서 소개한 `Design Syntax Tree`를 활용해서.


### 마무리

이 세션을 들으면서 개인 블로그 및 회사에 **Design System**을 구축하기 시작했다.


[![Do more, with less. - 디자인 시스템, 그다음은?](https://i.ytimg.com/vi/LmLchZ4tCXc/hqdefault.jpg?sqp=-oaymwEcCPYBEIoBSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&amp;rs=AOn4CLDTS4XCN7j1Jr9QORaNF0wffIz-NQ)](https://youtu.be/LmLchZ4tCXc)
