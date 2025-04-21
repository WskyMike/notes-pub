## Памятка применения типизации TypeScript (с примерами вариантов) :ninja:

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
let text: string = "Hello";                // строка
let age: number = 25;                      // число
let isActive: boolean = true;              // булево
let ids: number[] = [1, 2, 3];             // массив чисел (квадратная запись)
let coords: [number, number] = [10, 20];   // кортеж (tuple) — массив фиксированной длины и типов
```
- `number[]` — массив элементов типа number (короткая запись).
- `[number, number]` — кортеж из двух чисел.

**Именованные типы и интерфейсы:**
```ts
type UserID = string;                      // псевдоним типа (alias)
let id: UserID = "user-123";

interface Product {                         // интерфейс объекта
  title: string;
  price: number;
}
let item: Product = { title: "Book", price: 150 };
```
- `type` — для простых, объединённых или сложных типов.
- `interface` — для описания структуры объектов и классов.

**Дженерики:**
```ts
let data: Array<number> = [1, 2, 3];       // Array<number> — дженерик-запись, массив чисел
let data2: number[] = [1, 2, 3];           // number[] — короткая запись, то же самое
let map: Map<string, number> = new Map();  // Map<K, V> — отображение: ключи типа string, значения типа number
let list: Array<Array<string>> = [["a"], ["b"]]; // массив массивов строк
let list2: string[][] = [["a"], ["b"]];    // то же самое, записано коротко
```
- `Array<T>` — дженерик для массива.
- `Map<K, V>`, `Set<T>` — коллекции с параметрами типа.
- `T[]` = `Array<T>`.

**Дженерики с extends:**
```ts
type WithId<T extends { id: number }> = T;
let user: WithId<{ id: number; name: string }> = { id: 1, name: "Mike" };

function onlyWithName<T extends { name: string }>(obj: T): string {
  return obj.name;
}
```
- `T extends SomeType` — ограничиваем тип-параметр.

**Объединённые и пересекающиеся типы (Union & Intersection):**
```ts
let value: string | number = 42;           // union (или)
type Admin = { admin: boolean };
type Person = { name: string };
let adminUser: Person & Admin = { name: "Anna", admin: true }; // intersection (и)
```
- `|` — объединение (union).
- `&` — пересечение (intersection).

**Литеральные типы:**
```ts
let direction: "left" | "right" | "center" = "left";
type Status = "success" | "error" | "loading";
```
- Значение может быть только одним из перечисленных.

**Тип any, unknown, never:**
```ts
let something: any = 5;                    // любой тип (небезопасно, избегать)
let anything: unknown = "test";            // любой, но требует проверки перед использованием
function fail(): never { throw new Error("fail"); } // никогда не возвращает значение
```
- `any` — отключает проверки типов.
- `unknown` — требует “сужения” типа перед использованием.
- `never` — функция не возвращает значение (например, всегда выбрасывает ошибку).

---

<br> 

## Функции

**Типизация параметров и возвращаемого значения:**
```ts
function sum(a: number, b: number): number { return a + b; }
const sum2 = function(a: number, b: number): number { return a + b; };
const sum3: (a: number, b: number) => number = (a, b) => a + b;
```
- Всегда указывай типы параметров, желательно — возвращаемого значения.

**Функции с опциональными и значениями по умолчанию:**
```ts
function greet(name: string, age?: number): void { /* ... */ }
function pow(x: number, y: number = 2): number { return x ** y; }
```
- `?` — параметр опционален.
- Значение по умолчанию (default) тоже делает параметр опциональным.

**Анонимные функции и стрелочные функции:**
```ts
const toUpper: (s: string) => string = s => s.toUpperCase();
const cb = function(x: number): void {};
```
- Тип функции — `(параметры) => возвращаемое_значение`.

**Дженерики:**
```ts
function identity<T>(value: T): T { return value; }
const identity2 = <T>(value: T): T => value;
let arr: Array<string> = identity<Array<string>>(["a"]);
let arr2 = identity<string[]>(["a"]);
```
- `function <T>()` — дженерик-функция.
- Тип можно указать явно при вызове: `identity<number>(5)`, `identity<string[]>(["a"])`.

**Дженерики с extends:**
```ts
function getId<T extends { id: number }>(obj: T): number { return obj.id; }
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}
```
- Ограничение параметра типа через `extends`.

**Union & Intersection для параметров и возвращаемых значений:**
```ts
function format(input: string | number): string { return input.toString(); }
function mergeUserAndRole(user: User, role: Admin): User & Admin { return { ...user, ...role }; }
```

**Использование type/interface для типов функций:**
```ts
type MathOp = (a: number, b: number) => number;
const multiply: MathOp = (a, b) => a * b;

interface Handler { (e: Event): void }
const handler: Handler = e => {};
```
- Можно описывать типы функций через type или interface.

---

<br> 

## Объекты и массивы

**Базовые типы массивов и объектов:**
```ts
let arr: number[] = [1, 2, 3];
let arr2: Array<number> = [1, 2, 3];
let obj: { name: string; age: number } = { name: "Mike", age: 20 };
```
- `number[]` и `Array<number>` — эквивалентны.
- Объекты можно описывать через литеральный тип или interface/type.

**Именованные типы и интерфейсы:**
```ts
interface Car { brand: string; year: number; }
type CarType = { brand: string; year: number; };
let myCar: Car = { brand: "BMW", year: 2020 };
let myCar2: CarType = { brand: "Audi", year: 2022 };
```
- Одинаково работают с объектами.

**Дженерики для массивов и объектов:**
```ts
let list: Array<string> = ["a", "b"];
let list2: string[] = ["a", "b"];
function wrapInArray<T>(x: T): T[] { return [x]; }
function wrapInArray2<T>(x: T): Array<T> { return [x]; }
```
- Для массивов можно использовать оба варианта.

**Дженерики с extends:**
```ts
type Response<T extends object> = { data: T; status: number };
let resp: Response<{ message: string }> = { data: { message: "ok" }, status: 200 };
```

**Record, Partial, Required, Readonly, Pick, Omit:**
```ts
type User = { id: number; name: string; email?: string };

// Record<K, V> — объект с ключами типа K и значениями типа V
let dict: Record<string, number> = { apples: 2, bananas: 3 };

// Partial<T> — все поля T становятся необязательными
let partialUser: Partial<User> = { name: "Mike" };

// Required<T> — все поля T обязательны
let requiredUser: Required<User> = { id: 1, name: "A", email: "a@a.a" };

// Readonly<T> — все поля T только для чтения
const roUser: Readonly<User> = { id: 1, name: "B" };

// Pick<T, K> — выбираем только указанные поля
type UserId = Pick<User, "id">;

// Omit<T, K> — исключаем указанные поля
type UserWithoutEmail = Omit<User, "email">;
```

---

<br> 

## Вызов функций

- При вызове функции отдельно указывать тип **не нужно** — TS проверит всё сам.
- Иногда можно явно указать generic-параметр:
```ts
identity<string>("hello"); // явно указываем тип
identity<Array<number>>([1,2,3]);
identity<number[]>([1,2,3]);
```
- Если типы аргументов однозначны, generic обычно выводится автоматически.

---

<br> 

## Применение TypeScript в React

### Компоненты-функции

**Базовая типизация пропсов (через interface/type):**
```ts
interface ButtonProps { label: string; onClick: () => void; }
const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);

type ButtonProps2 = { label: string; onClick: () => void };
function Button2({ label, onClick }: ButtonProps2) {
  return <button onClick={onClick}>{label}</button>;
}
```
- `React.FC<Props>` — тип для функционального компонента.
- Можно без React.FC: просто типизируй параметры.

**Дженерики для компонентов:**
```ts
type ListProps<T> = { items: T[]; render: (item: T) => React.ReactNode };
function List<T>({ items, render }: ListProps<T>) {
  return <ul>{items.map(render)}</ul>;
}
```
- Дженерики позволяют создавать универсальные компоненты.

**Типизация children:**
```ts
type CardProps = { children: React.ReactNode };
const Card = ({ children }: CardProps) => <div>{children}</div>;
```
- `React.ReactNode` — любой React-элемент или текст.

### Состояния, ссылки, обработчики

**useState:**
```ts
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);
```
- Тип состояния можно указать явно.

**useRef:**
```ts
const inputRef = useRef<HTMLInputElement>(null);
```
- Тип указывается в угловых скобках.

**Обработчики событий:**
```ts
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};
```
- React имеет собственные типы событий.

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
const usersByName: Map<string, User> = new Map();
usersById.set(1, "Mike");
usersByName.set("tom", { id: 2, name: "Tom" });
```
- `Map<K, V>` — отображение: ключ типа K, значение типа V.

### Set

```ts
const uniqueIds: Set<number> = new Set([1, 2, 3]);
const tags: Set<string> = new Set(["a", "b", "c"]);
```
- `Set<T>` — множество элементов типа T.

<br> 

## Типизация стандартных объектов

### Date

```ts
const now: Date = new Date();
function formatDate(date: Date): string {
  return date.toISOString();
}
```
- `Date` — стандартный объект даты.

<br> 

## Типизация Promise

```ts
const responsePromise: Promise<Response> = fetch("/api");
const numberPromise: Promise<number> = Promise.resolve(42);

function fetchData(): Promise<{ data: string }> {
  return Promise.resolve({ data: "hello" });
}
async function getUser(): Promise<User> {
  return await fetch(...).then(r => r.json());
}
```
- `Promise<T>` — промис, который вернёт значение типа T.

**Promise с дженериками и extends:**
```ts
function fetchWithId<T extends { id: number }>(): Promise<T[]> {
  // ...
  return Promise.resolve([]);
}
```
- Ограничение типа для результата промиса.

<br> 

## Типизация асинхронных функций (async/await)

**Общие правила:**
- Асинхронная функция всегда возвращает `Promise<T>`, где `T` — тип возвращаемого значения.
- Тип результата должен быть указан явно для читаемости и надёжности.

**Примеры:**

```ts
// Функция возвращает Promise<number>
async function getNumber(): Promise<number> {
  return 42;
}

// Можно использовать типизацию с дженериками
async function fetchUser<T>(url: string): Promise<T> {
  const res = await fetch(url);
  return res.json() as Promise<T>;
}

// Асинхронная функция без возвращаемого значения
async function logMessage(msg: string): Promise<void> {
  await someAsyncLogFunction(msg);
}
```

**Варианты вызова с await:**

```ts
const value: number = await getNumber();
const user = await fetchUser<User>("api/user");
```

**Типизация промиса вне async/await:**

```ts
const resultPromise: Promise<number> = getNumber();
resultPromise.then((n: number) => { /* ... */ });
```

**Особенности:**
- Если функция ничего не возвращает, используем `Promise<void>`.
- Если функция возвращает массив, используем `Promise<Type[]>`.
- Для универсальных функций — `Promise<T>` с дженериками и ограничениями (`extends`).

---

**Кратко:**  
- Всегда явно указывай тип после `Promise<...>`, если тип не однозначен.
- `async function fn(): Promise<Type> { ... }` — стандартная типизация асинхронных функций.

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
- Для перехвата ошибок используем `unknown`, проверяем через `instanceof`.

<br> 

## Типизация стандартных функций и структур

### RegExp

```ts
const pattern: RegExp = /hello/;
function isMatch(str: string, regex: RegExp): boolean {
  return regex.test(str);
}
```
- `RegExp` — регулярное выражение.

### JSON

```ts
const jsonString: string = '{"a":1}';
const obj: unknown = JSON.parse(jsonString); // результат всегда unknown!
const arr = JSON.parse(jsonString) as number[]; // явное приведение типа (as)
```
- После `JSON.parse` всегда уточняй тип вручную (type guard или as).

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
- Используй стандартные типы для DOM API (`MouseEvent`, `HTMLElement` и др.)

<br> 

## Типизация функций-конструкторов и классов

```ts
class Person {
  constructor(public name: string, public age: number) {}
}
const person: Person = new Person("John", 30);

// Тип конструктора:
type PersonConstructor = new (name: string, age: number) => Person;
const fn: PersonConstructor = Person;
```
- Тип конструктора через `new`.

<br> 

## Типизация Symbol и BigInt

```ts
const uniqueKey: symbol = Symbol("key");
const bigNumber: bigint = 12345678901234567890n;
```
- `symbol` — уникальный идентификатор.
- `bigint` — большие целые числа.

<br> 

## Типизация ReadonlyMap, ReadonlySet

```ts
const roles: ReadonlyMap<string, number> = new Map([["admin", 1]]);
const types: ReadonlySet<string> = new Set(["a", "b"]);
```
- Только для чтения.

<br> 

## Типизация WeakMap и WeakSet

```ts
const weakMap: WeakMap<object, number> = new WeakMap();
const weakSet: WeakSet<object> = new WeakSet();
```
- Ключи — только объекты, значения — любой тип.

<br> 

## Типизация URL и URLSearchParams

```ts
const url: URL = new URL("https://example.com");
const params: URLSearchParams = new URLSearchParams("?a=1&b=2");
```
- Стандартные объекты для работы с адресами.

<br> 

## Типизация ArrayBuffer, DataView, TypedArray

```ts
const buffer: ArrayBuffer = new ArrayBuffer(8);
const view: DataView = new DataView(buffer);
const intArray: Int32Array = new Int32Array(buffer);
```
- Для работы с бинарными данными.

<br> 

## Типизация File, Blob, FormData

```ts
const file: File = new File(["content"], "file.txt");
const blob: Blob = new Blob(["hello"]);
const form: FormData = new FormData();
```
- Для файлов, данных и отправки форм.

---
