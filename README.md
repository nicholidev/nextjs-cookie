# nextjs-cookie

[![npm version](https://badge.fury.io/js/nextjs-cookie.svg)](https://www.npmjs.com/package/nextjs-cookie)
![GitHub code size in bytes](https://img.shields.io/nicholidev/min/nextjs-cookie?style=plastic)

## How to use

### Install
```
npm install nextjs-cookie
```
or 
```
yarn add nextjs-cookie
```


### Set cookie
```js
import { setCookie } from 'nextjs-cookie';

setCookie('key', 'value', options);
```

### Read Cookie
```js
import { getCookie } from 'nextjs-cookie';

const keyCookie = getCookie('key', options);

// If no cookie, [key] will be undefined 
```

### Read all cookies
```js
import { getCookies } from 'nextjs-cookie';

const cookies = getCookies(options);

/*
The [cookies] will be
{
  key: "value",
  key1: "value1"
  ...
}
*/
```

### Check cookie exists
```js
import { hasCookie } from 'nextjs-cookie';

const cookieExists = hasCookie('name', options);

// [cookieExists] is [true] or [false]
```

### Delete Cookie
```js
import { deleteCookie } from 'nextjs-cookie';

deleteCookie(name, options);
```
!IMPORTANT: 

When deleting a cookie and you're not relying on the default attributes, you must pass the exact same path and domain attributes that were used to set the cookie:

```js
import { deleteCookie } from 'nextjs-cookie';

deleteCookie(name, { path: '/path', domain: '.yourdomain.com' });
```

## Client & Server Side Rendering(SSR)

If you pass Next.js context(ctx) in function, then this function will be done on both client and server

If the function should be done only on client or can't get ctx, pass null or {}
as the first argument to the function and when server side rendering, this function `return undefined;`


#### Client Side Example

```js
import { getCookies, setCookie, deleteCookie } from 'nextjs-cookie';

getCookies();
getCookie('key');
setCookie('key', 'value');
deleteCookie('key');
```

#### Server Side Rendering Example

`/page/index.js`

```jsx
import React from 'react';
import { getCookies, getCookie, setCookie, deleteCookie } from 'nextjs-cookie';

const Home = () => {
  return <div>page content</div>;
};

export const getServerSideProps = ({ req, res }) => {
  setCookie('test', 'value', { req, res, maxAge: 60 * 6 * 24 });
  getCookie('test', { req, res });
  getCookies({ req, res });
  deleteCookie('test', { req, res });

  return { props: {} };
};

export default Home;
```

#### API Example

`/page/api/example.js`

```js
import type { NextApiRequest, NextApiResponse } from 'next';
import { getCookies, getCookie, setCookie, deleteCookie } from 'nextjs-cookie';

export default async function handler(req, res) {
  setCookie('server-key', 'value', { req, res, maxAge: 60 * 60 * 24 });
  getCookie('key', { req, res });
  getCookies({ req, res });
  deleteCookie('key', { req, res });

  return res.status(200).json({ message: 'ok' });
}
```

## API

## setCookie(key, value, options);

```js
setCookie('key', 'value', options);

setCookie('key', 'value'); // - client side
setCookie('key', 'value', { req, res }); // - server side
```
