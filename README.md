# What's New in React

This markdown file summarizes the major features and updates in **React 18** and upcoming React versions, along with an example.

---

## 1. React 18 Features

### a) Concurrent Rendering
React can prepare multiple versions of the UI **at the same time** without blocking the main thread.

```jsx
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```
> `createRoot` enables **concurrent features** automatically.

---

### b) Automatic Batching
React now batches multiple state updates **even across async operations**.

```jsx
setCount(count + 1);
setName("New Name"); // Both updates batched automatically
```

---

### c) `startTransition` API
Marks non-urgent updates as **transitions** to keep UI responsive.

```jsx
import { startTransition } from 'react';

startTransition(() => {
  setList(largeData);
});
```

---

### d) Suspense for Data Fetching
Allows components to **suspend rendering** until data is ready.

```jsx
import { Suspense } from 'react';

<Suspense fallback={<Spinner />}>
  <ProductList />
</Suspense>
```

---

### e) Streaming Server-Side Rendering (SSR)
Sends HTML to the browser **in chunks** as it’s ready.

```jsx
import { renderToPipeableStream } from 'react-dom/server';
```

---

### f) New Hooks
- `useId()` → Generates unique IDs for accessibility and forms.
- `useSyncExternalStore()` → For safely subscribing to external stores.
- `useInsertionEffect()` → Optimized for injecting styles.

---

### g) New JSX Transform
No need to `import React from 'react'` in every file.

```jsx
const App = () => <h1>Hello React 18!</h1>;
```

---

### h) Strict Mode Improvements
Double-invokes certain lifecycle methods in development to detect side effects.

---

## 2. React 19 & Beyond (Upcoming / Experimental)
- **Server Components (RSC):** Render components entirely on the server.
- **React Flight:** Incremental rendering and streaming of server components.
- **Better Suspense Support:** For async data fetching and caching.

---

## 3. Performance & Developer Experience Improvements
- Profiling concurrent updates in React DevTools.
- Smaller bundle sizes with automatic tree-shaking.
- Improved error handling with better stack traces.

---

## ✅ Summary of Major Changes in Modern React
1. Concurrent rendering + automatic batching  
2. Transitions (`startTransition`)  
3. Suspense for data fetching and streaming SSR  
4. Server components (experimental)  
5. New hooks: `useId`, `useSyncExternalStore`, `useInsertionEffect`  
6. JSX transform simplifies imports

---

## Example: Streaming Product List with Suspense

```jsx
import React, { Suspense, useState, useEffect } from 'react';

const Product = ({ product }) => <li>{product.name}</li>;

const ProductList = () => {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch('/api/products')
      .then(res => res.json())
      .then(data => setProducts(data));
  }, []);

  return (
    <ul>
      {products.map(p => <Product key={p.id} product={p} />)}
    </ul>
  );
};

const App = () => (
  <Suspense fallback={<div>Loading products...</div>}>
    <ProductList />
  </Suspense>
);

export default App;
```
