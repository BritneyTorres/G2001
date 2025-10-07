# 🧠 **Sistema CRÍTICO MERN – Documentación Completa**

## 🪶 Resumen Ejecutivo

El sistema **CRÍTICO MERN** es una plataforma educativa basada en inteligencia artificial diseñada para desarrollar la **lectura crítica** en estudiantes.
Se centra en ofrecer textos académicos, preguntas analíticas y retroalimentación inmediata sobre el razonamiento, detectando sesgos y áreas de mejora.

Está construido sobre el **stack MERN (MongoDB, Express, React, Node.js)** y utiliza modelos de procesamiento de lenguaje natural para analizar las respuestas de los usuarios y generar informes personalizados.

El objetivo principal es **fomentar el pensamiento crítico** en contextos educativos mediante la automatización y personalización del aprendizaje.

---

## 🧩 Arquitectura General

**Frontend:** React.js + Tailwind CSS
**Backend:** Node.js + Express
**Base de Datos:** MongoDB
**IA / NLP:** OpenAI API y librerías NLP.js
**Autenticación:** JWT (JSON Web Tokens)
**Almacenamiento:** Cloud o local según entorno

### 🔁 Flujo General

1. El usuario inicia sesión o se registra.
2. Accede a textos académicos seleccionados.
3. Responde preguntas de análisis crítico.
4. El sistema analiza las respuestas mediante IA.
5. Se genera retroalimentación textual y visual.
6. El docente puede revisar el progreso del alumno.

---

## 🧱 Módulos Principales

| Módulo           | Descripción                                         |
| ---------------- | --------------------------------------------------- |
| **Auth**         | Registro, inicio y cierre de sesión, JWT            |
| **Users**        | Gestión de roles (estudiante, docente, admin)       |
| **Texts**        | Administración y visualización de textos académicos |
| **Questions**    | Creación y asignación de preguntas                  |
| **Attempts**     | Registro de respuestas del usuario                  |
| **BiasAnalysis** | Análisis automático de sesgos                       |
| **Feedback**     | Retroalimentación generada por IA                   |
| **Reports**      | Visualización de resultados e informes              |
| **Enrollment**   | Relación entre usuarios y cursos o módulos          |

---

## 🗂️ Endpoints Principales

### 1. Autenticación

| Método | Endpoint             | Descripción               |
| ------ | -------------------- | ------------------------- |
| POST   | `/api/auth/register` | Registro de nuevo usuario |
| POST   | `/api/auth/login`    | Inicio de sesión          |
| GET    | `/api/auth/logout`   | Cierre de sesión          |

### 2. Usuarios

| Método | Endpoint         | Descripción                |
| ------ | ---------------- | -------------------------- |
| GET    | `/api/users/`    | Obtener todos los usuarios |
| GET    | `/api/users/:id` | Obtener usuario por ID     |
| PUT    | `/api/users/:id` | Actualizar usuario         |
| DELETE | `/api/users/:id` | Eliminar usuario           |

### 3. Textos Académicos

| Método | Endpoint         | Descripción          |
| ------ | ---------------- | -------------------- |
| GET    | `/api/texts/`    | Listar textos        |
| GET    | `/api/texts/:id` | Ver texto específico |
| POST   | `/api/texts/`    | Crear nuevo texto    |
| PUT    | `/api/texts/:id` | Editar texto         |
| DELETE | `/api/texts/:id` | Eliminar texto       |

### 4. Preguntas

| Método | Endpoint             | Descripción       |
| ------ | -------------------- | ----------------- |
| GET    | `/api/questions/`    | Listar preguntas  |
| POST   | `/api/questions/`    | Crear pregunta    |
| GET    | `/api/questions/:id` | Ver pregunta      |
| PUT    | `/api/questions/:id` | Editar pregunta   |
| DELETE | `/api/questions/:id` | Eliminar pregunta |

---

## 🧮 Modelos de Datos (MongoDB)

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

### **2. Text (Texto Académico)**

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

## 🧭 Diagrama Conceptual (Explicación)

```
User ───< Enrollment >─── Course
  │
  ├──< Attempt >── Question ───< Text
  │                    │
  │                    └── BiasAnalysis
  │
  └──< Feedback (por Attempt) >
```

👉 **Significado:**

* Un **usuario** puede estar en varios **cursos**.
* Cada curso tiene **textos** con **preguntas**.
* El usuario responde preguntas (**Attempt**).
* El sistema analiza sesgos (**BiasAnalysis**) y genera **Feedback** automático.

---

## ⚙️ Quickstart Técnico para Desarrolladores

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

3. **Configurar variables de entorno (.env)**

   ```
   MONGO_URI=mongodb://localhost:27017/critico
   JWT_SECRET=clave_secreta
   OPENAI_API_KEY=tu_api_key
   PORT=5000
   ```

4. **Iniciar el servidor**

   ```bash
   npm run dev
   ```

5. **Abrir en el navegador**

   ```
   http://localhost:3000
   ```

---


