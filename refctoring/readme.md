# 리팩토링

# 1. fetch 요청을 여러 개 해야 할 경우

다음과 같이 한 컴포넌트에 두 개 이상의 fetch 요청이 존재하는 경우 어떤 식으로 해결해야 할까?

```tsx
export default async function Home() {
  const responseA = await fetch("http://localhost:12345/book");
  const responseB = await fetch("http://localhost:12345/book/random");
  const allBooks: BookData[] = await responseA.json();
  const recoBooks: BookData[] = await responseB.json();
  

  console.log(allBooks);
  return (
    <div className={style.container}>
      <section>
        <h3>지금 추천하는 도서</h3>
        {recoBooks.map((book) => (
          <BookItem key={book.id} {...book} />
        ))}
      </section>
      <section>
        <h3>등록된 모든 도서</h3>
        {allBooks.map((book) => (
          <BookItem key={book.id} {...book} />
        ))}
      </section>
    </div>
  );
}
```

이렇게 코드가 작성되어 있으면 한 번에 컴포넌트의 로직을 판단하기가 쉽지 않다.

또한, 명확하게 어떤 작업을 수행하는 코드인지 확인하기 힘들기 때문에 각 fetch 요청에 해당하는 각 컴포넌트를 만들어서 불러오기 하는 방식으로 코드를 변경해야 한다.

```tsx
async function AllBooks() {
  const response = await fetch("http://localhost:12345/book");
  const allBooks: BookData[] = await response.json();
  console.log(allBooks);

  return (
    <div>
      {allBooks.map((book) => (
        <BookItem key={book.id} {...book} />
      ))}
    </div>
  );
}

async function RecoBooks() {

  const response = await fetch("http://localhost:12345/book/random");

  if (!response.ok) {
    throw new Error("Failed to fetch recommended books");
  }

  const recoBooks: BookData[] = await response.json();
  console.log(recoBooks);

  return (
    <div>
      {recoBooks.map((book) => (
        <BookItem key={book.id} {...book} />
      ))}
    </div>
  );
}

export default async function Home() {
  return (
    <div className={style.container}>
      <section>
        <h3>지금 추천하는 도서</h3>
        <AllBooks/>
      </section>
      <section>
        <h3>등록된 모든 도서</h3>
        <RecoBooks/>
      </section>
    </div>
  );
}
```

여기서 한 가지 더 변경할 부분이 있다.

바로, 반복적으로 재사용되는 fetch 요청 코드를 따로 함수로 만드는 부분이다.

백엔드 주소가 변경될 경우 일일이 바꿔야 한다는 문제가 존재하므로 환경변수로 바꿔주면 좋다.