# nlbdocs

Public files for [nicoleslittlebrother](https://nicoleslittlebrother.tech)'s apps. Nothing here is secret.

## porchlight/

The list of services Porchlight watches, so a status page that moves or a component that's renamed can be
fixed without an app update.

- `catalogue.json`: the list. `serial` rises by one with each publish.
- `catalogue.sig`: an Ed25519 signature over the exact bytes of `catalogue.json`, base64.

Porchlight only uses a list whose signature checks against the public key built into the app:

```
sfSz3peu88d1Y+k0OVTREsTMBxKXrwTA+ob6AsRXha8=
```

That's the raw 32-byte key, base64. To check a copy yourself with Node:

```js
import crypto from 'node:crypto'
import fs from 'node:fs'
const raw = Buffer.from('sfSz3peu88d1Y+k0OVTREsTMBxKXrwTA+ob6AsRXha8=', 'base64')
const key = crypto.createPublicKey({ key: Buffer.concat([Buffer.from('302a300506032b6570032100', 'hex'), raw]), format: 'der', type: 'spki' })
const ok = crypto.verify(null, fs.readFileSync('porchlight/catalogue.json'), key, Buffer.from(fs.readFileSync('porchlight/catalogue.sig', 'utf8').trim(), 'base64'))
console.log(ok ? 'signed by nicoleslittlebrother' : 'NOT signed by nicoleslittlebrother')
```

Reading these files sends nothing beyond a normal web request to GitHub.

## vernacular/

Vernacular's list of makers and apps: which apps have run natively since which version, the makers' driver
pages, and Apple's own words on Rosetta, so its advice stays current without an app update. Every sentence
Vernacular shows, and how it reads your Mac, stays in the app.

- `known.json`: the list. `serial` rises by one with each publish.
- `known.sig`: an Ed25519 signature over the exact bytes of `known.json`, base64.

Vernacular only uses a list whose signature checks against the public key built into the app:

```
wV43kBMH3bEQqF8CiIhJrN12nNoB1smG2vv92pHP8yw=
```

Check it with the Node snippet above, with this key and `vernacular/known.json` and `vernacular/known.sig`.
Reading these files sends nothing about your Mac.
