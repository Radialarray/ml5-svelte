# ML5 for svelte

This project implements an _easy_ way to use ml5js in svelte. The examples use ml5 and p5 together. Variables can be bound to svelte variables, so there are lots of possibilities to interact with UI element and machine learning models. For example using input via webcam or microphone, or using text input to generate new text from external machine learning APIs.

## How to use the huggingface example?

Create an account on [huggingface](https://huggingface.co/). Then go into your account settings and set up an API key for your account. Duplicate the `.env.example` file and rename it to `.env` . Paste the API key behind the `VITE_HUGGINGFACE_API_TOKEN` variable in the `.env` file.

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
