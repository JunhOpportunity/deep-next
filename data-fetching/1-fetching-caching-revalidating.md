# Fetching, Caching, Revalidating

## Fetching

> NextJS에서 Fetch 요청에 대한 캐싱, 재검증 등의 동작을 할 수 있다
> 

```jsx
async function getData() {
	const res = await fetch('https://...')
	if (!res.ok) { // 에러 처리
    throw new Error('Error')
  }
	return res.json()
}
```

※ 라우트 핸들러에서는 Fetch 요청이 메모이제이션 되지 않는다.

## Caching

> 요청 결과를 저장하기 때문에 같은 요청을 다시 할 경우 응답을 받지 않고 캐싱된 결과를 사용한다.
> 

기본적으로 NextJs는 Fetch 결과값을 자동으로 캐시하고 재사용한다.

+) 캐싱에 대한 자세한 내용은 MDN 사이트 참고https://developer.mozilla.org/ko/docs/Web/HTTP/Caching

## Revalidating

> 캐시 데이터를 제거하고 최신 데이터를 다시 가져오는 것
> 
- 주기적인 재검증 : 일정 시간이 지남에 따라 데이터를 `자동`으로 재검증한다.
- 명령형 재검증 : 이벤트를 기반으로 데이터를 `수동`으로 재검증한다.

### 주기적인 재검증

- fetch 요청할 때 함께 설정하기 `{ next: { revalidate: 3600 } }`

```jsx
fetch('https://...', { next: { revalidate: 3600 } })
```

- 위 또는 아래에 선언하기 (라이브러리 함수를 사용하는 경우에 사용.)

```jsx
// Segment Config Options - https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config
export const revalidate = 3600
```

### 명령형 재검증

1. 특정 태그로 요청을 지정

```jsx
export default async function Page() {
  const res = await fetch('https://...', { next: { tags: ['collection'] } })
  const data = await res.json()
}
```

1. 서버에서 revalidateTag를 호출해 특정 태그가 지정된 호출을 재검증

```jsx
// /app/actions.ts
'use server'
 
import { revalidateTag } from 'next/cache'
 
export default async function action() {
  revalidateTag('collection')
}
```