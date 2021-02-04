# learning-TypeScript

learning TypeScript

## 型を利用しよう

### コアな型

number = 1, 5.3, -10
string = 'Hi', "Hi", `Hi`
boolean = true, false

```
function add(n1:number, n2:number) {
  return n1 + n2
}

const number1 = '5';
const number2 = 2.8;
const result = add(number1, number2);
console.log(result) // 7.8
```

## js でも型チェックできる

```
function add(n1:number, n2:number) {
  if(typeof n1 !== 'number' || typeof n2 !== 'number') {
    throw new Error('数値が正しくありません')
    // ここでエラーでる
  }
  return n1 + n2
}

const number1 = '5';
const number2 = 2.8;
const result = add(number1, number2);
console.log(result) // 7.8
```

## 型の大文字・小文字

String ×

string ○

Number ×

number ○

## number, string, boolean の使い方

```
function add(n1:number, n2:number, showResult: boolean, phrase: string) {
  const result = n1 + n2
  if (showResult) {
    console.log(phrase + result)
  }

  return result
}

const number1 = 5;
const number2 = 2.8;
const printResult = true;
const resultPhrase = 'Result: ';

const result = add(number1, number2, printResult, phrase);
```

## 型の指定と型推論

変数は型推論してくれるので、型を明示的に示す必要はない。

```
const a1 = 1;
// a1 は number

let a2 = 1
a2 = 'hello'
// これはエラーになります。

let a3: string;
// これはany型になるので型を指定してあげる。
a3 = 'hello'
```

## Object 型

object = {age: 30}

```
const person = {
  name: 'test',
  age: 30
}

console.log(person.nickname) // error
```

こうやって明示的に書けるけど推論に任せた方がいいよ

```
const person: {name:string, age:number} = {
  name: 'test',
  age: 30
}
```

### 練習

```
type hoge = {
  id: string;
  price: number;
  tags: string[]
  details: {
    title: string;
    description: string;
  }
}

const product:hoge = {
  id: 'abc1',
  price: 12.99,
  tags: ['great-offer', 'hot-and-new'],
  details: {
    title: 'Red Carpet',
    description: 'A great carpet - almost brand-new!'
  }
}
```
