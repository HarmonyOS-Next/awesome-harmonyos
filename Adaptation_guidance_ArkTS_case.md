# TS 适配 ArtkTS 指导案例

本文通过更多应用场景中的案例，提供在 ArkTS 语法规则下将 TS 代码适配成 ArkTS 代码的建议。各章以 ArkTS 语法规则英文名称命名，每个案例提供适配前的 TS 代码和适配后的 ArkTS 代码。

## arkts-identifiers-as-prop-names

**应用代码**

```ts
interface W {
  bundleName: string
  action: string
  entities: string[]
}

let wantInfo: W = {
  bundleName: 'com.huawei.hmos.browser',
  action: 'ohos.want.action.viewData',
  entities: ['entity.system.browsable'],
}
```

**建议改法**

```ts
interface W {
  bundleName: string
  action: string
  entities: string[]
}

let wantInfo: W = {
  bundleName: 'com.huawei.hmos.browser',
  action: 'ohos.want.action.viewData',
  entities: ['entity.system.browsable'],
}
```

## arkts-no-any-unknown

### 按照业务逻辑，将代码中的`any, unknown`改为具体的类型

```ts
function printObj(obj: any) {
  console.log(obj)
}

printObj('abc')
```

**建议改法**

```ts
function printObj(obj: string) {
  console.log(obj)
}

printObj('abc')
```

### 标注 JSON.parse 返回值类型

**应用代码**

```ts
class A {
  v: number = 0
  s: string = ''

  foo(str: string) {
    let tmpStr = JSON.parse(str)
    if (tmpStr.add != undefined) {
      this.v = tmpStr.v
      this.s = tmpStr.s
    }
  }
}
```

**建议改法**

```ts
class A {
  v: number = 0
  s: string = ''

  foo(str: string) {
    let tmpStr: Record<string, Object> = JSON.parse(str)
    if (tmpStr.add != undefined) {
      this.v = tmpStr.v as number
      this.s = tmpStr.s as string
    }
  }
}
```

### 使用 Record 类型

**应用代码**

```ts
function printProperties(obj: any) {
  console.log(obj.name)
  console.log(obj.value)
}
```

**建议改法**

```ts
function printProperties(obj: Record<string, Object>) {
  console.log(obj.name)
  console.log(obj.value)
}
```

## arkts-no-call-signature

使用函数类型来替代。

**应用代码**

```ts
interface I {
  (value: string): void
}

function foo(fn: I) {
  fn('abc')
}

foo((value: string) => {
  console.log(value)
})
```

**建议改法**

```ts
type I = (value: string) => void

function foo(fn: I) {
  fn('abc')
}

foo((value: string) => {
  console.log(value)
})
```

## arkts-no-ctor-signatures-type

**应用代码**

```ts
class Controller {
  value: number = 0

  constructor(value: number) {
    this.value = value
  }
}

type ControllerConstrucotr = {
  new (value: number): Controller
}

class Menu {
  controller: ControllerConstrucotr = Controller
  createController() {
    if (this.controller) {
      return new this.controller(123)
    }
    return null
  }
}

let t = new Menu()
console.log(t.createController()!.value)
```

**建议改法**

```ts
class Controller {
  value: number = 0

  constructor(value: number) {
    this.value = value
  }
}

type ControllerConstrucotr = () => Controller

class Menu {
  controller: ControllerConstrucotr = () => {
    return new Controller(123)
  }

  createController() {
    if (this.controller) {
      return this.controller()
    }
    return null
  }
}

let t: Menu = new Menu()
console.log(t.createController()!.value)
```

## arkts-no-indexed-signatures

使用 Record 类型来替代。

**应用代码**

```ts
function foo(data: { [key: string]: string }) {
  data['a'] = 'a'
  data['b'] = 'b'
  data['c'] = 'c'
}
```

**建议改法**

```ts
function foo(data: Record<string, string>) {
  data['a'] = 'a'
  data['b'] = 'b'
  data['c'] = 'c'
}
```

## arkts-no-typing-with-this

**应用代码**

```ts
class C {
  getInstance(): this {
    return this
  }
}
```

**建议改法**

```ts
class C {
  getInstance(): C {
    return this
  }
}
```

## arkts-no-ctor-prop-decls

**应用代码**

```ts
class Person {
  constructor(readonly name: string) {}

  getName(): string {
    return this.name
  }
}
```

**建议改法**

```ts
class Person {
  name: string
  constructor(name: string) {
    this.name = name
  }

  getName(): string {
    return this.name
  }
}
```

## arkts-no-ctor-signatures-iface

**应用代码**

```ts
class Controller {
  value: number = 0

  constructor(value: number) {
    this.value = value
  }
}

interface ControllerConstrucotr {
  new (value: number): Controller
}

class Menu {
  controller: ControllerConstrucotr = Controller
  createController() {
    if (this.controller) {
      return new this.controller(123)
    }
    return null
  }
}

let t = new Menu()
console.log(t.createController()!.value)
```

**建议改法**

```ts
class Controller {
  value: number = 0

  constructor(value: number) {
    this.value = value
  }
}

type ControllerConstrucotr = () => Controller

class Menu {
  controller: ControllerConstrucotr = () => {
    return new Controller(123)
  }

  createController() {
    if (this.controller) {
      return this.controller()
    }
    return null
  }
}

let t: Menu = new Menu()
console.log(t.createController()!.value)
```

## arkts-no-props-by-index

可以转换成 Record 类型，用来访问对象的属性。

**应用代码**

```ts
import myRouter from '@ohos.router'
let params: Object = myRouter.getParams()
let funNum: number = params['funNum']
let target: string = params['target']
```

**建议改法**

```ts
import myRouter from '@ohos.router'
let params = myRouter.getParams() as Record<string, string | number>
let funNum: number = params.funNum as number
let target: string = params.target as string
```

## arkts-no-inferred-generic-params

**应用代码**

```ts
class A {
  str: string = ''
}
class B extends A {}
class C extends A {}

let arr: Array<A> = []

let originMenusMap: Map<string, C> = new Map(
  arr.map((item) => [item.str, item instanceof C ? item : null]),
)
```

**建议改法**

```ts
class A {
  str: string = ''
}
class B extends A {}
class C extends A {}

let arr: Array<A> = []

let originMenusMap: Map<string, C | null> = new Map<string, C | null>(
  arr.map<[string, C | null]>((item) => [
    item.str,
    item instanceof C ? item : null,
  ]),
)
```

**原因**

`(item instanceof C) ? item : null` 需要声明类型为`C | null`，由于编译器无法推导出`map`的泛型类型参数，需要显式标注。

## arkts-no-regexp-literals

**应用代码**

```ts
let regex: RegExp = /\s*/g
```

**建议改法**

```ts
let regexp: RegExp = new RegExp('\\s*', 'g')
```

**原因**

如果正则表达式中使用了标志符，需要将其作为`new RegExp()`的参数。

## arkts-no-untyped-obj-literals

### 从 SDK 中导入类型，标注 object literal 类型

**应用代码**

```ts
const area = {
  pixels: new ArrayBuffer(8),
  offset: 0,
  stride: 8,
  region: { size: { height: 1, width: 2 }, x: 0, y: 0 },
}
```

**建议改法**

```ts
import image from '@ohos.multimedia.image'

const area: image.PositionArea = {
  pixels: new ArrayBuffer(8),
  offset: 0,
  stride: 8,
  region: { size: { height: 1, width: 2 }, x: 0, y: 0 },
}
```

### 用 class 为 object literal 标注类型，需要 class 的构造函数无参数

**应用代码**

```ts
class Test {
  value: number = 1

  constructor(value: number) {
    this.value = value
  }
}

let t: Test = { value: 2 }
```

**建议改法 1**

```ts
// 去除构造函数
class Test {
  value: number = 1
}

let t: Test = { value: 2 }
```

**建议改法 2**

```ts
// 使用new
class Test {
  value: number = 1

  constructor(value: number) {
    this.value = value
  }
}

let t: Test = new Test(2)
```

**原因**

```ts
class C {
  value: number = 1

  constructor(n: number) {
    if (n < 0) {
      throw new Error('Negative')
    }
    this.value = n
  }
}

let s: C = new C(-2) //抛出异常
let t: C = { value: -2 } //ArkTS不支持
```

例如在上面的例子中，如果允许使用`C`来标注 object literal 的类型，那么上述代码中的变量`t`会导致行为的二义性。ArkTS 禁止通过 object literal 来绕过这一行为。

### 用 class/interface 为 object literal 标注类型，需要使用 identifier 作为 object literal 的 key

**应用代码**

```ts
class Test {
  value: number = 0
}

let arr: Test[] = [
  {
    value: 1,
  },
  {
    value: 2,
  },
  {
    value: 3,
  },
]
```

**建议改法**

```ts
class Test {
  value: number = 0
}
let arr: Test[] = [
  {
    value: 1,
  },
  {
    value: 2,
  },
  {
    value: 3,
  },
]
```

### 使用 Record 为 object literal 标注类型，需要使用字符串作为 object literal 的 key

**应用代码**

```ts
let obj: Record<string, number | string> = {
  value: 123,
  name: 'abc',
}
```

**建议改法**

```ts
let obj: Record<string, number | string> = {
  value: 123,
  name: 'abc',
}
```

### 函数参数类型包含 index signature

**应用代码**

```ts
function foo(obj: { [key: string]: string }): string {
  if (obj != undefined && obj != null) {
    return obj.value1 + obj.value2
  }
  return ''
}
```

**建议改法**

```ts
function foo(obj: Record<string, string>): string {
  if (obj != undefined && obj != null) {
    return obj.value1 + obj.value2
  }
  return ''
}
```

### 函数实参使用了 object literal

**应用代码**

```ts
;(fn) => {
  fn({ value: 123, name: '' })
}
```

**建议改法**

```ts
class T {
  value: number = 0
  name: string = ''
}

;(fn: (v: T) => void) => {
  fn({ value: 123, name: '' })
}
```

### class/interface 中包含方法

**应用代码**

```ts
interface T {
  foo(value: number): number
}

let t: T = {
  foo: (value) => {
    return value
  },
}
```

**建议改法 1**

```ts
interface T {
  foo: (value: number) => number
}

let t: T = {
  foo: (value) => {
    return value
  },
}
```

**建议改法 2**

```ts
class T {
  foo: (value: number) => number = (value: number) => {
    return value
  }
}

let t: T = new T()
```

**原因**

class/interface 中声明的方法应该被所有 class 的实例共享。ArkTS 不支持通过 object literal 改写实例方法。ArkTS 支持函数类型的属性。

### export default 对象

**应用代码**

```ts
import hilog from '@ohos.hilog'

export default {
  onCreate() {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Application onCreate')
  },
  onDestroy() {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Application onDestroy')
  },
}
```

**建议改法**

```ts
import hilog from '@ohos.hilog'

class Test {
  onCreate() {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Application onCreate')
  }
  onDestroy() {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Application onDestroy')
  }
}

export default new Test()
```

### 通过导入 namespace 获取类型

**应用代码**

```ts
import { BusinessError } from '@ohos.base'
import bluetooth from '@ohos.bluetooth'
let serverNumber = -1
function serverSocket(code: BusinessError, num: number) {
  console.log('bluetooth error code: ' + code.code)
  if (code.code == 0) {
    console.log('bluetooth serverSocket Number: ' + num)
    serverNumber = num
  }
}

let sppOption = { uuid: '', secure: false, type: 0 }
bluetooth.sppListen('', sppOption, serverSocket)
```

**建议改法**

```ts
import { BusinessError } from '@ohos.base'
import bluetooth from '@ohos.bluetooth'
let serverNumber = -1
function serverSocket(code: BusinessError, num: number) {
  console.log('bluetooth error code: ' + code.code)
  if (code.code == 0) {
    console.log('bluetooth serverSocket Number: ' + num)
    serverNumber = num
  }
}

let sppOption: bluetooth.SppOption = { uuid: '', secure: false, type: 0 }
bluetooth.sppListen('', sppOption, serverSocket)
```

**原因**

对象字面量缺少类型，根据`bluetooth.sppListen`分析可以得知，`sppOption`的类型来源于 SDK，那么只需要将类型导入即可。
注意到在`@ohos.bluetooth`中，`sppOption`是定义在 namespace 中的，所以在 ets 文件中，先导入 namespace，再通过名称获取相应的类型。

### object literal 传参给 Object 类型

**应用代码**

```ts
function emit(event: string, ...args: Object[]): void {}

emit('', {
  action: 11,
  outers: false,
})
```

**建议改法**

```ts
function emit(event: string, ...args: Object[]): void {}

let emitArg: Record<string, number | boolean> = {
  action: 11,
  outers: false,
}

emit('', emitArg)
```

## arkts-no-obj-literals-as-types

**应用代码**

```ts
type Person = { name: string; age: number }
```

**建议改法**

```ts
interface Person {
  name: string
  age: number
}
```

## arkts-no-noninferrable-arr-literals

**应用代码**

```ts
let permissionList = [
  { name: '设备信息', value: '用于分析设备的续航、通话、上网、SIM卡故障等' },
  { name: '麦克风', value: '用于反馈问题单时增加语音' },
  { name: '存储', value: '用于反馈问题单时增加本地文件附件' },
]
```

**建议改法**

为对象字面量声明类型

```ts
class PermissionItem {
  name?: string
  value?: string
}

let permissionList: PermissionItem[] = [
  { name: '设备信息', value: '用于分析设备的续航、通话、上网、SIM卡故障等' },
  { name: '麦克风', value: '用于反馈问题单时增加语音' },
  { name: '存储', value: '用于反馈问题单时增加本地文件附件' },
]
```

## arkts-no-method-reassignment

**应用代码**

```ts
class C {
  add(left: number, right: number): number {
    return left + right
  }
}

function sub(left: number, right: number): number {
  return left - right
}

let c1 = new C()
c1.add = sub
```

**建议改法**

```ts
class C {
  add: (left: number, right: number) => number = (
    left: number,
    right: number,
  ) => {
    return left + right
  }
}

function sub(left: number, right: number): number {
  return left - right
}

let c1 = new C()
c1.add = sub
```

## arkts-no-polymorphic-unops

**应用代码**

```ts
let a = +'5'
let b = -'5'
let c = ~'5'
let d = +'string'
```

**建议改法**

```ts
let a = Number.parseInt('5')
let b = -Number.parseInt('5')
let c = ~Number.parseInt('5')
let d = new Number('string')
```

## arkts-no-type-query

**应用代码**

```ts
// module1.ts
class C {
  value: number = 0
}

export let c = new C()

// module2.ts
import { c } from './module1'
let t: typeof c = { value: 123 }
```

**建议改法**

```ts
// module1.ts
class C {
  value: number = 0
}

export { C }

// module2.ts
import { C } from './module1'
let t: C = { value: 123 }
```

## arkts-no-in

**应用代码**

```ts
let arr = [10, 20, 30, 40]
let isIn = 5 in arr
```

**建议改法**

```ts
let arr = [10, 20, 30, 40]
let isIn = 5 < arr.length
```

## arkts-no-destruct-assignment

**应用代码**

```ts
let map = new Map<string, string>([
  ['a', 'a'],
  ['b', 'b'],
])
for (let [key, value] of map) {
  console.log(key)
  console.log(value)
}
```

**建议改法**

使用数组

```ts
let map = new Map<string, string>([
  ['a', 'a'],
  ['b', 'b'],
])
for (let arr of map) {
  let key = arr[0]
  let value = arr[1]
  console.log(key)
  console.log(value)
}
```

## arkts-no-types-in-catch

**应用代码**

```ts
import { BusinessError } from '@ohos.base'

try {
  // ...
} catch (e: BusinessError) {
  logger.error(e.code, e.message)
}
```

**建议改法**

```ts
import { BusinessError } from '@ohos.base'

try {
  // ...
} catch (error) {
  let e: BusinessError = error as BusinessError
  logger.error(e.code, e.message)
}
```

## arkts-no-for-in

**应用代码**

```ts
interface Person {
  [name: string]: string
}
let p: Person = {
  name: 'tom',
  age: '18',
}

for (let t in p) {
  console.log(p[t])
}
```

**建议改法**

```ts
let p: Record<string, string> = {
  name: 'tom',
  age: '18',
}

for (let ele of Object.entries(p)) {
  console.log(ele[1])
}
```

## arkts-no-mapped-types

**应用代码**

```ts
class C {
  a: number = 0
  b: number = 0
  c: number = 0
}
type OptionsFlags = {
  [Property in keyof C]: string
}
```

**建议改法**

```ts
class C {
  a: number = 0
  b: number = 0
  c: number = 0
}

type OptionsFlags = Record<keyof C, string>
```

## arkts-limited-throw

**应用代码**

```ts
import { BusinessError } from '@ohos.base'

function ThrowError(error: BusinessError) {
  throw error
}
```

**建议改法**

```ts
import { BusinessError } from '@ohos.base'

function ThrowError(error: BusinessError) {
  throw error as Error
}
```

**原因**

`throw`语句中值的类型必须为`Error`或者其继承类，如果继承类是一个泛型，会有编译期报错。建议使用`as`将类型转换为`Error`。

## arkts-no-standalone-this

### 函数内使用 this

**应用代码**

```ts
function foo() {
  console.log(this.value)
}

let obj = { value: 123 }
foo.apply(obj)
```

**建议改法 1**

使用类的方法实现,如果该方法被多个类使用,可以考虑采用继承的机制

```ts
class Test {
  value: number = 0
  constructor(value: number) {
    this.value = value
  }

  foo() {
    console.log(this.value)
  }
}

let obj: Test = new Test(123)
obj.foo()
```

**建议改法 2**

将 this 作为参数传入

```ts
function foo(obj: Test) {
  console.log(obj.value)
}

class Test {
  value: number = 0
}

let obj: Test = { value: 123 }
foo(obj)
```

**建议改法 3**

将属性作为参数传入

```ts
function foo(value: number) {
  console.log(value)
}

class Test {
  value: number = 0
}

let obj: Test = { value: 123 }
foo(obj.value)
```

### class 的静态方法内使用 this

**应用代码**

```ts
class Test {
  static value: number = 123
  static foo(): number {
    return this.value
  }
}
```

**建议改法**

```ts
class Test {
  static value: number = 123
  static foo(): number {
    return Test.value
  }
}
```

## arkts-no-spread

**应用代码**

```ts
import notification from '@ohos.notificationManager'

function buildNotifyLongRequest(): notification.NotificationRequest {
  // ...
}

let notificationRequest: notification.NotificationRequest = {
  ...buildNotifyLongRequest(),
  deliveryTime: new Date().getTime(),
}
```

**建议改法**

```ts
import notification from '@ohos.notificationManager'

function buildNotifyLongRequest(): notification.NotificationRequest {
  // ...
}

let notificationRequest: notification.NotificationRequest =
  buildNotifyLongRequest()
notificationRequest.deliveryTime = new Date().getTime()
```

**原因**

ArkTS 中，对象布局在编译期是确定的。如果需要将一个对象的所有属性展开赋值给另一个对象可以通过逐个属性赋值语句完成。在本例中，需要展开的对象和赋值的目标对象类型恰好相同，可以通过改变该对象属性的方式重构代码。

## arkts-no-ctor-signatures-funcs

在 class 内声明属性，而不是在构造函数上。

**应用代码**

```ts
class Controller {
  value: number = 0
  constructor(value: number) {
    this.value = value
  }
}

type ControllerConstrucotr = new (value: number) => Controller

class Menu {
  controller: ControllerConstrucotr = Controller
  createController() {
    if (this.controller) {
      return new this.controller(123)
    }
    return null
  }
}

let t = new Menu()
console.log(t.createController()!.value)
```

**建议改法**

```ts
class Controller {
  value: number = 0
  constructor(value: number) {
    this.value = value
  }
}

type ControllerConstrucotr = () => Controller

class Menu {
  controller: ControllerConstrucotr = () => {
    return new Controller(123)
  }
  createController() {
    if (this.controller) {
      return this.controller()
    }
    return null
  }
}

let t: Menu = new Menu()
console.log(t.createController()!.value)
```

## arkts-no-globalthis

由于无法为 globalThis 添加静态类型，只能通过查找的方式访问 globalThis 的属性，造成额外的性能开销。另外，无法为 globalThis 的属性标记类型，无法保证对这些属性操作的安全和高性能。因此 ArkTS 不支持 globalThis。

1. 建议按照业务逻辑根据`import/export`语法实现数据在不同模块的传递。

2. 必要情况下，可以通过构造的**单例对象**来实现全局对象的功能。(**说明：**不能在 har 中定义单例对象，har 在打包时会在不同的 hap 中打包两份，无法实现单例。)

**构造单例对象**

```ts
// 构造单例对象
export class GlobalContext {
  private constructor() {}
  private static instance: GlobalContext
  private _objects = new Map<string, Object>()

  public static getContext(): GlobalContext {
    if (!GlobalContext.instance) {
      GlobalContext.instance = new GlobalContext()
    }
    return GlobalContext.instance
  }

  getObject(value: string): Object | undefined {
    return this._objects.get(value)
  }

  setObject(key: string, objectClass: Object): void {
    this._objects.set(key, objectClass)
  }
}
```

**应用代码**

```ts
// file1.ts

export class Test {
  value: string = ''
  foo(): void {
    globalThis.value = this.value
  }
}

// file2.ts

print(globalThis.value)
```

**建议改法**

```ts
// file1.ts

import { GlobalContext } from '../GlobalContext'

export class Test {
  value: string = ''
  foo(): void {
    GlobalContext.getContext().setObject('value', this.value)
  }
}

// file2.ts

import { GlobalContext } from '../GlobalContext'

console.log(GlobalContext.getContext().getObject('value'))
```

## arkts-no-func-apply-bind-call

### 使用标准库中接口

**应用代码**

```ts
let arr: number[] = [1, 2, 3, 4]
let str = String.fromCharCode.apply(null, Array.from(arr))
```

**建议改法**

```ts
let arr: number[] = [1, 2, 3, 4]
let str = String.fromCharCode(...Array.from(arr))
```

### bind 定义方法

**应用代码**

```ts
class A {
  value: number = 0
  foo: Function = () => {}
}

class Test {
  value: number = 1234
  obj: A = {
    value: this.value,
    foo: this.foo.bind(this),
  }

  foo() {
    console.log(this.value)
  }
}
```

**建议改法 1**

```ts
class A {
  value: number = 0
  foo: Function = () => {}
}

class Test {
  value: number = 1234
  obj: A = {
    value: this.value,
    foo: (): void => this.foo(),
  }

  foo() {
    console.log(this.value)
  }
}
```

**建议改法 2**

```ts
class A {
  value: number = 0
  foo: Function = () => {}
}

class Test {
  value: number = 1234
  foo: () => void = () => {
    console.log(this.value)
  }
  obj: A = {
    value: this.value,
    foo: this.foo,
  }
}
```

### 使用 apply

**应用代码**

```ts
class A {
  value: number
  constructor(value: number) {
    this.value = value
  }

  foo() {
    console.log(this.value)
  }
}

let a1 = new A(1)
let a2 = new A(2)

a1.foo()
a1.foo.apply(a2)
```

**建议改法**

```ts
class A {
  value: number
  constructor(value: number) {
    this.value = value
  }

  foo() {
    this.fooApply(this)
  }

  fooApply(a: A) {
    console.log(a.value)
  }
}

let a1 = new A(1)
let a2 = new A(2)

a1.foo()
a1.fooApply(a2)
```

## arkts-limited-stdlib

ArkTS 不允许使用全局对象的属性和方法： `Infinity, NaN, isFinite, isNaN, parseFloat, parseInt`

可以使用`Number`的属性和方法： `Infinity, NaN, isFinite, isNaN, parseFloat, parseInt`

**应用代码**

```ts
console.log(NaN)
console.log(isFinite(123))
console.log(parseInt('123'))
```

**建议改法**

```ts
console.log(Number.NaN)
console.log(Number.isFinite(123))
console.log(Number.parseInt('123'))
```

## arkts-strict-typing(StrictModeError)

### strictPropertyInitialization

**应用代码**

```ts
interface I {
  name: string
}

class A {}

class Test {
  a: number
  b: string
  c: boolean
  d: I
  e: A
}
```

**建议改法**

```ts
interface I {
  name: string
}

class A {}

class Test {
  a: number
  b: string
  c: boolean
  d: I = { name: 'abc' }
  e: A | null = null
  constructor(a: number, b: string, c: boolean) {
    this.a = a
    this.b = b
    this.c = c
  }
}
```

### Type `*** | null` is not assignable to type `***`

**应用代码**

```ts
class A {
  bar() {}
}
function foo(n: number) {
  if (n === 0) {
    return null
  }
  return new A()
}
function getNumber() {
  return 5
}
let a: A = foo(getNumber())
a.bar()
```

**建议改法**

```ts
class A {
  bar() {}
}
function foo(n: number) {
  if (n === 0) {
    return null
  }
  return new A()
}
function getNumber() {
  return 5
}

let a: A | null = foo(getNumber())
a?.bar()
```

### 严格属性初始化检查

在 class 中，如果一个属性没有初始化，且没有在构造函数中被赋值，那么 ArkTS 将报错。

**建议改法**

1.一般情况下，**建议按照业务逻辑**在声明时初始化属性，或者在构造函数中为属性赋值。如：

```ts
//code with error
class Test {
  value: nameber
  flag: boolean
}

//方式一，在声明时初始化
class Test {
  value: number = 0
  flag: boolean = false
}

//方式二，在构造函数中赋值
class Test {
  value: number
  flag: boolean
  constructor(value: number, flag: boolean) {
    this.value = value
    this.flag = flag
  }
}
```

2.对于对象类型（包括函数类型）`A`，如果不确定如何初始化，建议按照以下方式之一进行初始化

​ 方式(i) `prop: A | null = null`

​ 方式(ii) `prop?: A`

​ 方式三(iii) `prop： A | undefined = undefined`

- 从性能角度来说，`null`类型只用在编译期的类型检查中，对虚拟机的性能无影响。而`undefined | A`被视为联合类型，运行时可能有额外的开销。
- 从代码可读性、简洁性的角度来说，`prop?:A`是`prop： A | undefined = undefined`的语法糖，**推荐使用可选属性的写法**

### 严格函数类型检查

**应用代码**

```ts
function foo(fn: (value?: string) => void, value: string): void {}

foo((value: string) => {}, '') //error
```

**建议改法**

```ts
function foo(fn: (value?: string) => void, value: string): void {}

foo((value?: string) => {}, '')
```

**原因**

例如，在以下的例子中，如果编译期不开启严格函数类型的检查，那么该段代码可以编译通过，但是在运行时会产生非预期的行为。具体来看，在`foo`的函数体中，一个`undefined`被传入`fn`（这是可以的，因为`fn`可以接受`undefined`），但是在代码第 6 行`foo`的调用点，传入的`(value： string) => { console.log(value.toUpperCase()) }`的函数实现中，始终将参数`value`当做 string 类型，允许其调用`toUpperCase`方法。如果不开启严格函数类型的检查，那么这段代码在运行时，会出现在`undefined`上无法找到属性的错误。

```ts
function foo(fn: (value?: string) => void, value: string): void {
  let v: string | undefined = undefined
  fn(v)
}

foo((value: string) => {
  console.log(value.toUpperCase())
}, '') // Cannot read properties of undefined (reading 'toUpperCase')
```

为了避免运行时的非预期行为，如果在编译时开启了严格类型检查，这段代码将编译不通过，从而可以提醒开发者修改代码，保证程序安全。

### 严格空值检查

**应用代码**

```ts
class Test {
  private value?: string

  public printValue() {
    console.log(this.value.toLowerCase())
  }
}

let t = new Test()
t.printValue()
```

**建议改法**

在编写代码时，建议减少可空类型的使用。如果对变量、属性标记了可空类型，那么在使用它们之间，需要进行空值的判断，根据是否为空值处理不同的逻辑。

```ts
class Test {
  private value?: string

  public printValue() {
    if (this.value) {
      console.log(this.value.toLowerCase())
    }
  }
}

let t = new Test()
t.printValue()
```

**原因**

在第一段代码中，如果编译期不开启严格空值检查，那么该段代码可以编译通过，但是在运行时会产生非预期的行为。这是因为`t`的属性`value`为`undefined`（这是因为`value?: string`是`value : string | undefined = undefined`的语法糖），在第 11 行调用`printValue`方法时，由于在该方法体内未对`this.value`的值进行空值检查，而直接按照`string`类型访问其属性，这就导致了运行时的错误。为了避免运行时的非预期行为，如果在编译时开起来严格空值检查，这段代码将编译不通过从而可以提醒开发者修改代码（如按照第二段代码的方式），保证程序安全。

### 函数返回类型不匹配

**应用代码**

```ts
class Test {
  handleClick: (action: string, externInfo?: DiaExternInfo) => void | null =
    null
}
```

**建议改法**

在这种写法下，函数返回类型被解析为 `void | undefined`，需要添加括号用来区分 union 类型。

```ts
class Test {
  handleClick:
    | ((action: string, externInfo?: DialogExternInfo) => void)
    | null = null
}
```

### '\*\*\*' is of type 'unknown'

**应用代码**

```ts
try {
} catch (error) {
  console.log(error.message)
}
```

**建议改法**

```ts
import { BusinessError } from '@ohos.base'

try {
} catch (error) {
  console.log((error as BusinessError).message)
}
```

### Type '\*\*\* | null' is not assignable to type '\*\*\*'

**应用代码**

```ts
class A {
  value: number
  constructor(value: number) {
    this.value = value
  }
}

function foo(v: number): A | null {
  if (v > 0) {
    return new A(v)
  }
  return null
}

let a: A = foo()
```

**建议改法 1**

修改变量`a`的类型：`let a: A | null = foo()`。

```ts
class A {
  value: number
  constructor(value: number) {
    this.value = value
  }
}

function foo(v: number): A | null {
  if (v > 0) {
    return new A(v)
  }
  return null
}

let a: A | null = foo(123)

if (a != null) {
  // 非空分支
} else {
  // 处理null
}
```

**建议改法 2**

如果可以断定此处调用`foo`一定返回非空值，可以使用非空断言`!`。

```ts
class A {
  value: number
  constructor(value: number) {
    this.value = value
  }
}

function foo(v: number): A | null {
  if (v > 0) {
    return new A(v)
  }
  return null
}

let a: A = foo(123)!
```

### Cannot invoke an object which possibly 'undefined'

**应用代码**

```ts
interface A {
  foo?: () => void
}

let a: A = { foo: () => {} }
a.foo()
```

**建议改法 1**

```ts
interface A {
  foo: () => void
}
let a: A = { foo: () => {} }
a.foo()
```

**建议改法 2**

```ts
interface A {
  foo?: () => void
}

let a: A = { foo: () => {} }
if (a.foo) {
  a.foo()
}
```

**原因**

在原先代码的定义中，foo 是可选属性，有可能为 undefined，对 undefined 的调用会导致报错。建议按照业务逻辑判断是否需要为可选属性。如果确实需要，那么在访问到该属性后需要进行空值检查。

### Variable '\*\*\*' is used before being assigned

**应用代码**

```ts
class Test {
  value: number = 0
}

let a: Test
try {
  a = { value: 1 }
} catch (e) {
  a.value
}
a.value
```

**建议改法**

```ts
class Test {
  value: number = 0
}

let a: Test | null = null
try {
  a = { value: 1 }
} catch (e) {
  if (a) {
    a.value
  }
}

if (a) {
  a.value
}
```

**原因**

对于 primitive types，可以根据业务逻辑赋值，例如 0，''，false。

对于对象类型，可以将类型修改为和 null 的联合类型，并赋值 null，使用时需要进行非空检查。

### Function lacks ending return statement and return type does not include 'undefined'.

**应用代码**

```ts
function foo(a: number): number {
  if (a > 0) {
    return a
  }
}
```

**建议改法 1**

根据业务逻辑，在 else 分支中返回合适的数值

**建议改法 2**

```ts
function foo(a: number): number | undefined {
  if (a > 0) {
    return a
  }
  return
}
```

## arkts-strict-typing-required

**应用代码**

```ts
// @ts-nocheck
var a: any = 123
```

**建议改法**

```ts
let a: number = 123
```

**原因**

ArkTS 不支持通过注释的方式绕过严格类型检查。首先将注释（`// @ts-nocheck`或者`// @ts-ignore`）删去，再根据报错信息修改其他代码。

## Importing ArkTS files to JS and TS files is not allowed

## arkts-no-tsdeps

不允许.ts、.js 文件`import`.ets 文件源码。

**建议改法**

方式 1.将.ts 文件的后缀修改成 ets，按照 ArkTS 语法规则适配代码。

方式 2.将.ets 文件中被.ts 文件依赖的代码单独抽取到.ts 文件中。

## arkts-no-special-imports

**应用代码**

```ts
import type { A, B, C, D } from '***'
```

**建议改法**

```ts
import { A, B, C, D } from '***'
```

## arkts-no-classes-as-obj

### 使用 class 构造实例

**应用代码**

```ts
class Controller {
  value: number = 0
  constructor(value: number) {
    this.value = value
  }
}

interface ControllerConstrucotr {
  new (value: number): Controller
}

class Menu {
  controller: ControllerConstrucotr = Controller
  createController() {
    if (this.controller) {
      return new this.controller(123)
    }
    return null
  }
}

let t = new Menu()
console.log(t.createController()!.value)
```

**建议改法**

```ts
class Controller {
  value: number = 0
  constructor(value: number) {
    this.value = value
  }
}

type ControllerConstrucotr = () => Controller

class Menu {
  controller: ControllerConstrucotr = () => {
    return new Controller(123)
  }
  createController() {
    if (this.controller) {
      return this.controller()
    }
    return null
  }
}

let t: Menu = new Menu()
console.log(t.createController()!.value)
```

### 访问静态属性

**应用代码**

```ts
class C1 {
  static value: string = 'abc'
}

class C2 {
  static value: string = 'def'
}

function getValue(obj: any) {
  return obj['value']
}

console.log(getValue(C1))
console.log(getValue(C2))
```

**建议改法**

```ts
class C1 {
  static value: string = 'abc'
}

class C2 {
  static value: string = 'def'
}

function getC1Value(): string {
  return C1.value
}

function getC2Value(): string {
  return C2.value
}

console.log(getC1Value())
console.log(getC2Value())
```

## arkts-no-side-effects-imports

改用动态 import

**应用代码**

```ts
import 'module'
```

**建议改法**

```ts
import('module')
```

## arkts-no-func-props

**应用代码**

```ts
function foo(value: number): void {
  console.log(value.toString())
}

foo.add = (left: number, right: number) => {
  return left + right
}

foo.sub = (left: number, right: number) => {
  return left - right
}
```

**建议改法**

```ts
class Foo {
  static foo(value: number): void {
    console.log(value.toString())
  }

  static add(left: number, right: number): number {
    return left + right
  }

  static sub(left: number, right: number): number {
    return left - right
  }
}
```

## 状态管理使用典型场景

### Struct 组件外使用状态变量

由于 struct 和 class 不同，不建议把 this 作为参数传递到 struct 外部使用，避免引起实例引用无法释放的情况，导致内存泄露。建议将状态变量对象传递到 struct 外面使用，通过修改对象的属性，来触发 UI 刷新。

**不推荐用法**

```ts
export class MyComponentController {
  item: MyComponent = null;

  setItem(item: MyComponent) {
    this.item = item;
  }

  changeText(value: string) {
    this.item.value = value;
  }
}

@Component
export default struct MyComponent {
  public controller: MyComponentController = null;
  @State value: string = 'Hello World';

  build() {
    Column() {
      Text(this.value)
        .fontSize(50)
    }
  }

  aboutToAppear() {
    if (this.controller)
      this.controller.setItem(this);
  }
}

@Entry
@Component
struct ObjThisOldPage {
  controller = new MyComponentController();

  build() {
    Column() {
      MyComponent({ controller: this.controller })
      Button('change value').onClick(() => {
        this.controller.changeText('Text');
      })
    }
  }
}
```

**推荐用法**

```ts
class CC {
  value: string = '1';

  constructor(value: string) {
    this.value = value;
  }
}

export class MyComponentController {
  item: CC = new CC('1');

  setItem(item: CC) {
    this.item = item;
  }

  changeText(value: string) {
    this.item.value = value;
  }
}

@Component
export default struct MyComponent {
  public controller: MyComponentController | null = null;
  @State value: CC = new CC('Hello World')

  build() {
    Column() {
      Text(`${this.value.value}`)
        .fontSize(50)
    }
  }

  aboutToAppear() {
    if (this.controller)
      this.controller.setItem(this.value);
  }
}

@Entry
@Component
struct StyleExample {
  controller: MyComponentController = new MyComponentController();

  build() {
    Column() {
      MyComponent({ controller: this.controller })
      Button('change value').onClick(() => {
        this.controller.changeText('Text')
      })
    }
  }
}
```

### Struct 支持联合类型的方案

下面这段代码有 arkts-no-any-unknown 的报错，由于 strcut 不支持泛型，建议使用联合类型，实现自定义组件类似泛型的功能。

**不推荐用法**

```ts
class Data {
  aa: number = 11;
}

@Entry
@Component
struct DatauionOldPage {
  @State array: Data[] = [new Data(), new Data(), new Data()];

  @Builder
  componentCloser(data: Data) {
    Text(data.aa + '').fontSize(50)
  }

  build() {
    Row() {
      Column() {
        ForEachCom({ arrayList: this.array, closer: this.componentCloser })
      }
      .width('100%')
    }
    .height('100%')
  }
}

@Component
export struct ForEachCom {
  arrayList: any[]
  @BuilderParam closer: (data: any) => void = this.componentCloser

  @Builder
  componentCloser() {
  }

  build() {
    Column() {
      ForEach(this.arrayList, (item: any) => {
        Row() {
          this.closer(item)
        }.width('100%').height(200).backgroundColor('#eee')
      })
    }
  }
}
```

**推荐用法**

```ts
class Data {
  aa: number = 11;
}

class Model {
  aa: string = '11';
}

type UnionData = Data | Model

@Entry
@Component
struct DatauionPage {
  array: UnionData[] = [new Data(), new Data(), new Data()];

  @Builder
  componentCloser(data: UnionData) {
    if (data instanceof Data) {
      Text(data.aa + '').fontSize(50)
    }
  }

  build() {
    Row() {
      Column() {
        ForEachCom({ arrayList: this.array, closer: this.componentCloser })
      }
      .width('100%')
    }
    .height('100%')
  }
}

@Component
export struct ForEachCom {
  arrayList: UnionData[] = [new Data(), new Data(), new Data()];
  @BuilderParam closer: (data: UnionData) => void = this.componentCloser

  @Builder
  componentCloser() {
  }

  build() {
    Column() {
      ForEach(this.arrayList, (item: UnionData) => {
        Row() {
          this.closer(item)
        }.width('100%').height(200).backgroundColor('#eee')
      })
    }
  }
}
```
