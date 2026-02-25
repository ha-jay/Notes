React + TypeScript를 체계적으로 학습할 수 있도록 단계별로 정리해드리겠습니다.

## 1단계: 프로젝트 설정

### 프로젝트 생성

bash

```bash
# Vite로 생성 (권장)
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev

# 또는 Create React App
npx create-react-app my-app --template typescript
```

### tsconfig.json 설정

json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    
    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

## 2단계: 기본 컴포넌트 타이핑

### 함수형 컴포넌트

typescript

```typescript
import { FC, ReactNode } from 'react';

// 기본 Props 타입
interface GreetingProps {
  name: string;
  age?: number; // 선택적 prop
}

// 방법 1: FC 사용 (children 자동 포함)
const Greeting: FC<GreetingProps> = ({ name, age }) => {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {age && <p>Age: {age}</p>}
    </div>
  );
};

// 방법 2: 일반 함수 (권장)
function Greeting({ name, age }: GreetingProps) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {age && <p>Age: {age}</p>}
    </div>
  );
}

// 방법 3: 화살표 함수
const Greeting = ({ name, age }: GreetingProps): JSX.Element => {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {age && <p>Age: {age}</p>}
    </div>
  );
};

// Children 포함
interface CardProps {
  title: string;
  children: ReactNode; // 또는 React.ReactNode
}

function Card({ title, children }: CardProps) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">{children}</div>
    </div>
  );
}

// 기본값 설정
interface ButtonProps {
  text?: string;
  variant?: 'primary' | 'secondary' | 'danger';
  onClick?: () => void;
}

function Button({ 
  text = 'Click me', 
  variant = 'primary',
  onClick 
}: ButtonProps) {
  return (
    <button 
      className={`btn btn-${variant}`}
      onClick={onClick}
    >
      {text}
    </button>
  );
}

// 제네릭 컴포넌트
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => ReactNode;
}

function List<T>({ items, renderItem }: ListProps<T>) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{renderItem(item)}</li>
      ))}
    </ul>
  );
}

// 사용
interface User {
  id: number;
  name: string;
}

function UserList() {
  const users: User[] = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' }
  ];
  
  return (
    <List
      items={users}
      renderItem={(user) => <span>{user.name}</span>}
    />
  );
}
```

### Props 타입 패턴

typescript

```typescript
// 1. 유니온 타입
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'danger';
  size: 'small' | 'medium' | 'large';
}

// 2. 인터섹션 타입
interface BaseProps {
  id: string;
  className?: string;
}

interface ClickableProps {
  onClick: () => void;
}

type ButtonProps = BaseProps & ClickableProps;

// 3. type vs interface
type UserProps = {
  name: string;
  age: number;
};

interface UserProps2 {
  name: string;
  age: number;
}

// 4. 확장
interface BaseButtonProps {
  text: string;
}

interface IconButtonProps extends BaseButtonProps {
  icon: string;
}

// 5. HTML 요소 Props 확장
interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label: string;
  error?: string;
}

function Input({ label, error, ...rest }: InputProps) {
  return (
    <div>
      <label>{label}</label>
      <input {...rest} />
      {error && <span className="error">{error}</span>}
    </div>
  );
}

// 사용
<Input
  label="Email"
  type="email"
  placeholder="Enter email"
  onChange={(e) => console.log(e.target.value)}
/>

// 6. 조건부 Props
type ConditionalProps =
  | { type: 'button'; onClick: () => void }
  | { type: 'link'; href: string };

function Action(props: ConditionalProps) {
  if (props.type === 'button') {
    return <button onClick={props.onClick}>Click</button>;
  }
  return <a href={props.href}>Link</a>;
}
```

## 3단계: Hooks 타이핑

### useState

typescript

```typescript
import { useState } from 'react';

// 1. 타입 추론 (간단한 경우)
function Counter() {
  const [count, setCount] = useState(0); // number로 추론
  const [name, setName] = useState(''); // string으로 추론
  
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
}

// 2. 명시적 타입 지정
function UserProfile() {
  const [age, setAge] = useState<number>(0);
  const [name, setName] = useState<string>('');
  
  return <div>{name} - {age}</div>;
}

// 3. 객체 타입
interface User {
  id: number;
  name: string;
  email: string;
}

function UserForm() {
  const [user, setUser] = useState<User>({
    id: 0,
    name: '',
    email: ''
  });
  
  const handleChange = (field: keyof User, value: string | number) => {
    setUser(prev => ({
      ...prev,
      [field]: value
    }));
  };
  
  return (
    <div>
      <input
        value={user.name}
        onChange={(e) => handleChange('name', e.target.value)}
      />
    </div>
  );
}

// 4. null 가능한 타입
function UserProfile() {
  const [user, setUser] = useState<User | null>(null);
  
  if (!user) {
    return <div>Loading...</div>;
  }
  
  return <div>{user.name}</div>;
}

// 5. 배열 타입
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

function TodoList() {
  const [todos, setTodos] = useState<Todo[]>([]);
  
  const addTodo = (text: string) => {
    const newTodo: Todo = {
      id: Date.now(),
      text,
      completed: false
    };
    setTodos(prev => [...prev, newTodo]);
  };
  
  const toggleTodo = (id: number) => {
    setTodos(prev =>
      prev.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };
  
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id} onClick={() => toggleTodo(todo.id)}>
          {todo.text}
        </li>
      ))}
    </ul>
  );
}
```

### useEffect

typescript

```typescript
import { useState, useEffect } from 'react';

// 기본 사용
function Timer() {
  const [seconds, setSeconds] = useState<number>(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
    
    // 클린업 함수
    return () => {
      clearInterval(interval);
    };
  }, []);
  
  return <div>Seconds: {seconds}</div>;
}

// 데이터 페칭
interface Post {
  id: number;
  title: string;
  body: string;
}

function PostList() {
  const [posts, setPosts] = useState<Post[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    let isCancelled = false;
    
    const fetchPosts = async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts');
        const data: Post[] = await response.json();
        
        if (!isCancelled) {
          setPosts(data);
        }
      } catch (err) {
        if (!isCancelled) {
          setError(err instanceof Error ? err.message : 'An error occurred');
        }
      } finally {
        if (!isCancelled) {
          setLoading(false);
        }
      }
    };
    
    fetchPosts();
    
    return () => {
      isCancelled = true;
    };
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

### useRef

typescript

```typescript
import { useRef, useEffect } from 'react';

// DOM 참조
function TextInput() {
  const inputRef = useRef<HTMLInputElement>(null);
  
  const focusInput = () => {
    inputRef.current?.focus(); // optional chaining
  };
  
  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus</button>
    </div>
  );
}

// 값 저장
function Timer() {
  const [count, setCount] = useState<number>(0);
  const intervalRef = useRef<NodeJS.Timeout | null>(null);
  
  const startTimer = () => {
    if (intervalRef.current) return;
    
    intervalRef.current = setInterval(() => {
      setCount(prev => prev + 1);
    }, 1000);
  };
  
  const stopTimer = () => {
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
      intervalRef.current = null;
    }
  };
  
  useEffect(() => {
    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, []);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={startTimer}>Start</button>
      <button onClick={stopTimer}>Stop</button>
    </div>
  );
}

// 이전 값 저장
function usePrevious<T>(value: T): T | undefined {
  const ref = useRef<T>();
  
  useEffect(() => {
    ref.current = value;
  }, [value]);
  
  return ref.current;
}

// 사용
function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);
  
  return (
    <div>
      <p>Current: {count}</p>
      <p>Previous: {prevCount}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### useContext

typescript

```typescript
import { createContext, useContext, useState, ReactNode } from 'react';

// Context 타입 정의
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

// Context 생성 (초기값 undefined)
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// Provider 컴포넌트
interface ThemeProviderProps {
  children: ReactNode;
}

function ThemeProvider({ children }: ThemeProviderProps) {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  const value: ThemeContextType = {
    theme,
    toggleTheme
  };
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom Hook
function useTheme(): ThemeContextType {
  const context = useContext(ThemeContext);
  
  if (context === undefined) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  
  return context;
}

// 사용
function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header className={theme}>
      <button onClick={toggleTheme}>
        Toggle Theme (Current: {theme})
      </button>
    </header>
  );
}

function App() {
  return (
    <ThemeProvider>
      <Header />
    </ThemeProvider>
  );
}
```

### useReducer

typescript

```typescript
import { useReducer } from 'react';

// State 타입
interface State {
  count: number;
  error: string | null;
}

// Action 타입 (Discriminated Union)
type Action =
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'reset' }
  | { type: 'set'; payload: number }
  | { type: 'error'; payload: string };

// Reducer 함수
function counterReducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + 1, error: null };
    case 'decrement':
      return { ...state, count: state.count - 1, error: null };
    case 'reset':
      return { count: 0, error: null };
    case 'set':
      return { ...state, count: action.payload, error: null };
    case 'error':
      return { ...state, error: action.payload };
    default:
      return state;
  }
}

// 컴포넌트
function Counter() {
  const [state, dispatch] = useReducer(counterReducer, {
    count: 0,
    error: null
  });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      {state.error && <p className="error">{state.error}</p>}
      
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
      <button onClick={() => dispatch({ type: 'set', payload: 10 })}>
        Set to 10
      </button>
    </div>
  );
}

// 복잡한 예시 - Todo
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

interface TodoState {
  todos: Todo[];
  filter: 'all' | 'active' | 'completed';
}

type TodoAction =
  | { type: 'add'; payload: string }
  | { type: 'toggle'; payload: number }
  | { type: 'remove'; payload: number }
  | { type: 'setFilter'; payload: 'all' | 'active' | 'completed' };

function todoReducer(state: TodoState, action: TodoAction): TodoState {
  switch (action.type) {
    case 'add':
      return {
        ...state,
        todos: [
          ...state.todos,
          { id: Date.now(), text: action.payload, completed: false }
        ]
      };
    case 'toggle':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
    case 'remove':
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload)
      };
    case 'setFilter':
      return {
        ...state,
        filter: action.payload
      };
    default:
      return state;
  }
}
```

### useMemo와 useCallback

typescript

```typescript
import { useMemo, useCallback, useState } from 'react';

// useMemo
interface Item {
  id: number;
  name: string;
  price: number;
}

function ProductList({ items }: { items: Item[] }) {
  const [filter, setFilter] = useState<string>('');
  
  // 필터링된 결과를 메모이제이션
  const filteredItems = useMemo(() => {
    console.log('Filtering...');
    return items.filter(item =>
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]);
  
  // 총 가격 계산을 메모이제이션
  const totalPrice = useMemo(() => {
    return filteredItems.reduce((sum, item) => sum + item.price, 0);
  }, [filteredItems]);
  
  return (
    <div>
      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter"
      />
      <p>Total: ${totalPrice}</p>
      <ul>
        {filteredItems.map(item => (
          <li key={item.id}>
            {item.name} - ${item.price}
          </li>
        ))}
      </ul>
    </div>
  );
}

// useCallback
interface TodoItemProps {
  todo: Todo;
  onToggle: (id: number) => void;
  onRemove: (id: number) => void;
}

const TodoItem = React.memo(({ todo, onToggle, onRemove }: TodoItemProps) => {
  console.log('TodoItem rendered:', todo.id);
  
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span>{todo.text}</span>
      <button onClick={() => onRemove(todo.id)}>Delete</button>
    </li>
  );
});

function TodoApp() {
  const [todos, setTodos] = useState<Todo[]>([]);
  
  // 함수를 메모이제이션하여 자식 컴포넌트 리렌더링 방지
  const handleToggle = useCallback((id: number) => {
    setTodos(prev =>
      prev.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  }, []);
  
  const handleRemove = useCallback((id: number) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  }, []);
  
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={handleToggle}
          onRemove={handleRemove}
        />
      ))}
    </ul>
  );
}
```

## 4단계: Custom Hooks

### 타입 안전한 Custom Hooks

typescript

```typescript
// useFetch Hook
interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => void;
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  
  const fetchData = useCallback(async () => {
    try {
      setLoading(true);
      const response = await fetch(url);
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const json = await response.json();
      setData(json);
      setError(null);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
      setData(null);
    } finally {
      setLoading(false);
    }
  }, [url]);
  
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  
  return { data, loading, error, refetch: fetchData };
}

// 사용
interface User {
  id: number;
  name: string;
  email: string;
}

function UserProfile({ userId }: { userId: number }) {
  const { data: user, loading, error } = useFetch<User>(
    `https://api.example.com/users/${userId}`
  );
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return null;
  
  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}

// useLocalStorage Hook
function useLocalStorage<T>(
  key: string,
  initialValue: T
): [T, (value: T | ((prev: T) => T)) => void] {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });
  
  const setValue = (value: T | ((prev: T) => T)) => {
    try {
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [storedValue, setValue];
}

// 사용
function App() {
  const [theme, setTheme] = useLocalStorage<'light' | 'dark'>('theme', 'light');
  
  return (
    <div className={theme}>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

// useDebounce Hook
function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  
  return debouncedValue;
}

// 사용
function SearchComponent() {
  const [search, setSearch] = useState<string>('');
  const debouncedSearch = useDebounce(search, 500);
  
  useEffect(() => {
    if (debouncedSearch) {
      console.log('Searching for:', debouncedSearch);
      // API 호출
    }
  }, [debouncedSearch]);
  
  return (
    <input
      value={search}
      onChange={(e) => setSearch(e.target.value)}
      placeholder="Search..."
    />
  );
}

// useToggle Hook
function useToggle(initialValue: boolean = false): [boolean, () => void] {
  const [value, setValue] = useState<boolean>(initialValue);
  
  const toggle = useCallback(() => {
    setValue(prev => !prev);
  }, []);
  
  return [value, toggle];
}

// 사용
function Modal() {
  const [isOpen, toggleModal] = useToggle(false);
  
  return (
    <div>
      <button onClick={toggleModal}>
        {isOpen ? 'Close' : 'Open'} Modal
      </button>
      {isOpen && (
        <div className="modal">
          <p>Modal Content</p>
          <button onClick={toggleModal}>Close</button>
        </div>
      )}
    </div>
  );
}
```

## 5단계: 이벤트 처리

### 이벤트 타입

typescript

```typescript
import { ChangeEvent, FormEvent, MouseEvent, KeyboardEvent } from 'react';

function EventExamples() {
  // 입력 이벤트
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };
  
  // 텍스트 영역 이벤트
  const handleTextAreaChange = (e: ChangeEvent<HTMLTextAreaElement>) => {
    console.log(e.target.value);
  };
  
  // 선택 이벤트
  const handleSelectChange = (e: ChangeEvent<HTMLSelectElement>) => {
    console.log(e.target.value);
  };
  
  // 폼 제출
  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log('Form submitted');
  };
  
  // 버튼 클릭
  const handleClick = (e: MouseEvent<HTMLButtonElement>) => {
    console.log('Button clicked', e.clientX, e.clientY);
  };
  
  // 키보드 이벤트
  const handleKeyPress = (e: KeyboardEvent<HTMLInputElement>) => {
    if (e.key === 'Enter') {
      console.log('Enter pressed');
    }
  };
  
  // 제네릭 클릭 핸들러
  const handleGenericClick = <T extends HTMLElement>(e: MouseEvent<T>) => {
    console.log('Clicked element:', e.currentTarget);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        onChange={handleChange}
        onKeyPress={handleKeyPress}
      />
      
      <textarea onChange={handleTextAreaChange} />
      
      <select onChange={handleSelectChange}>
        <option value="1">Option 1</option>
        <option value="2">Option 2</option>
      </select>
      
      <button type="button" onClick={handleClick}>
        Click me
      </button>
      
      <div onClick={handleGenericClick}>
        Click this div
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}

// 재사용 가능한 이벤트 핸들러 타입
type InputChangeHandler = (e: ChangeEvent<HTMLInputElement>) => void;
type FormSubmitHandler = (e: FormEvent<HTMLFormElement>) => void;
type ButtonClickHandler = (e: MouseEvent<HTMLButtonElement>) => void;

interface FormProps {
  onSubmit: FormSubmitHandler;
  onChange: InputChangeHandler;
}

function Form({ onSubmit, onChange }: FormProps) {
  return (
    <form onSubmit={onSubmit}>
      <input onChange={onChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## 6단계: 폼 처리

### 타입 안전한 폼

typescript

```typescript
interface FormData {
  email: string;
  password: string;
  age: number;
  terms: boolean;
}

interface FormErrors {
  email?: string;
  password?: string;
  age?: string;
  terms?: string;
}

function SignupForm() {
  const [formData, setFormData] = useState<FormData>({
    email: '',
    password: '',
    age: 0,
    terms: false
  });
  
  const [errors, setErrors] = useState<FormErrors>({});
  
  const handleChange = (
    e: ChangeEvent<HTMLInputElement>
  ) => {
    const { name, value, type, checked } = e.target;
    
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };
  
  const validate = (): FormErrors => {
    const newErrors: FormErrors = {};
    
    if (!formData.email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 8) {
      newErrors.password = 'Password must be at least 8 characters';
    }
    
    if (formData.age < 18) {
      newErrors.age = 'Must be 18 or older';
    }
    
    if (!formData.terms) {
      newErrors.terms = 'You must accept the terms';
    }
    
    return newErrors;
  };
  
  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    
    const validationErrors = validate();
    
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
      return;
    }
    
    console.log('Form submitted:', formData);
    setErrors({});
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Email:</label>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <label>Password:</label>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
      
      <div>
        <label>Age:</label>
        <input
          type="number"
          name="age"
          value={formData.age}
          onChange={handleChange}
        />
        {errors.age && <span className="error">{errors.age}</span>}
      </div>
      
      <div>
        <label>
          <input
            type="checkbox"
            name="terms"
            checked={formData.terms}
            onChange={handleChange}
          />
          Accept Terms
        </label>
        {errors.terms && <span className="error">{errors.terms}</span>}
      </div>
      
      <button type="submit">Sign Up</button>
    </form>
  );
}

// 제네릭 폼 Hook
function useForm<T extends Record<string, any>>(
  initialValues: T,
  validate: (values: T) => Partial<Record<keyof T, string>>
) {
  const [values, setValues] = useState<T>(initialValues);
  const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});
  const [touched, setTouched] = useState<Partial<Record<keyof T, boolean>>>({});
  
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    const { name, value, type, checked } = e.target;
    setValues(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };
  
  const handleBlur = (e: ChangeEvent<HTMLInputElement>) => {
    const { name } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const fieldErrors = validate(values);
    setErrors(prev => ({ ...prev, [name]: fieldErrors[name as keyof T] }));
  };
  
  const handleSubmit = (
    onSubmit: (values: T) => void
  ) => (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    
    const formErrors = validate(values);
    setErrors(formErrors);
    
    if (Object.keys(formErrors).length === 0) {
      onSubmit(values);
    }
  };
  
  return {
    values,
    errors,
    touched,
    handleChange,
    handleBlur,
    handleSubmit
  };
}
```

## 7단계: API 통합

### Axios with TypeScript

bash

```bash
npm install axios
```

typescript

```typescript
import axios, { AxiosError } from 'axios';

// API 응답 타입
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

interface User {
  id: number;
  name: string;
  email: string;
}

// API 클라이언트
const api = axios.create({
  baseURL: 'https://api.example.com',
  headers: {
    'Content-Type': 'application/json'
  }
});

// API 함수
class UserService {
  static async getUsers(): Promise<User[]> {
    try {
      const response = await api.get<ApiResponse<User[]>>('/users');
      return response.data.data;
    } catch (error) {
      if (axios.isAxiosError(error)) {
        throw new Error(error.response?.data.message || 'Failed to fetch users');
      }
      throw error;
    }
  }
  
  static async getUser(id: number): Promise<User> {
    const response = await api.get<ApiResponse<User>>(`/users/${id}`);
    return response.data.data;
  }
  
  static async createUser(user: Omit<User, 'id'>): Promise<User> {
    const response = await api.post<ApiResponse<User>>('/users', user);
    return response.data.data;
  }
  
  static async updateUser(id: number, user: Partial<User>): Promise<User> {
    const response = await api.patch<ApiResponse<User>>(`/users/${id}`, user);
    return response.data.data;
  }
  
  static async deleteUser(id: number): Promise<void> {
    await api.delete(`/users/${id}`);
  }
}

// 컴포넌트에서 사용
function UserList() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const data = await UserService.getUsers();
        setUsers(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'An error occurred');
      } finally {
        setLoading(false);
      }
    };
    
    fetchUsers();
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```