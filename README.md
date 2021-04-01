# SPOILERS

From the Documentation, what do we get?
https://openweathermap.org/current#current_JSON

The important data for us are:

![image](https://user-images.githubusercontent.com/45776359/113037648-fbfb2300-916b-11eb-8aab-e4e683e2ac23.png)

![image](https://user-images.githubusercontent.com/45776359/113037681-04535e00-916c-11eb-9365-e460f5d22ca6.png)


## The concept
So, if someone is in Rio, their offset is -3 from UTC (time 0).

If someone is in Berlin, their offset is +1.

Hence, if the time in Rio is 12h, the time in Berlin will be that plus the difference of the offsets: `12 + 4 = 16`.

The API, nonetheless, will give us that number in EPOCH -> https://en.wikipedia.org/wiki/Unix_time

## How to transform EPOCH in milisseconds into something that we can work with?

```js
// sometimes, "data.dt" is lagging some minutes. You can use "Date.now()" instead
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/now
const currentTimeUTC = data.dt; 

const localOffset = data.timezone; // Rio's localOffset in seconds is -10800 (-3h), for example
const localTime = currentTimeUTC + localOffset; // This is the current time locally in UNIX EPOCH in seconds -> https://en.wikipedia.org/wiki/Unix_time

console.log(localTime)
// If you test this number on https://www.epochconverter.com/, you should receive the desired time.
```
## But how to work with it in Javascript?

```js
// we need to transform the localTime to milliseconds (because JS is weird)
const localTimeInMilliseconds = localTime * 1000;
// and create a new date with that
const currentTime = new Date( localTimeInMilliseconds ); 

```

## There's a catch, though❗

When we run `new Date()`, Javascript will apply our current offset in the computer.

So, there are three offsets to handle:

1. the UTC (+0)
2. the offset (Where we're looking for. e.g. Rio is -3 )
3. the user's computer offset 

We did handle the first two. But not the last one.

How to do it then?
```js
// we need to discount the user's offset
const userCurrentTime = new Date()
const userOffsetInMinutes = userCurrentTime.getTimezoneOffset()
const currentUserOffsetInMilliseconds = userOffsetInMinutes * 60 * 1000;

// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date
const localDateTime = new Date( localTimeInMilliseconds + currentUserOffsetInMilliseconds ); 
```

## Refactoring

We can also represent all of that as: 

```js
const localDateTime = new Date( Date.now() + (data.timezone * 1000) + new Date().getTimezoneOffset() * 60 * 1000 )
```

## Another solution
```js
// get current time for the user
const localDateTime = new Date();

// get location offset and add to the user offset (in hours)
const locationOffsetInHours = data.timezone / 60 / 60
const userOffsetInHours = new Date().getTimezoneOffset() / 60
const totalOffset = userOffsetInHours + locationOffsetInHours

// set "now" to the expected result
localDateTime.setUTCHours( now.getUTCHours() + totalOffset);

console.log(localDateTime)
```


## And how to turn it into a nice message?
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
  const niceMessage = localDateTime.toLocaleDateString('en', timeOptions); 
```

## Going further

* https://ayushgp.github.io/date-and-time-in-javascript/
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString
