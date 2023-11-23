# Link

> NextJS의 Link는 Pre-Fetching 된다.
> 

## 기본 사용 방법

```jsx
<Link href=”경로”>경로이름</Link>
```

## 동적 세그먼트 연결

```jsx
<Link href={`/user/${user.userName}`}>{user.job}</Link>
```

- scroll 속성에 대해서는 좀 더 알아보아야 할 것 같다.
https://nextjs.org/docs/app/api-reference/components/link#scroll

## 클라이언트 컴포넌트

- `usePathname()` : 현재 링크 확인하기
- `href="/url#id"` : 특정 항목으로 스크롤
- `useRouter().push('경로')` : userRouter 훅을 사용해 이동