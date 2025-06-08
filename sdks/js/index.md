---
pagination_next: null
pagination_prev: null
title: Get started
---

# Get started with JavaScript SDK

## Installation

Install the package:

```bash npm2yarn
npm install @cognite/sdk --save
```

## Usage

```js showLineNumbers
const { CogniteClient } = require('@cognite/sdk');
```

### Using ES modules

```js
import { CogniteClient } from '@cognite/sdk';
```

### Using TypeScript

The language of the SDK is native TypeScript, and you don't have to define any extra types.

## Quickstart

### Web

```js
import { CogniteClient } from '@cognite/sdk';

async function quickstart() {
  const client = new CogniteClient({ appId: 'YOUR APPLICATION NAME' });
  client.loginWithOAuth({
    project: 'publicdata',
  });

  const assets = await client.assets.list().autoPagingToArray({ limit: 100 });
}
quickstart();
```

For more details, see the [authentication guide](https://github.com/cognitedata/cognite-sdk-js/blob/master/guides/authentication.md).

### Backend

```js
import { ConfidentialClientApplication } from "@azure/msal-node";
import { CogniteClient } from "@cognite/sdk";

async function quickstart() {
  const pca = new ConfidentialClientApplication({
    auth: {
        clientId: 'YOUR CLIENT ID',
        clientSecret: 'YOUR CLIENT SECRET',
        authority: 'INSERT AUTHORITY HERE'
    }
  });

  async function getToken() {
    var response = await pca.acquireTokenByClientCredential({
        scopes: ['INSERT SCOPE HERE'],
        skipCache: true,
      });
      return response.accessToken;
  }

  const client = new CogniteClient({
    appId: 'Cognite SDK samples',
    baseUrl: 'YOUR BASE URL',
    project: 'YOUR CDF PROJECT NAME',
    getToken: getToken
  });

  const assets = await client.assets.list();
  console.log(assets);
}

quickstart();
```

## Hello, world! - React App

<!--<iframe width="560" height="315" src="https://www.youtube.com/watch?v=29Cuv6OhBmA" frameBorder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowFullScreen></iframe>-->

This guide shows you how to create a simple app using the [Cognite JavaScript SDK](https://github.com/cognitedata/cognitesdk-js). The app authenticates with, and fetches data from, [Cognite Data Fusion (CDF)](https://docs.cognite.com/cdf/).

The simplest way to retrieve data from CDF is to use an SDK. In this guide, we use data from the [Open Industrial Data project](https://www.youtube.com/watch?v=g4DAAo55IS8).

To use this guide you need to have [npm](https://youtu.be/jHDhaSSKmB0) installed on your machine.

1. Install the [create-react-app](https://facebook.github.io/create-react-app/docs/getting-started).

```sh
npx create-react-app hello-world
cd hello-world
```

2. Install the Cognite JavaScript SDK.

```sh
npm install @cognite/sdk --save
```

Now that everything is ready, proceed with coding.

To authenticate the SDK and get access to data, you can either use the OAuth login flow or use API keys. In this example, we will use the OAuth login flow to avoid storing sensitive API keys in the source code. You can find a comprehensive guide to Cognite JS SDK authentication [here](https://github.com/cognitedata/cognite-sdk-js/blob/master/guides/authentication.md).

There are two ways to sign in the user:

- [Redirect to the sign-in page](#authentication-with-redirect)
- [Use a sign-in popup window](#authentication-with-popup)

### Authentication with redirect

Replace the code in `./src/App.js`:

```js
import React, { Component } from 'react';
import { CogniteClient } from '@cognite/sdk';

const project = 'publicdata';

class App extends Component {
  async componentDidMount() {
    const client = new CogniteClient({ appId: 'Cognite SDK tutorial' });
    client.loginWithOAuth({ project });
    client.authenticate();
  }

  render() {
    return <div className="App"></div>;
  }
}

export default App;
```

To test the authentication, it's important to enable SSL or HTTPS. Cognite only lets trusted web pages authenticate.

1. In a terminal window, run:

```sh
HTTPS=true npm run start
```

2. Check the result in a browser. You should be able to sign in to the `publicdata` project.

3. When we've authenticated the SDK, we're ready to fetch data. In this example, we will fetch 10 assets. `client.authenticate()` is redundant here, as the SDK calls it automatically on API request, and we've removed it from the code below. In `./src/App.js`:

```js
import React, { Component } from 'react';
import { CogniteClient } from '@cognite/sdk';

const project = 'publicdata';

class App extends Component {
  state = {
    assets: null,
  };

  async componentDidMount() {
    const client = new CogniteClient({ appId: 'Cognite SDK tutorial' });
    client.loginWithOAuth({ project });
    const assets = await client.assets.list().autoPagingToArray({ limit: 10 });

    this.setState({ assets });
  }

  render() {
    return <div className="App"></div>;
  }
}

export default App;
```

4. We'll do a basic visualization of the assets just fetched and only print out the asset names. In `./src/App.js`:

```js
import React, { Component } from 'react';
import { CogniteClient } from '@cognite/sdk';

const project = 'publicdata';

class App extends Component {
  state = { assets: null };

  async componentDidMount() {
    const client = new CogniteClient({ appId: 'Cognite SDK tutorial' });
    client.loginWithOAuth({ project });
    const assets = await client.assets.list().autoPagingToArray({ limit: 10 });

    this.setState({ assets });
  }

  render() {
    const { assets } = this.state;
    return (
      <div className="App">
        {assets && assets.map((asset) => <p key={asset.id}>{asset.name}</p>)}
      </div>
    );
  }
}

export default App;
```

That's it - you have now successfully fetched data using the SDK.

### Authentication with popup

You can authenticate the SDK by providing a pop-up window. The benefit is that you don't lose the state of the application. The example below changes the code to use a pop-up window. Also, there's some refactoring to make the code more readable; there's a button to call the pop-up window on user interaction and avoid having it blocked by the browser. In `./src/App.js`:

```js
import React, { Component } from 'react';
import {
  CogniteClient,
  POPUP,
  loginPopupHandler,
  isLoginPopupWindow,
} from '@cognite/sdk';

const project = 'publicdata';

class App extends Component {
  state = {
    assets: null,
    client: null,
  };

  async componentDidMount() {
    if (isLoginPopupWindow()) {
      loginPopupHandler();
      return;
    }

    const client = new CogniteClient({ appId: 'Cognite SDK tutorial' });
    client.loginWithOAuth({
      project,
      onAuthenticate: POPUP,
    });
    this.setState({ client });
  }

  fetchAssets = async () => {
    const { client } = this.state;
    const assets = await client.assets.list().autoPagingToArray({ limit: 10 });

    this.setState({ assets });
  };

  renderAssets = () => {
    return (
      <>
        {this.state.assets.map((asset) => (
          <p key={asset.id}>{asset.name}</p>
        ))}
      </>
    );
  };

  render() {
    const { assets } = this.state;
    return (
      <div className="App">
        {assets ? (
          this.renderAssets()
        ) : (
          <button onClick={this.fetchAssets}>
            <h1>Click here to fetch assets from Cognite</h1>
          </button>
        )}
      </div>
    );
  }
}

export default App;
```
