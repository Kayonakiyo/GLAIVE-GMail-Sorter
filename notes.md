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


later :

- add delay tuning for GET requests on messages enpoint, don't trigger 429!
- add option to override saved data in browser (like when u have new emails you want to analyze, maybe a timestamp system?)
- cool pie chart
- code cleanup
- ability to do API deletions with a checkbox system per sender. [or view a few sample messages to exclude important or recent emails but dont mind deleting the rest]