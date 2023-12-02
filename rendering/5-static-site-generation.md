### `SSG` (Static Site Generation)

> 렌더링 하는 주체자가 서버, **빌드할 때** 렌더링
> 


**장점**

- 페이지 로딩 시간(TTV)이 빠르다 (서버에서 미리 만들어둔 HTML 파일만 가져오면 되기 때문이다)
- 자바스크립트 필요 없다
- SEO 최적화가 좋음
- 보안이 뛰어남
- CDN에 캐시가 됨

**문제점**

- 데이터가 정적임 (빌드할 때 렌더링 하므로 바뀌지 않음)
- 사용자별 정보 제공이 어렵다

→ 위 문제점 해결을 위해 나온 것이 `ISR` , `SSR` 이다

**실제 사용 예시**

특정 데이터에 대한 페이지들을 미리 생성해두고 싶다면 이 방법을 사용하면 된다.

```tsx
export function generateStaticParams() {
  const products = ['pants', 'skirt'];
  return products.map(product => ({
    slug: product
  }))
}
```

### `I`