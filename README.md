# Vue.js API Mastery
## The Complete Guide to Data Fetching

## 📑 Table of Contents
- [Introduction](#introduction)
- [What You'll Learn](#what-youll-learn)
- [The Core Problem](#the-core-problem)
- [Project Setup & Structure](#project-setup--structure)
- [The API Lifecycle](#the-api-lifecycle)
- [Strategy 1: The Inline Component Pattern](#strategy-1-the-inline-component-pattern)
- [Strategy 2: The Composable Pattern (Professional)](#strategy-2-the-composable-pattern-professional)
- [Strategy 3: Advanced State (TanStack Query)](#strategy-3-advanced-state-tanstack-query)
- [Global Configurations (Axios Interceptors)](#global-configurations-axios-interceptors)
- [UI/UX Handling (Loading, Error, Success)](#uiux-handling-loading-error-success)
- [Comparison Table](#comparison-table)
- [Summary & Best Practices](#summary--best-practices)
- [Author](#author)

---

## Introduction
This project is a comprehensive guide to handling API communications in **Vue 3**.  
In modern frontend development, fetching data is a fundamental task, but managing it efficiently at scale is the real challenge.

This repository acts as a technical reference for building scalable, maintainable, and professional API layers in Vue.js applications.

---

## What You'll Learn
- Proper architectural structuring of an API layer in Vue.js projects
- Building reusable **Composables** to encapsulate fetching logic
- Managing UI states (**Loading, Error, Success**) on both global and local levels
- Advanced usage of **Axios Interceptors** for authentication and error handling
- Integrating **TanStack Query (Vue Query)** for enterprise-grade caching and synchronization

---

## ⚠️ The Core Problem
Many Vue applications start by placing API logic directly inside components.  
While this may work initially, it introduces several technical debts:

### Code Duplication
Repeating `try/catch`, loading states, and error handling across components

### Maintenance Complexity
Any change to endpoints or headers requires updates in multiple files

### Resource Management Issues
Unhandled component unmounting can cause race conditions or memory leaks

---

## Project Setup & Structure
A production-ready Vue project must clearly separate the **Data Layer** from the **Presentation Layer**.

```bash
src/
├── api/
│   ├── index.js          # Axios instance, interceptors, base configuration
│   └── services/         # Pure API functions grouped by entity (Users, Products, etc.)
├── composables/
│   └── useApi.js         # Core logic for handling API requests
├── components/
│   ├── Skeletons/        # Loading placeholders
│   └── ErrorStates/      # Standardized error UI
└── views/                # Page-level components


```

## The API Lifecycle
Understanding the lifecycle of an API request is essential for stability and debugging.

### Setup
Initialize reactive references such as `data`, `loading`, and `error`.

### Trigger
Execute requests via lifecycle hooks (`onMounted`) or user interactions.

### Processing
Axios interceptors inject required metadata like authorization headers.

### State Update
The UI reacts automatically to success or failure states.

### Clean-up
Pending requests should be cancelled using `AbortController`.

---

## 1️⃣ Strategy 1: The Inline Component Pattern
The simplest approach where API logic is written directly inside the Vue component.

### Use Case
- Small demos or proof-of-concept projects

### Drawbacks
- Strong coupling between UI and data logic  
- No reusability  
- Difficult to scale or maintain  

---

## 2️⃣ Strategy 2: The Composable Pattern (Professional)
The industry-standard approach for medium to large Vue applications.

### The Engine (`composables/useApi.js`)
```js
import { ref } from 'vue';

export function useApi(apiService) {
  const data = ref(null);
  const loading = ref(false);
  const error = ref(null);

  const execute = async (...args) => {
    loading.value = true;
    error.value = null;

    try {
      const response = await apiService(...args);
      data.value = response.data;
    } catch (err) {
      error.value =
        err.response?.data?.message || 'An unexpected error occurred';
    } finally {
      loading.value = false;
    }
  };

  return { data, loading, error, execute };
}
```

### Component Implementation
```vue
<script setup>
import { useApi } from '@/composables/useApi';
import { getUserProfile } from '@/api/services/user';

const { data: user, loading, error, execute } = useApi(getUserProfile);

// Trigger request
execute(123);
</script>
```

### Why This Works
- Clean and readable components  
- Centralized request logic  
- Easy to maintain and scale  
- Encourages code reuse  

---

## 3️⃣ Strategy 3: Advanced State (TanStack Query)
Recommended for enterprise-level applications.

### Key Advantages
- Automatic caching  
- Background refetching  
- Request deduplication  
- Window focus revalidation  

### Result
- Fewer network requests  
- Less boilerplate  
- Highly predictable server state management  

---

## ⚙️ Global Configurations (Axios Interceptors)
Interceptors allow centralized handling of cross-cutting concerns.

```js
// src/api/index.js
import axios from 'axios';

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
});

// Request Interceptor: Token Injection
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response Interceptor: Global Error Handling
api.interceptors.response.use(
  response => response,
  error => {
    if (error.response?.status === 401) {
      // Example: redirect to login
    }
    return Promise.reject(error);
  }
);

export default api;
```

## UI/UX Handling (Loading, Error, Success)

### Loading State
Skeleton screens should be displayed while data is loading to reduce perceived latency and improve user experience.

### Error Feedback & Recovery
Errors should:
- Be clear and user-friendly  
- Avoid blocking the UI  
- Provide a **Retry** mechanism that re-executes the request  

---

## Comparison Table

| Feature | Inline Call | Composables | TanStack Query |
|------|------------|-------------|----------------|
| Complexity | Low | Medium | High |
| Reusability | None | High | High |
| Caching | No | Manual | Automatic |
| State Management | Manual | Manual | Automatic |
| Project Scale | Small | Medium / Large | Enterprise |

---

## Summary & Best Practices

### DRY Principle
Encapsulate repeated logic into composables.

### Single Responsibility
Services handle data fetching, components handle presentation.

### Environment Security
Use `.env` files for base URLs and sensitive configurations.

### UX First
Always handle loading and error states properly.

---

## Author
**Ameen Mohamed**  
Frontend Developer @ NEOP
