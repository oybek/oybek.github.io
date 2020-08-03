---
layout: post
title: "Case classes vs Smart constructors"
tags: [scala]
---

*I just learned this trick from @OlegYch and it seems pretty good. Wondering if anyone knows of any problems with it.*

> Reposted from [tpolecat](https://gist.github.com/tpolecat/a5cb0dc9adeacc93f846835ed21c92d2)

People often try to constrain construction of a case class by making the constructor `private` and providing a partial factory method on the companion. For instance, a natural number class wrapping an `Int`:

```scala
case class Nat private (toInt: Int)
object Nat {
  def fromInt(n: Int): Option[Nat] =
    if (n >= 0) Some(Nat(n)) else None
}
```

We can construct instances with `fromInt` that behave as expected.

```scala
scala> Nat.fromInt(1)
res4: Option[Nat] = Some(Nat(1))

scala> Nat.fromInt(-1)
res5: Option[Nat] = None

scala> new Nat(42)
<console>:14: error: constructor Nat in class Nat cannot be accessed in object $iw
       new Nat(42)
       ^

```

However there are two holes:

```scala
scala> Nat.fromInt(42).get.copy(-67) // copy method is unsafe
res17: Nat = Nat(-67)

scala> Nat(-1) // oops, companion apply
res18: Nat = Nat(-1)
```

The traditional solution to this problem is to use a sealed trait or non-case class and hack up the rest of the machinery that makes it look like a case class, which is boilerplatey and irritating.

---

So, it turns out we can get around this by using the unlikely-sounding `sealed abstract case class`.

```scala
sealed abstract case class Nat(toInt: Int)
object Nat {
  def fromInt(n: Int): Option[Nat] =
    if (n >= 0) Some(new Nat(n) {}) else None
}
```

When we do this the behavior is what we want.

There's no companion `apply`.

```scala
scala> val n = Nat(10)
<console>:15: error: Nat.type does not take parameters
       val n = Nat(10)
                  ^
```

So we use the factory method.

```scala
scala> val on = Nat.fromInt(-1)
on: Option[Nat] = None

scala> val on = Nat.fromInt(10)
on: Option[Nat] = Some(Nat(10))
```

Pattern-matching and `unapply` work.

```scala
scala> val oi = on.collect { case Nat(n) => n }
oi: Option[Int] = Some(10)

scala> val n = on.get
n: Nat = Nat(10)

scala> Nat.unapply(n)
res11: Option[Int] = Some(10)
```

Our `toInt` field is public, and `equals` seems to do the right thing.

```scala
scala> n.toInt
res12: Int = 10

scala> Nat.fromInt(3) == Nat.fromInt(3)
res13: Boolean = true
```

And `copy` doesn't work.

```scala
scala> n.copy(-1)
<console>:18: error: value copy is not a member of Nat
       n.copy(-1)
         ^
```

