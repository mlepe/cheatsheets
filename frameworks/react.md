# React Cheatsheet

## Quick Start
```jsx
import React from "react";
function App() {
  return <h1>Hello, React!</h1>;
}
```

## Common Commands
```bash
npx create-react-app my-app
cd my-app
npm start
```

## Useful Snippets
```jsx
// Functional Component
const Button = ({ text }) => <button>{text}</button>;

// State in function
const [count, setCount] = React.useState(0);
```