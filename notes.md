// Without maxing out the api via asyncs.
// run 1 : 25278 ms
// run 2 : 18313 ms
// run 3 : 15155 ms.
// run 4 : 14717 ms.
// run 5 : 15885 ms.
// With maxing out the api via asyncs. [multi-fetch + unpack promises + process headers send to Maps]
// run 1 : 3688 ms [OBSCENE SPEEDUP WHOOOO!!!], about half a minute for 1000 emails so the math is working out pretty well! 
// 4.8x speedup compared to avg of non-optimal. could tune delay to maximize fetches without triggering 429.

// seems multiple runs go better... avg is 17,869.6 ms for 100 emails (way too slow) about 178.7ms per email fetch + analysis

// rememeber that the raw emails are unique, while the names are not
// since a company may use its name, like Google or Indeed across multiple
// email senders! the names just give a general idea of what orgs are sending these!

5/28/24 9:00pm fixed a devious bug in which the async awaits would be rejected because json.payload wouldn't exist sometimes? played it safe and modified the unpacking of the promise objects so that it wont have to search for parameters that the json may not have at that time. in essence, i simplified the email fetching even more by not automatically stripping the json payload headers i need in that async function, just modifying my unpacking code to handle that too to avoid having rejected promises!

currently testing batch limits and timings to avoid 429s which 
can throw a wrench in it all, i can def ingore those rejections via 429 and process what i can but idk

Table :
| Email Count/Delay | 150ms | 300ms | 450ms | 600ms |
|-------------------|-------|-------|-------|-------|
| 0                 | ✅     | ✅     | ✅     | ✅     |
| 100               | ✅     | ✅     | ✅     | ✅     |
| 200               | ✅     | ✅     | ✅     | ✅     |
| 300               | ❌     | ✅     | ✅     | ✅     |
| 400               | ❌     | ✅     | ✅     | ✅     |
| 500               | ❌     | ❌     | ✅     | ✅     |
| 600               | ❌     | ❌     | ✅     | ✅     |
| 700               | ❌     | ❌     | ❌     | ✅     |
| 800               | ❌     | ❌     | ❌     | ✅     |
| 900               | ❌     | ❌     | ❌     | ✅     |
| 1000              | ❌     | ❌     | ❌     | ✅     |
| 2000              | -     | -     | -     | -     |
| 3000              | -     | -     | -     | -     |
| 4000              | -     | -     | -     | -     |
| 5000              | -     | -     | -     | -     |
| 10000             | -     | -     | -     | -     |


* also tested edge case of negative email count and typing NaN values.
** naturally, any timings higher than the minimum will also work,
as we are trying to see how much we can comfortably get away with speed wise. not all combinations were tested (e.g. 1k+ emails w/ 150ms delay is too much that would definitely yield some 429 errors, those are assumed to be invalid combinations by that merit).


new idea, for bigger batches, take a 'big break' every 1000 emails or even smaller as the api is based on a 'moving average' which is kinda hard for my mind to wrap around right now, so I'll keep on testing.

later :

- add delay tuning for GET requests on messages enpoint, don't trigger 429!
- add option to override saved data in browser (like when u have new emails you want to analyze, maybe a timestamp system?)
- cool pie chart
- code cleanup
- ability to do API deletions with a checkbox system per sender. [or view a few sample messages to exclude important or recent emails but dont mind deleting the rest]