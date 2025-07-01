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

### 레이아웃

> 현재 적용된 경로의 모든 하위 경로에 똑같이 적용됨
> 

여기서 주의할 점은, 레이아웃에 page를 전달할 수 있도록 children을 추가해 주어야 한다는 점이다.

하위에 여러 개의 레이아웃들이 존재한다면 모두 중첩해서 적용된다.

Q. 특정 페이지에만 레이아웃을 설정하고 싶다면? 
⇒ 라우트 그룹 사용. 소괄호로 감싸서 그룹화함. 경로에는 아무런 영향을 미치지 않는 폴더이다. 따라서 이 폴더를 생성한 뒤에 여기로 옮겨두면 된다.

## 서버 컴포넌트

> 서버 측에서 단 한 번만 실행되는 컴포넌트
NEXTJS 공식 문서 - 대부분의 페이지를 서버 컴포넌트로 구성하고 꼭 필요한 경우에만 클라이언트 컴포넌트 사용.
> 

페이지 라우터의 경우에는 자바스크립트 번들로 한꺼번에 다 묶은 다음에 브라우저에게 전달하기 때문에 불필요하게 많은 컴포넌트들이 자바스크립트 번들에 포함될 수밖에 없어서 자바스크립트 번들의 용량이 쓸데없이 커지게 된다.

| 서버 컴포넌트 | 클라이언트 컴포넌트 |
| --- | --- |
| 서버측에서 사전 렌더링 진행할 때 딱 한 번만 실행 | 사전 렌더링 진행할 때 한 번, 하이드레이션 진행할 때 한 번 실행됨 |
| 총 1번 실행 | 총 2번 실행 |
| 기본적으로 서버 컴포넌트임 | “use client” 작성 |
| 안에서 클라이언트 컴포넌트 import 가능 | 안에서 서버 컴포넌트 import 불가능 ⇒ 에러 대신에 서버 컴포넌트 자체를 클라이언트 컴포넌트로 바꿔버린다. |

app router 에서 모든 컴포넌트는 기본적으로 서버 컴포넌트이다.
서버 컴포넌트는 브라우저에서 실행되지 않기 때문에 console.log 출력 안된다.
즉, 브라우저에서 사용하는 작업은 서버 컴포넌트에서 사용할 수 없다.(useEffect 등등)

Q. 반드시 클라이언트 컴포넌트 안에서 서버 컴포넌트를 써야 하는 경우라면 어떻게 해야할까?
⇒ children 으로 넘겨주면 서버 컴포넌트를 클라이언트 컴포너트로 바꾸지 않는다!

```tsx
// 클라이언트 컴포넌트
"use client";
export default function ClientComponent({children} : {children: ReactNode }) {
	return <div>{children}</div>
}

// index page
<ClientComponent>
	<ServerComponent/>
</ClientComponent>
```

### 주의사항

1. 클라이언트 컴포넌트에서 서버 컴포넌트를 import 할 수 없다
2. 서버 컴포넌트에서 클라이언트 컴포넌트에게 직렬화 되지 않은 Props는 전달이 불가능하다
자바스크립트의 함수는 클로저나 렉시컬 함수 때문에 직렬화가 불가능하다.

ex) 직렬화 예시
<img width="464" alt="image" src="https://github.com/user-attachments/assets/c8a35f5e-5e3b-4002-9386-f2c389ebd588" />


사전 렌더링 과정
⇒ 서버 컴포넌트 따로 실행(RSC payload 문자열 생성) - 완성된 HTML 페이지 실행

### ✋네비게이팅

Static 에 해당하는 페이지들은 RSC payload와 JS 번들을 모두 불러오고
Dynamic 에 해당하는 페이지들은 RSC payload만 프리패칭하고 JS 번들은 향후 실제 페이지 이동이 발생했을 때만 불러온다

## 데이터 패칭

기존에 page-router 방식에서는 getServerSideProps나 getStaticProps 를 통해 props를 자식 컴포넌트에 일일이 넘겨주어야 했는데, 이 방식을 사용하지 않고 서버 컴포넌트를 사용해서 필요한 곳에서 직접 바로 데이터를 가져옴.
⇒ 비동기 함수를 사용해 fetch한다.

## 쿼리 스트링 전달

> useSearchParams() 를 사용해 전달한다.
> 

```tsx
import { useRouter, useSearchParams } from "next/navigation"

const searchParams = useSearchParams();
const q = searchParams.get("q") // 이전 방식: router.query.q
```
## 데이터 캐시

> fetch의 두 번째 인자로 옵션을 전달해 사용
`fetch("api/address”, {cache: “force-cache"})`
> 

fetch 메서드를 활용해 불러온 데이터를 Next 서버에 보관
⇒ 불필요한 요청 줄여서 성능 향상 가능

여기서 주의할 점은, Next 에서는 fetch에 cache를 제공하지만 axios 같은 경우에는 제공하지 않는다는 것이다.

- `cache: “no-store”` ⇒ 캐싱 아예 하지 않음. 기본값 (14버전 까지는 캐싱되는 게 기본 값이었다.)
- `cache: “force-cache"` ⇒ 항상 캐싱함.
- `next: {revalidate: 3}` ⇒ 특정 시간 주기로 캐시 업데이트함. (마치 ISR 방식)
- `next: {tags: ['a']}` ⇒ 요청이 들어왔을 때만 캐시 업데이트함.(마치 on-demand ISR 방식)

## 리퀘스트 메모이제이션

> 요청을 기억하고 같은 요청의 경우 해당 요청을 캐싱해 최적화
한 번의 서버 렌더링 주기 후 초기화된다.
> 

렌더링이 종료되면 모든 리퀘스트 메모이제이션이 소멸된다.
따라서 데이터 캐시와는 비슷해보이지만 차이점이 존재한다.

Next 에서 자동으로 적용된다.
