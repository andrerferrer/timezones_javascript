# SPOILERS

How to transform UTC shift in seconds into a nice message?

```js
const currentTimeUTC = data.dt;
const timezoneOffset = data.timezone;
const currentTimeLocally = currentTimeUTC + timezoneOffset; // This is the current time locally in UNIX EPOCH in seconds -> https://en.wikipedia.org/wiki/Unix_time
const currentTimeLocallyInMilliseconds = currentTimeLocally * 1000;
const currentTime = new Date( currentTimeLocallyInMilliseconds ); // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date
```
