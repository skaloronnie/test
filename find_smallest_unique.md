# Find first unique

https://jbt.github.io/markdown-editor/#pZHNDoIwEITv+xSTcNHEC38h8ebFxIsXXwCkRZrAFksbX9+C+O+FeGi22enMfm0DbBULVMr0Fo7V2Umwa4/SEO24c3aNPEKCCJlfIVK/z3y99eKcKAgCHKzsEBLttZBD/en0Z7cvY7yc5FhcjObTkmjTNJPQD8o4JPW6KVjoFtoIaVYQspMsemhGXfQ1KselVZqnAOG6RpWFHTOiOe6RPHqSf97yHW/Q4r/gfO4c/+ORvxDvHzJA0hU=

https://jbt.github.io/markdown-editor/#pVXfa+NGEH7fv2IgL5Yah8p2CIT2jpJeaR96Kb1CH8qB1tbaWiLtqvsjTij93/vNSrLlcy5HW45w45ndmW9mv290QT9oU5FvZdMoHyga/WdUQlxc0C/OrhvVkg8yqFaZIMSc3lPXyGflPHV680CSTGzXyiHyW62GGO1rm8KqogBvfsjeH87hlYFyY9e2eibVeDUcz2mvjU/V76zxulJOBm171wV90GbXqLmxlSJvm8ghIYor/HAhldKmi4E9W+6KPWed3dm2a9STDs+3VN7PDOXU2B2ZrOyLfK99cHodA9Afi6BzG0gZG3c16S0puampB6JM5bmUdki0QbXns6JEcwwFPeMmIsMcPEmnepCyVW+/WINvntbp04/pUhmn/QPZLW1lGqv1OuhHRTP58iVq9a7Gw6DwWtGusevJmewUUrAJSMIxJPkq/ahi1+gNWOIpz3GqSKjz/NW++wzc9UA0j8cPNUWPV0ae1laxsXnOZaX3emcO1+HhAv7V/G1sgsZL8zB4xn0+mnEHsQNTDpNPybLEundPktkhxE9MJBBkQSta0A3+ClrCvsH/C/y7Blu+M36vHA4tB+p8CKqjQoh348OhuSAfMBaJl3DMpBFNT9R5Yq4nzaYEMBsD/Hy+AQ358NjSvlYDWSbT5j6d6pRkslqDhwt7vUnyBZz3TJ3iM70w/m0AARgBJo5439mi7xPx+wTmEOjdNJvUf1BdmNTNvqiuAdTiCOqTkb4IaUXXLwHq3f8X0J00Y0GWgHy0WDsVba3DtLXH+lPd288PkfdUcHETqJYeGpGg8q0QZVmKVna3o8zmb2hjI3boXyDtgn8uYazYKGDcjEYxGsve+DtlOna+wolUHe+B1h1IY1uyDpvykipQgRcFaJbAbKPZMOlOxgCm7ZThxQpNMEKeSlYS/DpM3Oxvs/JyYF7Z0jffkimn1+2IKp1EJM/vHxFqmjwf8wrxe60MJhsCawVCLdP86U1qQF+pK65gSHads09XVHKo+LoE6KOoQJgfpx3RLHr+uliqtO9kgN7OJYPoYYVmt2mXsSCA7A8m0PLjUP9y3KxU+KO9mNhLn43XF3w9Me9jec6ubLoJlqeboJf6VPyj2tk3gv9kpZ8q+VflO/4ugmNM0EEJTMORnkcNJ2tKHR53ksZpiX8j2vP6ozSncj7Ktfdd/9fC/RhXQvwsPe+FYZD8bfcvf9wT2unpA7Bl2i+TYbxeHQoU/wA=

# Find smallest unique

## Problem statement

- N players pick a number
- The player who picked the *smallest number* that *nobody else picked* wins

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

Each node
- takes a portion of the input
- sorts it
- and outputs a list of numbers where the duplicates are repeated only twice

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

Hash function (used to dispatch a list of numbers to each node):
- Node 1: `[1 2 3]` (i.e., all the 1s, all the 2s, all the 3s)
- Node 2: `[4 5 7]`

Complexity: `O(n)`

### Step 3

Each node
- sorts the input
- outputs the list of unique numbers

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

Master node
- finds the smallest unique

#### Master node

Input: `3 5`

Output: `3`

Complexity: `O(n * log n)`


  
