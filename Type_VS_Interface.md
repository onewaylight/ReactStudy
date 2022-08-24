<h1 align="center">type vs interface</h1>

https://blog.logrocket.com/types-vs-interfaces-in-typescript/

<p>
The difference between types and interfaces in TypeScript used to be more clear, but with the latest versions of TypeScript, they’re becoming more similar.
</p>

```Interfaces``` are basically a way to describe data shapes, for example, an object.

```Type``` is a definition of a type of data, for example, a union, primitive, intersection, tuple, or any other type.

<h3>Way of Extends</h3>

```ts
interface AnimalInterface {
  species: string
  height: number
  weight: number
}

interface AfricaAnimal extends AnimalInterface {
  continent: string
}
```
```interface``` use to extend __extends__

```ts
type AnimalType = {
  species: string,
  height: number,
  weight: number
}

type AfricaAnimal = AnimalInterface & {
  continent: string
}
```
```type``` use __&__


# 선언적 확장 #
> interface에서 할 수 있는 대부분의 기능들은 type에서 가능하지만, 한 가지 중요한 차이점은 type은 새로운 속성을 추가하기 위해서 다시 같은 이름으로 선언할 수 없지만, interface는 항상 선언적 확장이 가능하다는 것입니다. 그 차이에 대한 예제가 아래 내용입니다.


```ts
interface Animal {
  weight: number;
}

interface Animal {
  height: number;
}

const tiger: Animal = {
  weight: 100,
  height: 200,
};
console.log(tiger);

type _Animal = {
  weight: number;
};

type _Animal = {//error : 식별자가 중복됨
  height: string;
};
```

>interface는 객체에만 사용이 가능하다
```ts
interface AnimalInterface {
  name: string
}

type AnimalType = {
  name: string
}

type AnimalOnlyString = string
type AnimalTypeNumber = number

interface X extends string {} // 불가능
```
interface는 리터럴타입으로 확장할 수 없다.

<hr />


>Computed Value의 사용
```
type names = 'firstName' | 'lastName'

type NameTypes = {
  [key in names]: string
}

const yc: NameTypes = { firstName: 'hi', lastName: 'yc' }

interface NameInterface {
  // error
  [key in names]: string
}
```
__reference__

https://yceffort.kr/2021/03/typescript-interface-vs-type
