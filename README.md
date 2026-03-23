# Dynamic Workers Starter

Deploys a Worker that can load and execute Dynamic Workers at runtime — sandboxed, fast, and disposable.

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/dinasaur404/dynamic-workers-starter)

## Get started

```sh
npm install
npm run dev
```

Visit `http://localhost:8787`

## How it works

The host Worker uses `LOADER.load(code)` to spin up a one-off dynamic isolate on each request:

```js
const worker = env.LOADER.load({
  compatibilityDate: "2026-03-17",
  mainModule: "worker.js",
  modules: {
    "worker.js": `
      export default {
        fetch() {
          return new Response("Hello from a dynamic Worker!");
        },
      };
    `,
  },
  globalOutbound: null,
});

return worker.getEntrypoint().fetch(request);
```

- `LOADER.load(code)` — creates a one-off dynamic isolate (no caching, no ID needed)
- `mainModule` / `modules` — the code that runs inside the isolate
- `globalOutbound: null` — blocks all outbound network access from the dynamic Worker

## Deploy

```sh
npm run deploy
```

## Learn more

- [Dynamic Worker Loaders docs](https://developers.cloudflare.com/workers/runtime-apis/bindings/worker-loader/)
- [Cloudflare Workers docs](https://developers.cloudflare.com/workers/)
