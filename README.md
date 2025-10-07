
# 🧠 **Sistema CRÍTICO MERN – Documentación Completa (Versión Final con Pruebas)**

## 🪶 Resumen Ejecutivo

El sistema **CRÍTICO MERN** es una plataforma educativa inteligente enfocada en mejorar la **lectura crítica** mediante el análisis de respuestas, detección de sesgos y retroalimentación automática.
Usa el stack **MERN (MongoDB, Express, React, Node.js)** con integración de **IA para procesamiento del lenguaje natural**.
Su objetivo es fomentar la reflexión y el razonamiento crítico en contextos académicos.

---

## 🧩 Arquitectura General

**Frontend:** React + Tailwind CSS
**Backend:** Node.js + Express
**Base de Datos:** MongoDB
**IA / NLP:** OpenAI API + NLP.js
**Autenticación:** JWT
**Control de versiones:** GitHub
**Pruebas automáticas:** Jest + Supertest

---

## 🔁 Flujo General del Sistema

1. El usuario se registra o inicia sesión.
2. Selecciona un texto académico.
3. Responde preguntas de análisis.
4. El backend evalúa la respuesta con IA.
5. Se genera retroalimentación y puntuación.
6. El usuario visualiza su progreso y resultados.

---

## 🧱 Módulos Principales

| Módulo           | Descripción                             |
| ---------------- | --------------------------------------- |
| **Auth**         | Registro e inicio de sesión             |
| **Users**        | Gestión de usuarios y roles             |
| **Texts**        | Almacenamiento de textos académicos     |
| **Questions**    | Administración de preguntas             |
| **Attempts**     | Registro de respuestas de usuarios      |
| **BiasAnalysis** | Análisis automático de sesgos           |
| **Feedback**     | Retroalimentación generada por IA       |
| **Reports**      | Resultados y estadísticas               |
| **Testing**      | Scripts y casos de prueba automatizados |

---

## 🧮 Modelos de Datos (MongoDB)

*(los modelos ya revisados se mantienen iguales, solo se agrega el contexto final)*

### **1. User**

```js
{
  _id: ObjectId,
  name: String,
  email: String,
  password: String,
  role: { type: String, enum: ['student', 'teacher', 'admin'] },
  createdAt: Date,
  updatedAt: Date
}
```

### **2. Text**

```js
{
  _id: ObjectId,
  title: String,
  author: String,
  content: String,
  difficulty: { type: String, enum: ['easy', 'medium', 'hard'] },
  tags: [String],
  createdAt: Date
}
```

### **3. Question**

```js
{
  _id: ObjectId,
  textId: { type: ObjectId, ref: 'Text' },
  questionText: String,
  type: { type: String, enum: ['multiple', 'open'] },
  options: [String],
  correctAnswer: String
}
```

### **4. Attempt**

```js
{
  _id: ObjectId,
  userId: { type: ObjectId, ref: 'User' },
  questionId: { type: ObjectId, ref: 'Question' },
  response: String,
  score: Number,
  analyzedBias: { type: ObjectId, ref: 'BiasAnalysis' },
  createdAt: Date
}
```

### **5. BiasAnalysis**

```js
{
  _id: ObjectId,
  attemptId: { type: ObjectId, ref: 'Attempt' },
  detectedBias: String,
  confidence: Number,
  aiExplanation: String
}
```

### **6. Feedback**

```js
{
  _id: ObjectId,
  attemptId: { type: ObjectId, ref: 'Attempt' },
  feedbackText: String,
  aiGenerated: Boolean,
  createdAt: Date
}
```

### **7. Enrollment**

```js
{
  _id: ObjectId,
  userId: { type: ObjectId, ref: 'User' },
  courseName: String,
  progress: Number,
  enrolledAt: Date
}
```

---

## 🧪 **Sección de Pruebas (Testing y Automatización)**

### 🗂️ Estructura de Carpetas para Pruebas

```
backend/
│
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   └── tests/
│       ├── auth.test.js
│       ├── texts.test.js
│       ├── questions.test.js
│       └── feedback.test.js
│
├── package.json
└── jest.config.js
```

---

### ⚙️ **Configuración de Jest y Supertest**

**Instalación:**

```bash
npm install --save-dev jest supertest
```

**En package.json:**

```json
"scripts": {
  "test": "jest --runInBand",
  "test:watch": "jest --watchAll"
}
```

**jest.config.js:**

```js
export default {
  testEnvironment: 'node',
  verbose: true,
  transform: {}
}
```

---

### 🧾 **Ejemplo de Test Unitario (Auth)**

**Archivo:** `tests/auth.test.js`

```js
import request from 'supertest';
import app from '../server.js';

describe('Auth API Tests', () => {
  test('Registro de usuario exitoso', async () => {
    const res = await request(app)
      .post('/api/auth/register')
      .send({
        name: 'Usuario Test',
        email: 'test@example.com',
        password: '123456'
      });
    expect(res.statusCode).toBe(201);
    expect(res.body).toHaveProperty('token');
  });

  test('Inicio de sesión con credenciales válidas', async () => {
    const res = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'test@example.com',
        password: '123456'
      });
    expect(res.statusCode).toBe(200);
    expect(res.body).toHaveProperty('token');
  });
});
```

---

### 🧾 **Ejemplo de Test de Integración (Textos Académicos)**

**Archivo:** `tests/texts.test.js`

```js
import request from 'supertest';
import app from '../server.js';

describe('Textos Académicos API', () => {
  test('Debe crear un nuevo texto', async () => {
    const res = await request(app)
      .post('/api/texts')
      .send({
        title: 'El pensamiento crítico en la educación',
        author: 'Autor de Prueba',
        content: 'Contenido de ejemplo...',
        difficulty: 'medium',
        tags: ['lectura crítica']
      });
    expect(res.statusCode).toBe(201);
    expect(res.body).toHaveProperty('title');
  });

  test('Debe listar todos los textos', async () => {
    const res = await request(app).get('/api/texts');
    expect(res.statusCode).toBe(200);
    expect(Array.isArray(res.body)).toBe(true);
  });
});
```

---

### 🤖 **Automatización de Pruebas en GitHub Actions**

**Archivo:** `.github/workflows/tests.yml`

```yaml
name: Run MERN Tests

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

---

## ⚙️ Quickstart para Desarrolladores

1. **Clonar repositorio**

   ```bash
   git clone https://github.com/usuario/critico-mern.git
   cd critico-mern
   ```

2. **Instalar dependencias**

   ```bash
   npm install
   cd client && npm install
   ```

3. **Configurar variables**

   ```
   MONGO_URI=mongodb://localhost:27017/critico
   JWT_SECRET=clave_secreta
   OPENAI_API_KEY=tu_api_key
   ```

4. **Ejecutar tests**

   ```bash
   npm test
   ```
