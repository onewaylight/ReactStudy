### Local Storage

#### There are 5 important methods in localStorages:

```js
// Add a new key-value pair or set a new value for existing key
localStorage.setItem('Key', 'value')
// Get value by key name
localStorage.getItem('Key')
// Get value by key index
localStorage.key(index)
// Remove value by key
localStorage.removeItem('Key')
// Clear all local storage
localStorage.clear()
// Find all keys
Object.keys(localStorage)
```

#### Usage examples

```js
localStorage.setItem('strawberry', 'pancake');
localStorage.getItems('strawberry'); // pancake`

localStorage.chocolate = 'waffle';
localStorage.chocolate; // waffle

localStorage['blueberry'] = 'donut';
localStorage['blueberry']; // donut;

delete localStorage.strawberry;
```

```js
localStorage.desserts = JSON.stringify({choco: "waffle", fruit: "pancake", sweet: "donut"});
const favDessert = JSON.parse(localStorage.desserts)['choco']; 
// waffle
```

### Session

#### There are 4 important methods in sessionStorages:
```js
// Add a new key-value pair or set a new value for existing key
sessionStorage.setItem('Key', 'Value')
// Get value by key name
sessionStorage.getItem('Key')
// Clear all local storage
sessionStorage.clear()
// Find all keys
Object.keys(sessionStorage)
```

#### Usage Examples
```js
sessionStorage.setItem('strawberry', 'pancake');
sessionStorage.getItems('strawberry'); // pancake`

sessionStorage.chocolate = 'waffle';
sessionStorage.chocolate; // waffle

sessionStorage['blueberry'] = 'donut';
sessionStorage['blueberry']; // donut;

delete sessionStorage.strawberry;
```

### Cookies
```js
// Set cookie
document.cookie = 'username=Gyanendra'
// Set cookie with expiry date
document.cookie = 'username=Gyanendra; expires=any upcoming date and time; path=/'
```

### Summary

#### Cookies
- About 4kb limit as they pass back and forth between client and server.
- Has expiration date
- Accessed by both the server and client
- Provides SSL support
- Data is transferred on each HTTP request

#### Local storage
- About 5mb limit
- Has no expiration date
- Accessed by client only
- Provides SSL support
- Data is not transferred on each HTTP request


#### Session storage
- About 10m limit
- Data is cleared when you close the browser tab
- Accessed by client only
- Provides no SSL support
- Data is not transferred on each HTTP request



https://medium.com/@jatin.krr/cookies-vs-local-storage-vs-session-7d278df5fe4f
