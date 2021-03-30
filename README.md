# SPOILERS

From the Documentation, what do we get?
https://openweathermap.org/current#current_JSON

The important ones for us are:

![image](https://user-images.githubusercontent.com/45776359/113037648-fbfb2300-916b-11eb-8aab-e4e683e2ac23.png)

![image](https://user-images.githubusercontent.com/45776359/113037681-04535e00-916c-11eb-9365-e460f5d22ca6.png)


How to transform UTC shift in seconds into a nice message?

```js
const currentTimeUTC = data.dt;
const timezoneOffset = data.timezone;
const currentTimeLocally = currentTimeUTC + timezoneOffset; // This is the current time locally in UNIX EPOCH in seconds -> https://en.wikipedia.org/wiki/Unix_time
const currentTimeLocallyInMilliseconds = currentTimeLocally * 1000;
const currentTime = new Date( currentTimeLocallyInMilliseconds ); // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date
```

