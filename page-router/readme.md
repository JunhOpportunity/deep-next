### 기본 세팅

`npx prisma studio` : DB 깔끔한 UI로 확인 가능

`npx create-next-app@14 폴더이름` : 최신 버전 node package 다운

`import alias` : 절대 경로를 사용하는 것

# 기본 지식

### 사전 렌더링

브라우저의 요청에 사전에 렌더링이 완료된 HTML을 응답하는 렌더링 방식
CSR의 단점을 효율적으로 해결하는 기술

# Page Router

파일 또는 폴더 이름을 기준으로 페이지 라우팅을 제공

동적 경로의 경우에는 대괄호를 사용한다. /item/[id].js

## pages 폴더

### _app.tsx

모든 페이지에 공통적으로 적용될 레이아웃이나 데이터 적용을 위한 페이지이다.

_app.tsx 파일의 App 컴포넌트는, 모든 페이지의 부모가 되는 컴포넌트라고 보면 된다.

App 컴포넌트에 Nav bar 또는 header 를 추가하면 모든 페이지에 적용된다.

### _document.tsx

기존 React의 index.html 이라고 보면 된다.

Meta, font 등등의 html 태그 관리

### next.config.mjs

nextjs의 기본 설정들을 관리하는 곳.

reactStrictMode 설정도 가능하다. (잠재적 에러를 확인하기 위해 두 번 실행되는 것)

## 폴더 구조 및 페이지 구조

### 페이지 구성

페이지를 구성하기 위해서는 pages 폴더 내부에 해당 페이지의 파일을 생성하거나(/pages/search.tsx) 해당 페이지의 폴더와 파일(/pages/search/index.tsx)을 생성하면 된다.

Q. /pages/search.tsx 와 /pages/search/index.tsx 이렇게 두 개가 중복된 형태로 만들었다면 에러가 뜰까? ⇒ 에러 발생. 각 경로에 단 하나의 컴포넌트만 존재해야 한다.

### 쿼리 스트링 적용

쿼리 스트링을 사용하기 위해서는 `import {useRouter} from "next/router"` 을 활용해야 한다.

여기서 주의할 점은, app router의 경우에는 next/navigation 을 사용하므로 import 할 때 혼동하지 않도록 신경써야 한다.

```jsx
// localhost:3000/search?q=abc 접속

import { useRouter } from "next/router"

export default function Search() {
  const router = useRouter()

  const {q} = router.query;

  return <h1>Search {q}</h1>
}
```

### 동적 경로

`/pages/book/[id].tsx` 로 구성

이 경우에도 마찬가지로 쿼리 스트링처럼 useRouter 사용해서 값을 받아온다.

`[id]` 로 만들었으므로 query 안에 id로 저장된다.

<img width="260" alt="image" src="https://github.com/user-attachments/assets/ec064261-26d1-49ad-b90d-7452140cb4cb" />

[…id].tsx : 를 사용하면 여러개의 동적 경로도 처리하고 싶은 경우에는 `catch all segment` 라고 하는 방식으로 구현하면 된다.
query 안에 id로 배열 형태로 저장된다.
이 경우에는 아무런 파라미터가 오지 않는 경우에는 에러가 발생한다
(localhost:3000/book 접속한 경우)

[[…id]].tsx : `optional catch segment` 아무런 파라미터가 오지 않는 경우까지 포함해서 모든 경우에 대해 대응할 수 있는 방법이다.

### 404 페이지

pages/404.tsx 생성 하면 바로 만들 수 있다.

### 네비게이팅

> 페이지 이동을 할 때는 `<Link href={”/”}` 컴포넌트를 사용한다.
함수 내부에서는 `router.push(”/”)` 를 사용한다.
> 

CSR 방식으로 페이지를 이동하기 때문에 빠르게 페이지 이동이 가능하다.

- 프로그래매틱한 페이지 이동 : 특정 조건이 만족했을 경우 함수 내부에서 페이지를 이동하도록 하는 것
- router.push(”/”) : 기본 이동. 히스토리에 기록됨
- router.back() : push와 비슷하지만 히스토리를 덮어쓰기 때문에 뒤로가기 안됨 (로그인 후 로그인 페이지를 안보이게 하거나 구글 폼 제출 후 URL 초기화 할 때 사용)
- router.replace() : 뒤로 가기

### 프리패칭

<Link> 로 구현한 페이지들은 프리패칭이 자동으로 되지만 프로그래메틱한 페이지 이동으로 구현한 페이지들은 프리팿이이 자동으로 이루어지지 않는다.

따라서 현재 컴포넌트가 마운트 되었을 때 프리패칭 되도록 구현하면 된다.
⇒ useEffect 사용

```jsx
useEffec(()=>{
	router.prefetch('/test')
}, [])
```

### API Routes

> API 응답을 설정할 수 있는 파일 (잘 쓰이진 않는다)
> 

`/pages/api/파일.ts`

localhost:3000/api/파일 

```jsx
import type { NextApiRequest, NextApiResponse } from "next";

type Data = {
  name: string;
};

export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<Data>,
) {
  res.status(200).json({ name: "John Doe" });
}
```

### 스타일링

> index.module.css 로 이름을 작성한다.
> 

css 는 평소처럼 작성하고, 사용할 때는 `import style from “./index.module.css”` 로 불러와서 `style.h1` 이렇게 사용한다

```jsx
import style from "./index.module.css";

<h1 className={style.h1}>인덱스<h1>
```

## SSR

> getServerSideProps 사용
> 

| 장점 | 단점 |
| --- | --- |
| 페이지 내부의 데이터를 항상 최신으로 유지 | 데이터 요청이 늦어질 경우 모든게 늦어진다 |

페이지 컴포넌트보다 먼저 실행 되어서 필요한 데이터 미리 가져옴

```tsx
// SSR 구현
export const getServerSideProps = () ⇒ {
	const data = {}
	
	return {
		props : {
			data,
		}
	}
} 

// 사용 시 타입 선언
export default function Home({
	data,
}: InferGetServerSidePropsType<typeof getServerSideProps>) {

	return (<div></div>)
}
```

getServerSideProps에서 쿼리 스트링을 가져올 때는 `GetServerSidePropsContext` 를 사용한다.

```tsx
export const getServerSideProps = async (
	context: GetServerSidePropsContext
) => {
	const q = context.query.q;
	
}
```

URL의 파라미터를 가져올 때는 `context.params.id`를 사용하면 되는데, `undefined` 일 가능성이 있다는 이유로 에러가 발생한다.
따라서 `context.params!.id` 로 단언해주면 해결된다.

## SSG

> 빌드 타임에 미리 사전 렌더링 하는 방식
`getStaticProps` 사용
> 

| 장점 | 단점 |
| --- | --- |
| 매우 빠른 속도로 페이지 보여줌 | 매번 똑같은 페이지만 응답함 |
|  | 최신 데이터 반영 어려움 |

```tsx
// 사용 시 타입 선언
export default function Home({
	data,
}: InferGetStaticPropsType<typeof getStaticProps>) {

	return (<div></div>)
}
```

개발 모드에서는 SSR처럼 들어갈 때마다 재요청한다. 따라서 프로덕션 모드로 실행해야 확인이 가능.

아무 설정도 없으면 기본적으로 SSG 방식으로 수행된다.

사전 렌더링 과정에서는 쿼리 스트링을 가져올 수 없다. 왜냐하면 페이지가 이미 다 만들어지 상태이고, 이후에 사용자의 입력으로 쿼리 스트링이 변경되기 때문이다. 따라서 이런 경우에는 그냥 평소처럼 페이지 컴포넌트 내에서 데이터를 요청해서 가져오는 방식으로 해야한다. (useEffect)

### 동적 경로 설정 방법

그래서 동적 경로의 경우에는 미리 경로를 지정해주어야 한다.
여기서 path 부분이 동적 경로 목록을 의미하고
fallback 은 그 외의 경로를 의미한다.
false 로 설정할 경우 not-found 반환

- false : 404 not found
- blocking : 미리 지정하지 않은 경우 실시간으로 즉시 생성
처음 만들땐 SSR 방식으로 생성되고, 이후에는 한 번 생성되었으니 다시 SSG 방식으로 페이지를 반환한다.
- true : 즉시 생성 + 페이지만 미리 반환
props 없는 데이터가 없는 페이지만 미리 렌더링하고, 이후에 데이터 있는 페이지 렌더링

```tsx
// 동적 경로 지정
export const getStaticPaths = () => {
	return {
		path: [
			{params: {id: "1"}},
			{params: {id: "2"}},
			{params: {id: "3"}},
		],
		fallback: false,
	}
}
```

## ISR

> 사전 렌더링 이후 일정 시간을 주기로 새로운 페이지 렌더링
SSR과 SSG 두 방식의 장점을 모두 갖고있음
revalidate 사용
> 

초단위로 설정 가능

```tsx
export const getStaticProps = () ⇒ {
	const data = {}
	
	return {
		props : {
			data,
		},
		revalidate: 3,
	}
} 
```

### on-demand ISR

시간과 관계 없이 사용자가 요청할 때마다 데이터가 변하는 경우 문제가 생김.

아래 코드에서 localhost:3000/api/revalidate 로 요청을 보내면 페이지가 바뀌게 된다.

```tsx
// pages/api/revalidate.ts
import {NextApuRequest, NextApiResponse} from "next";

export default async function hanlder(
	req: NextApiRequest,
	res: NextApiResponse
) {

	try {
		await res.revalidate("/");
		return res.json({revalidate: true});
	} catch (err) {
		res.status(500).send("revalidation Failed")
	}

```
