# learning-TypeScript

learning TypeScript

# 基本と型

## 型を利用しよう

### コアな型

数字や文字列や真偽はよく使う型です。
例えばこんな感じ。
number は 1, 5.3, -10

string は 'Hi', "Hi", `Hi`

boolean は true, false

```
function add(n1:number, n2:number) {
  return n1 + n2
}

const number1 = 5;
const number2 = 2.8;
const result = add(number1, number2);
console.log(result) // 7.8
```

### js でも型チェックはできる

typescript を使わず javascript だけでも型チェックは可能です。
ただ、typeof を駆使してあちこちで型チェックをするのはとても面倒です。
そこで TypeScript の出番です。

```
function add(n1:number, n2:number) {
  if(typeof n1 !== 'number' || typeof n2 !== 'number') {
    throw new Error('数値が正しくありません');
  }
  return n1 + n2
}

const number1 = 5;
const number2 = 2.8;
const result = add(number1, number2);
console.log(result) // 7.8
```

## 型の大文字・小文字

型の指定はすべて小文字で記述します。

String ×

string ○

Number ×

number ○

## number, string, boolean の使い方

変数名:型 = 値
こんな感じです。

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

変数の型推論してくれた部分は型を明示的に示す必要はありません。

```
const a1 = 1;
// a1 は number

let a2 = 1
a2 = 'hello'
// これは指定した型と代入する値が違うのでエラーになります。

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

## Array 型

```
const person = {
  hobbies: ['Sports', 'Cooking']
}

for ( const hobby of person.hobbies) {
  console.log(hobby) // Sports, Cooking
}

```

## Tuple 型

幾つの要素があるべきか。
またその要素の型はなにかを指定する。
push できてしまうので注意。

```
const person:{
  hobbies: string[];
  role:[number, string] // これがTuple型
} = {
  hobbies: ['Sports', 'Cooking'],
  role:[2, 'author']
}
```

role の型推論は(string|number)[]になっていて、他の値を入れられてしまう
role:[number, string]することで一つ目は number, 二つ目は string になる

## Enum 型

Enum 型は定数の集合に名前を付けて管理する
列挙型ともいうよ
複数の定数が出てきたら使った方が便利な事が多い

```
enum Role {
  ADMIN,
  READ_ONLY,
  AUTHOR
}

const person = {
  hobbies: ['Sports', 'Cooking'],
  role: Role.ADMIN
}
```

## any 型

なんでも入れられる。なるべく使わない方がいい

## Union 型

input1: number|string
のように A or B を指定する型。

```
function combine(input1:number|string ,input2:number|string) {
  let result:number|string;

  if (typeof input1 === 'number' && typeof input2 === 'number'){
    result = input1 + input2
  } else {
    result = input1.toString() + input2.toString()
  }
  return result;
}

const combinedAges = combine(36, 26);
console.log(combinedAges) // 62

```

## Literal 型

値そのものを厳密に指定することです。
type には admin, user, guest の文字列しか受け付けない状態です。

これは Literral 型と Union 型の合わせ技

```
const type : 'admin' | 'user' | 'guest'
```

## 型エイリアス / カスタム型

何度も同じような型を記述するのは面倒なのでエイリアスを使うと便利です。
type で記述することで複数回利用できます。

```
type Combinable = number | string;

hoge: Combinable = 'hoge';
```

## 関数の型

関数の()の後に指定するのが関数の戻り値の型
型推論で住む場合は指定する必要がない。

```
funciton add(n1: number, n2:number):number {
  return n1 + n2;
}
```

### void 型について

戻り値がない場合は void 型を指定する
void の関数は undefined を返す
undefined を返却するかと言って:undefined を指定する必要はなく、void 型を利用する

```
funciton printResult(num: number):void {
  console.log('Result:' + num)
}
printResult(15);
```

## 関数型を明確に指定する

```
let combineValues;
// 現時点でany型

let combineValues: Function;
// これでもまだ不完全。関数ならなんでも入るから。

let combinevalues:(a:number, b:number) => number;
// これでadd関数だけ入れられる型になる

combineValues = add;
combineValues = 5 // anyなので代入できてしまう
console.log(combineValues(8,8));
```

## 関数型とコールバック

戻り値に void 型を指定すると戻り値があってもそれを関数ないで利用しないという意味になるので
仮にコールバック関数で return をしてもエラーになることはない。

```
function addAndHandle(n1: number, n2: number, c:(num:number) => void) {
  const result = n1 + n2;
  cb(result)
}

addAndHandle(10, 20, (result) => {
  console.log(result);
})

```

## unknown 型

最終的にどんな型になるかわからない型。

```
let userInput: unknown;
let userName: string;

if ( typeof === 'string') {
  userName = userInput; //エラーにならない
}
userName = userInput; //エラーになる
```

## never 型

関数の戻り値として利用できる
絶対になにも返さないという型

```
function generateError(mes:string, code: number ):void {
  throw {mes: mes, errorCode:code}
}

const result = generateError('error', 500)
console.log(result);
```

# 設定とコンパイラ

コンパイルは tsc コマンドを行っていた。

## watch モードについて

今までは

```
tsc test.ts
```

で都度コンパイルしてました。
watch モードを使うと変更があれば自動でコンパイルできる

```
tsc test.ts --watch or -w
```

これは一つのファイルしか watch できない

## 全体のコンパイル方法

まずはじめに下記を実行する

```
tsc --init
// ここがプロジェクトのフォルダだよと知らせる rootで実行する
```

tsconfig.json が作られる

```
// これでtscするとtsファイルをすべてコンパイルしてくる
tsc
// watchモードをすることもできる
tsc --watch or -w
```

## ファイルの Inclued & Exclude の設定

exclude に配列で指定するとコンパイル対象から除外できる。
exclude を指定する場合は node_modules を入れる必要がある

```
tsconfig.json
exclude:['*.dev.ts', 'hoge.ts', '**/*.dev.ts', 'node_modules']
```

include に配列で指定するとコンパイル対象の指定できる。
指定した物以外は除外される。
exclude と include は同時に使える。その場合は include の中から exclude を除外してコンパイルされる

```
include:['test.ts']
```

特定のファイルだけコンパイルしたい。
それ以外はコンパイルしたくない。そんな時は files を利用する

```
files:['test.ts']
```

## コンパイルターゲットの指定

| 項目                             | 備考                                                                                                                                            |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| target                           | どのバージョンで ts をコンパイルするか                                                                                                          |
| module                           |                                                                                                                                                 |
| lib                              | どのような機能やオブジェクトが使えるかを設定する。コメントアウトしてる場合はデフォルトは target の指定で決まる                                  |
| allowjs                          | js もコンパイルできるようになる。                                                                                                               |
| checkJs                          | js ファイルの文法チェックをしてくれる。                                                                                                         |
| jsx                              | react で必要になる                                                                                                                              |
| declaration                      | ライブラリで公開したい場合に必要 .d.ts を作る                                                                                                   |
| sourceMap                        | ソースマップを生成するか                                                                                                                        |
| outFile                          | ファイルを１つにまとめて出力する                                                                                                                |
| outDir                           | js の出力先の設定 ./dist 　など。src フォルダの構造を dist にも反映させる                                                                       |
| rootDir                          | ソースが配置させる root を設定する ./src など。ここがコンパイルの対象だよ。                                                                     |
| removeComment                    | コメントを削除するかどうか                                                                                                                      |
| noEmit                           | js を出力しないようにする                                                                                                                       |
| downlevelIteration               | 古いブラウザで for ループを正常に動くようにする。基本 OFF でいい                                                                                |
| noEmitOnError                    | コンパイルエラーがある場合は js を出力しない                                                                                                    |
| strict                           | 厳格な型チェックをする                                                                                                                          |
| noImplicitAny                    | any を許すか許さないか                                                                                                                          |
| strictNullChecks                 | null かもしれないオブジェクトを参照するときに null の可能性がある場合はエラーを出す。null じゃないのが確定しているなら!を付けるのがお手軽な対応 |
| strictFunctionTypes              | 保留                                                                                                                                            |
| strictBindCallApply              | bind, call apply を使うときに正しく型が定義してないとエラー                                                                                     |
| noUnusedLocals                   | 使ってない変数がある場合にエラー                                                                                                                |
| noUnusedParameters               | 使ってないパラメータがある場合はエラー                                                                                                          |
| noImlicitReturns                 | 分岐がある場合、すべての分岐でちゃんとリターンしてないとエラー                                                                                  |
| forceconsistentcasinginfilenames | ファイルの大文字小文字を判別する                                                                                                                |

## VSCode でデバッグ

ESLint, Prettier, Debugger for Chrome,

# JS と TS

## let と const

### const

定数を定義します。
定数なので値を変更できません。

### let

変数を定義します。
変数なので値を変更できます。

## アロー関数

```
const add = (a:number, b:number) => {
  return a + b;
}
const add2 = (a:number, b:number) => a + b;
const add3: (s:string) => string = s => `my name is ${s}`
console.log(add(1,2));
```

## デフォルト関数パラメータ

左側だけにデフォルトパラメータは設定できないよ

```
const add = (a:number, b:number = 2) => a + b;
```

## スプレッドオペレータ

```
const hobbies = ['Sports', 'cooking'];
const activehobbies = ['Hiking'];
activehobbies.push(...hobbies)

const person = {
  name: "make",
  age: 30
}

const copiedPerson = {...person};

```

## レストパラメータ

```
const add = (...numbers:number[]) => {
  let result = 0;
  return numbers.reduce((curResult, curValue) => {
    return curResult + curValue
  }, 0)
}

const addedNumbers = add(5,10,2,43)
console.log(addedNumbers)

```

## 配列とオブジェクトの分割代入

```
const hobbies = ['Sports', 'cooking'];
const [hobby1, hobby2, ...hoge] = hobbies;
```

```
const person = {
  nickName: "make",
  age: 30
}

const {nickName:name2, age} = person;
// nickNameではなくname2で受け取ることもできる。
// :は型指定の:ではない。
```

# クラスとインターフェイス

## クラスとは

### オブジェクトとは

プログラミングコードで扱うもの

クラスのインスタンスである

オブジェクトリテラルの定義の代わりにオブジェクトを作成する手段

### クラス

オブジェクトの設計図

オブジェクトの構造を定義する

## 最初のクラス

```
class Department {
  name: string;

  constructor(n: string){
    this.name = n;
  }
}

const accounting = new Department('Accounting');
console.log(accounting);
```

## コンストラクタ関数と this

```
class Department {
  name: string;

  // コンストラクタ
  constructor(n: string){
    this.name = n;
  }

  // メソッド
  describe() {
    console.log("Department: " + this.name)
  }
  // メソッド2
  // ここにthisを持たせることでDepartmentクラスからしか利用できないようにしてる
  describe2(this:Department) {
    console.log("Department: " + this.name)
  }
}

const accounting = new Department('Accounting');
console.log(accounting);
accounting.describe();

const copyAccounting = { describe: accounting.discribe }
copyAccounting.sescribe(); // undefined

const copyAccounting2 = { name: 'DUMMY', describe2: accounting.discribe2 }
copyAccounting.sescribe2(); // DUMMY
```

## private and public

```
class Department {
  name: string;
  employees: string[] = [];

  // コンストラクタ
  constructor(n: string){
    this.name = n;
  }

  // メソッド
  describe() {
    console.log("Department: " + this.name)
  }

  addEmployee(employee: string) {
    this.employees.push(employee)
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees)
  }
}
const accounting = new Department('Accounting');
accounting.addEmployee("Asan");
accounting.addEmployee("Bsan");
accounting.printEmployeeInformation(); // 2, [Asan, Bsan]
accounting.describe(); // Accounting
```

このままだと値をメソッドを使わずに変更できてしまいます。

accounting.employees[2] = "Csan";

それを防ぐために private を利用します。

```
private employees: string[] = [];
```

public はデフォルトの設定でどこからでもアクセスできる。

## プロパティの初期化とショートカット構文

```
class Department {
  employees: string[] = [];

  // コンストラクタ
  constructor(private id:string, public name: string){
    // プライベートのidとパブリックのnameのプロパティを作れる。
  }

  // メソッド
  describe() {
    console.log(`Department ${this.id} ${this.name}`)
  }

  addEmployee(employee: string) {
    this.employees.push(employee)
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees)
  }
}
const accounting = new Department('d1','Accounting');
accounting.describe(); // 1 Accounting
```

## readonly プロパティ

読み取り専用になり、変更しようとするとエラーになる。

```
class Department {
  employees: string[] = [];

  // コンストラクタ
  constructor(private readonly id:string, public name: string){
    // プライベートのidとパブリックのnameのプロパティを作れる。
  }

  // メソッド
  describe() {
    console.log(`Department ${this.id} ${this.name}`)
  }

  addEmployee(employee: string) {
    this.employees.push(employee)
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees)
  }
}
const accounting = new Department('d1','Accounting');
accounting.describe(); // 1 Accounting
```

## 継承

```
class Department {
  employees: string[] = [];

  // コンストラクタ
  constructor(private readonly id:string, public name: string){
    // プライベートのidとパブリックのnameのプロパティを作れる。
  }

  // メソッド
  describe() {
    console.log(`Department ${this.id} ${this.name}`)
  }

  addEmployee(employee: string) {
    this.employees.push(employee)
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees)
  }
}

// 継承
class ITDepartiment extends Department {
  // admins: string[];

  constructor(id: string, private admins: string[]) {
    // 継承しているのでsuperを使ってベースクラスのコンストラクタを実行する
    super(id, 'IT');
    this.admins = admins;
  }
}
```

## プロパティのオーバーライドと protected

```
class Department {
  private employees: string[] = [];
  // このクラスと継承したクラスからアクセスできる。
  protected employees: string[] = [];

  // コンストラクタ
  constructor(private readonly id:string, public name: string){
    // プライベートのidとパブリックのnameのプロパティを作れる。
  }

  // メソッド
  describe() {
    console.log(`Department ${this.id} ${this.name}`)
  }

  addEmployee(employee: string) {
    this.employees.push(employee)
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees)
  }
}

class ITDepartiment extends Department {
  // admins: string[];

  constructor(id: string, private admins: string[]) {
    // 継承しているのでsuperを使ってベースクラスのコンストラクタを実行する
    super(id, 'IT');
    this.admins = admins;
  }

  // 継承元のメソッドを上書きして定義できる（オーバーライド）
  addEmployee(name:string) {
    if(name === 'Max') {
      return
    }
    // empolyeesはpravateなのでアクセスできない。
    // 継承したクラスからアクセスしたい場合は protectedを使う。
    this.employees.push(name);
  }
}
```

## Getter and Setter

```
class AccountingDepartment extends Department {
  prrivate lastReport: string;

  get mostRecentReport() {
    if ( this.lastReport) {
      return this.lastReport;
    }
    throw new Error('レポートがありません')
  }

  set mostRecentReport(value: string){
    if(value) {
      return new Error('正しい値を設定してください')
    }
    this.addReport(value);
  }

  constructor(id: string, private admins: string[]) {
    super(id, 'Accounting');
    this.admins = admins;
    this.lastReport: reports[0];
  }

  addReport(text: string) {
    this.reports.push(text)
    this.lastReport = test;
  }
}
const accounting = new AccountingDepartment('d2', [])
console.log(accounting.mostRecentReport)
accounting.addReport('SomeThings')
accounting.mostRecentReport('レポート');

```

## static メソッドとプロパティ

Math.random()などは Math の random メソッドですので、
本来なら Math のインスタンスを生成してそこからアクセししないと利用できないはずですが、
それをしなくても Math.random()は利用できます。
これが static メソッドです。
また、インスタンスからアクセスはできません。

```
class Department {
  // staticプロパティ
  static fiscalYear = 2021;
  private employees: string[] = [];
  // このクラスと継承したクラスからアクセスできる。
  protected employees: string[] = [];

  // staticメソッド
  static createEmployee(name: string) {
    return {name: name};
  }

  // コンストラクタ
  constructor(private readonly id:string, public name: string){
    // プライベートのidとパブリックのnameのプロパティを作れる。

    //エラーになる
    console.log(this.fiscalYear)
    // アクセスしたい場合は
    console.log(Department.fiscalYear)
  }

  // メソッド
  describe() {
    console.log(`Department ${this.id} ${this.name}`)
  }

  addEmployee(employee: string) {
    this.employees.push(employee)
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees)
  }
}

const employee1 = Department.createEmployee('Max', Departiment.fiscalYear); // Max, 2021
```

## abstract クラス

継承元のメソッドなどを継承した先で強制的にオーバーライドする必要があるときに
abstract を定義することで継承先でオーバーライドを必須にすることができる。

継承元で共通のメソッドや値を定義して詳しい実装は継承先に任せることができます。
abstract されたクラスはインスタンス化できません
継承専用のクラスになります。

```
abstract class Department {
  protected employees: string[] = [];

  // コンストラクタ
  constructor(private readonly id:string, public name: string){
  }

  // メソッド
  abstract describe(this: Department) : void;
}

class B extends Department {
  describe() {
    console.log('オーバーライドしたよ')
  }
}
```

## シングルトンと praivate コンストラクタ

常に１つのオブジェクトしか存在しない状態にするのがシングルトン
これは static が使えない環境だと便利です。

このクラスは一つだけにしたい場合

```
class AccountingDepartment extends Department {
  prrivate lastReport: string;
  private static instance: AccountingDepartment;

  get mostRecentReport() {
    if ( this.lastReport) {
      return this.lastReport;
    }
    throw new Error('レポートがありません')
  }

  set mostRecentReport(value: string){
    if(value) {
      return new Error('正しい値を設定してください')
    }
    this.addReport(value);
  }

  // プライベートコンストラクタにする
  private constructor(id: string, private admins: string[]) {
    super(id, 'Accounting');
    this.admins = admins;
    this.lastReport: reports[0];
  }

  static getInstance() {
    if ( this.instance ) {
      return this.instance
    }
    this.instanc = new AccountingDepartment('D2', []);
    return this.instanc;
  }

  addReport(text: string) {
    this.reports.push(text)
    this.lastReport = test;
  }
}

// エラーになる。
new AccountingDepartment('d2', [])
// OK
const accounting = AccountingDepartment.getInstance();

```

## インターフェース

オブジェクトがどんな形であるかを表します。

```
interface Person {
  name: string;
  age: number;
  greet(phrase: string): void;
}

let user1: Person;
user1 = {
  name: 'Max',
  age: 30,
  greet: (phrase:string) => {
    console.log(phrase + this.name)
  }
}
```

## クラスでのインターフェイスを実装

interface はオブジェクトの構造だけを記述することができる。

inteface を使うことでオブジェクトを定義したいという意図が伝えられる

クラスのインターフェイスは inteface, type どちらも利用できる。

```
interface Greetable = {
  name: string;
  greet(phrase: string): void;
}

class Person implements Greetable {
  name: string;
  age = 30;

  constructor(n:string) {
    this.name = n;
  }

  greet(phrase:string) {
    console.log(phrase + this.name)
  }
}

let user1: Greetable;
user1 = {
  name: 'Max',
  age: 30, // エラー
  greet: (phrase:string) => {
    console.log(phrase + this.name)
  }
}

```

## 読み取り専用インターフェイスプロパティ

初期化のときに一度だけ設定できるプロパティになる

```
interface Greetable {
  readonly name: string;
}
```

## interface の拡張

```
interface Named {
  readonly name: string;
}

interface Greetable extends Named {
  greet(phrase: string): void;
}
```

## 関数型としてのインターフェイス

```
// こっちの方が一般的に
type addFn = (a: number, b: number) => number;

let add:addFn;
add = (n1: number, n2:number) => {
  return n1 + n2;
}

interface AddFn2 {
  (a:number, b:number): number;
}
```

## 任意のパラメータとプロパティ

```
interface Named {
  // FIXME: readonly name?: string;
  readonly name: string;
  outputName?: string; // これは任意のプロパティにする
}

interface Greetable extends Named {
  greet(phrase: string): void;
}

class Person implements Greetable {
  name?: string; // クラスにも任意のプロパティにできる。
  // このままではエラーになる。なぜならNamedのnameが任意になってないから。
  age = 30;

  constructor(n:string) {
    if (n) {
      this.name = n;
    }
  }

  greet(phrase:string) {
    console.log(phrase + this.name)
  }
}
```
