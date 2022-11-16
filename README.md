# ML5-svelte

This project implements an _easy_ way to use ml5js as a component in svelte. The examples use ml5 and p5 together. Variables can be bound to svelte variables, so there are lots of possibilities to interact with UI element and machine learning models. For example using input via webcam or microphone, or using text input to generate new text from external machine learning APIs.

Many thanks to the contributors of the P5 and ML5 community for developing these great tools! You can find the projects here:

- [Ml5](https://ml5js.org/)
- [P5](https://p5js.org/)

## How to use the huggingface example?

Create an account on [huggingface](https://huggingface.co/). Then go into your account settings and set up an API key for your account. Duplicate the `.env.example` file and rename it to `.env` . Paste the API key behind the `VITE_HUGGINGFACE_API_TOKEN` variable in the `.env` file.
