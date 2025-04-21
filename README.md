## Памятка применения типизации TypeScript (с примерами вариантов)

<br> 

- [Переменные](#переменные)
- [Функции](#функции)
- [Объекты и массивы](#объекты-и-массивы)
- [Вызов функций](#вызов-функций)
- [Применение TypeScript в React](#применение-typescript-в-react)
  - [Компоненты-функции](#компоненты-функции)
  - [Состояния, ссылки, обработчики](#состояния-ссылки-обработчики)
  - [useReducer с типами](#usereducer-с-типами)
- [Типизация стандартных коллекций](#типизация-стандартных-коллекций)
  - [Map](#map)
  - [Set](#set)
- [Типизация стандартных объектов](#типизация-стандартных-объектов)
  - [Date](#date)
- [Типизация Promise](#типизация-promise)
- [Типизация Error и других стандартных ошибок](#типизация-error-и-других-стандартных-ошибок)
- [Типизация стандартных функций и структур](#типизация-стандартных-функций-и-структур)
  - [RegExp](#regexp)
  - [JSON](#json)
- [Типизация глобальных структур и утилит](#типизация-глобальных-структур-и-утилит)
  - [Event, HTMLElement и прочее из DOM API](#event-htmlelement-и-прочее-из-dom-api)
- [Типизация функций-конструкторов и классов](#типизация-функций-конструкторов-и-классов)
- [Типизация Symbol и BigInt](#типизация-symbol-и-bigint)
- [Типизация ReadonlyMap, ReadonlySet](#типизация-readonlymap-readonlyset)
- [Типизация WeakMap и WeakSet](#типизация-weakmap-и-weakset)
- [Типизация URL и URLSearchParams](#типизация-url-и-urlsearchparams)
- [Типизация ArrayBuffer, DataView, TypedArray](#типизация-arraybuffer-dataview-typedarray)
- [Типизация File, Blob, FormData](#типизация-file-blob-formdata)

<br>
<br> 

>[!NOTE]
> #### Где обычно НЕ надо указывать тип:
>- Внутри тела функции для временных переменных, если тип выводится автоматически.
>- При вызове функций.
>- При инициализации переменной, если значение сразу ясно:
>```ts
>let sum = 5 + 3; // sum: number
>```

<br>
<br> 

> [!TIP]
>### Универсальные советы
>- **Указывай типы явно для параметров, возвращаемых значений, пропсов, сложных структур.**
>- Используй именованные типы (type, interface) для читаемости и переиспользования.
>- Используй дженерики для универсальных функций и структур.
>- Используй утилиты (Partial, Required и др.) для гибкости типов.

<br> 

## Переменные

**Базовая типизация:**
```ts
let text: string = "Hello";
let age: number = 25;
let isActive: boolean = true;
let ids: number[] = [1, 2, 3];
```

**Именованные типы и интерфейсы:**
```ts
type UserID = string;
let id: UserID = "user-123";

interface Product {
  title: string;
  price: number;
}
let item: Product = { title: "Book", price: 150 };
```

**Дженерики:**
```ts
let data: Array<number> = [1, 2, 3];
let map: Map<string, number> = new Map();
```

**Дженерики с extends:**
```ts
type WithId<T extends { id: number }> = T;
let user: WithId<{ id: number; name: string }> = { id: 1, name: "Mike" };
```

**Объединённые и пересекающиеся типы (Union & Intersection):**
```ts
let value: string | number = 42;
type Admin = { admin: boolean };
type Person = { name: string };
let adminUser: Person & Admin = { name: "Anna", admin: true };
```

**Литеральные типы:**
```ts
let direction: "left" | "right" | "center" = "left";
```

**Тип any, unknown, never:**
```ts
let something: any = 5;
let anything: unknown = "test";
function fail(): never { throw new Error("fail"); }
```

---

<br> 

## Функции

**Типизация параметров и возвращаемого значения:**
```ts
function sum(a: number, b: number): number {
  return a + b;
}
```

**Функции с опциональными и значениями по умолчанию:**
```ts
function greet(name: string, age?: number): void { /* ... */ }
function pow(x: number, y: number = 2): number { return x ** y; }
```

**Анонимные функции и стрелочные функции:**
```ts
const toUpper: (s: string) => string = s => s.toUpperCase();
```

**Дженерики:**
```ts
function identity<T>(value: T): T {
  return value;
}
```

**Дженерики с extends:**
```ts
function getId<T extends { id: number }>(obj: T): number {
  return obj.id;
}
```

**Union & Intersection для параметров и возвращаемых значений:**
```ts
function format(input: string | number): string {
  return input.toString();
}
```

**Использование type/interface для типов функций:**
```ts
type MathOp = (a: number, b: number) => number;
const multiply: MathOp = (a, b) => a * b;
```
---

<br> 

## Объекты и массивы

**Базовые типы массивов и объектов:**
```ts
let arr: number[] = [1, 2, 3];
let obj: { name: string; age: number } = { name: "Mike", age: 20 };
```

**Именованные типы и интерфейсы:**
```ts
interface Car { brand: string; year: number; }
let myCar: Car = { brand: "BMW", year: 2020 };
```

**Дженерики для массивов и объектов:**
```ts
let list: Array<string> = ["a", "b"];
function wrapInArray<T>(x: T): T[] { return [x]; }
```

**Дженерики с extends:**
```ts
type Response<T extends object> = { data: T; status: number };
let resp: Response<{ message: string }> = { data: { message: "ok" }, status: 200 };
```

**Record, Partial, Required, Readonly, Pick, Omit:**
```ts
type User = { id: number; name: string; email?: string };

// Record
let dict: Record<string, number> = { apples: 2, bananas: 3 };

// Partial
let partialUser: Partial<User> = { name: "Mike" };

// Required
let requiredUser: Required<User> = { id: 1, name: "A", email: "a@a.a" };

// Readonly
const roUser: Readonly<User> = { id: 1, name: "B" };

// Pick & Omit
type UserId = Pick<User, "id">;
type UserWithoutEmail = Omit<User, "email">;
```

---

<br> 

## Вызов функций

- При вызове функции отдельно указывать тип **не нужно** — TS проверит всё сам.
- Однако, иногда можно уточнить тип через generic:
```ts
identity<string>("hello"); // явно указываем тип
```
---

<br> 

## Применение TypeScript в React

### Компоненты-функции

**Базовая типизация пропсов:**
```ts
interface ButtonProps {
  label: string;
  onClick: () => void;
}
const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);
```

**Без React.FC (рекомендуется):**
```ts
type ButtonProps = { label: string; onClick: () => void };
function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>;
}
```

**Дженерики для компонентов:**
```ts
type ListProps<T> = { items: T[]; render: (item: T) => React.ReactNode };
function List<T>({ items, render }: ListProps<T>) {
  return <ul>{items.map(render)}</ul>;
}
```

**Типизация children:**
```ts
type CardProps = { children: React.ReactNode };
const Card = ({ children }: CardProps) => <div>{children}</div>;
```

### Состояния, ссылки, обработчики

**useState:**
```ts
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);
```

**useRef:**
```ts
const inputRef = useRef<HTMLInputElement>(null);
```

**Обработчики событий:**
```ts
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};
```

### useReducer с типами
```ts
type State = { count: number };
type Action = { type: "inc" } | { type: "dec" };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "inc": return { count: state.count + 1 };
    case "dec": return { count: state.count - 1 };
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
```
---

<br> 

## Типизация стандартных коллекций

### Map

```ts
const usersById: Map<number, string> = new Map();
usersById.set(1, "Mike");
usersById.set(2, "Anna");
```
- Первый generic-параметр — тип ключа, второй — тип значения.

### Set

```ts
const uniqueIds: Set<number> = new Set([1, 2, 3]);
const tags: Set<string> = new Set(["a", "b", "c"]);
```
- Generic-параметр — тип элементов множества.

<br> 

## Типизация стандартных объектов

### Date

```ts
const now: Date = new Date();
function formatDate(date: Date): string {
  return date.toISOString();
}
```

<br> 

## Типизация Promise

```ts
const responsePromise: Promise<Response> = fetch("/api");
const numberPromise: Promise<number> = Promise.resolve(42);

function fetchData(): Promise<{ data: string }> {
  return Promise.resolve({ data: "hello" });
}
```
- Тип внутри Promise указывает, что будет передано в then/catch.

**Promise с дженериками и extends:**
```ts
function fetchWithId<T extends { id: number }>(): Promise<T[]> {
  // ...
  return Promise.resolve([]);
}
```

<br> 

## Типизация Error и других стандартных ошибок

```ts
function throwError(): never {
  throw new Error("Something went wrong");
}

try {
  // ...
} catch (e: unknown) {
  if (e instanceof Error) {
    console.error(e.message);
  }
}
```
- Для перехвата ошибок рекомендуется использовать `unknown` и делать проверку через `instanceof Error`.


<br> 

## Типизация стандартных функций и структур

### RegExp

```ts
const pattern: RegExp = /hello/;
function isMatch(str: string, regex: RegExp): boolean {
  return regex.test(str);
}
```

### JSON

```ts
const jsonString: string = '{"a":1}';
const obj: unknown = JSON.parse(jsonString); // результат всегда unknown!
```
- После JSON.parse всегда лучше уточнять тип вручную (например, через type guard).


<br> 

## Типизация глобальных структур и утилит

### Event, HTMLElement и прочее из DOM API

```ts
function onClick(e: MouseEvent) {
  const target = e.target as HTMLElement;
  // или, безопаснее:
  if (e.target instanceof HTMLButtonElement) {
    e.target.disabled = true;
  }
}
```

<br> 

## Типизация функций-конструкторов и классов

```ts
class Person {
  constructor(public name: string, public age: number) {}
}
const person: Person = new Person("John", 30);

// Тип конструктора:
type PersonConstructor = new (name: string, age: number) => Person;
```

<br> 

## Типизация Symbol и BigInt

```ts
const uniqueKey: symbol = Symbol("key");
const bigNumber: bigint = 12345678901234567890n;
```
<br> 

## Типизация ReadonlyMap, ReadonlySet

```ts
const roles: ReadonlyMap<string, number> = new Map([["admin", 1]]);
const types: ReadonlySet<string> = new Set(["a", "b"]);
```
<br> 

## Типизация WeakMap и WeakSet

```ts
const weakMap: WeakMap<object, number> = new WeakMap();
const weakSet: WeakSet<object> = new WeakSet();
```

<br> 

## Типизация URL и URLSearchParams

```ts
const url: URL = new URL("https://example.com");
const params: URLSearchParams = new URLSearchParams("?a=1&b=2");
```
<br> 

## Типизация ArrayBuffer, DataView, TypedArray

```ts
const buffer: ArrayBuffer = new ArrayBuffer(8);
const view: DataView = new DataView(buffer);
const intArray: Int32Array = new Int32Array(buffer);
```
<br> 

## Типизация File, Blob, FormData

```ts
const file: File = new File(["content"], "file.txt");
const blob: Blob = new Blob(["hello"]);
const form: FormData = new FormData();
```

---
