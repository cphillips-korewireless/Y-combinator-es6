# Y-combinator-es6 with Luhn validation and byte conversion
An example of a Y-combinator along with the Luhn algorithm and human readable bytes in es6.
For details on es6, please refer to the official guide from AirBnb: 
        https://github.com/airbnb/javascript
```javascript
// Return the recursive pure function
const Y = f => ((g=>(f((...x)=>g(g)(...x)))) (g=>(f((...x)=>g(g)(...x))))),
    unitTags = [' Bytes', ' KB', ' MB', ' GB', ' TB', ' PB', ' EB', ' ZB'];
    
// returns the sum of applying log and dividing the bytes by 1024
const getLogValue = value =>(amount) => amount === 0 ? 0 : Math.floor(Math.log(amount) / Math.log(1024));

// converts bytes to the next human readable unit where the unit is determined by the result of the log.
const getConvertedValue = value => (amount, log, precision=2) => 
                                    parseFloat((amount / Math.pow(1024, log)).toFixed(precision));

// use the value from the logarithmic function above to determine the unit/label.
const getUnitLabel = index => (value) => unitTags[value];

// Return the recursive pure function that can be executed to return a human readable string.
const getHumanReadableString = readable => (bytes, log=Y(getLogValue)(bytes)) => 
                                            Y(getConvertedValue)(bytes, log) + Y(getUnitLabel)(log);

// Credit card and IMEI validation
const calc = e => (e*2 < 10) ? e*2 : e*2-9;
const luhnValidation = digits => (card) => 
                                    card.split("").reduce( (sum, cur, i) => 
                                                            i % 2 == 0 ? 
                                                            calc(cur)  : 
                                                            parseInt(cur) + sum,0) % 10 == 0;
                                                            
// Example usages
console.log(Y(getHumanReadableString)(10500000));
console.log(Y(luhnValidation)("5105105105105100"));
```
