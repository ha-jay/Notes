Node.js를 체계적으로 학습할 수 있도록 단계별 로드맵을 정리해드리겠습니다.

## 1단계: Node.js 기초 (1주)

### 설치 및 설정

bash

```bash
# Node.js 설치 확인
node --version
npm --version

# 프로젝트 초기화
mkdir my-node-app
cd my-node-app
npm init -y

# package.json 생성됨
```

### 기본 모듈 시스템

javascript

```javascript
// CommonJS (기본)
// math.js
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

module.exports = {
  add,
  multiply
};

// 또는
exports.add = add;
exports.multiply = multiply;

// app.js
const math = require('./math');
// 또는
const { add, multiply } = require('./math');

console.log(add(2, 3));        // 5
console.log(multiply(2, 3));   // 6

// ES Modules (package.json에 "type": "module" 추가)
// math.mjs 또는 math.js
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

// default export
export default function subtract(a, b) {
  return a - b;
}

// app.mjs 또는 app.js
import subtract, { add, multiply } from './math.js';

console.log(add(2, 3));
console.log(subtract(5, 2));
```

### 내장 모듈 - fs (파일 시스템)

javascript

```javascript
const fs = require('fs');

// 1. 동기식 읽기/쓰기 (Sync - 블로킹)
try {
  const data = fs.readFileSync('file.txt', 'utf8');
  console.log(data);
  
  fs.writeFileSync('output.txt', 'Hello World');
} catch (err) {
  console.error(err);
}

// 2. 비동기식 콜백 (권장하지 않음)
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});

fs.writeFile('output.txt', 'Hello World', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File written');
});

// 3. Promise 기반 (권장)
const fs = require('fs').promises;

async function readWriteFile() {
  try {
    const data = await fs.readFile('file.txt', 'utf8');
    console.log(data);
    
    await fs.writeFile('output.txt', 'Hello World');
    console.log('File written');
    
    // 파일 추가
    await fs.appendFile('output.txt', '\nNew line');
    
    // 파일 삭제
    await fs.unlink('output.txt');
    
    // 디렉토리 생성
    await fs.mkdir('new-folder');
    
    // 디렉토리 읽기
    const files = await fs.readdir('./');
    console.log(files);
    
    // 파일 정보
    const stats = await fs.stat('file.txt');
    console.log(stats.isFile());
    console.log(stats.isDirectory());
    console.log(stats.size);
    
  } catch (err) {
    console.error(err);
  }
}

readWriteFile();

// 파일 존재 확인
async function fileExists(path) {
  try {
    await fs.access(path);
    return true;
  } catch {
    return false;
  }
}

// JSON 파일 읽기/쓰기
async function readJSON(filename) {
  const data = await fs.readFile(filename, 'utf8');
  return JSON.parse(data);
}

async function writeJSON(filename, data) {
  await fs.writeFile(filename, JSON.stringify(data, null, 2));
}

// 사용
const users = await readJSON('users.json');
users.push({ id: 3, name: 'Alice' });
await writeJSON('users.json', users);
```

### 내장 모듈 - path

javascript

```javascript
const path = require('path');

// 경로 결합
const filePath = path.join(__dirname, 'files', 'data.txt');
console.log(filePath);

// 절대 경로 생성
const absolutePath = path.resolve('files', 'data.txt');

// 확장자 추출
console.log(path.extname('file.txt'));      // .txt
console.log(path.extname('index.html'));    // .html

// 파일명 추출
console.log(path.basename('/foo/bar/file.txt'));           // file.txt
console.log(path.basename('/foo/bar/file.txt', '.txt'));   // file

// 디렉토리명 추출
console.log(path.dirname('/foo/bar/file.txt'));  // /foo/bar

// 경로 파싱
const parsed = path.parse('/foo/bar/file.txt');
console.log(parsed);
// {
//   root: '/',
//   dir: '/foo/bar',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file'
// }

// 정규화
console.log(path.normalize('/foo/bar/../baz'));  // /foo/baz

// __dirname과 __filename
console.log(__dirname);   // 현재 파일의 디렉토리 경로
console.log(__filename);  // 현재 파일의 전체 경로
```

### 내장 모듈 - http

javascript

```javascript
const http = require('http');

// 기본 서버
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});

// 라우팅 추가
const server = http.createServer((req, res) => {
  const { method, url } = req;
  
  res.setHeader('Content-Type', 'application/json');
  
  if (url === '/' && method === 'GET') {
    res.statusCode = 200;
    res.end(JSON.stringify({ message: 'Home Page' }));
  } 
  else if (url === '/about' && method === 'GET') {
    res.statusCode = 200;
    res.end(JSON.stringify({ message: 'About Page' }));
  }
  else if (url === '/api/users' && method === 'POST') {
    let body = '';
    
    req.on('data', chunk => {
      body += chunk.toString();
    });
    
    req.on('end', () => {
      const data = JSON.parse(body);
      res.statusCode = 201;
      res.end(JSON.stringify({ message: 'User created', data }));
    });
  }
  else {
    res.statusCode = 404;
    res.end(JSON.stringify({ message: 'Not Found' }));
  }
});

server.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### process와 환경 변수

javascript

```javascript
// process 객체
console.log(process.version);        // Node.js 버전
console.log(process.platform);       // 운영체제
console.log(process.cwd());          // 현재 작업 디렉토리
console.log(process.pid);            // 프로세스 ID
console.log(process.memoryUsage());  // 메모리 사용량

// 명령행 인자
console.log(process.argv);
// node app.js arg1 arg2
// ['node경로', 'app.js경로', 'arg1', 'arg2']

const args = process.argv.slice(2);
console.log(args);  // ['arg1', 'arg2']

// 환경 변수
console.log(process.env.NODE_ENV);
console.log(process.env.PORT);

// .env 파일 사용 (dotenv 패키지)
// npm install dotenv

// .env 파일
// PORT=3000
// DB_HOST=localhost
// DB_PASSWORD=secret123

require('dotenv').config();

const PORT = process.env.PORT || 3000;
const DB_HOST = process.env.DB_HOST;

console.log(`Server will run on port ${PORT}`);

// 프로세스 종료
process.exit(0);  // 성공
process.exit(1);  // 에러

// 이벤트
process.on('exit', (code) => {
  console.log(`About to exit with code: ${code}`);
});

process.on('uncaughtException', (err) => {
  console.error('Uncaught Exception:', err);
  process.exit(1);
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason);
});
```

## 2단계: Express.js 기초 (1-2주)

### Express 설치 및 기본 서버

bash

```bash
npm install express
```

javascript

```javascript
const express = require('express');
const app = express();

// 미들웨어 - JSON 파싱
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// 기본 라우트
app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/api/users', (req, res) => {
  res.json({ users: [] });
});

app.post('/api/users', (req, res) => {
  const { name, email } = req.body;
  res.status(201).json({ message: 'User created', data: { name, email } });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### 라우팅

javascript

```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

// GET /api/users
router.get('/', (req, res) => {
  res.json({ users: [] });
});

// GET /api/users/:id
router.get('/:id', (req, res) => {
  const { id } = req.params;
  res.json({ user: { id, name: 'John' } });
});

// POST /api/users
router.post('/', (req, res) => {
  const { name, email } = req.body;
  res.status(201).json({ message: 'User created', data: { name, email } });
});

// PUT /api/users/:id
router.put('/:id', (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;
  res.json({ message: 'User updated', data: { id, name, email } });
});

// DELETE /api/users/:id
router.delete('/:id', (req, res) => {
  const { id } = req.params;
  res.json({ message: 'User deleted', id });
});

module.exports = router;

// app.js
const express = require('express');
const app = express();
const usersRouter = require('./routes/users');

app.use(express.json());

// 라우터 연결
app.use('/api/users', usersRouter);

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### 미들웨어

javascript

```javascript
const express = require('express');
const app = express();

// 1. 내장 미들웨어
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static('public'));  // 정적 파일 제공

// 2. 커스텀 미들웨어 - 로깅
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next();  // 다음 미들웨어로 이동
});

// 3. 특정 경로에만 적용
app.use('/api', (req, res, next) => {
  console.log('API request');
  next();
});

// 4. 인증 미들웨어
const authenticate = (req, res, next) => {
  const token = req.headers['authorization'];
  
  if (!token) {
    return res.status(401).json({ message: 'No token provided' });
  }
  
  // 토큰 검증 로직
  if (token === 'valid-token') {
    req.user = { id: 1, name: 'John' };
    next();
  } else {
    res.status(401).json({ message: 'Invalid token' });
  }
};

// 보호된 라우트
app.get('/api/protected', authenticate, (req, res) => {
  res.json({ message: 'Protected data', user: req.user });
});

// 5. 에러 핸들링 미들웨어 (반드시 마지막에)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: 'Something went wrong!', error: err.message });
});

// 404 핸들러
app.use((req, res) => {
  res.status(404).json({ message: 'Route not found' });
});

// 모듈화된 미들웨어
// middleware/logger.js
function logger(req, res, next) {
  console.log(`${req.method} ${req.url}`);
  next();
}

module.exports = logger;

// middleware/validate.js
function validateUser(req, res, next) {
  const { name, email } = req.body;
  
  if (!name || !email) {
    return res.status(400).json({ message: 'Name and email are required' });
  }
  
  if (!/\S+@\S+\.\S+/.test(email)) {
    return res.status(400).json({ message: 'Invalid email' });
  }
  
  next();
}

module.exports = validateUser;

// 사용
const logger = require('./middleware/logger');
const validateUser = require('./middleware/validate');

app.use(logger);
app.post('/api/users', validateUser, (req, res) => {
  // 여기 도달하면 검증 통과
  res.json({ message: 'User created' });
});
```

### 요청과 응답 처리

javascript

```javascript
const express = require('express');
const app = express();

app.use(express.json());

app.get('/api/demo', (req, res) => {
  // 1. 쿼리 파라미터
  // GET /api/demo?name=John&age=30
  const { name, age } = req.query;
  console.log(name, age);
  
  // 2. URL 파라미터
  // (아래 라우트에서)
  
  // 3. 헤더 읽기
  const userAgent = req.get('User-Agent');
  const contentType = req.get('Content-Type');
  
  // 4. 다양한 응답
  res.send('Simple text');
  res.json({ message: 'JSON response' });
  res.status(201).json({ created: true });
  
  // 5. 헤더 설정
  res.set('X-Custom-Header', 'value');
  res.cookie('token', 'abc123', { maxAge: 900000, httpOnly: true });
  
  // 6. 리다이렉트
  res.redirect('/new-url');
  res.redirect(301, '/new-url');  // 영구 리다이렉트
  
  // 7. 파일 다운로드
  res.download('./files/document.pdf');
  res.sendFile(__dirname + '/public/index.html');
});

// URL 파라미터
app.get('/api/users/:id', (req, res) => {
  const { id } = req.params;
  res.json({ userId: id });
});

// 여러 파라미터
app.get('/api/posts/:year/:month', (req, res) => {
  const { year, month } = req.params;
  res.json({ year, month });
});

// 선택적 파라미터
app.get('/api/users/:id/:action?', (req, res) => {
  const { id, action } = req.params;
  res.json({ id, action: action || 'view' });
});

// POST 요청 본문
app.post('/api/users', (req, res) => {
  const { name, email, age } = req.body;
  
  // 데이터 저장 로직
  
  res.status(201).json({
    message: 'User created',
    data: { id: Date.now(), name, email, age }
  });
});

// 파일 업로드 (multer 사용)
// npm install multer
const multer = require('multer');

const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + '-' + file.originalname);
  }
});

const upload = multer({ storage });

app.post('/api/upload', upload.single('file'), (req, res) => {
  if (!req.file) {
    return res.status(400).json({ message: 'No file uploaded' });
  }
  
  res.json({
    message: 'File uploaded',
    filename: req.file.filename,
    size: req.file.size
  });
});

// 여러 파일 업로드
app.post('/api/upload-multiple', upload.array('files', 10), (req, res) => {
  res.json({
    message: 'Files uploaded',
    count: req.files.length,
    files: req.files.map(f => f.filename)
  });
});
```

## 3단계: 데이터베이스 연동 (1-2주)

### MongoDB (Mongoose)

bash

```bash
npm install mongoose
```

javascript

```javascript
const mongoose = require('mongoose');

// 연결
mongoose.connect('mongodb://localhost:27017/myapp', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.error('MongoDB connection error:', err));

// 스키마 정의
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    minlength: 2,
    maxlength: 50
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: [/\S+@\S+\.\S+/, 'Invalid email']
  },
  age: {
    type: Number,
    min: 0,
    max: 120
  },
  role: {
    type: String,
    enum: ['user', 'admin', 'moderator'],
    default: 'user'
  },
  isActive: {
    type: Boolean,
    default: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
}, {
  timestamps: true  // createdAt, updatedAt 자동 생성
});

// 인스턴스 메서드
userSchema.methods.getPublicProfile = function() {
  return {
    id: this._id,
    name: this.name,
    email: this.email
  };
};

// 정적 메서드
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email });
};

// 미들웨어 (훅)
userSchema.pre('save', function(next) {
  console.log('About to save user:', this.name);
  next();
});

// 모델 생성
const User = mongoose.model('User', userSchema);

// CRUD 작업
async function userOperations() {
  try {
    // Create
    const user = await User.create({
      name: 'John Doe',
      email: 'john@example.com',
      age: 30
    });
    console.log('User created:', user);
    
    // 또는
    const newUser = new User({
      name: 'Jane Doe',
      email: 'jane@example.com'
    });
    await newUser.save();
    
    // Read
    const users = await User.find();  // 모든 사용자
    const oneUser = await User.findById(userId);
    const activeUsers = await User.find({ isActive: true });
    const adminUser = await User.findOne({ role: 'admin' });
    
    // 쿼리 체이닝
    const results = await User
      .find({ isActive: true })
      .where('age').gt(18)
      .sort({ createdAt: -1 })
      .limit(10)
      .select('name email')
      .exec();
    
    // Update
    await User.findByIdAndUpdate(
      userId,
      { age: 31 },
      { new: true }  // 업데이트된 문서 반환
    );
    
    await User.updateMany(
      { isActive: false },
      { role: 'inactive' }
    );
    
    // Delete
    await User.findByIdAndDelete(userId);
    await User.deleteMany({ isActive: false });
    
  } catch (error) {
    console.error(error);
  }
}

// Express 라우터와 통합
const express = require('express');
const router = express.Router();

// GET /api/users
router.get('/', async (req, res) => {
  try {
    const users = await User.find().select('-__v');
    res.json({ users });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// GET /api/users/:id
router.get('/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    
    res.json({ user });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// POST /api/users
router.post('/', async (req, res) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json({ message: 'User created', user });
  } catch (error) {
    if (error.name === 'ValidationError') {
      return res.status(400).json({ message: error.message });
    }
    res.status(500).json({ message: error.message });
  }
});

// PUT /api/users/:id
router.put('/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(
      req.params.id,
      req.body,
      { new: true, runValidators: true }
    );
    
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    
    res.json({ message: 'User updated', user });
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

// DELETE /api/users/:id
router.delete('/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndDelete(req.params.id);
    
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    
    res.json({ message: 'User deleted' });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

module.exports = router;
```

### PostgreSQL (pg 또는 Sequelize)

bash

```bash
npm install pg
# 또는 ORM
npm install sequelize
```

javascript

```javascript
// 1. pg 라이브러리 직접 사용
const { Pool } = require('pg');

const pool = new Pool({
  user: 'your_username',
  host: 'localhost',
  database: 'myapp',
  password: 'your_password',
  port: 5432,
});

// 쿼리 실행
async function getUsers() {
  try {
    const result = await pool.query('SELECT * FROM users');
    return result.rows;
  } catch (error) {
    console.error(error);
    throw error;
  }
}

async function createUser(name, email) {
  const query = 'INSERT INTO users (name, email) VALUES ($1, $2) RETURNING *';
  const values = [name, email];
  
  const result = await pool.query(query, values);
  return result.rows[0];
}

async function getUserById(id) {
  const query = 'SELECT * FROM users WHERE id = $1';
  const result = await pool.query(query, [id]);
  return result.rows[0];
}

// 2. Sequelize ORM
const { Sequelize, DataTypes } = require('sequelize');

const sequelize = new Sequelize('myapp', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});

// 모델 정의
const User = sequelize.define('User', {
  name: {
    type: DataTypes.STRING,
    allowNull: false,
    validate: {
      len: [2, 50]
    }
  },
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true,
    validate: {
      isEmail: true
    }
  },
  age: {
    type: DataTypes.INTEGER,
    validate: {
      min: 0,
      max: 120
    }
  },
  role: {
    type: DataTypes.ENUM('user', 'admin', 'moderator'),
    defaultValue: 'user'
  }
}, {
  timestamps: true
});

// 동기화 (개발 환경에서만)
await sequelize.sync({ alter: true });

// CRUD
async function sequelizeOperations() {
  // Create
  const user = await User.create({
    name: 'John Doe',
    email: 'john@example.com',
    age: 30
  });
  
  // Read
  const users = await User.findAll();
  const oneUser = await User.findByPk(1);
  const filtered = await User.findAll({
    where: {
      age: { [Sequelize.Op.gte]: 18 }
    },
    order: [['createdAt', 'DESC']],
    limit: 10
  });
  
  // Update
  await User.update(
    { age: 31 },
    { where: { id: 1 } }
  );
  
  // Delete
  await User.destroy({
    where: { id: 1 }
  });
}
```

## 4단계: 인증과 보안 (1-2주)

### JWT 인증

bash

```bash
npm install jsonwebtoken bcryptjs
```

javascript

```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

const JWT_SECRET = process.env.JWT_SECRET || 'your-secret-key';

// 비밀번호 해싱
async function hashPassword(password) {
  const salt = await bcrypt.genSalt(10);
  return bcrypt.hash(password, salt);
}

async function comparePassword(password, hashedPassword) {
  return bcrypt.compare(password, hashedPassword);
}

// JWT 생성
function generateToken(userId) {
  return jwt.sign(
    { userId },
    JWT_SECRET,
    { expiresIn: '7d' }
  );
}

// JWT 검증
function verifyToken(token) {
  try {
    return jwt.verify(token, JWT_SECRET);
  } catch (error) {
    return null;
  }
}

// 회원가입
router.post('/register', async (req, res) => {
  try {
    const { name, email, password } = req.body;
    
    // 이미 존재하는 이메일 확인
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(400).json({ message: 'Email already exists' });
    }
    
    // 비밀번호 해싱
    const hashedPassword = await hashPassword(password);
    
    // 사용자 생성
    const user = await User.create({
      name,
      email,
      password: hashedPassword
    });
    
    // 토큰 생성
    const token = generateToken(user._id);
    
    res.status(201).json({
      message: 'User registered successfully',
      token,
      user: {
        id: user._id,
        name: user.name,
        email: user.email
      }
    });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// 로그인
router.post('/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    
    // 사용자 찾기
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({ message: 'Invalid credentials' });
    }
    
    // 비밀번호 확인
    const isValidPassword = await comparePassword(password, user.password);
    if (!isValidPassword) {
      return res.status(401).json({ message: 'Invalid credentials' });
    }
    
    // 토큰 생성
    const token = generateToken(user._id);
    
    res.json({
      message: 'Login successful',
      token,
      user: {
        id: user._id,
        name: user.name,
        email: user.email
      }
    });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// 인증 미들웨어
const authenticate = async (req, res, next) => {
  try {
    // 헤더에서 토큰 추출
    const authHeader = req.headers.authorization;
    
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      return res.status(401).json({ message: 'No token provided' });
    }
    
    const token = authHeader.substring(7);
    
    // 토큰 검증
    const decoded = verifyToken(token);
    if (!decoded) {
      return res.status(401).json({ message: 'Invalid token' });
    }
    
    // 사용자 찾기
    const user = await User.findById(decoded.userId).select('-password');
    if (!user) {
      return res.status(401).json({ message: 'User not found' });
    }
    
    // req 객체에 사용자 정보 추가
    req.user = user;
    next();
  } catch (error) {
    res.status(401).json({ message: 'Authentication failed' });
  }
};

// 권한 확인 미들웨어
const authorize = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ message: 'Forbidden' });
    }
    next();
  };
};

// 보호된 라우트
router.get('/profile', authenticate, async (req, res) => {
  res.json({ user: req.user });
});

// 관리자 전용 라우트
router.get('/admin', authenticate, authorize('admin'), async (req, res) => {
  res.json({ message: 'Admin data' });
});
```

### 보안 모범 사례

bash

```bash
npm install helmet cors express-rate-limit express-mongo-sanitize xss-clean
```

javascript

````javascript
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const rateLimit = require('express-rate-limit');
const mongoSanitize = require('express-mongo-sanitize');
const xss = require('xss-clean');

const app = express();

// 1. Helmet - HTTP 헤더 보안
app.use(helmet());

// 2. CORS 설정
app.use(cors({
  origin: process.env.CLIENT_URL || 'http://localhost:3000',
  credentials: true
}));

// 3. Rate Limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15분
  max: 100, // 최대 100 요청
  message: 'Too many requests from this IP'
});
app.use('/api', limiter);

// 로그인 엔드포인트에 더 엄격한 제한
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
  message: 'Too many login attempts'
});
app.use('/api/auth/login', loginLimiter);

// 4. Body 파싱 (크기 제한)
app.use(express.json({ limit: '10kb' }));

// 5. NoSQL Injection 방지
app.use(mongoSanitize());

// 6. XSS 공격 방지
app.use(xss());

// 7. 환경 변수 검증
if (!process.env.JWT_SECRET) {
  throw new Error('JWT_SECRET is not defined');
}

// 8. 에러 핸들링
app.use((err, req, res, next) => {
  console.error(err.stack);
  
  // 프로덕션에서는 상세 에러 정보 숨기기
  if (process.env.NODE_ENV === 'production') {
    res.status(500).json({ message: 'Internal server error' });
  } else {
    res.status(500).json({ message: err.message, stack: err.stack });
  }
});
```

## 5단계: 실전 프로젝트 구조

### 폴더 구조
```
my-node-app/
├── src/
│   ├── config/
│   │   ├── database.js
│   │   └── env.js
│   ├── controllers/
│   │   ├── authController.js
│   │   └── userController.js
│   ├── middleware/
│   │   ├── auth.js
│   │   ├── errorHandler.js
│   │   └── validate.js
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   ├── auth.js
│   │   └── users.js
│   ├── services/
│   │   └── userService.js
│   ├── utils/
│   │   ├── logger.js
│   │   └── helpers.js
│   └── app.js
├── tests/
├── .env
├── .env.example
├── .gitignore
├── package.json
└── server.js
````

### 실전 예시

javascript

```javascript
// config/database.js
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true
    });
    console.log('MongoDB connected');
  } catch (error) {
    console.error('MongoDB connection error:', error);
    process.exit(1);
  }
};

module.exports = connectDB;

// controllers/userController.js
const userService = require('../services/userService');

exports.getUsers = async (req, res, next) => {
  try {
    const users = await userService.getAllUsers();
    res.json({ users });
  } catch (error) {
    next(error);
  }
};

exports.getUserById = async (req, res, next) => {
  try {
    const user = await userService.getUserById(req.params.id);
    
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    
    res.json({ user });
  } catch (error) {
    next(error);
  }
};

// services/userService.js
const User = require('../models/User');

exports.getAllUsers = async () => {
  return User.find().select('-password');
};

exports.getUserById = async (id) => {
  return User.findById(id).select('-password');
};

exports.createUser = async (userData) => {
  return User.create(userData);
};

// routes/users.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');
const { authenticate } = require('../middleware/auth');

router.get('/', authenticate, userController.getUsers);
router.get('/:id', authenticate, userController.getUserById);

module.exports = router;

// app.js
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const userRoutes = require('./routes/users');
const errorHandler = require('./middleware/errorHandler');

const app = express();

app.use(helmet());
app.use(cors());
app.use(express.json());

app.use('/api/users', userRoutes);

app.use(errorHandler);

module.exports = app;

// server.js
require('dotenv').config();
const app = require('./src/app');
const connectDB = require('./src/config/database');

const PORT = process.env.PORT || 3000;

connectDB().then(() => {
  app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
  });
});
```