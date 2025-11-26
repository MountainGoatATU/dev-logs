#research #cryptography 
## Definition
Cryptographic Pseudorandom Random Number Generators (also known as CPRNGs) are random number generators which properties that make it suitable for cryptography. Essentially it means that there should be no way to predict the numbers that will be generated, and you can’t reproduce past states of the CPRNG.


These are usually available built-in to programming languages. Often these are backed by the CPRNG used in the OS it’s running on. For example in Python, instead of using `random`, you would use the `secrets` module, or the direct system CPRNG via `os.urandom()`. In C#, you would use `System.Security.Cryptography.RandomNumberGenerator` as opposed to `Random`. In Flutter / Dart, you would use the `cryptography` package instead of `dart:math`. For Kotlin / Java, you would use `SecureRandom` instead of `Random`.


In JS and Node.js, you would use the `crypto` module instead of `math.random()`. Examples below:
## Browser example:
```js
const array = new Uint8Array(16);` `crypto.getRandomValues(array);`
`console.log(array);`
```
## Node.js example:
```js
const crypto = require('crypto');` `const buffer = crypto.randomBytes(16);
console.log(buffer.toString('hex'));
```
