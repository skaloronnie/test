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


