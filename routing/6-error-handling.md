# 🤔 에러 처리

> React의 ErrorBoundary 컴포넌트 처럼 에러를 처리할 수 있다.
> 
- 주의할 점은 error.js는 동일 위치에서 발생한 오류는 포착하지 않는다는 점이다. 즉, 트리 구조로 보았을 때 형제 파일에 대한 에러는 처리하지 않고 하위 파일들에 대한 에러를 처리하는 것이다.
따라서 루트 컴포넌트의 에러를 처리하려는 경우에 상위 폴더가 없으므로 global-error.js를 사용해 에러 처리를 할 수 있다.

## 사용 방법

1. error.js 파일을 추가
2. 에러 핸들링 컴포넌트 생성 (클라이언트 컴포넌트)

```jsx
'use client'
 
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  )
}
```