---
layout: post
title: "8 Queen problem"
tags: [haskell]
comments: true
---

Post about solving classical 8 queen problem using Haskell.
![]()
![](https://upload.wikimedia.org/wikipedia/commons/1/1f/Eight-queens-animation.gif)

# Solution idea

I represented queen placement as pair of integers `(Int, Int)`. Then generated
all possible N queen placements on the chessboard.

For example `[(1, 1), (2, 2), (3, 3)]` is this placement

```
Q..
.Q.
..Q
```

After this I filtered out `good` placements. I called queen placement `good` if no queen attacks another.

# Coding

Let's define function which tells does two queens conflict (attack each other):

```haskell
doesConflict :: () => (Int, Int) -> (Int, Int) -> Bool
doesConflict (x1, y1) (x2, y2) = (x1 == x2) || (y1 == y2) || (abs(x1-x2) == abs(y1-y2))
```

We will generate queens and need to have a function which detects is placement good:

```haskell
doConflict :: () => [(Int, Int)] -> (Int, Int) -> Bool
doConflict queens candidate = True `elem` (map (doesConflict candidate) queens)

isGood :: () => [(Int, Int)] -> Bool
isGood [] = True
isGood (queen:queens)
  | doConflict queens queen = False
  | otherwise = isGood queens
```

Now we have a function which can tell us is given placement of queens is good or not.

Now let's write function to generate possible queen placements:

```haskell
generatePlacements :: () => Int -> [[(Int, Int)]]
generatePlacements x = (map (zip [1..x]) (permutations [1..x]))
```

In the code above we generate permutations (y coordinates) and zip x coordinates to each
permutations so getting all possible queen placements.

Now all we need to do is to filter out `good` placements:

Run algorithm for `8` queens

```haskell
(filter (isGood) . generatePlacements) 8
```

Result:

```haskell
[
  [(1,6),(2,4),(3,7),(4,1),(5,3),(6,5),(7,2),(8,8)],
  [(1,6),(2,3),(3,5),(4,7),(5,1),(6,4),(7,2),(8,8)],
  ...
]
```

Whole code:

```haskell
import Data.List

doesConflict :: () => (Int, Int) -> (Int, Int) -> Bool
doesConflict (x1, y1) (x2, y2) = (x1 == x2) || (y1 == y2) || (abs(x1-x2) == abs(y1-y2))

doConflict :: () => [(Int, Int)] -> (Int, Int) -> Bool
doConflict queens candidate = True `elem` (map (doesConflict candidate) queens)

isGood :: () => [(Int, Int)] -> Bool
isGood [] = True
isGood (queen:queens)
  | doConflict queens queen = False
  | otherwise = isGood queens

generatePlacements :: () => Int -> [[(Int, Int)]]
generatePlacements x = (map (zip [1..x]) (permutations [1..x]))
```

[Link to source with comments](https://github.com/oybek/haskell-tutorials/blob/master/own-problems/queen.hs)
