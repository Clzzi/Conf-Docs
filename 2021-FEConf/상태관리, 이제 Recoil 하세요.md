## 상태관리, 이제 Recoil 하세요.

### 목차
1. [Recoil 선택 배경](#Recoil-선택-배경)
2. [Recoil 사용 방법](#사용-방법)
3. [Selector](#Selector)
4. [(번외)초기 설계시 고려할 점](#초기-설계시-고려할-점)

### Recoil 선택 배경
- 상태관리 라이브러리에 대한 고민
	- 새로운 기술을 도입할 수 있는 절호의 기회
	- 무조건 빠르게 개발해야 한다
	- 최대한 네이티브 앱스럽게(?) 만들자
	- 갑자기 라이브러리 운영이 중단된다면?
	- 서비스가 폭발적으로 성장할 것만 같은 착각
	- 힙해야함

Redux? => 여전히 많이 사용하고 익숙하지만 귀찮고 번거롭다..  
Mobx? => 쉽지만 리액트잡지 못하달까..  
Context API? => 복잡성이 커지면 Context 분리 어려움..  
Recoil? => 페이스북이 만들었다고? 간결하다!
 
- Recoil 핵심 컨셉
	- React의 **내부 상태**만을 이용한다.
	- **작은 단위의 Atom**으로 관리된다.
	- **Selector라는 순수함수**를 제공한다.
	- 상태가 변경되면 **Atom을 참조하는 Component만 Re-Render** 된다.
	- Facebook 에서 만들었기 때문에 **새로운 React 의 기능과 호환성**이 좋을 것이다.

### 사용 방법
```javascript
import {atom} from 'recoil';

export const countState = atom({
	key: 'countState',
	default: 0,
});
```
```javascript
import {useRecoilState, useRecoilValue, useSetRecoilValue} from 'recoil';
import {modalState} from '../store/atoms';

function test() {
	const [count, setCount] = useRecoilState(countState);
	const readCount= useRecoilValue(countState); // 읽기 전용
	const changeCount = useSetRecoilvalue(countState); // 쓰기 전용
}
```
위와 같이 Hooks 처럼 호출해서 사용한다.

### Selector
- selector 는 Recoil의 순수함수이다.
- selector를 사용하면 상태를 읽고 쓸때 데이터를 가공하거나 유효성 검사를 하는 등 부가적인 로직을 추가할 수 있다.
- selector 내부에서 다른 atom 이나 selector를 참조할 수 있다. (Recoil 에선 구독한다고 표현)
- selectorFamily를 사용하면 매개변수를 받을 수 있다.

```javascript
export const countInputState = selector({
    key: 'countTitleState',
    get: ({ get }) => { // async를 붙이면 비동기처리 가능
        return `현재 카운트는 ${get(countState)}입니다.`;
    },
    set: ({ set }, newValue) => { // 2번째 파라미터 에는 추가로 받을 인자를 나타냅니다.
        set(countState, Number(newValue)); // count atom 수정
    },
});
```

### 초기 설계시 고려할 점
- 꼭 전역으로 관리되어야만 하는가?
	- 간단한건 Hooks와 Props로 충분
- 여러 상태값을 사용 용도에 따라 분류
	- UI 전용 상태 / 폼 데이터 처리 상태 / 서버 데이터 조회용 상태
- 어떤 데이터를 캐싱하고, 실시간으로 반영되어야 하는지 파악
	- 캐싱: 코드성 데이터, 사용자 정보 등 / 주기적: 자주 바뀌지 않는 정보 / 실시간: 주요 데이터
- 데이터가 어느 시점에서 변경되고 어떤 부분에 영향을 주는지 예측