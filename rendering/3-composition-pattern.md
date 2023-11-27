# 구조 패턴 (Composition pattern)
> 어떤 경우에 어떤 컴포넌트를 사용해야 할까?

## 서버 컴포넌트 VS 클라이언트 컴포넌트

| 기능 | 서버 컴포넌트 | 클라이언트 컴포넌트 |
| --- | --- | --- |
| 데이터 가져오기 | ✅ |  |
| 백엔드 리소스에 직접 접근 | ✅ |  |
| 민감한 정보 서버에 보관(액세스 토큰, API 키 등) | ✅ |  |
| 서버 의존성 유지 / 클라이언트 JavaScript 감소 | ✅ |  |
| 상호작용 및 이벤트 리스너( onClick(), onChange()등) 추가 |  | ✅ |
| 상태 및 수명주기 효과 사용( useState(), useReducer(), useEffect()등) |  | ✅ |
| 브라우저 전용 API 사용 |  | ✅ |
| state, effect, 브라우저 전용 API에 따라 달라지는 커스텀 훅 사용 |  | ✅ |
| React class component 사용 |  | ✅ |

## 서버 컴포넌트 패턴

### 1. 컴포넌트 간 데이터 공유

💡 서버에서 사용할 수 없는 React Context, Prop 데이터 전달 대신에 캐시 함수를 사용해 필요한 컴포넌트에 동일한 데이터를 가져오도록 할 수 있다. React는 fetch를 확장해 데이터 요청을 자동으로 메모이제이션 하고 fetch를 사용할 수 없을 때 캐시 함수를 사용할 수 있기 때문이다.

### 2. 클라이언트 환경에서 서버 전용 코드

서버에서만 실행되도록 의도된 코드가 클라이언트에 들어갈 수 있기 때문에 직접 작성하지 말고 함수를 호출하는 방식으로 사용해야 한다. (예를 들면 getUser, getData 등)

💡 여기서 주의해야 할 것은 환경 변수를 사용하는 경우에 클라이언트 측에서만 NextJS가 환경 변수를 빈 문자열로 바꾸기 때문에 클라이언트 환경에서 환경 변수를 사용하는 함수가 있다면 작동하지 않는다.

### 3. 서버 컴포넌트에서 클라이언트 컴포넌트 전용 패키지 사용

클라이언트 컴포넌트에서만 사용할 수 있는 API를 서버 컴포넌트에서 사용하려고 하는 경우 에러가 발생한다. 이때 해당 기능을 사용하는 클라이언트 컴포넌트를 하나 생성하고, 이 컴포넌트를 서버 컴포넌트에 넣으면 사용할 수 있다.

```jsx
// 클라이언트 컴포넌트
'use client'
 
import { Carousel } from 'acme-carousel'
 
export default Carousel

// 서버 컴포넌트
<Carousel />
```

## 클라이언트 컴포넌트 패턴

### 1. 클라이언트 컴포넌트를 트리 아래로 이동하기

Javascript 번들 크기를 줄이기 위해 클라이언트 컴포넌트를 컴포넌트 트리 아래로 이동하는 것이 좋다.
예를 들면, 상호작용을 반드시 해야하는 기능을 가진 것들은 컴포넌트로 따로 만들어 불러오고, 상호작용이 필요 없는 기능을 가진 것들은 서버 컴포넌트로 유지하는 것이다. 

모든 컴포넌트를 클라이언트 컴포넌트로 만들 필요가 없기 때문이다.

## 서버 컴포넌트 또는 클라이언트 컴포넌트 끼워넣기

클라이언트 컴포넌트는 서버 컴포넌트 다음에 렌더링되기 때문에 서버 컴포넌트를 클라이언트 컴포넌트 모듈로 가져올 수 없다. 대신에 서버 컴포넌트를 props 로 클라이언트 컴포넌트에 전달하는 것은 가능하다.

일반적으로는 children 프로퍼티를 사용해 클라이언트 컴포넌트에서 슬롯을 만든다.

```jsx
'use client'

import { useState } from 'react'
 
export default function ClientComponent({
  children,
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
      {children}
    </>
  )
}

```

```jsx
import ClientComponent 
import ServerComponent 

<ClientComponent>
   <ServerComponent />
</ClientComponent>
```