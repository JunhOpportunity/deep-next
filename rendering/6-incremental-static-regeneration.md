### `ISR` (Incremental Static Regeneration)

> 렌더링 하는 주체자가 서버, **주기적**으로 렌더링
> 

SSG와 동일한 원리이지만 일정 주기로 렌더링 되어 페이지를 생성한다는 점에서 다르다

**장점**

- 페이지 로딩 시간(TTV)이 빠르다
- 자바스크립트 필요 없다
- SEO 최적화가 좋음
- 보안이 뛰어남
- CDN에 캐시가 됨
- 일정 주기로 렌더링 된다

**문제점**

- 일정 주기로 렌더링 되지만 여전히 실시간 데이터가 아니다
- 사용자별 정보 제공이 어렵다

→ 위 문제점 해결을 위해 나온 것이 `SSR` 이다

**실제 사용 예시**

- `**fetch` 요청**에서 `revalidate` 를 사용하고 싶다면 `fetch` 요청 내부에 `revalidate` 시간을 적어주면 된다.
`cache: ‘no-store’` 로 설정해도 SSR 처럼 행동한다
    
    ```jsx
    fetch('https://...', { next: { revalidate: 60 } })
    ```
    
- **fetch 요청을 사용하지 않는 상황이나 fetch 요청을 사용하고 싶지 않은 경우** layout 또는 page 파일에 `**revalidate**` 를 적어주면 된다.
`false` : Default Value, SSG로 동작
`0` : SSR처럼 요청 올 때마다 생성
`number` : 특정 초 마다 revalidate
    
    ```jsx
    export const revalidate = 3; // 3초마다 revalidate
    ```