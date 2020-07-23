# SPOILERS

```
        const today = new Date();
        const localOffset = timezone + today.getTimezoneOffset() * 60;
        const localDate = new Date(today.setUTCSeconds(localOffset));
```