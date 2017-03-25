# Find first unique

https://jbt.github.io/markdown-editor/#pZHNDoIwEITv+xSTcNHEC38h8ebFxIsXXwCkRZrAFksbX9+C+O+FeGi22enMfm0DbBULVMr0Fo7V2Umwa4/SEO24c3aNPEKCCJlfIVK/z3y99eKcKAgCHKzsEBLttZBD/en0Z7cvY7yc5FhcjObTkmjTNJPQD8o4JPW6KVjoFtoIaVYQspMsemhGXfQ1KselVZqnAOG6RpWFHTOiOe6RPHqSf97yHW/Q4r/gfO4c/+ORvxDvHzJA0hU=

https://jbt.github.io/markdown-editor/#pVZtT9tIEP7uXzESXxIfiXASVAn1iiqO0/GhUJWT7kNVyRt7kqywd327ayCq7r/fzKydGAj0XoSQNrPzPvM86yP4VZsSfK2qCn2A1ug/W0ySoyP47Oyywhp8UAFrNCFJJnANTaW26Dw0urgDBaatl+jo5vcNdnfwsLFyjSUEkqY771E5JakKkBq7tOUWsPLYqafwoI2X6BfWeF2iU0HbKDqCW23WFU6MLRG8rVq+SpJsSj9ckFDaNG2A/GZkIIXKrsGMc1ZYcZGs8KxQUSWV5MLWTYWPOmzPnptz6MOxYbTEENCNJQkdOFukMM62642EU86pLSgKXtjWBLBF0TqHpkBoPXmkBm6U30Ctmj6V+WFPOzV25jC0zrxWUD3Oj2kG6OhXDe9/BnOgwK4w+EX74PSyDTSsfU+72dNYs5MT4KI91UL13CtdKbqCYME3SEZtE/PobSeAqtiIDeVZoL5nW0gdrlAqT9l2NyzSj+4LZcAjFSfmlhQclCqoJPn4+cqTWq6pgZ+216R9VY4o/050LUt1s+IL38nvrS7hlryNWMOIyTHwmV2OcxKZybKyxR0NoXf0JWZLLmB3FYf/fBkJBzYAGpmOXg0q5go8V6cdLVBBs9m+ABfAhMZDCCBLuulQERssrVQ1nv8wBls+jdNtQOdOwjjt78CuYKUEZNbrQBXCSB02glqvN9yvAEuEdWWXA52xeMTpehrTyGiRMpjBnLZNBDMRzGFBAieOlPEP6Eg8Ex1CZpY/LYzXiIfO1XSp/CQ/yrapdEEo8JCmpJVJjDR9s3vRw3nkF8npwbZVycXYe3SEiarGkjLo9tsT34RNB8U0rW3ZVjaVBVXe67XZeSeJrOmb4eu2CppQxh3nQUZ/MOICCSba7MYrzsZCdJePipGZJFcMB+4VLKir7+hfmkkn7vIMTgmxH/uGzjv43gZsIKONnEyS5LJfEaowqDvBXUPUyFTVpdRjjhnTE9HQkQnFtoHkrF8RH7ByX1fkETYdTISLddigYtawhlYkPOgCuzQEMteyIq9UxZWsiOQkDep9t0n8JxXT/Y1ktLuIYhgNkrjDJgyCj/8BiUtSs31Sz5p7MKUFnB5KKIr/b0IXTHoxIC+pYt6ilq6so5ZrT28vNuevN5F5Kbi2CPF5CMzMZ0mS53lCT8VZj+rJh/j8JN9pfWf8c06HBR8yOrzrD1l/mMfDX+JpX/mCNCQ6gx5GjjbH1mAdMeMxlLQPzEu0a5LMqjUFb96TNtC6rdHE100yjE8RkLx/9ET82jM2MLd9VqJJN2l6QxgnTKbp/on7Y4MG4it9xpDNpf/wQQrQU5xyBAOqaZx9nELOV9lJTknv4TXr9vq3YVkwaj1/31gotW9UIOS9BA/d7mibXF4Sdwr9dez5VejzW5fJcU/pkPn9eTY4z/24N2eu/So7+O2Vx72HYqxhfoAiIgcMWaGnAZb1tTx7VQ5A/Av6hh9I/iygzd09C/u93YNbTsOd4jkIZp7G+Tdofhm/x+wQ53scR9npfw0cG7roOvFJeWaNrqX8nekPfZcN+zY02aU4FwoatOXtPAikyd8=

# Find smallest unique

## Problem statement

- N players pick a number
- The player who picked the *smallest number* that *nobody else picked* wins

## Considerations

### Single-node solution

1. sort the input `O(n * log n)`
1. find the smallest unique `O(n)`

Complexity: `O(n * log n)`

#### Single-node solution (better)

1. iterate through the array and count occurrence using a hash map `O(n)`
3. iterate through the hash map and return the smallest unique `O(m)`, where `m <= n`

Complexity: `O(n)`

### Distributed solution

Problem
- 100 nodes are available to speed up the solution
- each node receives a *reference* to the input
- nodes can send each other data

APIs
- `int MyNodeId()`
- `int NumberOfNodes()`
- `void Send(int nodeId, int data)` non-blocking
- `int Receive()` blocking

#### Considerations

- Not enough if each node sends their locally smallest unique
  - what if all numbers are the same?
- Not enough if each node sends all their locally unique numbers
  - risk of false positive (a locally unique number might not be globally unique)
  - e.g. node 1: `1 2 3`, node 2: `1 3 4`, right answer: `2`, not `1`
- Not enough to send all unique + all duplicates **to 1 node**
  - what if all numbers are unique? that node would be overwhelmed
- Problems with using **modulo** to assign numbers to nodes
  - what if all numbers are multiple of the modulo (end up in the same node)

## Example

Input: `2 4 2 7 2 1 3 4 7 1 2 2 5`

Answer: `3`

### Step 1

---

Each node
- takes a portion of the input
- sorts it
- and outputs a list of numbers where the duplicates are repeated only twice

---

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

---

Hash function (used to dispatch a list of numbers to each node)

E.g.
- Node 1: `[1 2 3]` (i.e., all the 1s, all the 2s, all the 3s)
- Node 2: `[4 5 7]`

Complexity: `O(n)`

---

### Step 3

---

Each node
- sorts the input
- outputs the list of unique numbers

---

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

---

Master node
- finds the smallest unique

---

#### Master node

Input: `3 5`

Output: `3`

Complexity: `O(n * log n)`


  
