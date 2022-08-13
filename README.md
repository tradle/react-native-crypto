# react-native-crypto

Note: this module is a clone of [crypto-browserify](https://github.com/crypto-browserify/crypto-browserify), with randombytes replaced.

A port of node's `crypto` module to React Native.

## DEPRECATED

you should probably use a combination of:
- react-native-get-random-values to shim getRandomValues
- crypto-browserify

## install

Because this module depends on some node core modules, and react-native doesn't currently have a [resolve.alias a la webpack](https://productpains.com/post/react-native/packager-support-resolvealias-ala-webpack), you will need to use [rn-nodeify](https://github.com/tradle/rn-nodeify) for your shimming needs.

A suggested workflow:

1. Install
  ```sh
  npm i --save react-native-crypto
  # install peer deps
  npm i --save react-native-randombytes
  react-native link react-native-randombytes # on RN >= 0.60, instead do: cd iOS && pod install
  # install latest rn-nodeify
  npm i --save-dev rn-nodeify
  # install node core shims and recursively hack package.json files
  # in ./node_modules to add/update the "browser"/"react-native" field with relevant mappings
  ./node_modules/.bin/rn-nodeify --hack --install
  ```

2. `rn-nodeify` will create a `shim.js` in the project root directory
  ```js
  // index.ios.js or index.android.js
  // make sure you use `import` and not require!  
  import './shim.js'
  import crypto from 'crypto'
  // ...the rest of your code
  ```

## example

see https://github.com/mvayngrib/rncryptoexample

## the crypto in this box

What follows is unedited text from crypto-browserify

The goal of this module is to reimplement node's crypto module so that it can run in react-native supported environments.

Here is the subset that is currently implemented:

* createHash (sha1, sha224, sha256, sha384, sha512, md5, rmd160)
* createHmac (sha1, sha224, sha256, sha384, sha512, md5, rmd160)
* pbkdf2
* pbkdf2Sync
* randomBytes
* pseudoRandomBytes
* createCipher (aes)
* createDecipher (aes)
* createDiffieHellman
* createSign (rsa, ecdsa)
* createVerify (rsa, ecdsa)
* createECDH (secp256k1)
* publicEncrypt/privateDecrypt (rsa)
* randomFillSync
* randomFill

## todo

these features from node's `crypto` are still unimplemented.

* createCredentials
* scryptSync

these features would benefit from native implementations

* pbkdf2
* createSign
* createVerify
* createECDH
* publicEncrypto/privateDecrypt (rsa)

## contributions

If you are interested in writing a feature, please implement as a new module,
which will be incorporated into crypto-browserify as a dependency.

All deps must be compatible with node's crypto
(generate example inputs and outputs with node,
and save base64 strings inside JSON, so that tests can run in the browser.
see [sha.js](https://github.com/dominictarr/sha.js)

Crypto is _extra serious_ so please do not hesitate to review the code,
and post comments if you do.

## License

MIT
