# 🚀 Vue.js API Mastery: The Ultimate Data Fetching Guide

[![Vue Version](https://img.shields.io/badge/Vue.js-3.x-4fc08d?style=for-the-badge&logo=vue.js)](https://vuejs.org/)
[![Axios](https://img.shields.io/badge/Axios-Latest-5a29e4?style=for-the-badge&logo=axios)](https://axios-http.com/)
[![Vite](https://img.shields.io/badge/Vite-5.x-646cff?style=for-the-badge&logo=vite)](https://vitejs.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

## 📌 Table of Contents
- [Introduction](#introduction)
- [The Core Problem](#the-problem)
- [Prerequisites](#prerequisites)
- [Project Architecture](#architecture)
- [Data Fetching Strategies](#strategies)
    - [1. Inline Component Pattern](#1-inline-pattern)
    - [2. The Composable Pattern (Clean Code)](#2-composable-pattern)
    - [3. Advanced State (TanStack Query)](#3-tanstack-query)
- [Global Configuration (Axios Interceptors)](#global-config)
- [Handling UI/UX States](#ui-ux-handling)
- [Folder Structure](#structure)
- [Best Practices & Tips](#best-practices)
- [Getting Started](#getting-started)

---

## 🌟 Introduction <a name="introduction"></a>
This is a comprehensive documentation and practical application for handling API layers in **Vue 3**. In modern frontend development, fetching data is easy, but managing **Asynchronous State**, **Caching**, and **Error Handling** at scale is what separates a junior from a senior developer.

### What You'll Learn:
- How to structure a scalable API layer.
- Implementing the **Reusable Composable Pattern**.
- Global error and authentication handling using **Axios Interceptors**.
- Enhancing User Experience (UX) with **Skeleton Screens** and **Error States**.

---

## 🛠 The Problem <a name="the-problem"></a>
Most developers start by writing API calls directly inside components using `fetch` or `axios`. This leads to:
- **Spaghetti Code:** Logic mixed with UI.
- **Redundancy:** Repeating loading/error logic in every single file.
- **Maintenance Hell:** Changing one API header requires editing 50 files.

---

## 🏗 Project Architecture <a name="architecture"></a>
We follow a **Layered Architecture** to decouple the UI from the Data source:

1. **Service Layer:** Pure functions that only know about endpoints.
2. **Logic Layer (Composables):** Functions that manage the *state* of the request (loading, data, error).
3. **UI Layer:** Components that only care about *displaying* the state.

---

## 🚀 Data Fetching Strategies <a name="strategies"></a>

### 1. The Inline Pattern (Not Recommended for Scale) <a name="1-inline-pattern"></a>
Directly calling the API inside `onMounted`.
- **When to use:** Prototypes or extremely small components.
- **Cons:** Zero reusability.

### 2. The Composable Pattern (Professional Standard) <a name="2-composable-pattern"></a>
Encapsulating logic into a reusable `useApi` function.

```javascript
// src/composables/useApi.js
export function useApi(apiCall) {
  const data = ref(null);
  const isLoading = ref(false);
  const error = ref(null);

  const execute = async (...args) => {
    isLoading.value = true;
    error.value = null;
    try {
      const res = await apiCall(...args);
      data.value = res.data;
    } catch (err) {
      error.value = err.message || 'Something went wrong';
    } finally {
      isLoading.value = false;
    }
  };
  return { data, isLoading, error, execute };
}
