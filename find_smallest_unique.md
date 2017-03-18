# Find smallest unique

Input: `2 4 2 7 2 1 3 4 7 1 2 2 5`

Answer: `3`

# Step 1

## Node 1

Input: `2 4 2 7 2 1 3`

After sorting: `1 2 2 2 3 4 7`

Output: `1 2 2 3 4 7` (duplicates kept only twice)

Complexity: `O(n * log n)`


![](www.forkosh.com/cgi-bin/mimetex.cgi?formdata=O(n^2))

## Node 2

Input: `4 7 1 2 2 5`

After sorting: `1 2 2 4 5 7`

Output: `1 2 2 4 5 7` (duplicates kept only twice)

# Step 2

Hash function:
- Node 1: `[1 2 3]`
- Node 2: `[4 5 7]`

# Step 3

## Node 1

Responsible for: `1 2 3`

Input: `1 2 2 2 3 1 2 2`

Output: `3` (only unique numbers)

## Node 2

Responsible for: `4 5 7`

Input: `4 7 4 5 7`

Ouput: `5` (only unique numbers)

# Step 4

## Master node

Input: `3 5`

Output: `3`

