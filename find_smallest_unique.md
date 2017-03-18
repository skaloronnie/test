# Find first unique

https://jbt.github.io/markdown-editor/#pZHNDoIwEITv+xSTcNHEC38h8ebFxIsXXwCkRZrAFksbX9+C+O+FeGi22enMfm0DbBULVMr0Fo7V2Umwa4/SEO24c3aNPEKCCJlfIVK/z3y99eKcKAgCHKzsEBLttZBD/en0Z7cvY7yc5FhcjObTkmjTNJPQD8o4JPW6KVjoFtoIaVYQspMsemhGXfQ1KselVZqnAOG6RpWFHTOiOe6RPHqSf97yHW/Q4r/gfO4c/+ORvxDvHzJA0hU=

# Find smallest unique

## Problem statement

- N players pick a number
- The player who picked the *smallest* number* that *nobody else picked* wins

## Considerations

### Single-node solution

1. sort the input
1. find the smallest unique

Complexity: `O(n * log n)`

### Distributed solution

- Not enough if each node sends their locally smallest unique
  - what if all numbers are the same?
- Not enough if each node sends all their locally unique numbers
  - risk of false positive (a locally unique number might not be globally unique)
- Not enough to send all unique + all duplicates **to 1 node**
  - what if all numbers are unique?
- Problems with using **modulo** to assign numbers to nodes
  - what if all numbers are multiple of the modulo (end up in the same node)

## Example

Input: `2 4 2 7 2 1 3 4 7 1 2 2 5`

Answer: `3`

### Step 1

#### Node 1

Input: `2 4 2 7 2 1 3`

After sorting: `1 2 2 2 3 4 7`

Output: `1 2 2 3 4 7` (duplicates kept only twice)

Complexity: `O(n * log n)`

#### Node 2

Input: `4 7 1 2 2 5`

After sorting: `1 2 2 4 5 7`

Output: `1 2 2 4 5 7` (duplicates kept only twice)

Complexity: `O(n * log n)`

#### Can sorting be avoided for this step?

Input: `2 4 2 7 2 1 3`

Construct hash table:

```
map: number -> count
{
  2 -> 3
  4 -> 1
  7 -> 1
  1 -> 1
  3 -> 1
}
```

Output: `4 1  7 2 2 3` (random order, depends on hash function)

Complexity
- generate map: `O(n)` 
- iterate map:  `O(m)`, where `m <= n`
- generate output: `O(m)`
- **Overall**: `O(n)`

When better: if `log n > 3` (i.e. when approx. `n > 10`)


### Step 2

Hash function:
- Node 1: `[1 2 3]`
- Node 2: `[4 5 7]`

Complexity: `O(n)`

### Step 3

#### Node 1

Responsible for: `1 2 3`

Input: `1 2 2 2 3 1 2 2`

Output: `3` (only unique numbers)

Complexity: `O(n * log n)`

#### Node 2

Responsible for: `4 5 7`

Input: `4 7 4 5 7`

Ouput: `5` (only unique numbers)

Complexity: `O(n * log n)`

### Step 4

#### Master node

Input: `3 5`

Output: `3`

Complexity: `O(n * log n)`


  
