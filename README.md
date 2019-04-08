# typechain

Learning Typescript by making a Blockchain with it

## Configuration

### tsconfig.json 파일 생성

- [`node.js`](https://nodejs.org/ko/)는 [`TpyeScript`](https://www.typescriptlang.org/)를 이해하지 못하기 때문에 `JaveScript` 코드로 컴파일하는 작업이 필요하다.

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "ES2015",
    "sourceMap": true
  },
  "include": ["index.ts"],
  "exclude": ["node-modules"]
}
```

### 컴파일 옵션

- **module:** [`node.js`](https://nodejs.org/ko/)를 평범하게 사용하고 다양한 걸 `import`하거나 `export`할 수 있게 만드는 것.
- **target:** 어떤 버전의 자바스크립트로 컴파일 시킬 것인지 적음.
- **sourceMap:** `true`로 설정해서 sourcemap 처리를 할 것인지 알려줌.

### 컴파일 과정

- **include:** `index.ts`를 생성하고 추가.
- **exclude:** `node-modules`를 추가. (디폴트로 제외하는 것이 좋음)

## TypeScript의 시작

```sh
tsc
```

- **tsc:** 터미널에서 tsc를 입력. `index.ts`의 코드를 컴파일해서 `index.js`/`index.js.map`을 만들어 준다.

```json
"scripts": {
    "start": "node index.js",
    "prestart": "tsc"
  }
```

> `tsc` 명령어 대신 `yarn start`를 사용하기 때문에 package.json을 다음과 같이 설정한다.

### TypeScript의 마법

#### Typed

- 어떤 종류의 변수와 데이터인지 설정해줘야 한다.

```sh
string, boolean, number[array]
```

#### TypeScript의 기능

- TypeScript에서 변수와 함수를 생성하여 매개변수 값을 넘겨준다.

```ts
const name = "Suwan",
  age = 24,
  gender = "male";

const sayHi = (name, age, gender) => {
  console.log(`Hello ${name}, you are ${age}, you are a ${gender}`);
};

sayHi(name, age, gender);

export {};
```

> `export {};` 가 없으면 에러가 나기 때문에 꼭 적어 넣는다.

위 `sayHi` 함수의 arguments `gender`를 제거하고 다시 실행을 해보면 아래와 같은 에러가 발생한다.

[![](img/error.png)]

이 에러를 해석해보면, `나는 3개의 args가 올 것을 예상했지만, 2개 밖에 얻지 못했어. 그래서 난 이 함수를 실행하지 않을거야!` 라는 뜻이다.

이것이 TypeScript가 보호해주는 방법이다. 바로 나의 멍청한 실수들로부터 말이다.

그리고 다음과 같이 `gender` 뒤에 `?`를 붙여주게 되면 다시 실행이 된다.

```ts
const sayHi = (name, age, gender?)
```

`gender` 파라미터는 필수적이 아니라 선택적으로 바뀌게 되면서 다음과 같은 결과가 나온다.

```sh
Hello Suwan, you are 24, you are a undefined
```

> 대단한 기능이다..!! 이것이 바로 TypeScript의 장점

#### Types

인수에 데이터값을 지정하기.

```ts
const sayHi = (name: string, age: number, gender: string) => {
  console.log(`Hello ${name}, you are ${age}, you are a ${gender}`);
};

sayHi("Suwan", 28, "male");

export {};
```

> `name: string, age: number, gender: string`

인수에 데이터값을 지정해줌으로써 실수를 줄이고 본인이 만드는 코드를 더 예측하기 쉽도록 한다.

그리고 sayHi 함수에 마우스를 올려보면 `...) => void` 라고 나오는데, `void`는 빈공간이라는 뜻이다.

#### 자동 새로고침

- `yarn start`로 새로고침 하는게 지겹다. 다음과 같이 설정해준다.

```sh
yarn add tsc-watch --dev
```

위와 같이 인스톨한다.

- 그 다음 package.json 의 script에서 아래와 같이 설정해준다
- src파일과 dist파일을 새로 생성 후, tsconfig.json도 아래와 같이 설정해준다.

##### package.json 설정

```json
"scripts": {
    "start": "tsc-watch --onSuccess \" node dist/index.js\" "
  }
```

##### tsconfig.json 설정

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "ES2015",
    "sourceMap": true,
    "outDir": "dist"
  },
  "include": ["src/**/*"],
  "exclude": ["node-modules"]
}
```

> `index.ts` 파일을 src에 옮겨준다. 그리고 `index.js`/`index.js.map`을 삭제. 그리고 dist파일을 ignore해준다.

- **error가 발생하면 [`typescript`](https://www.typescriptlang.org/)를 다시 한 번 설치해준다**

```sh
yarn add typescript
```

> `tsc-wacth`를 설치하면서 typescript가 지워진 것 같다...(왜지?)

### TypeScript Object

#### Object를 넘겨줘볼까

함수에 `Object`를 넘겨주고 싶다면, `TypeScript`가 `Object`를 이해할 수 있게 해야 되고, 그 `Object`가 올바른 `type`인지 아닌지 분별하게끔 해야 된다.

위의 예제를 사용해 예를 들어보자.

```ts
const person = {
  name: "Suwan",
  age: 28,
  gender: "male"
};
```

> 위와 같이 `person` 변수를 생성하고 값을 넣어준다.

이제 `person`을 `sayHi`함수에 넘겨주면 어떻게 될까?

```ts
console.log(sayHi(person));
```

> 물론 동작하지 않는다. 위에서도 말했듯이 `TypeScript`는 3개의 args가 올 것을 예상했는데, 내가 1개만 주니까 빡쳐서 실행을 하지 않는 것이다.

이제부터 `interface` 어떻게 생성하는지 봐보자.

```ts
interface Human {
  name: string;
  age: number;
  gender: string;
}
```

> 임의적으로 `Human`으로 했지만 뭐든지 상관없다. 여기서 아까 전과 같이 작업. `name: string, age: number, gender: string`

그리고 다음과 같이 작업해준다.

```ts
const sayHi = (person: Human): string => {
  return return `Hello ${person.name}, you are ${person.age}, you are a ${
    person.gender
  }`;
}
```

> 이렇게 해주면 처음과 같이 멋지게 동작할 것이다. (참고로 JS에서는 동작하지 않는다.)

`sayHi`의 인수에 `person`을 넘겨주고 타입은 `Human`으로 설정한다.

다음으로 `person`을 부르면 함수와 연관한 `interface`가 있으므로, `age, gender, name` 속성을 갖고 있는 걸 알 수 있다.

이전과 똑같이 동작하지만 이제 나의 `Object`는 더욱 예측할 수 있다. `interface` 있기 때문이다. 예를 들어, 블록체인의 경우 하나의 block을 `interface`로 정의할 수 있게 된다.
