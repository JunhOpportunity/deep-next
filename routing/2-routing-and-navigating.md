# 라우팅 및 탐색 작동 방식

1. 프리패칭 : Link 태그 또는 router.prefetch() 를 사용하여 경로를 미리 가져올 수 있다.
2. 캐싱 : Router Cache 라는 메모리 내 클라이언트 측 캐시가 존재하는데, 미리 가져온 로드된 컴포넌트가 캐시에 저장된다. 따라서 최대한 캐시를 재사용하면서 서버 요청과 전송을 최소화 하며 성능을 향상시킨다.
3. 부분 렌더링 : 클라이언트에서 리렌더링 할 때 공통적으로 사용되는 세그먼트는 유지된다. 이를 통해 페이지 탐색 시에 전체 페이지가 다시 렌더링 되는 현상을 막을 수 있다.
4. 기본적으로 뒤로 가기 또는 앞으로 가기 했을 때 이전 스크롤 위치를 유지한다.