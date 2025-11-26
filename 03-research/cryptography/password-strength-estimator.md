#research 
https://github.com/dropbox/zxcvbn
## zxcvbn
`zxcvbn` is a password strength estimator inspired by password crackers. Through pattern matching and conservative estimation, it recognizes and weighs 30k common passwords, common names and surnames according to US census data, popular English words from Wikipedia and US television and movies, and other common patterns like dates, repeats (`aaa`), sequences (`abcd`), keyboard patterns (`qwertyuiop`), and l33t speak.


Consider using zxcvbn as an algorithmic alternative to password composition policy — it is more secure, flexible, and usable when sites require a minimal complexity score in place of annoying rules like "passwords must contain three of {lower, upper, numbers, symbols}".


* __More secure__: policies often fail both ways, allowing weak passwords (`P@ssword1`) and disallowing strong passwords.
* __More flexible__: zxcvbn allows many password styles to flourish so long as it detects sufficient complexity — passphrases are rated highly given enough uncommon words, keyboard patterns are ranked based on length and number of turns, and capitalization adds more complexity when it's unpredictaBle.
* __More usable__: zxcvbn is designed to power simple, rule-free interfaces that give instant feedback. In addition to strength estimation, zxcvbn includes minimal, targeted verbal feedback that can help guide users towards less guessable passwords.


For further detail and motivation, please refer to the USENIX Security '16 [paper and presentation](https://www.usenix.org/conference/usenixsecurity16/technical-sessions/presentation/wheeler).
## Installation
zxcvbn detects and supports CommonJS (node, browserify) and AMD (RequireJS). In the absence of those, it adds a single function `zxcvbn()` to the global namespace.
### Bower
Install [`node`](https://nodejs.org/download/) and [`bower`](http://bower.io/) if you haven't already.


Get `zxcvbn`:
``` shell
cd /path/to/project/root
bower install zxcvbn
```
Add this script to your `index.html`:
``` html
<script src="bower_components/zxcvbn/dist/zxcvbn.js">
</script>
```
To make sure it loaded properly, open in a browser and type `zxcvbn('Tr0ub4dour&3')` into the console.


To pull in updates and bug fixes:
``` shell
bower update zxcvbn
```
### Node / npm / MeteorJS
zxcvbn works identically on the server.
``` shell
$ npm install zxcvbn
$ node
> var zxcvbn = require('zxcvbn');
> zxcvbn('Tr0ub4dour&3');
```
### RequireJS
Add [`zxcvbn.js`](https://raw.githubusercontent.com/dropbox/zxcvbn/master/dist/zxcvbn.js) to your project (using bower, npm or direct download) and import as usual:
``` javascript
requirejs(["relpath/to/zxcvbn"], function (zxcvbn) {
    console.log(zxcvbn('Tr0ub4dour&3'));
});
```
### Browserify / Webpack
If you're using `npm` and have `require('zxcvbn')` somewhere in your code, browserify and webpack should just work.
``` shell
$ npm install zxcvbn
$ echo "console.log(require('zxcvbn'))" > mymodule.js
$ browserify mymodule.js > browserify_bundle.js
$ webpack mymodule.js webpack_bundle.js
```
But we recommend against bundling zxcvbn via tools like browserify and webpack, for three reasons:


* Minified and gzipped, zxcvbn is still several hundred kilobytes. (Significantly grows bundle size.)
* Most sites will only need zxcvbn on a few pages (registration, password reset).
* Most sites won't need `zxcvbn()` immediately upon page load; since `zxcvbn()` is typically called in response to user events like filling in a password, there's ample time to fetch `zxcvbn.js` after initial html/css/js loads and renders.


See the [performance](#perf) section below for tips on loading zxcvbn stand-alone.


Tangentially, if you want to build your own standalone, consider tweaking the browserify pipeline used to generate `dist/zxcvbn.js`:
``` shell
$ browserify --debug --standalone zxcvbn \
    -t coffeeify --extension='.coffee' \
    -t uglifyify \
    src/main.coffee | exorcist dist/zxcvbn.js.map >| dist/zxcvbn.js
```
* `--debug` adds an inline source map to the bundle. `exorcist` pulls it out into `dist/zxcvbn.js.map`.
* `--standalone zxcvbn` exports a global `zxcvbn` when CommonJS/AMD isn't detected.
* `-t coffeeify --extension='.coffee'` compiles `.coffee` to `.js` before bundling. This is convenient as it allows `.js` modules to import from `.coffee` modules and vice-versa. Instead of this transform, one could also compile everything to `.js` first (`npm run prepublish`) and point `browserify` to `lib` instead of `src`.
* `-t uglifyify` minifies the bundle through UglifyJS, maintaining proper source mapping.
### Manual installation
Download [zxcvbn.js](https://raw.githubusercontent.com/dropbox/zxcvbn/master/dist/zxcvbn.js).
Add to your .html:
``` html
<script type="text/javascript" src="path/to/zxcvbn.js"></script>
```
## Usage
[try zxcvbn interactively](https://lowe.github.io/tryzxcvbn/) to see these docs in action.
``` javascript
zxcvbn(password, user_inputs=[])
```
`zxcvbn()` takes one required argument, a password, and returns a result object with several properties:
``` coffee
result.guesses            # estimated guesses needed to crack password
result.guesses_log10      # order of magnitude of result.guesses


result.crack_times_seconds # dictionary of back-of-the-envelope crack time
                          # estimations, in seconds, based on a few scenarios:
{
  # online attack on a service that ratelimits password auth attempts.
  online_throttling_100_per_hour


  # online attack on a service that doesn't ratelimit,
  # or where an attacker has outsmarted ratelimiting.
  online_no_throttling_10_per_second


  # offline attack. assumes multiple attackers,
  # proper user-unique salting, and a slow hash function
  # w/ moderate work factor, such as bcrypt, scrypt, PBKDF2.
  offline_slow_hashing_1e4_per_second


  # offline attack with user-unique salting but a fast hash
  # function like SHA-1, SHA-256 or MD5. A wide range of
  # reasonable numbers anywhere from one billion - one trillion
  # guesses per second, depending on number of cores and machines.
  # ballparking at 10B/sec.
  offline_fast_hashing_1e10_per_second
}


result.crack_times_display # same keys as result.crack_times_seconds,
                           # with friendlier display string values:
                           # "less than a second", "3 hours", "centuries", etc.


result.score      # Integer from 0-4 (useful for implementing a strength bar)


  0 # too guessable: risky password. (guesses < 10^3)


  1 # very guessable: protection from throttled online attacks. (guesses < 10^6)


  2 # somewhat guessable: protection from unthrottled online attacks. (guesses < 10^8)


  3 # safely unguessable: moderate protection from offline slow-hash scenario. (guesses < 10^10)


  4 # very unguessable: strong protection from offline slow-hash scenario. (guesses >= 10^10)


result.feedback   # verbal feedback to help choose better passwords. set when score <= 2.


  result.feedback.warning     # explains what's wrong, eg. 'this is a top-10 common password'.
                              # not always set -- sometimes an empty string


  result.feedback.suggestions # a possibly-empty list of suggestions to help choose a less
                              # guessable password. eg. 'Add another word or two'


result.sequence   # the list of patterns that zxcvbn based the
                  # guess calculation on.


result.calc_time  # how long it took zxcvbn to calculate an answer,
                  # in milliseconds.
````
The optional `user_inputs` argument is an array of strings that zxcvbn will treat as an extra dictionary. This can be whatever list of strings you like, but is meant for user inputs from other fields of the form, like name and email. That way a password that includes a user's personal information can be heavily penalized. This list is also good for site-specific vocabulary — Acme Brick Co. might want to include ['acme', 'brick', 'acmebrick', etc].
