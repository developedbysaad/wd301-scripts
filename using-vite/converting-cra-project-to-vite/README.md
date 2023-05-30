# Text

To use `vite` instead of `create-react-app` in your existing project, you will have to follow the following steps.

## Install Vite

Change into your project directory.

```sh
cd smarter-tasks
```

And install required packages as development dependency.

```sh
npm install vite @vitejs/plugin-react --save-dev
```

## Create `vite.config.js` file

Next, you have to specify the configuration for `vite`. Create a file named `vite.config.js` in the root of your project. ie, where the `package.json` resides.

Add the following content to `vite.config.js`

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default ({ mode }) => {
  return defineConfig({
    plugins: [react()],
    define: {
      "process.env.NODE_ENV": `"${mode}"`,
    },
  });
};
```

## Create `index.html`

Next, you will have to specify the html file that should be used to render the react app. Create an `index.html` with following content in the root of the project.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Smarter Tasks</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/index.tsx"></script>
  </body>
</html>
```

Here, we specify the entry point as `src/index.tsx`. If your react app uses some other file as entry point, specify it here instead.

## Removing existing create-react-app dependencies

You should remove following package from `package.json`.

```sh
react-scripts
```

You should edit the `scripts` section in `package.json` as follows:

```json
"scripts": {
    "start": "vite",
    "build": "tsc && vite build"
  },
```

You should also specify the `type` as `module` in `package.json` so that all `.js` files will be used as ES modules.

```json
"type": "module",
"dependencies": {
  ...
}
```

## Configuring output directory

`Vite` by default outputs the build to `dist` folder relative to the project root. You can customize this by adding `outDir` key under `build` in `vite.config.js` file.

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default ({ mode }) => {
  return defineConfig({
    build: {
      outDir: "build",
    },
    plugins: [react()],
    define: {
      "process.env.NODE_ENV": `"${mode}"`,
    },
  });
};
```

## Configuring Tailwind CSS

Install necessary packages

```sh
npm install -D tailwindcss postcss autoprefixer
```

Add default configuration files:

```sh
npx tailwindcss init -p
```

Edit `tailwind.config.js` to mark the entry point:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

## Accessing environment variables

`Vite` exposes environment variables under `import.meta.env`. You should change any existing `process.env` to `import.meta.env` in your source code.

To prevent accidentally leaking env variables to the client, only variables prefixed with VITE\_ are exposed to your Vite-processed code. e.g. for the following env variables:

```
VITE_SOME_KEY=123
DB_PASSWORD=foobar
```

Only `VITE_SOME_KEY` will be exposed as `import.meta.env.VITE_SOME_KEY` to your client source code, but `DB_PASSWORD` will not.

Reference: [Vite env mode](https://vitejs.dev/guide/env-and-mode.html)

## optionally move development dependencies

You can also move packages like `typescript`, `@types/node` etc to development dependencies.

```json
  "devDependencies": {
    "@vitejs/plugin-react": "^3.1.0",
    "vite": "^4.2.1",
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "@types/jest": "^27.5.2",
    "@types/node": "^16.18.22",
    "@types/react": "^18.0.31",
    "@types/react-dom": "^18.0.11",
    "typescript": "^4.9.5"
  }

```

## Verify everything works

Now, let's run `npm install` to make sure only required packages are included. Next, we can up the server using the command:

```sh
npm start
```

The react project should be up and running in port `5173`. You can verify by visiting `http://localhost:5173` in your browser.
