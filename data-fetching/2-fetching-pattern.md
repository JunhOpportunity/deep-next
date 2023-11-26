# Fetching 패턴

## 1. 서버에서 데이터 가져오기

> 가능하다면 서버에서 데이터를 가져오는 것이 좋다. 클라이언트에서 서버에 접근하지 않도록 하자.
> 

## 2. 필요한 곳에서 데이터 가져오기

## 3. 스트리밍

> UI를 점진적으로 보여주는 React의 기능
> 

## 4. 병렬적 또는 순차적으로 데이터 가져오기

> 목적에 따라 알맞게 사용해야 한다
> 
- 만약 특정 데이터를 받아와야지만 다음 동작을 수행할 수 있다면 당연히 순차적으로 데이터를 받아와야 한다.
- 여러 요청을 불필요하게 순차적으로 수행한다면 시간 절약을 위해 병렬적으로 데이터를 받아올 수도 있다. `Promise.all`을 사용해 해당하는 모든 요청이 수행될 때까지 기다리며 렌더링 결과를 볼 수 없으니 주의해서 사용해야 한다. 이때는 Loading.ts 를 함께 사용하면 된다.

## 5. 데이터 미리 로딩하기

> 다른 코드들을 수행하기 전에 미리 데이터를 받아오는 코드를 수행하도록 하는 것
> 

```jsx
import { getItem } from '@/utils/get-item'
 
export const preload = (id: string) => {
  void getItem(id)
}
export default async function Item({ id }: { id: string }) {
  const result = await getItem(id)
  // ...
}
```

## 6. React cache, server-only, preload  패턴 사용하기

> 데이터를 적극적으로 가져오고 응답을 캐시하며 이 데이터 가져오기가 서버에서만 발생하도록 보장할 수 있다.
> 

```jsx
import { cache } from 'react'
import 'server-only'
 
export const preload = (id: string) => {
  void getItem(id)
}
 
export const getItem = cache(async (id: string) => {
  // ...
})
```