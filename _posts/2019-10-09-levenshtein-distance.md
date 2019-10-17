---
layout: post
title:  "Levenshtein distance"
tags: [scala, algorithms]
---

Levenshtein distance algorithm, scala implementation.

```scala
object Levenshtein extends Memoizable {
  def calc(s1: String, s2: String): Int =
    levenshtein(s1.toList, s2.toList)

  lazy val levenshtein: ((List[Char], List[Char])) => Int
    = memoized {
      case (Nil, Nil) => 0
      case (x, Nil) => x.length
      case (Nil, y) => y.length
      case (x::xs, y::ys) =>
        Seq(levenshtein(x::xs, ys) + 1,
          levenshtein(xs, y::ys) + 1,
          levenshtein(xs, ys) + (if (x == y) 0 else 1)
        ).min
    }
}
```
