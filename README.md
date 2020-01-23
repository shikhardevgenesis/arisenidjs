# Arisenid JS

| type | version | package |
| ---- | ------- | ------- |
| core | [![npm version](https://badge.fury.io/js/%40arisenidjs%2Fcore.svg)](https://badge.fury.io/js/%40arisenidjs%2Fcore) | @arisenidjs/core |
| blockchain | [![npm version](https://badge.fury.io/js/%40arisenidjs%2Feosjs.svg)](https://badge.fury.io/js/%40arisenidjs%2Feosjs) | @arisenidjs/eosjs |
| blockchain | [![npm version](https://badge.fury.io/js/%40arisenidjs%2Feosjs2.svg)](https://badge.fury.io/js/%40arisenidjs%2Feosjs2) | @arisenidjs/eosjs2 |
| blockchain | [![npm version](https://badge.fury.io/js/%40arisenidjs%2Fweb3.svg)](https://badge.fury.io/js/%40arisenidjs%2Fweb3) | @arisenidjs/web3 |
| blockchain | [![npm version](https://badge.fury.io/js/%40arisenidjs%2Ftron.svg)](https://badge.fury.io/js/%40arisenidjs%2Ftron) | @arisenidjs/tron |
| wallet | [![npm version](https://badge.fury.io/js/%40arisenidjs%2Flynx.svg)](https://badge.fury.io/js/%40arisenidjs%2Flynx) | @arisenidjs/lynx |


---------------



# Want some quick code?
Here's some boilerplates for you to just get starts quickly.

<details><summary>eosjs@16.0.9</summary>
<p>

Installation: `npm i -S @arisenidjs/core @arisenidjs/eosjs eosjs@16.0.9`
```js
import ArisenidJS from '@arisenidjs/core';
import ArisenidEOS from '@arisenidjs/eosjs';
import Arisen from 'eosjs';

ArisenidJS.plugins( new ArisenidEOS() );

const network = ArisenidJS.Network.fromJson({
    blockchain:'arisen',
    chainId:'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
    host:'nodes.get-arisenid.com',
    port:443,
    protocol:'https'
});

ArisenidJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no arisenid');

    const arisen = ArisenidJS.arisen(network, Arisen);

    ArisenidJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = ArisenidJS.account('arisen');
        const options = {authorization:[`${account.name}@${account.authority}`]};
        arisen.transfer(account.name, 'safetransfer', '0.0001 ARISEN', account.name, options).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>

<details><summary>eosjs@20.0.0</summary>
<p>

Installation: `npm i -S @arisenidjs/core @arisenidjs/eosjs2 eosjs@20.0.0`
```js
import ArisenidJS from '@arisenidjs/core';
import ArisenidEOS from '@arisenidjs/eosjs2';
import {JsonRpc, Api} from 'eosjs';

ArisenidJS.plugins( new ArisenidEOS() );

const network = ArisenidJS.Network.fromJson({
    blockchain:'arisen',
    chainId:'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
    host:'nodes.get-arisenid.com',
    port:443,
    protocol:'https'
});
const rpc = new JsonRpc(network.fullhost());

ArisenidJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no arisenid');

    const arisen = ArisenidJS.arisen(network, Api, {rpc});

    ArisenidJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = ArisenidJS.account('arisen');

        arisen.transact({
            actions: [{
                account: 'eosio.token',
                name: 'transfer',
                authorization: [{
                    actor: account.name,
                    permission: account.authority,
                }],
                data: {
                    from: account.name,
                    to: 'safetransfer',
                    quantity: '0.0001 ARISEN',
                    memo: account.name,
                },
            }]
        }, {
            blocksBehind: 3,
            expireSeconds: 30,
        }).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>




<details><summary>tronweb</summary>
<p>

Installation: `npm i -S @arisenidjs/core @arisenidjs/tron tronweb`
```js
import ArisenidJS from '@arisenidjs/core';
import ArisenidTron from '@arisenidjs/tron';
import TronWeb from 'tronweb';

ArisenidJS.plugins( new ArisenidTron() );

const network = ArisenidJS.Network.fromJson({
    blockchain:'tron',
    chainId:'1',
    host:'api.trongrid.io',
    port:443,
    protocol:'https'
});

const httpProvider = new TronWeb.providers.HttpProvider(network.fullhost());
let tron = new TronWeb(httpProvider, httpProvider, network.fullhost());
tron.setDefaultBlock('latest');

ArisenidJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no arisenid');

    tron = ArisenidJS.trx(network, tron);

    ArisenidJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = ArisenidJS.account('trx');
        tron.trx.sendTransaction('TX...', 100).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>




<details><summary>web3</summary>
<p>

Installation: `npm i -S @arisenidjs/core @arisenidjs/web3 web3`
```js
import ArisenidJS from '@arisenidjs/core';
import ArisenidETH from '@arisenidjs/web3';
import Web3 from 'web3';

ArisenidJS.plugins( new ArisenidETH() );

const network = ArisenidJS.Network.fromJson({
    blockchain:'eth',
    chainId:'1',
    host:'YOUR ETHEREUM NODE',
    port:443,
    protocol:'https'
});

ArisenidJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no arisenid');

    const web3 = ArisenidJS.web3(network, Web3);

    ArisenidJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = ArisenidJS.account('trx');
        web3.eth.sendTransaction({
            from: account.address,
            to: '0x...',
            value: '1000000000000000'
        }).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>

----------------


<br/><br/>
# Supported Wallets

## Automatically Supported Wallets
### **Disclaimer: Wallets being supported by this SDK does not mean they are endorsed or vetted.**

These wallets do not require you include any plugins. They run Arisenid Protocols inside of
their wallet and mimic our existing APIs.

*Does your wallet support Arisenid Protocols? [Issue a Pull Request on the README.md and add it here.](https://github.com/GetArisenid/arisenid-js/edit/revamp/README.md)*

| dapp supported blockchains | platform | wallet | libs |
| ---------- | -------- | -------- | -------- |
| EOSIO, Tron, Ethereum | [Arisenid Desktop](https://get-arisenid.com/) | Desktop | eosjs@16.0.9, eosjs@20+, tronweb, web3 | 
| EOSIO, Ethereum | Arisenid Extension | Desktop | eosjs@16.0.9, web3 |
| EOSIO | [TokenPocket](https://www.tokenpocket.pro/) | Mobile | eosjs@16.0.9, eosjs@20+ |
| EOSIO | [MEET.ONE](https://meet.one/) | Mobile | eosjs@16.0.9, eosjs@20+ |
| EOSIO | [imToken](https://token.im/) | Mobile | eosjs@16.0.9 |
| EOSIO | [PocketEOS](https://pocketeos.com/) | Mobile | eosjs@16.0.9 |
| EOSIO | [MoreWallet](https://more.top/) | Mobile | eosjs@16.0.9 |
| EOSIO | [NOVAWallet](http://eosnova.io/) | Mobile | eosjs@16.0.9 |
| EOSIO | [Chaince Wallet](https://chaince.com/) | Mobile | eosjs@16.0.9 |
| EOSIO | [ARISEN LIVE](https://arisen.live/) | Mobile | eosjs@16.0.9 |
| EOSIO | [Starteos](http://starteos.io/) | Mobile | eosjs@16.0.9, eosjs@20+ |
| EOSIO | [CoinUs Wallet](https://coinus.io/) | Mobile | eosjs@16.0.9 |
| EOSIO | [TokenBase Wallet](http://tokenbase.one) | Mobile | eosjs@16.0.9, eosjs@20+ |
| EOSIO | [ET Wallet](http://www.eostoken.im/) | Mobile | eosjs@16.0.9, eosjs@20+ |
| EOSIO | [Wombat Wallet](https://www.getwombat.io/) | Mobile | eosjs@16.0.9, eosjs@20+ |

## Plugin Supported Wallets
These wallets require a plugin to support.
ArisenidJS will mutate standardized blockchain library requests for you into their required formats.

| dapp supported blockchains | wallet | platform | plugin | libs |
| ---------- | -------- | -------| -------| -------|
| EOSIO | [Lynx](https://eoslynx.com/) | Mobile | `arisenidjs-plugin-lynx` | eosjs@20+ |



<br/><br/>
# Installation

To use ArisenidJS you must have _at least_ the core.
From that point forward you can mix-match the plugins you require.

| blockchain library | installation command |
| ---------- | -------- |
| eosjs | `npm i -S @arisenidjs/core @arisenidjs/eosjs eosjs@16.0.9` |
| eosjs2 (@20+) | `npm i -S @arisenidjs/core @arisenidjs/eosjs2 eosjs@20.0.0` |
| tronweb | `npm i -S @arisenidjs/core @arisenidjs/tron tronweb` |
| web3 | `npm i -S @arisenidjs/core @arisenidjs/web3 web3` |

### CDN
<!-- TODO: FIX CDN now that we are using an org scope -->
```
<script src="https://cdn.arisenidcdn.com/file/arisenid-cdn/js/latest/arisenidjs-core.min.js"></script>
<script src="https://cdn.arisenidcdn.com/file/arisenid-cdn/js/latest/arisenidjs-plugin-eosjs.min.js"></script>
<script src="https://cdn.arisenidcdn.com/file/arisenid-cdn/js/latest/arisenidjs-plugin-eosjs2.min.js"></script>
<script src="https://cdn.arisenidcdn.com/file/arisenid-cdn/js/latest/arisenidjs-plugin-web3.min.js"></script>
<script src="https://cdn.arisenidcdn.com/file/arisenid-cdn/js/latest/arisenidjs-plugin-tron.min.js"></script>
<script src="https://cdn.arisenidcdn.com/file/arisenid-cdn/js/latest/arisenidjs-plugin-lynx.min.js"></script>
```

### Building the minified bundles from Git

If you don't want to use Arisenid's CDN, and you can't/don't want to use the NPM packages, then you can also
build the Arisenid-JS bundles from source. 

Webpack will automatically add a version/license header to the top of the bundle files, so that you can identify 
the version of each Arisenid-JS component after you've copied them into a project.

To generate the `.min.js` files from the source code in this repository, simply run the following commands:

```bash
git clone https://github.com/GetArisenid/arisenid-js.git
cd arisenid-js
# Install NPM dependencies
yarn install        # alternative: npm install
# Generate the .min.js minified JS bundles into the folder 'bundles/' using Webpack
yarn run pack       # alternative: npm run pack
```


<br/><br/>
# Instantiation
As early as you can in your project, instantiate both ArisenidJS and your selected plugins.

#### Nodejs
```js
import ArisenidJS from '@arisenidjs/core';
import ArisenidEOS from '@arisenidjs/eosjs'

ArisenidJS.plugins( new ArisenidEOS() );
```

#### Vanilla
```html
<script src="arisenidjs-core.min.js"></script>
<script src="arisenidjs-plugin-eosjs.min.js"></script>

<script>
    ArisenidJS.plugins( new ArisenidEOS() );
</script>
```

#### Multiple Plugins

```js
import ArisenidJS from '@arisenidjs/core';
import ArisenidEOS from '@arisenidjs/eosjs'
import ArisenidTron from '@arisenidjs/tron'
import ArisenidLynx from 'arisenidjs-plugin-lynx'

ArisenidJS.plugins( new ArisenidEOS(), new ArisenidTron(), new ArisenidLynx(Arisen || {Api, JsonRpc}) );
```


<br/><br/>
# Build the network object
Networks tell Arisenid which blockchain nodes you're going to be working with.

```js
const network = ArisenidJS.Network.fromJson({
    blockchain:'arisen',
    chainId:'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
    host:'nodes.get-arisenid.com',
    port:443,
    protocol:'https'
});
```


<br/><br/>
# Connect to an available wallet
Once you are connected you can then call API methods on `ArisenidJS`

```js

ArisenidJS.connect('MyAppName', {network}).then(connected => {
    if(!connected) return false;
    // ArisenidJS.someMethod();
});
```

[You can see full @arisenidjs/core API docs here](https://get-arisenid.com/docs/api-reference)


<br/><br/>
# Getting Blockchain Accounts

Login with the network passed into `ArisenidJS.connect`
```js
ArisenidJS.login().then(...);
```

Login with multiple networks
```js
ArisenidJS.login({accounts:[network1, network2]).then(...);
```

Logout
```js
ArisenidJS.logout().then(...);
```

**After a successful login, the "Identity" will be available at `ArisenidJS.identity`**.
If a user refreshes the page and has already logged in, the `ArisenidJS.identity` property will be auto-filled.


<br/><br/>
# Get blockchain accounts from the identity
Because accounts are nested within the Identity there is an easy method for fetching them.

#### Using the helper
```js
const account = ArisenidJS.account('arisen')
// Result: {name:'...', authority:'active', publicKey:'...', blockchain:'arisen', chainId:'...'}

const account = ArisenidJS.account('eth')
// Result: {address:'...', blockchain:'eth', chainId:'1'}

const account = ArisenidJS.account('trx')
// Result: {address:'...', blockchain:'trx', chainId:'1'}
```

#### From the Identity
```js
const account = ArisenidJS.identity.accounts.find(x => {
    return x.blockchain === 'arisen';
});
```

---------------------


<br/><br/>
# Using Blockchain Wrappers

Blockchain wrappers wrap the actual blockchain libraries (eosjs, tronweb, web3, etc) that you pass in.
That way you don't have to relearn any APIs or be forced to use any specific version.

**You can click on the libraries here below to go directly to their respective githubs**.
<br/>
<br/>

[eosjs@16.0.9 ( @arisenidjs/eosjs )](https://github.com/EOSIO/eosjs/tree/v16.0.9)
```js
import Arisen from 'eosjs';
const arisen = ArisenidJS.arisen(network, Arisen, eosjsOptions);

const result = await arisen.transfer(...);
```

<br/>

[eosjs@20.0.0 ( @arisenidjs/eosjs2 )](https://github.com/EOSIO/eosjs)
```js
import {JsonRpc, Api} from 'eosjs'
const rpc = new JsonRpc(network.fullhost());
const arisen = ArisenidJS.arisen(network, Api, {rpc});

const result = await arisen.transact({...});
```

<br/>

[tronweb](https://github.com/tronprotocol/tron-web)
```js
import TronWeb from 'tronweb';
const httpProvider = new TronWeb.providers.HttpProvider(network.fullhost());
let tron = new TronWeb(httpProvider, httpProvider, network.fullhost());
tron.setDefaultBlock('latest');
tron = ArisenidJS.trx(network, tron);

const result = await tron.trx.sendTransaction(...)
```

<br/>

[web3](https://github.com/ethereum/web3.js/)
```js
import Web3 from 'web3';
const web3 = ArisenidJS.web3(network, Web3);

const result = await web3.eth.sendTransaction(...)
```


-------------


<br/><br/>
# NodeJS and babel/webpack issues.
If you're having trouble packaging or compiling your project you probably need to add a babel transpiler.
- `npm i -D @babel/runtime` <-- run this command and it should compile.


<br/><br/>
# Fetch issues.
If you're having trouble with `fetch` not being defined in your nodejs environment you can add it by doing
- `npm i -S isomorphic-fetch`
- and then `require('isomorphic-fetch');` at the start of your application


<br/><br/>
# What now?
Head over to the [Arisenid Developer Documentation](https://get-arisenid.com/docs/getting-started) to learn about
all the amazing things you can do with Arisenid.

There's also a lot more information about proper setup in the
[Setting up for Web Applications](https://get-arisenid.com/docs/setting-up-for-web-apps)
section which will help you get the most out of ArisenidJS, and make sure
you aren't exposing your users to malicious non-Arisenid plugins.



-------------





