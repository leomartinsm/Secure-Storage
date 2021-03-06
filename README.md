# SecureStorage
A simple wrapper for localStorage/sessionStorage that allows one to encrypt/decrypt the data being stored.

### Install:
```
$ npm install secure-web-storage-without-hash-key
```
# What SecureStorage Looks Like
<!-- * [Docs](docs/javascript-api.md) -->
<!-- * [JSFiddle](https://jsfiddle.net/fypyk2jp/4/) -->

### app.js:

```JavaScript
var CryptoJS = require("crypto-js");

// NOTE: Your Secret Key should be user inputed or obtained through a secure authenticated request.
//       Do NOT ship your Secret Key in your code.
var SECRET_KEY = 'my secret key';

var secureStorage = new SecureStorage(localStorage, {
    hash: function hash(key) {
        key = CryptoJS.SHA256(key, SECRET_KEY);

        return key.toString();
    },
    encrypt: function encrypt(data) {
        data = CryptoJS.AES.encrypt(data, SECRET_KEY);

        data = data.toString();

        return data;
    },
    decrypt: function decrypt(data) {
        data = CryptoJS.AES.decrypt(data, SECRET_KEY);

        data = data.toString(CryptoJS.enc.Utf8);

        return data;
    }
});

var data = {
    secret: 'data'
};

// there is no need to stringify/parse you objects before and after storing.

secureStorage.setItem('data', data);
// stores in localStorage like:
// key => value
// "ad36d572..." => "w1svi6n..."

var decryptedData = secureStorage.getItem('data');
// returns { secret: 'data' }

secureStorage.removeItem('data');
// removes the entry 'data'

secureStorage.key(id)
// returns the hashed version of the key you passed into setItem with the given id.

secureStorage.clear();
// clears all data in the underlining sessionStorage/localStorage.

secureStorage.length;
// the number of entries in the underlining sessionStorage/localStorage.

```
## Security Best Practices 
Handing someone a locked box with the key taped to it is not a good security practice. When using this library it is highly suggested that the key is not delivered with your code.  Secret Keys should be a) user inputed or b) obtained through a secure authenticated request. Do NOT ship your Secret Key in your code.
