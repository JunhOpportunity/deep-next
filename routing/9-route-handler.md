# 🤔 Route Handler

> 라우트 핸들러를 사용하면 요청 및 응답 API를 만들 수 있다. 즉, NextJS에서 백엔드를 구현하는 것이다.
> 

※ 라우트 핸들러는 NextJS 13 버전부터 app 디렉터리 내부에서만 사용 가능하다. (12 버전에서는 pages 폴더 내부에 api 파일이 존재했었다.)

## 사용 방법

/app/api/route.ts 또는 /app/api/특정이름/route.ts 로 생성.

```jsx
import { NextResponse } from 'next/server';

export async function GET(request: NextResponse ) {
	const user = await getUser();
}
```

페이지의 URL과 마찬가지로 라우트 핸들러의 폴더 이름도 세그먼트이기 때문에 알맞게 이름을 붙여주면 된다.

HTTP 메소드 중  GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS 가 지원된다.

만약 지원되지 않느 메소드를 호출하면 Next는 `405 Method Not Allowed` 에러를 반환하니 혹시라도 이 에러가 뜬다면 잘못된 메소드를 호출했다고 이해하면 된다.

HTTP 메소드에 대해 자세히 알고 싶다면 이 문서를 읽어보자.

https://developer.mozilla.org/ko/docs/Web/HTTP/Methods

## 동적 세그먼트

동적으로 생성되는 페이지에 해당하는 데이터를 요청할 수 있다.

route.ts 파일은 api 폴더 내부에만 생성할 수 있는 것이 아니라 일반 폴더에서도 생성이 가능하다.

 /app/user/[slug]/route.ts 파일을 만들고 /user/a 에 접속한다면 a를 받아와서 a에 해당하는 값을 요청할 수 있다.

```jsx
export async function GET(
  request: Request,
  { params }: { params: { slug: string } }
) {
  const slug = params.slug
	const userData = async getUser(slug);
	// ...
}
```