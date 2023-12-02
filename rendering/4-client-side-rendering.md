### `CSR` (Client Side Rendering)

> 렌더링 하는 주체자가 클라이언트(브라우저)
> 

**장점**

- 한번 로딩 되는데는 시간이 걸리지만, 로딩 후에는 빠른 UX를 제공한다
- 서버의 부하가 작다

**문제점**

- 페이지 로딩 시간(TTV = Time To View  ≒ FCP = First Contentful Paint)이 길다 (HTML 파일과 React 라이브러리, JS 코드까지 모두 다운받은 뒤에 렌더링이 가능하기 때문)
- 자바스크립트 활성화가 필수이다(HTML 파일은 비어있기 때문)
- **SEO** 최적화가 힘들다 (페이지 로딩 시간이 길기 때문에 웹 크롤러들이 모두 다운 받을 때까지 기다린 뒤에 React 라이브러리와 JS 코드를 깊게 탐색하지 않기 때문이다.)
- 보안에 취약하다 (서버와의 통신 과정이 모두 클라이언트에게 가야하기 때문)
- **CDN**에 캐시가 안된다

→ 위 문제점 해결을 위해 나온 것이 `SSG`, `SSR` 이다

**실제 사용 예시**

`use client` 를 작성해 클라이언트 컴포넌트를 생성해서 useState와 useEffect를 사용해 구현

```jsx
'use client'

export default function ClientComponent() {
	const [text, setText] = useState();
	useEffect(() => {
		fetch('https://...')
			.then((res) => res.json())
			.then((data) => setText(data.data[0]))
	}, []);
}
```