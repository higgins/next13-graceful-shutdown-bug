# Next 13 graceful shutdown not honored

This repo was created via:
```
npx create-next-app@latest
```

and [following the 'Manual Graceful Shutdowns' instructions](https://nextjs.org/docs/app/building-your-application/deploying#manual-graceful-shutdowns)

## Background
Next documents a means for gracefully shutting down the server via an
environment variable `NEXT_MANUAL_SIG_HANDLE`.

Running the server via:
```
NEXT_MANUAL_SIG_HANDLE=true next start
```

after having built it with a custom event handler setup in `app/layout.tsx`:

```
if (process.env.NEXT_MANUAL_SIG_HANDLE) {
  // this should be added in your custom _document
  process.on('SIGTERM', () => {
    console.log('Received SIGTERM: ', 'cleaning up')
    process.exit(0)
  })

  process.on('SIGINT', () => {
    console.log('Received SIGINT: ', 'cleaning up')
    process.exit(0)
  })
}
```

We expect to see our custom event handler executed when we send
`SIGINT` to the process.

## How to reproduce this bug:

### Start the next server:
```
git clone https://github.com/higgins/next13-graceful-shutdown-bug.git
cd next13-graceful-shutdown-bug
npm i
npm start
```

### Kill the server:
`Ctrl-C` to the running process

### Observe:
You do not see `Received SIGINT: cleaning up` in the logs
