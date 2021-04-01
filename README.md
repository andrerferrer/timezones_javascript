# SPOILERS

From the Documentation, what do we get?
https://openweathermap.org/current#current_JSON

The important data for us are:

![image](https://user-images.githubusercontent.com/45776359/113037648-fbfb2300-916b-11eb-8aab-e4e683e2ac23.png)

![image](https://user-images.githubusercontent.com/45776359/113037681-04535e00-916c-11eb-9365-e460f5d22ca6.png)


## The idea
So, if someone is in Rio, their offset is -3 from UTC (time 0).
If someone is in Berlin (+1) 

## How to transform EPOCH in milisseconds into something that we can work with?

```js
const currentTimeUTC = data.dt;
const timezoneOffset = data.timezone;
const localTime = currentTimeUTC + timezoneOffset; // This is the current time locally in UNIX EPOCH in seconds -> https://en.wikipedia.org/wiki/Unix_time
const localTimeInMilliseconds = localTime * 1000;

// we need to handle the user's offset
const userTime = new Date()
const userOffsetInMinutes = userTime.getTimezoneOffset()
const currentUserOffsetInMilliseconds = userOffsetInMinutes * 60 * 1000;

// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date
const currentTime = new Date( localTimeInMilliseconds + currentUserOffsetInMilliseconds ); 
```

 ## And how to make it a nice message?
 ```js
  const timeOptions = {
    weekday: 'long',
    hour12: true,
    hour: '2-digit',
    minute:'2-digit',
    month: 'long',
    day: 'numeric'
  };
  
  // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString
  const niceMessage = currentTime.toLocaleDateString('en', timeOptions); 
```
