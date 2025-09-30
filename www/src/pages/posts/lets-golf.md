+++
layout = "../../layouts/MarkdownPostLayout.astro"
title = "Let's Golf!"
date = 2025-08-27
+++

One of my favorite time-killers I've picked up recently is code golf. It's a way to turn programming problems in to puzzles by solving them in as few characters as possible. It also can lead to some pretty ridiculous code -- code that I would never give to someone else for review unless they were also trying to golf.

If you only look at solutions or character counts, code golf can seem intimidating. How can you possible just write a 59 character solution to Fizz Buzz off the dome?! Well the secret is that you don't. Here is a simple and _hopefully_ intuitive approach to golfing code:

- Find a problem and solve it like you would any other problem, without thinking too much about golfing
- Rid your solution of unnecessary whitespace and character repetition
- Find tricks in the language or structural ways to reduce the code
- Repeat until satisfied with you golf game

Another thing I like about Code Golf is that there is no real perfect score. All scores are language dependent, and you can always stop once you feel your solution is sufficiently satisfying. It can be a quite meditative approach to coding, and the results are often code that looks brittle and incomprehensible. For example, here is a solution to Fizz Buzz:

```python
for i in range(100):print(i%3//2*'Fizz'+i%5//4*"Buzz"or-~i)
# 59 chars
```

What, exactly is going on here? First off, let's quickly review Fizz Buzz. Fizz Buzz is a problem that asks you to list every number from 1 to 100 (inclusive). If the number is divisible by 3, you would print "Fizz". If the number is divisible by 5, you would print "Buzz". If the number is divisible by both 3 and 5, you would print "FizzBuzz". Otherwise the program should just print the current number.

Here is what the typical Fizz Buzz solution looks like. I'll include a character count at the bottom of each chunk.

```python
for i in range(1, 101):
 s = ''
 if i % 3 == 0:
  s += "Fizz"
 if i % 5 == 0:
  s += "Buzz"
 print(s or i)
# 106 chars
```

## Getting Out the Drivers

Now that we have a solution we can start golfing it. The very first thing to do when golfing is remove unnecessary whitespace.

```python
for i in range(1,101):
 s=''
 if i%3==0:s+="Fizz"
 if i%5==0:s+="Buzz"
 print(s or i)
# 85 chars
```

Dropping unnecessary whitespace characters already brought our character count down by 20%. Now that `s=''` looks unnecessary since we're only using `s` once per iteration. How can we take that out of the code? We can evaluate it live, but now we've run into a problem, since we still need to evaluate the if statement... Or do we?

One trick you can employ while golfing is taking advantage of boolean logic. A condition in an if statement evaluates to `true` or `false`, which in most languages are equivalent to 1 or 0. With this in mind, we can skip assigning `s` by using the conditional as a multiplier on the string.

```python
for i in range(1,101):
 print((i%3==0)*"Fizz"+(i%5==0)*"Buzz"or i)
# 66 chars
```

We just dropped the character count by another 20%! And, if we remove whitespace again we can turn this into a one-liner.

```python
for i in range(1,101):print((i%3==0)*"Fizz"+(i%5==0)*"Buzz"or i)
# 64 chars
```

## Putt Putt

Once you've gotten to a one-liner you enter what I consider to be the "fiddling around the edges" zone. This is usually where, unless I'm feeling particularly excited about a problem, I will call it good and move onto another problem (or with my day). However, this blog post isn't about moving on, this blog post is about code golf, so let's get our putters out.

The most immediate areas for improvement are the range call, where that `1,` is staring us down; and the extra parentheses around the conditionals. Let's start with those.

Why are those parentheses necessary? We need them in order to evaluate the conditional separately from the boolean multiplication. Without them we would be evaluating whether `i%3` is equal to 0\*"Fizz", which doesn't make sense! To remove those parentheses we'll need take a different approach to applying boolean logic.

Before we go right into it, it's important to understand that Python allows for strings to be manipulated using addition and multiplication. For example:

```python
s = "green"
s + "blue" # "greenblue"
s * 3 # "greengreengreen"
```

Because Python allows you to manipulate strings like this, instead of thinking about this as a conditional we can think about it as a math expression. If we just want `i%3==0` to evaluate to 0 or 1 then we can use the floor division operator `//` to get the same value. Since i%3 can only be one of `[0, 1, 2]`, we can take advantage of that. Let's take a quick look at what I'm talking about.

```python
print([i%3 for i in range(10)])
# [0, 1, 2, 0, 1, 2, 0, 1, 2, 0]
print([i%3//2 for i in range(10)])
# [0, 0, 1, 0, 0, 1, 0, 0, 1, 0]
```

And the same is true for `i%5` if we floor to 4!

```python
print([i%5 for i in range(10)])
# [0, 1, 2, 3, 4, 0, 1, 2, 3, 4]
print([i%5//4 for i in range(10)])
# [0, 0, 0, 0, 1, 0, 0, 0, 0, 1]
```

Let's give it a shot on a subset of the range we want to work on.

```python
for i in range(1, 16):print(i%3//2*"Fizz"+i%5//4*"Buzz"or i)
# 1
# Fizz
# 3
# Buzz
# Fizz
# 6
# 7
# Fizz
# Buzz
# 10
# Fizz
# 12
# 13
# FizzBuzz
# 15
```

This essentially does what we want, but not _exactly_ what we want. Notice how we're getting the expected values one step ahead of where we want them. This is essentially an off by one error. But wait! Remember that `1,` that was staring us down in the `range` call? We can take advantage of that off-by-one error here in combination with another trick, and this is one of my favorites.

One approach we could take to addressing this off-by-one error is to add 1 to `i` and remove `1,` from the `range` call like so:

```python
for i in range(100):print(i%3//2*"Fizz"+i%5//4*"Buzz"or i+1)
# 60 chars
```

This addresses the off-by-one error and gives us a working solution, but it leaves us with a pesky space. How can we remove that? Well, that's where we can pull out the bitwise inversion operator, represented in Python as `~`. This operator, alongside the unary `-` operator, will help us remove the last bit of unnecessary whitespace from this program.

Pop quiz! What does `-~1` evaluate to?

`-~1 == 2`, and to extend this, `-~0 == 1`, `-~2 == 3`, and so on. This is because the `~` operator, when applied to an integer, is equivalent to `(-x) - 1`. So `-~1 == -((-1) - 1) == 2`. On top of this, `-` and `~` can be placed next to a statement like `or` without confusing the interpreter.

This means that we can represent `i` in two different ways within the same loop! This trick allows us to remove that pesky `1,` from our range call. This also means that the off-by-one issue we introduced with our floor division won't be a problem. Let's take a look:

```python
for i in range(100):print(i%3//2*'Fizz'+i%5//4*'Buzz'or-~i)
# 1
# 2
# Fizz
# 4
# Buzz
# Fizz
# 7
# 8
# Fizz
# Buzz
# 11
# Fizz
# 13
# 14
# FizzBuzz
# 16
# ...
# 59 chars
```

And there we have it! We just golfed a full-sized Fizz Buzz solution down to 59 characters. If it wasn't for the "Fizz" and "Buzz" I'm not sure someone could tell you what this code does on a cursory read, and that's what makes gofling such a fun, goofy activity!

I'm certain that there are ways to further golf this problem, but I'll leave you to try and find them. If you're looking for resources, [code.golf](https://code.golf) is a FOSS platform for golfing that also hosts a wiki with tips for a large number of languages. You can find me on the platform as [ethanjantz](https://code.golf/golfers/EthanJantz).

Happy golfing!
