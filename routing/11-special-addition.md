# 💡 프로젝트 구현에 도움이 될만한 것들

## 💡 개인 폴더

폴더 이름 앞에 언더바(_) 를 붙이면 비공개 모드로 된다. 즉, 해당 폴더의 하위 폴더와 파일들은 모두 라우팅에서 제외된다. 

괄호 안에 폴더 이름을 작성해서 라우트 그룹에 포함되지 않도록 하는 것과 비슷하지만, 둘의 차이점을 명확히 알고 있어야 한다.

| 종류 | (folder) | _folder |
| --- | --- | --- |
| 기능 | 하위 폴더와 파일은 라우팅 가능 | 하위 폴더와 파일 모두 라우팅 제외 |
| 목적 | 폴더 그룹화 | 폴더와 파일 비공개(Private) |

## 기능 별 폴더 분류

- /app/components
- /app/lib
- /app/ui
- /app/utils
- /app/hooks
- /app/styles

## 추가적인 것들

### 💡 다양한 폴더, 파일 생성 규칙들이 정리된 문서

https://nextjs.org/docs/getting-started/project-structure#app-routing-conventions

### 💡 모듈 경로 별명

NextJS에서 제공하는 import를 쉽게 만들어주는 기술 (`@`)

```jsx
// before
import { Button } from '../../../components/button'
 
// after
import { Button } from '@/components/button'
```