## 1단계: React 기초 (1-2주)

### 환경 설정 및 첫 컴포넌트

bash

```bash
# Vite로 React 프로젝트 생성 (권장)
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev

# 또는 Create React App
npx create-react-app my-app
```

javascript

```javascript
// App.jsx - 기본 구조
function App() {
  return (
    <div className="App">
      <h1>Hello React!</h1>
    </div>
  );
}

export default App;
```

### JSX 문법

javascript

```javascript
// JSX 기본
function Welcome() {
  const name = "John";
  const isLoggedIn = true;
  
  return (
    <div>
      {/* 중괄호 안에 JavaScript 표현식 */}
      <h1>Hello, {name}!</h1>
      
      {/* 조건부 렌더링 */}
      {isLoggedIn ? <p>Welcome back!</p> : <p>Please sign in</p>}
      
      {/* && 연산자 사용 */}
      {isLoggedIn && <button>Logout</button>}
      
      {/* 인라인 스타일 (camelCase) */}
      <p style={{ color: 'red', fontSize: '20px' }}>Styled text</p>
      
      {/* className 사용 (class가 아님) */}
      <div className="container">Content</div>
      
      {/* 주석은 중괄호 안에 */}
      {/* 이것은 주석입니다 */}
    </div>
  );
}
```

### 컴포넌트 기초

javascript

```javascript
// 함수형 컴포넌트 (권장)
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// 화살표 함수
const Greeting = (props) => {
  return <h1>Hello, {props.name}!</h1>;
};

// 간단한 형태
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;

// 여러 컴포넌트 조합
function App() {
  return (
    <div>
      <Greeting name="John" />
      <Greeting name="Jane" />
    </div>
  );
}

// Props 기본값
function Button({ text = "Click me", onClick }) {
  return <button onClick={onClick}>{text}</button>;
}

// children prop
function Card({ children, title }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

// 사용
<Card title="My Card">
  <p>This is the content</p>
  <Button text="Action" />
</Card>
```

### 리스트 렌더링

javascript

```javascript
function TodoList() {
  const todos = [
    { id: 1, text: "Learn React", completed: false },
    { id: 2, text: "Build a project", completed: false },
    { id: 3, text: "Deploy app", completed: true }
  ];
  
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          {todo.text}
          {todo.completed && " ✓"}
        </li>
      ))}
    </ul>
  );
}

// 조건부 필터링
function ActiveTodos() {
  const todos = [...]; // 위와 동일
  
  return (
    <ul>
      {todos
        .filter(todo => !todo.completed)
        .map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))}
    </ul>
  );
}
```

## 2단계: State와 이벤트 (1-2주)

### useState Hook

javascript

```javascript
import { useState } from 'react';

// 기본 사용법
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>-1</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

// 이전 상태 기반 업데이트 (권장)
function Counter() {
  const [count, setCount] = useState(0);
  
  const increment = () => {
    setCount(prev => prev + 1);
    setCount(prev => prev + 1); // 두 번 실행됨
  };
  
  return <button onClick={increment}>Count: {count}</button>;
}

// 객체 상태
function UserForm() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setUser(prev => ({
      ...prev,
      [name]: value
    }));
  };
  
  return (
    <form>
      <input
        name="name"
        value={user.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        name="email"
        value={user.email}
        onChange={handleChange}
        placeholder="Email"
      />
    </form>
  );
}

// 배열 상태
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');
  
  const addTodo = () => {
    setTodos(prev => [
      ...prev,
      { id: Date.now(), text: input, completed: false }
    ]);
    setInput('');
  };
  
  const removeTodo = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  };
  
  const toggleTodo = (id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };
  
  return (
    <div>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Add todo"
      />
      <button onClick={addTodo}>Add</button>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <span
              style={{
                textDecoration: todo.completed ? 'line-through' : 'none'
              }}
              onClick={() => toggleTodo(todo.id)}
            >
              {todo.text}
            </span>
            <button onClick={() => removeTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### 이벤트 처리

javascript

```javascript
function EventExamples() {
  // 클릭 이벤트
  const handleClick = (e) => {
    console.log('Clicked!', e);
  };
  
  // 폼 제출
  const handleSubmit = (e) => {
    e.preventDefault(); // 기본 동작 방지
    console.log('Form submitted');
  };
  
  // 입력 변경
  const handleChange = (e) => {
    console.log(e.target.value);
  };
  
  // 키보드 이벤트
  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      console.log('Enter pressed');
    }
  };
  
  // 마우스 이벤트
  const handleMouseEnter = () => {
    console.log('Mouse entered');
  };
  
  return (
    <div>
      <button onClick={handleClick}>Click me</button>
      
      <form onSubmit={handleSubmit}>
        <input
          onChange={handleChange}
          onKeyPress={handleKeyPress}
        />
        <button type="submit">Submit</button>
      </form>
      
      <div
        onMouseEnter={handleMouseEnter}
        onMouseLeave={() => console.log('Mouse left')}
      >
        Hover me
      </div>
    </div>
  );
}

// 인자 전달하기
function ItemList() {
  const items = ['Apple', 'Banana', 'Orange'];
  
  const handleItemClick = (item) => {
    console.log('Clicked:', item);
  };
  
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index} onClick={() => handleItemClick(item)}>
          {item}
        </li>
      ))}
    </ul>
  );
}
```

## 3단계: 생명주기와 Side Effects (1-2주)

### useEffect Hook

javascript

```javascript
import { useState, useEffect } from 'react';

// 기본 사용법 - 매 렌더링마다 실행
function Example1() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    console.log('Effect ran!');
  });
  
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}

// 마운트시 한 번만 실행 (빈 의존성 배열)
function Example2() {
  useEffect(() => {
    console.log('Component mounted!');
  }, []);
  
  return <div>Hello</div>;
}

// 특정 값 변경시에만 실행
function Example3() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  
  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]); // count가 변경될 때만 실행
  
  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}

// 클린업 함수
function Timer() {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
    
    // 클린업: 컴포넌트 언마운트시 또는 effect 재실행 전에 실행
    return () => {
      clearInterval(interval);
      console.log('Cleanup!');
    };
  }, []);
  
  return <div>Seconds: {seconds}</div>;
}

// 데이터 페칭
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let isCancelled = false;
    
    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        
        if (!isCancelled) {
          setUser(data);
        }
      } catch (err) {
        if (!isCancelled) {
          setError(err.message);
        }
      } finally {
        if (!isCancelled) {
          setLoading(false);
        }
      }
    };
    
    fetchUser();
    
    return () => {
      isCancelled = true; // 컴포넌트 언마운트시 상태 업데이트 방지
    };
  }, [userId]); // userId 변경시마다 재실행
  
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

// 이벤트 리스너 등록
function WindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });
  
  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };
    
    window.addEventListener('resize', handleResize);
    
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);
  
  return <div>Width: {size.width}, Height: {size.height}</div>;
}
```

### 다른 유용한 Hooks

javascript

```javascript
import { useRef, useMemo, useCallback } from 'react';

// useRef - DOM 참조 및 값 유지
function TextInput() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

// useRef - 이전 값 저장
function Counter() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();
  
  useEffect(() => {
    prevCountRef.current = count;
  });
  
  const prevCount = prevCountRef.current;
  
  return (
    <div>
      <p>Current: {count}, Previous: {prevCount}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

// useMemo - 비싼 계산 메모이제이션
function ExpensiveComponent({ items }) {
  const [filter, setFilter] = useState('');
  
  // items나 filter가 변경될 때만 재계산
  const filteredItems = useMemo(() => {
    console.log('Filtering...');
    return items.filter(item => 
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]);
  
  return (
    <div>
      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter"
      />
      <ul>
        {filteredItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

// useCallback - 함수 메모이제이션
function TodoApp() {
  const [todos, setTodos] = useState([]);
  
  // todos가 변경될 때만 함수 재생성
  const addTodo = useCallback((text) => {
    setTodos(prev => [...prev, { id: Date.now(), text }]);
  }, []);
  
  const removeTodo = useCallback((id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  }, []);
  
  return (
    <div>
      <TodoInput onAdd={addTodo} />
      <TodoList todos={todos} onRemove={removeTodo} />
    </div>
  );
}
```

## 4단계: 컴포넌트 패턴 (2주)

### 컴포넌트 구성

javascript

```javascript
// 컴포넌트 분리
// components/TodoItem.jsx
function TodoItem({ todo, onToggle, onRemove }) {
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span style={{
        textDecoration: todo.completed ? 'line-through' : 'none'
      }}>
        {todo.text}
      </span>
      <button onClick={() => onRemove(todo.id)}>Delete</button>
    </li>
  );
}

// components/TodoList.jsx
function TodoList({ todos, onToggle, onRemove }) {
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={onToggle}
          onRemove={onRemove}
        />
      ))}
    </ul>
  );
}

// components/TodoInput.jsx
function TodoInput({ onAdd }) {
  const [input, setInput] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (input.trim()) {
      onAdd(input);
      setInput('');
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Add todo"
      />
      <button type="submit">Add</button>
    </form>
  );
}

// App.jsx
function App() {
  const [todos, setTodos] = useState([]);
  
  const addTodo = (text) => {
    setTodos(prev => [
      ...prev,
      { id: Date.now(), text, completed: false }
    ]);
  };
  
  const toggleTodo = (id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };
  
  const removeTodo = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  };
  
  return (
    <div>
      <h1>Todo List</h1>
      <TodoInput onAdd={addTodo} />
      <TodoList
        todos={todos}
        onToggle={toggleTodo}
        onRemove={removeTodo}
      />
    </div>
  );
}
```

### Custom Hooks

javascript

```javascript
// hooks/useFetch.js
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const json = await response.json();
        setData(json);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
}

// 사용
function UserList() {
  const { data: users, loading, error } = useFetch('/api/users');
  
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

// hooks/useLocalStorage.js
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });
  
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  
  return [value, setValue];
}

// 사용
function App() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  
  return (
    <div className={theme}>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

// hooks/useDebounce.js
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(timer);
    };
  }, [value, delay]);
  
  return debouncedValue;
}

// 사용
function SearchComponent() {
  const [search, setSearch] = useState('');
  const debouncedSearch = useDebounce(search, 500);
  
  useEffect(() => {
    if (debouncedSearch) {
      // API 호출
      console.log('Searching for:', debouncedSearch);
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
```

### Context API (상태 공유)

javascript

```javascript
import { createContext, useContext, useState } from 'react';

// 1. Context 생성
const ThemeContext = createContext();

// 2. Provider 컴포넌트
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Custom Hook
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// 4. 사용
function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
    </ThemeProvider>
  );
}

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

// 복잡한 예시 - Auth Context
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // 초기 인증 확인
    checkAuth();
  }, []);
  
  const checkAuth = async () => {
    try {
      const response = await fetch('/api/auth/me');
      const data = await response.json();
      setUser(data);
    } catch (error) {
      setUser(null);
    } finally {
      setLoading(false);
    }
  };
  
  const login = async (email, password) => {
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    const data = await response.json();
    setUser(data.user);
  };
  
  const logout = async () => {
    await fetch('/api/auth/logout', { method: 'POST' });
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

function useAuth() {
  return useContext(AuthContext);
}

// 사용
function LoginPage() {
  const { login } = useAuth();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    await login(email, password);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit">Login</button>
    </form>
  );
}

function Profile() {
  const { user, logout } = useAuth();
  
  if (!user) return <div>Not logged in</div>;
  
  return (
    <div>
      <h2>Welcome, {user.name}</h2>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

## 5단계: 라우팅과 폼 (1-2주)

### React Router

bash

```bash
npm install react-router-dom
```

javascript

```javascript
import { BrowserRouter, Routes, Route, Link, useNavigate, useParams } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:id" element={<UserDetail />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// 동적 라우트
function UserDetail() {
  const { id } = useParams();
  const { data: user } = useFetch(`/api/users/${id}`);
  
  if (!user) return <div>Loading...</div>;
  
  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}

// 프로그래밍 방식 네비게이션
function LoginPage() {
  const navigate = useNavigate();
  
  const handleLogin = async (credentials) => {
    await login(credentials);
    navigate('/dashboard'); // 로그인 후 대시보드로 이동
  };
  
  return <LoginForm onSubmit={handleLogin} />;
}

// Protected Route
function ProtectedRoute({ children }) {
  const { user, loading } = useAuth();
  
  if (loading) return <div>Loading...</div>;
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  return children;
}

// 사용
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

### 폼 처리

javascript

```javascript
// 기본 폼
function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [errors, setErrors] = useState({});
  const [submitted, setSubmitted] = useState(false);
  
  const validate = () => {
    const newErrors = {};
    
    if (!formData.name.trim()) {
      newErrors.name = 'Name is required';
    }
    
    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!formData.message.trim()) {
      newErrors.message = 'Message is required';
    }
    
    return newErrors;
  };
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
    // 입력시 해당 필드 에러 제거
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    const newErrors = validate();
    
    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }
    
    try {
      await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      setSubmitted(true);
      setFormData({ name: '', email: '', message: '' });
    } catch (error) {
      console.error(error);
    }
  };
  
  if (submitted) {
    return <div>Thank you for your message!</div>;
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Name:</label>
        <input
          type="text"
          name="name"
          value={formData.name}
          onChange={handleChange}
        />
        {errors.name && <span className="error">{errors.name}</span>}
      </div>
      
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
        <label>Message:</label>
        <textarea
          name="message"
          value={formData.message}
          onChange={handleChange}
        />
        {errors.message && <span className="error">{errors.message}</span>}
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}

// Custom Form Hook
function useForm(initialValues, validate) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues(prev => ({ ...prev, [name]: value }));
  };
  
  const handleBlur = (e) => {
    const { name } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const fieldErrors = validate({ ...values, [name]: values[name] });
    setErrors(prev => ({ ...prev, [name]: fieldErrors[name] }));
  };
  
  const handleSubmit = (onSubmit) => (e) => {
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

// 사용
function SignupForm() {
  const validate = (values) => {
    const errors = {};
    if (!values.email) errors.email = 'Required';
    if (!values.password) errors.password = 'Required';
    return errors;
  };
  
  const {
    values,
    errors,
    touched,
    handleChange,
    handleBlur,
    handleSubmit
  } = useForm({ email: '', password: '' }, validate);
  
  const onSubmit = (values) => {
    console.log('Form values:', values);
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        name="email"
        value={values.email}
        onChange={handleChange}
        onBlur={handleBlur}
      />
      {touched.email && errors.email && <span>{errors.email}</span>}
      
      <input
        name="password"
        type="password"
        value={values.password}
        onChange={handleChange}
        onBlur={handleBlur}
      />
      {touched.password && errors.password && <span>{errors.password}</span>}
      
      <button type="submit">Sign Up</button>
    </form>
  );
}
```

## 6단계: 상태 관리 라이브러리 (선택사항)

### Zustand (간단하고 권장)

bash

```bash
npm install zustand
```

javascript

```javascript
import create from 'zustand';

// Store 생성
const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 })
}));

// 사용
function Counter() {
  const { count, increment, decrement, reset } = useStore();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

// 복잡한 예시
const useTodoStore = create((set) => ({
  todos: [],
  addTodo: (text) =>
    set((state) => ({
      todos: [...state.todos, { id: Date.now(), text, completed: false }]
    })),
  toggleTodo: (id) =>
    set((state) => ({
      todos: state.todos.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    })),
  removeTodo: (id) =>
    set((state) => ({
      todos: state.todos.filter((todo) => todo.id !== id)
    }))
}));
```

## 학습 로드맵 추천

1. **1-2주**: JSX, 컴포넌트, Props
2. **1-2주**: useState, 이벤트 처리, 폼
3. **1-2주**: useEffect, 데이터 페칭, 생명주기
4. **2주**: Custom Hooks, Context API
5. **1-2주**: React Router, 실전 프로젝트
6. **추가**: TypeScript + React, 상태 관리 라이브러리