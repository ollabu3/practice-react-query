# practice-react-query

[React Query: Server State Management in React](https://www.udemy.com/course/learn-react-query/?couponCode=REACT-QUERY-GITHUB)

- [쿼리 생성 및 로딩/에러 상태](#section-01-쿼리-생성-및-로딩에러-상태)
  - [isFetching vs isLoading](#isfetching-vs-isloading)
  - [React Query Dev Tools](#react-query-dev-tools)
  - [staleTime vs cacheTime](#staletime-vs-cachetime)

# Section 01. 쿼리 생성 및 로딩/에러 상태

> react-query는 기본적으로 sucess아닐 시 세번까지 요청(조정할 수 있다)

## isFetching vs isLoading

### isFetching

- 비동기 쿼리가 해결되지 않았음을 의미

### isLoading

- **isFetching** 하위 집합
- 쿼리 함수가 아직 해결되지 않음(데이터를 가져오는 중), 캐시된 데이터 X

> isFetching, isLoading은 pagination때 구분 잘해야 함

## React Query Dev Tools

- 쿼리 키로 쿼리를 표시해줌
  - 쿼리의 상태를 알려줌(active, unactive, stale)
  - 마지막으로 업데이트된 타임스탬프 알려줌
- 결과 데이터, 쿼리를 볼수 있음
- testing을 위한 것이며, **NODE_ENV === production** 일 시 보이지 않음

## staleTime vs cacheTime

### Stale Data (만료된 데이터)

- 데이터 리페칭(refetching)은 만료된 데이터에서만 실행되며, 데이터 리페칭 실행에는 만료된 데이터 외에도 여러 trigger가 있음
  - ex) 컴포넌트가 다시마운트되거나 다시 포커스 될 때 (단, 만료된 데이터일때만 리패칭됨)

### staleTime

- 데이터가 만료될 수 있는 최대 시간, 즉 **데이터가 만료됐다고 판단하기 전까지 허용하는 시간**
- ex) 10초까지 만료된 데이터 가능 -> staleTime = 10 seconds

### staleTime default가 0인 이유 ?

> 데이터는 항상 만료 상태이므로 서버에서 다시 가녀와야 한다고 가정하기 때문에

### cacheTime

- staleTime === refetching
- cache는 나중에 다시 필요할 수 있는 데이터용
  - 특정 쿼리에 대한 활성 useQuery가 없는 경우 => 해당 데이터는 **cold storge** 로 이동
  - 구성된 cacheTime이 지나면 캐시의 데이터가 만료됨 (유효시간 기본값 5분)
  - cacheTime이 관찰하는 시간의 양은 특정 쿼리에 대한 useQuery가 활성화된 후 경과한 시간
    - 즉, 페이지에 표시되는 컴포넌트가 특정 쿼리에 대해 useQuery를 사용할 시간
  - 캐시가 만료되면 가비지 컬렉션이 실행되고, 클라이언트는 데이터를 사용할 수 X
- 데이터가 캐시에 있는 동안에는 fetching 할 때 사용될 수 있음
  - ex) 데이터가 fetch 될 때 이전 데이터가 존재하면 화면이 빈 페이지로 안보일 수 있음
