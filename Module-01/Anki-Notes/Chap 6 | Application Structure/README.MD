# Chap 6 | Application Structure

Anki => 06-application-structure

**Q1**:Describe the application structure in a typical react app ?

**Solution**:

**index.js** ➡️

```jsx
import React from 'react';
import { createRoot } from 'react-dom/client';

import App from './components/App';

const root = createRoot(
  document.querySelector('#root')
);

root.render(<App />);
```

Typically the first bit of code which will be executed.It's responsible for rendering our React application , turning the React elements we write into live DOM nodes

Its common to do "setup" tasks in this file as well (For example , this file is a good place to import CSS files , or set up any error-logging services)

index.js is more of a setup file than an active part of the application so we dont want to render a bunch of JSX here

This file generally only renders a single element: `<App/>`

**App**

This is the "home base" React component in our project,the component that is an ancestor to every other component.Pick a random component in a React app, and you should be able to trace its lineage to App. If we are using a routing solution like React Router ,our top-level routes are often included in this file . App should show developers how the application is structured.Its a home base for developers to check , to get acquaninted with how things work
