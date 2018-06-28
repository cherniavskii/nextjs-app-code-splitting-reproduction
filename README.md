nextjs-app-code-splitting-reproduction

This repo contains reproduction example of code-splitting issue, when using `_app.js` page for common stuff.

## Context

This app contains 6 pages:
- `_app.js` - initializes redux store

- `index.js` - connects with redux store
- `home.js` - connects with redux store

- `about.js` - renders single element, doesn't connect with redux
- `contacts.js` - renders single element, doesn't connect with redux
- `offer.js` - renders single element, doesn't connect with redux

## The issue
When checking bundle splitting with `bundle-analyzer`, Redux and Redux-related libraries are duplicated between pages.

### Expected behaviour:
`redux` and `react-redux` are bundled once.

### Actual behaviour:
`redux` and `react-redux` are bundled many times - once in `_app.js` and in every page which is connected with Redux (`index.js` and `home.js`)

This issue reproduces only when most of pages aren't connected with Redux (that's the way how code splitting works in Next.js).

But IMHO `_app.js` bundle should be treated like the "second `main`" bundle - since every page is wrapped in `_app` page.
There's no sense to duplicate libraries bundled in `_app`, since `_app` is always loaded.

## Steps to reproduce:

1. Checkout repo locally
2. Run `npm install`
3. Run `npm run analyze`
