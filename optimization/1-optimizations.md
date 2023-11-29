# Optimizations

## NextJS에서 기본적으로 제공하는 최적화

- <Image> : 이미지 지연 로딩을 통해 성능을 최적화하고 장치의 크기에 따라 이미지 크기를 자동 조절한다.
- <Link> : 더 빠른 페이지 전환을 위해 마우스가 Hover 되었을 때 백그라운드에서 페이지를 미리 가져온다.
- <Script>
- SEO 최적화 : 메타데이터를 사용해 검색 엔진 최적화를 사용할 수 있다.

# 이미지 최적화

> 이미지는 웹사이트 페이지에서 무게의 큰 부분을 차지한다.
> 

```jsx
<Image src={} alt=""/>
```

- 이미지 최적화를 사용하기 위해서는 src에 로컬 이미지를 정적으로 import 해서 사용하거나 URL을 작성하고, 해당 URL에 대한 정보를 net.config.js에 프로토콜과 호스트이름을 작성해야 한다. 그렇지 않다면 이미지가 정상적으로 로드되지 않는다.
- ※ 정적 이미지를 사용하는 경우에는 이미지를 public 폴더 내부에 저장해야 한다. 정적 자산은 렌더링 될 때 public이 기본 루트 경로이기 때문이다.
- src에 URL을 사용한다면 반드시 width, height 속성을 적어주어야 한다. width, height 속성은 layout shift가 발생하지 않도록 하기 위해 작성하는 것이다. 이미지가 아직 로드되지 않은 상태에서 이미지가 들어갈 위치에 해당하는 크기를 그대로 유지시키기 위함이라고 생각하면 된다.
- 만약 이미지의 크기를 모르는 경우에는 fill을 사용할 수 있다. fill은 상위 요소를 꽉 채운다는 것을 의미하는데, <Image>를 특정 태그로 한 번 감싸는 방식으로 구현을 한다. 여기서 중요한 것은 상위 요소는 `position: relative` 를 적용시켜주어야 한다는 것이다.
- 규칙을 만들어 이미지의 특정 크기를 지정해주는 것이 더 좋다.
- 이미지가 많은 경우 가장 먼저 보여져야 하는 이미지가 있다면 우선순위를 지정해주는 것이 좋다.

```jsx
<Image src={} alt="" priority />
```

더 많은 속성이 존재하니 NextJS의 [API Reference](https://nextjs.org/docs/app/api-reference/components/image) 페이지를 참고하는 게 좋을 것 같다.

또한 반응형, Grid, 배경 등 다양한 유형의 스타일을 적용시키는 예시도 [공식 문서](https://nextjs.org/docs/app/building-your-application/optimizing/images#styling)에서 알려주고 있으니 필요할 때 다시 들어가서 참고해보자.

# Metadata

## 정적 메타데이터

```jsx
export const metadata: Metadata = {
  title: '...',
  description: '...',
}
```

## 동적 메타데이터

> generateMetadata 함수를 사용한다,
> 

```jsx
export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {

  const id = params.id

  const product = await fetch(`https://.../${id}`).then((res) => res.json())

  return {
    title: product.title,
  }
}
```

# Lazy Loading

> 렌더링하는 데 필요한 JavaScript의 양을 줄여 애플리케이션의 초기 로딩 성능을 향상시킬 수 있다.
> 

## 클라이언트 컴포넌트 필요할 때 동적으로 import

```jsx
// 컴포넌트 외부에 전역으로 생성
const ComponentA = dynamic(() => import('../components/A'))
const ComponentB = dynamic(() => import('../components/B'))
```

## 서버 컴포넌트 필요할 때 동적으로 import

※ 서버 컴포넌트를 동적으로 가져오는 경우에는 서버 컴포넌트 자체를 동적으로 가져오는 게 아니라 서버 컴포넌트의 하위에 있는 클라이언트 컴포넌트만 지연 로딩이 된다.