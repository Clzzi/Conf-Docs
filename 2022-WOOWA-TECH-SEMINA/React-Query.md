# 우아한 테크 세미나 - React Query

### 목차

1. [FE상태관리 대하여](##-FE상태관리에-대하여)
2. [배민주문팀이 프로덕트를 보며 가진 고민](##-배민주문팀이-프로덕트를-보며-가진-고민)
3. [React Query 살펴보기](\##-React-Query-살펴보기)

## FE상태관리에 대하여

- 상태관리하면 떠오르는 것 FE생태계에서 상태관리라 하면 먼저 떠오르는것은 라이브러리(redux, recoil, mobx)등
- 상태란? **주어진 시간**에 대해 시스템을 나타내는 것으로 언제든 변경될 수 있는 저장된 데이터를 의미, **개발자입장**에선 관리해야 하는 데이터들을 의미
- 모던 웹프론트엔드 개발 UI / UX 중요성이 커지고 규모가 늘어남에따라 수행해야하는 역할이 늘어남 → 관리하는 상태가 많아짐
- 모던 웹 프론트엔드 생태계에서 상태관리는 어떻게되었는가? 프로덕트가 커지면 상태관리하는것도 빡세지는데 상태들은 시간에 따라 유연하게 변경될 수 있고 React에서는 단방향 바인딩으로 Props Drilling이슈도 있게됨, 이것들을 Redux, MobX 같은 상태관리 lib를 활용해 해결

## 배민주문팀이 프로덕트를 보며 가진 고민


> 배민에서 주문을 처리하는 거의 모든 화면은 웹뷰를 활용하여 처리함

- 문제점1 기존 주문 페이지를 관리하는 인원은 3명에 과도한 MSA구조로 인한 레포지토리 분산때문에 레거시코드에 대한 고민이 많았음 → 레거시를 청산하면서 레포를 합치고 신기술 도입하면서 FE아키텍처를 통합함
- 문제점2 기존 Redux Store에서 API통신, 상태관리를 모두 담당하는것이 맞는지, 반복되는 isFetching, isError상태가 필연적인지, 비슷한 구조의 보일러플레이트 사용이 정말 최선인지등의 문제점도 있었음
- 문제점3 배민 주문FE 특성상 사장과 유저들간의 서버 데이터 업데이트 문제가 있는데, 이는 알림이 있지 않는이상 바로 확인할 수 없고, 해당 주문건에 관심이 없어진다면 out of date가 날 확률이 높음
- ⇒ 이런 문제점 + 고민들로 인해서 주문 FE팀은 React Query이라는 솔루션을 쓰기로함

## React Query 살펴보기

[React Query](https://react-query.tanstack.com/)

> Fetch, cache and update data in your React and RN applications all without touching any “global state”

장점

- 캐싱 용이
- 데이터 업데이트시 자동 Get 수행
- 데이터 stale일 때 자동 GET
- 무한 스크롤 지원
- 동일 데이터가 일정시간에 여러번 요청될 때 한번만 요청됨
- 비동기 과정을 선언적으로 관리가능
- 여러 인터페이스를 옵션으로 제공해 DX가 뛰어남

단점

- 프로젝트 규모가 작다면 배보다 배꼽이 더 커지는 현상
- 컴포넌트가 비대해짐

핵심 컨셉

- [Queries](https://react-query.tanstack.com/guides/queries)
- [Mutations](https://react-query.tanstack.com/guides/mutations)
- [Query Invalidation](https://react-query.tanstack.com/guides/query-invalidation)

Caching 과 Syncronization 배경이 된 기술?
![기술](https://user-images.githubusercontent.com/62810965/157586566-630e27a4-72c7-4526-9517-2596acb30504.png)

Query 흐름
![흐름1](https://user-images.githubusercontent.com/62810965/157586481-1dafe815-9ec0-403f-b0cb-6aa8347aa38b.png)
![흐름2](https://user-images.githubusercontent.com/62810965/157586489-85e1053a-6ec2-4216-805f-12213df79a04.png)