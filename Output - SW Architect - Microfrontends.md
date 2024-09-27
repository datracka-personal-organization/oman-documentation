To achieve a developer experience (DX) where each microservice (section) of the web app is self-contained but can add the shell and other sections as dependencies, you can follow a slightly different approach. This approach is more modular and flexible from the developer's point of view, focusing on ease of setup and integration. The goal is to allow a developer to work on their individual section of the app without needing to know about the others, but still allow smooth integration when necessary.

### **1. Set Up the Project Structure**

First, create a **monorepo** structure using `npm workspaces` so that each microfrontend can be a package within the workspace.

1. **Initialize the root project:**

```javascript
mkdir microfrontend-app
cd microfrontend-app
npm init -y
```

2. **Enable npm workspaces** in the root `package.json`:

```javascript
{
  "name": "microfrontend-app",
  "private": true,
  "workspaces": [
    "packages/*"
  ]
}

```

3. **Create the packages folder** to hold all microfrontends and the shell:

`mkdir packages`

### **2. Create the Shell**

1. **Create the shell package:**

```javascript
cd packages
mkdir shell
cd shell
npm init -y

```

2. **Install dependencies for the shell:**

```javascript
npm install react react-dom react-router-dom redux react-redux @reduxjs/toolkit typescript vite @types/react @types/react-dom vite-plugin-federation

```

3. **Set up TypeScript configuration:**

Create `tsconfig.json` in the `shell` folder:

```javascript
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "jsx": "react",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "strict": true
  },
  "include": ["src"]
}
```

4. **Create the basic shell structure:**

Inside `shell/src` folder:

- **`index.tsx`** – Entry point of the app
- **`store.ts`** – Redux store
- **`routing.tsx`** – Handles routing

```javascript
mkdir src
touch src/index.tsx src/store.ts src/routing.tsx

```

5. **Configure Redux in `store.ts`:**

```javascript
import { configureStore } from '@reduxjs/toolkit';

// Define initial state and reducer
const initialState = {
  user: null,
  theme: 'light'
};

const rootReducer = (state = initialState, action: any) => {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'TOGGLE_THEME':
      return { ...state, theme: state.theme === 'light' ? 'dark' : 'light' };
    default:
      return state;
  }
};

const store = configureStore({ reducer: rootReducer });

export default store;
```

6. **Set up React Router in `routing.tsx`:**

```javascript
import React from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

const routes: Array<{ path: string; component: React.ComponentType }> = [];

export function registerRoute(routeConfig: { path: string; component: React.ComponentType }) {
  routes.push(routeConfig);
}

export function ShellRoutes() {
  return (
    <BrowserRouter>
      <Routes>
        {routes.map((route, index) => (
          <Route key={index} path={route.path} element={<route.component />} />
        ))}
      </Routes>
    </BrowserRouter>
  );
}

```

7. **Vite configuration for the shell (`vite.config.ts`):**

Install `vite-plugin-federation` to support module federation in Vite:

```javascript
Install `vite-plugin-federation` to support module federation in Vite:
```

Now configure Vite to expose the shell and allow other micro frontends to consume it:

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'shell',
      filename: 'remoteEntry.js',
      exposes: {
        './ShellRoutes': './src/routing.tsx',
        './store': './src/store.ts',
      },
      shared: ['react', 'react-dom', 'redux', 'react-redux'],
    }),
  ],
  build: {
    target: 'esnext',
  },
});

```

8. **Create the Shell entry point in `index.tsx`:**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store';
import { ShellRoutes } from './routing';

function App() {
  return (
    <Provider store={store}>
      <ShellRoutes />
    </Provider>
  );
}

ReactDOM.render(<App />, document.getElementById('root'));

```

### **3. Create a Microfrontend (e.g., Dashboard)**

Now let's create a self-contained microfrontend, such as a `dashboard`, that can use the shell as a dependency.

1. **Create the `dashboard` package:**

```javascript
cd ../
mkdir dashboard
cd dashboard
npm init -y
```

2. **Install dependencies for the `dashboard`:**

```javascript
npm install react react-dom redux react-redux @reduxjs/toolkit typescript vite @types/react @types/react-dom vite-plugin-federation shell
```

3. **Set up TypeScript configuration for `dashboard`:**

Create `tsconfig.json`:

```javascript
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "jsx": "react",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "strict": true
  },
  "include": ["src"]
}
```

4. **Create the basic structure for `dashboard`:**

Inside `dashboard/src` folder:

- **`index.tsx`** – Entry point of the app
- **`DashboardApp.tsx`** – The actual dashboard component

```javascript
mkdir src
touch src/index.tsx src/DashboardApp.tsx
```

5. **Create `DashboardApp.tsx`:**

```javascript

import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

export default function DashboardApp() {
  const user = useSelector((state: any) => state.user);
  const dispatch = useDispatch();

  const updateUser = () => {
    dispatch({ type: 'SET_USER', payload: { name: 'John Doe' } });
  };

  return (
    <div>
      <h1>Dashboard</h1>
      {user ? <p>Welcome, {user.name}</p> : <p>No user logged in</p>}
      <button onClick={updateUser}>Set User</button>
    </div>
  );
}
```

6. **Vite configuration for `dashboard`:**

Configure `vite.config.ts` to use module federation and import the shell:

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'dashboard',
      filename: 'remoteEntry.js',
      remotes: {
        shell: 'shell@http://localhost:3000/remoteEntry.js',
      },
      exposes: {
        './DashboardApp': './src/DashboardApp.tsx',
      },
      shared: ['react', 'react-dom', 'redux', 'react-redux'],
    }),
  ],
  build: {
    target: 'esnext',
  },
});

```

7. **Create `index.tsx` as entry point for `dashboard`:**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from 'shell/store';
import DashboardApp from './DashboardApp';

ReactDOM.render(
  <Provider store={store}>
    <DashboardApp />
  </Provider>,
  document.getElementById('root')
);

```

8. **Register route in the shell from `dashboard`:**

In `dashboard/src/index.tsx`, import the shell routing and register the dashboard route:

```javascript
import { registerRoute } from 'shell/ShellRoutes';
import DashboardApp from './DashboardApp';

registerRoute({
  path: '/dashboard',
  component: DashboardApp,
});

```

### **4. Run and Develop Locally**

1. **Start the shell:**

```javascript
cd packages/shell
npm install
npm run dev
```

2. **Start the dashboard:**

```javascript
cd packages/dashboard
npm install
npm run dev
```

3. **Access the dashboard route in the shell:**

Go to `http://localhost:3000/dashboard`, and you should see the `DashboardApp` rendered, seamlessly integrated with the shell.

### Conclusion

By following this approach, each **microfrontend** is self-contained and can add other sections (like the **shell**) as dependencies. The shell handles global state management, routing, and services, while each section can independently use these shared features. The system is modular, allowing for easy integration and development of new sections.