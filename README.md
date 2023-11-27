# deep-next
✨ Next.JS의 공식 문서를 공부하며 새롭게 깨달은 내용들을 정리해보자
🤔 : 이 표시는 제대로 이해가 되지 않아 다시 공부해야 하는 것이 존재함을 의미한다.

## 목차 및 요약
### [라우팅](https://github.com/JunhOpportunity/deep-next/tree/main/routing)
- [기본 개념](https://github.com/JunhOpportunity/deep-next/blob/main/routing/0-basic.md)
> 요약 없음
- [Link](https://github.com/JunhOpportunity/deep-next/blob/main/routing/1-link.md)
> NextJS의 Link는 Pre-Fetching 된다.
- [라우팅 및 탐색](https://github.com/JunhOpportunity/deep-next/blob/main/routing/2-routing-and-navigating.md)
> 요약 없음
- [라우트 그룹](https://github.com/JunhOpportunity/deep-next/blob/main/routing/3-route-groups.md)
> 폴더 이름을 괄호 안에 넣으면 URL 경로에 영향을 주지 않는다.
- [동적 라우팅](https://github.com/JunhOpportunity/deep-next/blob/main/routing/4-dynamic-routing.md)
> 폴더의 이름을 [id] 또는 [slug] 로 선언한 뒤 page.js 파일 생성
- [UI 로딩과 스트리밍](https://github.com/JunhOpportunity/deep-next/blob/main/routing/5-ui-loading-and-streaming.md)
> 요약 없음
- 🤔 [에러 핸들링](https://github.com/JunhOpportunity/deep-next/blob/main/routing/6-error-handling.md)
> React의 ErrorBoundary 컴포넌트 처럼 에러를 처리할 수 있다.
- 🤔 [병렬적인 경로](https://github.com/JunhOpportunity/deep-next/blob/main/routing/7-parallel-path.md)
> 병렬 라우팅을 사용하면 동일한 레이아웃에서 하나 이상의 페이지를 동시 또는 조건부로 렌더링 할 수 있다.
- 🤔 [경로 가로채기](https://github.com/JunhOpportunity/deep-next/blob/main/routing/8-intercept-path.md)
> 어떤 경우에 사용하는지 좀 더 알아보자.
- 🤔 [Route Handler](https://github.com/JunhOpportunity/deep-next/blob/main/routing/9-route-handler.md)
> 라우트 핸들러를 사용하면 요청 및 응답 API를 만들 수 있다. 즉, NextJS에서 백엔드를 구현하는 것이다.
- [미들웨어](https://github.com/JunhOpportunity/deep-next/blob/main/routing/10-middleware.md)
> 공통적으로 수행해야 하는 요청에 대해서 한 번에 입구에서 처리하는 기술
- 💡 [프로젝트 구현에 도움이 될만한 것들](https://github.com/JunhOpportunity/deep-next/blob/main/routing/11-special-addition.md)
> 요약 없음
- Fetching, Caching, Revalidating
> 요약 없음
- Fetching 패턴
> 요약 없음
- 서버 컴포넌트
> 서버에서 렌더링하고 선택적으로 캐시할 수 있는 UI를 작성 (Next는 기본적으로 서버 컴포넌트에서 작동한다.)
- 클라이언트 컴포넌트
> 반응형 UI를 작성할 때 사용하는 컴포넌트이다. 파일의 맨 위에 `"use client"` 를 추가해야 한다.
- 구조 패턴 (Composition pattern)
> 어떤 경우에 어떤 컴포넌트를 사용해야 할까?