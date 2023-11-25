# 미들웨어

> 공통적으로 수행해야 하는 요청에 대해서 한 번에 입구에서 처리하는 기술
> 

![image](https://github.com/JunhOpportunity/deep-next/assets/89464762/5446f491-518d-46a6-aa62-5fde841f4bd0)


## 사용 방법

기본적으로 모든 요청에 대해서 미들웨어를 거친다.

```jsx
export function middleware(request: NextRequest) {
	// 필요한 코드
}
```

## 특정 페이지에서만 미들웨어를 거치는 방법

```jsx
export function middleware() {
	if (request.nextUrl.pathname.startsWith('/user')) {
    return NextResponse.redirect('/')
  }
}

export const config = {
	matcher : ['/user/:path*'] //여기에 해당하는 URL만 미들웨어를 거친다.
}
```

## 어떤 경우에 사용할까

- 로그인 인증 확인
- 데이터 검증
- 잘못된 경로로 진입했을 때 다른 경로로 이동
