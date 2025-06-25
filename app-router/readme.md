# App Router

### 페이지 라우팅 설정

> 경로 폴더 생성 후 반드시 page.tsx 파일을 생성해야만 한다.
> 

동적 경로 역시 [id] 라는 폴더 생성 후 해당 폴더 내부에 page.tsx 파일을 생성해야 한다.

- app/book/page.tsx
- app/book/[id]/page.tsx

### 쿼리 스트링 전달

app router의 경우에는 쿼리 스트링이 모두 props 로 전달된다.

이때, 객체 형태의 promise 로 전달되므로 사용하기에 앞서 타입 정의가 필요하다

```jsx
export default async function Page({
  searchParams,
}: {
  searchParams: Promise<{ q: string }>;
}) {
  const { q } = await searchParams;
  return <h1>Search {q}</h1>;
}
```

Q. 어떻게 함수 컴포넌트에 async 키워드를 붙일 수 있는가?
⇒ 서버 컴포넌트이기 때문에가능하다.

### 동적 경로

동적 경로의 경우에는 param 객체 안에 promise 형태로 id가 들어있다.

```jsx
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;

  return <div>{id}</div>;
}
```