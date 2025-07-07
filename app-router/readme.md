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

## 풀 라우트 캐시

> Next 서버에서 렌더링 결과를 캐싱하는 것
> 

정적으로 생성되는 것과 비슷하다고 볼 수 있다.

| 동적 페이지 | 정적 페이지 |
| --- | --- |
| 쿠키, 헤더, 쿼리스트링
사용한 컴포넌트 | 그 외의 컴포넌트 |

동적 페이지의 기준 : 쿠키, 헤더, 쿼리스트링을 사용한 컴포넌트

정적 페이지의 기준 : 동적 페이지가 아닌 것

정적 페이지에만 풀 라우트 캐시가 적용된다.

리퀘스트 메모이제이션이나 데이터 캐시 등 하나라도 revalidate 옵션이 붙은 경우 풀 라우트 캐시도 같이 업데이트 한다

이 기능을 구현하기 위해서는 Suspense 로 감싸주면 된다.

Suspense 태그로 감싸진 부분은 곧바로 렌더링 되지 않는다.
내부의 비동기 작업이 끝나기 전까지 fallback 부분을 보여준다.

```tsx
// layout.tsx

<Suspense fallback={<div>대체 ui</div>}>

</Suspense>
```

동적 페이지의 경우에는 generateStaticParams 를 사용한다.
이걸 사용하면 미리 페이지를 렌더링해둔다.

```tsx
export function generateStaticParams () {
	return [{id: "1"}, {id: "2"}]
}
```

## 라우트 세그먼트 옵션

> 페이지의 동작(revalidate, dynamic, static) 강제 설정
> 

```tsx
export consy dynamic = "옵션";
```

- auto : 아무것도 강제로 선택하지 않는 기본값
- force-dynamic : 강제로 dynamic 페이지로 설정
- force-static : 강제로 static 페이지로 설정
- error : 강제로 static 페이지로 설정 (동적 페이지로 해야하는 경우 에러를 발생시킴)

## 클라이언트 라우터 캐시

> 브라우저에 저장되는 캐시(데이터, 페이지 정보, 상태 등)
> 

새로고침 하거나 탭을 닫았다가 다시 열면 캐시가 초기화된다

## 서버 액션

> 브라우저에서 호출할 수 있는 서버에서 실행되는 비동기 함수
> 

서버에서만 실행되는 함수를 브라우저(클라이언트)가 직접 호출 가능

```tsx
async function createReviewAction(formData: FormData) {
	"use server";
	console.log("서버액션");
	console.log(formData);

	const author = formData.get("author")?.toString();
}

<form action={createReviewAction}>
	<input name="content" placeholder="" />
</form>
```

### 서버 액션 파일 분리하기

- 경로 : `src/actions/이름.action.ts`
- 함수 내부에 있던 `"use server"` 최상단으로 옮기기

### 재검증

> `revalidatePath()` 사용
> 

만약 새로운 댓글을 작성한 뒤에 해당 페이지에서 바로 새로운 댓글을 업데이트 해야한다면 revalidatePath 를 사용해 재검증한다.

이 함수는 서버측에서만 호출할 수 있는 메서드이다.

재검증 할 경우 데이터 캐시 뿐만 아니라 풀 라우트 캐시까지 삭제됨
⇒ 즉, 새롭게 업데이트 된 페이지는 풀라우트 캐시에 저장되지 않는다.

```tsx
// 1. 특정 주소에 해당하는 페이지만 재검증
revalidatePath(`/book/${booId}`)

// 2. 특정 경로의 모든 동적 페이지를 재검증
revalidatePath("/book/[id]", "page")

// 3. 특정 레이아웃을 갖는 모든 페이지 재검증
revalidatePath("/(with-searchbar)", "layout")

// 4. 모든 데이터 재검증
revalidatePath("/", "layout")

// 5. 태그 기준 데이터 캐시 재검증
revalidateTag(`review-${bookId}`)

{next: {tags:[`review-${bookId}`]}}
```

### 🖐️ 클라이언트에서 중복 제출 방지

> React의 `useActionState` 훅 사용
> 
- 전달 : 첫 번째 인수로 액션, 두 번째 인수로 초기값 넘겨준다.
- 반환 : state, formAction, isPending(로딩값)

```tsx
const [state, formAction, isPending] = useActionState(
	createReviewAction, 
	null
)

<form action={formAction}>
	<button disabled={isPending} type="submit"/>
</form>
```

## 병렬 라우트

> 하나의 화면에 여러 개를 렌더링 시키는 것
> 

```tsx

// src/app/parallel/@sidebar/page.tsx

// src/app/parallel/layout.tsx
import { ReactNode } from "react";

export default function Layout({
  children,
  sidebar,
}: {
  children: ReactNode;
  sidebar: ReactNode;
}) {
  return (
    <div>
      {children}
      {sidebar}
    </div>
  );
}
```

소셜 미디어 서비스나 관리자 대시보드 등 복잡한 구조를 갖는 UI에 유용하게 활용된다.

폴더 이름 앞에 골뱅이가 붙은 것을 `슬롯`이라고 부른다.

슬롯 : 병렬로 렌더링 될 페이지 컴포넌트를 보관하는 폴더

부모 폴더의 레이아웃에 props로 전달된다.

슬롯의 경로는 무시하기 때문에 슬롯 내부에 하위 경로 폴더가 없는 경우 그냥 무시하고 이전 페이지를 보여준다.
이때 주의 할 점은 새로고침 할 경우에는 이전 페이지가 존재하지 않기 때문에 404 에러를 발생시킨다는 것이다.
따라서 슬롯별로 렌더링 할 페이지가 없을 경우를 대비해 default 페이지를 만들어야 한다. (각 슬롯 내부에 `default.tsx` 생성)

![image](https://github.com/user-attachments/assets/36a53ebd-3cf6-4071-a9cc-a964855200ff)


여기서 만약 `http://localhost:3000/parallel/a` 로 접속했다면, 보라색 페이지만 업데이트 되고, 나머지 파란색과 초록색 페이지는 그대로 유지된다.

- 만약 에러 발생하면 .next 폴더 제거 후 다시 실행
- 슬롯 개수는 제한이 없다

## 인터셉팅 라우트

> 특정 조건에 따라 원래 페이지가 아닌 다른 페이지 렌더링 하는 것
> 

### 설명

여기서 특정 조건이란? 개발자가 임의로 설정할 수 없다. 기본적으로 세팅되어있는 조건이고, 초기 접속이 아닌 경우에 동작하도록 설정된다.

ex) 인스타그램에서 게시글을 눌렀을 때 모달 형태로 보여주다가, 새로고침 할 경우 아예 상세 페이지로 넘어가버리는 것

### 구현

`(.)book/[id]` 이렇게 작성하면 현재 동일 경로에 존재하는 `book/[id]` 가로챈다.

만약 한 경로 상위에 있다면 `(..)`, 두 경로 상위에 있다면 `(..)(..)` 를 적어준다.

`(…)` 은 app 폴더 바로 아래에 있는 경로를 말한다.

```tsx
// 인터셉팅 페이지 (기존 페이지 그대로 렌더링 되도록 porps 설정)
import BookPage from "@/app/book/[id]/page";

export default function Page(props:any) {
	return(
		<div>
			<Modal>
				<BookPage {...props} />
			</Modal>
		</div>
	)
}
```

```tsx
// src/components/modal.tsx 모달생성
"use client";

import {ReactNode} from "react";
import style from "./modal.module.css";
import {createProtal} from "react-dom";
import {useRouter} from "next/navigation"

export default function Modal({children} : {children: ReactNode}) {
	
	const dialogRef = useRef<HTMLDialogElement>(null);
	const router = useRouter();
	
	useEffect(()=> {
		if(!dialogRef.current?.open) { // 모달이 닫혀있는 경우
			dialogRef.current?.showModal(); // 모달 열어두기
			dialogRef.current?.scrollTo({top:0}); // 스크롤 위치 맨 위로
		}
	}, []);
	
	return createPortal(
		<dialog 
			onClose={() => router.back()} // ESC 눌렀을때
			onClick={(e) => { // 바깥쪽 눌렀을때
				if((e.target as any).nodeName === "DIALOG") {
					router.back();
				}
			}}
			className={style.modal}
			ref={dialogRef}
		>
			{children}
		</dialog>,
		document.getElementById("modal-root") as HTMLElement
	)
}

// src/app/layout.tsx
	body 바로 아래에
	<div id="modal-root"></div>
</body>
```

```tsx
// 모달 스타일
.modal {
	width: 80%;
	max-width: 500px;
	
	margin-top: 20px;
	border-radius: 5px;
	border:none;
}

.modal::backdrop { // 모달 뒷부분 흐릿하게
	background: rgba(0, 0, 0, 0.7);
}

```

🤚 패럴랠 라우트와 인터셉팅 라우트 같이 쓰는 거 좋음

## 이미지 최적화

Next에서 제공해주는 <Image/> 컴포넌트를 사용하면 기본적인 최적화가 다 이루어진다.

1. 이미지 미리 등록하기

```tsx
// next.config.mjs

images: {
	domain: ["도메인 입력"]
}
```

1. 이미지 사이즈 작게 설정하기 (width={240} height={300})

## SEO 최적화

### Metadata 설정하기

```tsx
export const metadata : Metadata = {
	title: "타이틀",
	description: "설명",
	images: ["/"], // public 디렉토리 안에있는 이미지
}
```

## 배포하기

1. `$ sudo npm i -g vercel`
2. `$ vercel login`
3. `$ vercel`
4. `$ vercel --prod` (재배포)
