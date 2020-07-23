# SPOILERS

How to transform UTC shift in seconds into a nice message?

```js
const today = new Date();
const localOffset = timezone + today.getTimezoneOffset() * 60;
const localDate = new Date(today.setUTCSeconds(localOffset));
```
