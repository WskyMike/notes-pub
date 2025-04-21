### Расширенные правила применения типизации TypeScript (с примерами вариантов)

## 1. Переменные

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

## 2. Функции

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

## 3. Объекты и массивы

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

## 4. Вызов функций

- При вызове функции отдельно указывать тип **не нужно** — TS проверит всё сам.
- Однако, иногда можно уточнить тип через generic:
```ts
identity<string>("hello"); // явно указываем тип
```
---

## 5. Где обычно НЕ надо указывать тип

- Внутри тела функции для временных переменных, если тип выводится автоматически.
- При вызове функций.
- При инициализации переменной, если значение сразу ясно:
```ts
let sum = 5 + 3; // sum: number
```

---

## 6. Универсальные советы

- **Указывай типы явно для параметров, возвращаемых значений, пропсов, сложных структур.**
- Используй именованные типы (type, interface) для читаемости и переиспользования.
- Используй дженерики для универсальных функций и структур.
- Используй утилиты (Partial, Required и др.) для гибкости типов.

---

## 7. Применение TypeScript в React

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

