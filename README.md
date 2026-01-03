# Vue.js API Mastery: The Complete Guide to Data Fetching

### Table of Contents
1. [Introduction](#introduction)
2. [What You'll Learn](#learning-outcomes)
3. [The Core Problem](#core-problem)
4. [Project Setup & Structure](#setup)
5. [The API Lifecycle](#lifecycle)
6. [Strategy 1: The Inline Component Pattern](#strategy-1)
7. [Strategy 2: The Composable Pattern (Professional)](#strategy-2)
8. [Strategy 3: Advanced State (TanStack Query)](#strategy-3)
9. [Global Configurations (Axios Interceptors)](#global-config)
10. [UI/UX Handling (Loading, Error, Success)](#ui-ux)
11. [Summary & Best Practices](#summary)

---

## Introduction <a name="introduction"></a>
This project is a comprehensive guide to handling API communications in Vue 3. In modern frontend development, fetching data is a fundamental task, but managing it efficiently at scale is the real challenge. This repository serves as a technical reference for building scalable, maintainable, and professional API layers.

## What You'll Learn <a name="learning-outcomes"></a>
* Architectural structuring of an API layer in Vue.js projects.
* Implementation of Reusable Composables to encapsulate fetching logic.
* State management for UI (Loading, Error, Success) on both global and local levels.
* Advanced use of Axios Interceptors for authentication and error propagation.
* Integration of TanStack Query (Vue Query) for industrial-grade caching and synchronization.

---

## The Core Problem <a name="core-problem"></a>
Standard implementations often involve writing API logic directly inside the components. This approach leads to several technical debts:
* **Code Duplication:** Repetitive use of try/catch blocks and loading indicators across multiple components.
* **Maintenance Complexity:** Any change in the endpoint or header configuration requires manual updates in every file where the API is called.
* **Resource Management:** Failing to handle component unmounting often leads to race conditions or memory leaks from unresolved promises.

---

## Project Setup & Structure <a name="setup"></a>
A production-ready Vue project must strictly separate the Data Layer from the Presentation Layer.

```bash
src/
├── api/
│   ├── index.js          # Axios Instance, Interceptors & Base Config
│   └── services/         # Pure functions categorized by entity (e.g., Users, Products)
├── composables/          
│   └── useApi.js         # The Logic Engine: Manages the reactive state of requests
├── components/
│   ├── Skeletons/        # UX: Loading placeholders for better perceived performance
│   └── ErrorStates/      # UX: Standardized feedback components for failure scenarios
└── views/                # Feature implementations and page-level components
