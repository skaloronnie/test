https://jbt.github.io/markdown-editor/#1VZRa+NGEH7XrxjIi5SzjeX06aAPPXqFg7smtH07DrSWxtb2pF2xu4rjlv73frOKFNlOQ6BQriTGq92Z+Wa++WblK3rnVPmVA7UqlLU2+yT5pW/YJ8mSijQr6KMO7FTTHBf0e+8DKUO2YwNL6pRjE2r22mO7IkVlY/3ZySoGukOkH6hzdu9USwcdAAX7TmlHdkdsLjx58Ly7K+i3gx1dPaXGBjJcsvfK6eZIMCavWs4WtEUpFGz8XiXJbR+6PkglH3bRbMRHvveq0dUi7pa27XTDjnxt+6aClTaBlvmK4HkLC3fQnhekw6mF+DZs9qGWEuKTNXsGR53jnX7AlgqILh5bJn4IbCquCL4WtccMppS2R1JVJQz8wc6SddRax3EvaGtUQ2Wt0Co0w0uJAodwMcdYnZpgUV08bbtwfNx7qdD1EMMI9UGXfaMcah0K0gYEkg9OEkNc4f4sc7AOkxKymNd4WeFLKVwyOQdGfsn7B9V2j7L8IGdvRZ0p/jMo67YfdpZ5QWlEzWZ2M4M1zrW5sECceZjvYIX8XLjo5VmVJ50cks2S5CfbNPZAfRezbfsmaKRO4dixl/q2w8R54H7+868vAE63FqWL/hteGltB0Lbppe9xriotsUEzIMcD4Fxd0a/PuAgoRGegFGnDV+aOgkAKtozulAAMDzWei7SQCRx8GFOhTem4xRyOgSbL7Myy4ktLaGfE3zIajqoN71XQ9xgix6F3hq7L3smgU4exl6yv4ajCKOvFPAh0t54clzkMufGzUKfKGdpwHen58RnikiS2Wz7F4zpLsXqvypoikaxl5gHjAybCx9uKUjxjKOM6GxmmGn1ulTlSFuXtGe2GZqCR8T6bmo30IKV7bXuPS0v6gKRmneAJf6jL04ZM324x7zgdVhLkMrCj9ZPFDo9Cg4N2hYMrehwdysfK41+xGGSbJD8LZj4MgRARnzdodTSbzsesTpKJDEk2EEYcL2FNpkTgT2dZhDzGKPInnNfEzV8bFwkkn5QX1cyphPfeslyLzvb7engX4NUDj88ptJVnlOYLWmdfBGiKFlzPTxfKCZmbGZnZ68jM/m9kLjf/is3l5ozOncLYzi/gE0ZvIqPpC4ym/xmjN98Yozf/wOgzAsV7fI+rBb/YTpBeQlitVpRu5iiErYgk2fddpfAmnK7jYly8+Z5SMX5DN/ikyzyLXxt05W8=

# Bracket matching

Rules

- `()` Literally, just an opening parenthesis and a closing parenthesis.
- `(P)` A program within a pair of enclosing parentheses.
- `PP` Two programs (not necessarily the same), back to back.

Output

- If the program is valid, the compiler should print -1. 
- Otherwise, it should print the length of the longest prefix that could be extended into a valid program by adding zero or more additional characters to the end. 
- If that prefix is the empty prefix, the compiler should print 0. 
- In particular, if the input string is not a valid program, but can be extended to a valid program, the compiler should print the length of the input string. 

Examples

- Input: `()(()())` Ouput: `-1` (valid)
- Input: `)` Ouput: `0` (invalid)
- Input: `(()))` Ouput: `4` (shortest prefix that can be extended into a valid string)

Follow ups

- multiple types of brackets `([{}])` (both single-node solution and distributed solution)

## Single-node solution

- counter to keep track of open brackets
- when `(` encountered, increment counter
- when `)` encountered, decrement counter
- if counter becomes negative, return *current position*
- at the end, if counter is 0, return -1
- else, return *length of the string*

## Distributed solution

`(())(())`

`(()))(`

Each node either
- starts with (
- or with )
- count how many ), these must be closing brackets of previously opened brackets
- each node returns 2 numbers
- number of closing brackets or 0
- number for the rest

### Example 1

`(())()()()`, valid

Node 1 `(())(`

Node 2 `)()()`

Node 1 returns 
- number of starting `)`: `0`
- scan rest of the input and returns `1`

Node 2 returns 
- number of starting `)`: `1`
- scan rest of the input and returns `0`

Master node returns
- goes through the pairs `[(0, 1) (1, 0)]`
- returns `true` (valid)

### Example 2

`(())()()))`, valid

Node 1 `(())(`

Node 2 `)()))`

Node 1 returns 
- number of starting `)`: `0`
- scan rest of the input and returns `1`

Node 2 returns 
- number of starting `)`: `1`
- scan rest of the input and returns `-2`

Master node returns
- goes through the pairs `[(0, 1) (1, -2)]`
- returns `false` (invalid)

### Example 3

`((()()()))`, valid

Node 1 `((()(`

Node 2 `)()))`

Node 1 returns 
- number of starting `)`: `0`
- scan rest of the input and returns `3`

Node 2 returns 
- number of starting `)`: `1`
- scan rest of the input and returns `-2`

Master node returns
- goes through the pairs `[(0, 3) (1, -2)]`
- returns `true` (valid)

### In general

Master node
- goes through the pairs `[... (2, 3) (1, -2) ...]`
- and updates counter `counter += (-2) + 3 + (-1) + (-2)`
