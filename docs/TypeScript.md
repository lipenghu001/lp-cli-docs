# TypeScript

## 基础知识

```ts
let isDone: boolean = false
let age: number = 10

let firstName : string = 'viking' 
let message: string = `Hello, ${firstName}`

// 这俩是所有其他类型的子类型
let u: undefined = undefined
let n: null = null

let num: string = undefined

// 尽量不定义为any
let notSure: any = 4
notSure = 'maybe a string'
notSure = true

notSure.myName
notSure.getName()


// array 
let arrOfNumbers: number[] = [1, 2, 3, 4] 
arrOfNumbers.push(3)

// tuple
let user : [string, number] = ['viking', 20]

// function
function add( x: number, y: number, z?: number ): number {
  return x + y
}

let result = add(2, 3, 5)

let add2 = (x: number, y: number ): number => {
  return x + y 
}

const add3: (x: number, y: number ) => number = add2

// type inference
let str = 'str'
// str = 1
```



## 接口

- 普通接口

```ts
/**
 * 不用接口时
 * @param labelledObj 
*/
// function printLabel(labelledObj: { label: string }) {
//   console.log(labelledObj.label);
// }

// let myObj = { size: 10, label: "Size 10 Object" };
// printLabel(myObj);

/**
 * 接口方式重写
*/
// interface LabelledValue {
//   label: string;
// }

// function printLabel(labelledObj: LabelledValue) {
//   console.log(labelledObj.label);
// }

// let myObj = {size: 10, label: "Size 10 Object"};
// printLabel(myObj);

/**
 * 可选属性
*/
// interface SquareConfig {
//   color?: string;
//   width?: number;
// }

// function createSquare(config: SquareConfig): {color: string; area: number} {
//   let newSquare = {color: "white", area: 100};
//   if (config.color) {
//     newSquare.color = config.color;
//   }
//   if (config.width) {
//     newSquare.area = config.width * config.width;
//   }
//   return newSquare;
// }

// let mySquare = createSquare({color: "black"});

interface Person {
  name : string ;
  age ?: number ;
  readonly id?: number ;
}
let viking: Person = {
  name: 'viking',
  age: 20,
  id: 2,
}
// viking.id = 3

const sum = (x: number, y: number) => {
  return x + y
}
// 描述函数
interface ISum {
  (x: number, y: number ): number
}
const sum2 : ISum = sum

interface RandomMap {
  [propName: string]: string;
}

const test: RandomMap = {
  a: 'hello',
  b: 'test',
}
interface LikeArray {
  [index: number]: string
}

const likeArray: LikeArray = ['1', '2', '3']

// duck typing
interface FunctionWithProps {
  (x: number): number;
  name: string;
}
const a: FunctionWithProps = (x: number) => {
  return x
}
a.name = 'abc'

```

- 类与接口

```ts
//类的基本定义
// public private protected
class Animal {
  protected name: string ;
  constructor(name: string) {
    this.name = name
  }
  run() { 
    console.log('111');
    return `${this.name} is running`
  }
}
const snake = new Animal('lily')
// snake.run()
// snake.name

//继承
class Dog extends Animal {
  bark( ) {
    return `${this.name} is barking`
  }
}
const xiaobao = new Animal('xoaobao')
// xiaobao.run()
// xiaobao.bark()

// 多态
class Cat extends Animal {
  constructor(name: any) {
    super(name)
    console.log(this.name);
  }
  run() {
    return 'Meow, ' + super.run()
  }
}
const maomao = new Cat(222)
// maomao.run()

// implements
interface ClockInterface {
  currentTime: number;
  alert(): void;
}

interface ClockStaticInterface {
  new (h: number, m: number): void;
  // time: string;
}

interface GameInterface {
  play(): void;
}

const Clock: ClockStaticInterface = class Clock implements ClockInterface {
  constructor(h: number, m: number) {

  }
  static time = 12
  currentTime: number = 123;
  alert() {

  }
}

class Cellphone implements ClockInterface, GameInterface {
  currentTime: number = 456;
  alert() {

  }
  play() {

  }
}

```



## 泛型

1. 函数与泛型

```ts
// 没有泛型时，类型推断无法流动到函数中

function echo<T>(arg: T): T {
  return arg;
}

function swap<T, U>(tuple: [T, U] ): [U, T] {
  return [tuple[1], tuple[0]]
}

// const result = echo(123)
const result = swap(['string', 123])

interface GithubResp {
  name: string;
  count: number;
}

interface CountryResp {
  name: string;
  area: number;
  population: number;
}

function withAPI<T>(url: string): Promise<T> {
  return fetch(url).then(resp => resp.json())
}

withAPI<GithubResp>('github.user').then(resp => {

})

withAPI<CountryResp>('github.user').then(resp => {

})
```

- 2. 接口与泛型

```tsx
interface FunctionComponent<P = {}> {
    (props: PropsWithChildren<P>, context?: any): ReactElement<any, any> | null;
    propTypes?: WeakValidationMap<P> | undefined;
    contextTypes?: ValidationMap<any> | undefined;
    defaultProps?: Partial<P> | undefined;
    displayName?: string | undefined;
}
```





## 高级特性

- 类型别名

```ts
let sum: (x: number, y: number) => number
const result = sum(1, 2)
type PlusType = (x: number, y: number) => number
let sum2: PlusType
sum2(1, 2)
```

- 交叉类型

```ts
interface IName {
  name: string;
}
type Iperson = IName & { age: number }
let person: Iperson = {name: 'aaa', age: 3}
```

- 联合类型

```ts
let numberOrString: number | string;
// numberOrString.length
// numberOrString = {}
```

- 类型断言

```ts
function getLength(input: number | string) {
  const str = input as string
  if (str.length) {
    return str
  } else {
    const number = input as number
    return number.toString().length
  }
}
```

- 字面量

```ts
const str = '123'
// let str2 = '123'
```

- 泛型约束

```ts
// extends in generics
interface IWithLength {
  length: number;
}
function echoWithArr<T extends IWithLength>(arg: T): T {
  console.log(arg.length);
  return arg
}
const arr = echoWithArr(['1', 2, 3])
const arr1 = echoWithArr('123')
const arr2 = echoWithArr({length: 123})
// const arr3 = echoWithArr(123)
```



- Partial及原理

```ts
interface Person {
  name: string;
  age: number;
}
type PersonOptional = Partial<Person>
let viking2: PersonOptional = { }
// Parcial原理
interface CountryResp {
  name: string;
  area: number;
  population: number;
}
type Keys = keyof CountryResp
// keyof
let key: Keys = 'area'
// lookup types
type NameType = CountryResp['name']
// mapped types
type Test = {
  [key in Keys]: any;
}
type CountryOpt = {
  [p in Keys]?: CountryResp[p]
}
```

- WeakValidationMap

```ts

```



## 总结

![image-20210915092219865](/Users/hulipeng/Library/Application Support/typora-user-images/image-20210915092219865.png)

