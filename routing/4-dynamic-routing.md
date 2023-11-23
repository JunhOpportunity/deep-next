# 동적 라우팅

## 생성 방법

> 폴더의 이름을 [id] 또는 [slug] 로 선언한 뒤 page.js 파일 생성
> 

❗ 여기서 폴더 이름은 아무거나 넣어주어도 상관 없다. 중요한 것은 괄호에 넣은 이름을 params를 통해 불러올 때도 같은 이름을 사용해야 한다는 것이다. ([abc], [name] 등 상관 없다.)

1. app/user/[slug]/page.js 생성
2. 사용자가 /user/a 접속
3. { slug: ‘a’ } 객체를 params로 반환

## slug 사용 방법

> params.slug
> 

```jsx
export default function Profile({ params }: { params: { slug: string } }) {
  return <div>User : {params.slug}</div>
}
```

## 동적 라우팅을 정적으로 생성하는 방법

> generateStaticParams를 사용해 빌드될 때 정적으로 경로를 생성할 수 있다.
> 

```jsx
export function generateStaticParams() {
	const users = ['junho', 'woowa'];
  return users.map(user => ({
    slug: user
  }))
}
```

## Catch-all 세그먼트

> […slug] 를 사용해 다음 세그먼트들을 모두 이용 (배열 형태로 반환됨)
> 

app/user/[…slug]/page.js 생성했을 때

/user/a 접속 ⇒ { slug: [’a’] }

/user/a/b 접속 ⇒ { slug: [’a’, ‘b’] }

/user/a/b 접속 ⇒ { slug: [’a’, ‘b’] }

## ****Optional Catch-all 세그먼트****

> [[…slug]] 를 사용해 다음 세그먼트들을 모두 이용 (아예 없을 때도 반환된다)
> 

app/user/[…slug]/page.js 생성했을 때

/user 접속 ⇒ {}

/user/a 접속 ⇒ { slug: [’a’] }

/user/a/b 접속 ⇒ { slug: [’a’, ‘b’] }

/user/a/b 접속 ⇒ { slug: [’a’, ‘b’] }

## ⭐ Catch-all VS ****Optional Catch-all****

👀  여기서 주의할 점은, Catch-all 세그먼트를 사용했을 때와 Optional Catch-all 세그먼트를 사용했을 때 보여지는 페이지에 해당하는 파일 자체가 다르다는 것이다.

| 세그먼트 종류 | 접속 경로 | 접속 되는 파일 |
| --- | --- | --- |
| Catch-all | /user | app/user/page.js |
| Optional Catch-all | /user | app/user/[[…slug]]/page.js |

## [참고) TypeScript 에서는 어떻게 slug의 타입을 정의할까?](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#typescript)

| app/blog/[slug]/page.js | { slug: string } |
| --- | --- |
| app/shop/[...slug]/page.js | { slug: string[] } |
| app/[categoryId]/[itemId]/page.js | { categoryId: string, itemId: string } |