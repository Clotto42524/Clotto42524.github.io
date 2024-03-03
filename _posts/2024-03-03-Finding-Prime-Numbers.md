---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/primes_image.jpeg"
tags: [Python, Primes]
---

This is a function in Python that can quickly find all the Prime numbers below a given value. 

---

First let's start by setting up a variable that will act as the upper limit of numbers we want to search through. We'll start with 20.

```ruby
n = 20
```

The smallest true Prime number is 2, so we'll start by creating a list of numbers between 2 and our upper bound which in this case was 20. 

We use n + 1, as the range logic is not inclusive of the upper limit we set there. Instead of using a list, we're going to use a set due to their special functions.

```ruby
number_range = set(range(2, n+1))
```
Let's also create a place where we can store any primes we discover. A list works here.

```ruby
primes_list = []
```

We have our set of numbers (number_range) to check all integers between 2 and 20. 

```ruby
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

Now, we know that the very first value in our range is going to be a prime, as there is nothing smaller than it so therefore nothing else could possible divide evenly into it. 
We'll use the *min* function to get the smallest value of the set, then the *remove* method to remove it from our set.

```ruby
prime = min(number_range)
print(prime)
>>> 2
number_range.remove(prime)
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

Finally, since we know it's a prime, we use the *append* method to add to our **primes_list**

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

For the prime number we just checked (in this case, 2) we the want to generate all the multiples of that up to our upper range (in our case, 20).

Again we'll use a set rather than a list.

```ruby
multiples = set(range(prime * 2, n + 1, prime))
```

For the starting point, we don't need our number as that has already been added as a prime, so we start our range of multiples at 2 * our number as that is the first multiple (in our case, 2) so the first multiple will be 4. 
If the number we were checking was 3 then the first multiple would be 6 - and so on.

For the stopping point of our range - we specify that we want our range to go up to 20, so again, n + 1.

Since we want multiples of our number, we want to increment in steps *our* number so we can put in **prime** here

Lets have a look at our list of multiples...

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

We then use the set method **difference_update** to remove any values from **number_range** that are multiples of the number we just checked. 
This is because if a number is a multiple of anything other than 1 or itself then it is **not a prime number** and we can remove it from the list to be checked.

Before we apply the **difference_update**, let's look at our two sets.

```ruby
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

**difference_update** works in a way that will update one set to only include the values that are *different* from those in a second set

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```

When we look at our number range now, all values that were also present in the **multiples** set have been removed as we *know* they were not primes

This also means the smallest number in our range *is a prime number* as we know nothing smaller than it divides into it, meaning we can run all that logic again from the top.

Let's run it for any primes below 1000...

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n + 1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = min(number_range)
    number_range.remove(prime)
    primes_list.append(prime)
    multiples = set(range(prime * 2, n + 1, prime))
    number_range.difference_update(multiples)
```

Let's print the primes_list to have a look at what we found!

```ruby
print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

We can also get some interesting stats from our list which we can use to summarize our findings:
- The number of primes that were found
- The largest prime in the list!

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```

The next thing to do would be to put it into a function, which you can see below:

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n + 1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = min(number_range)
        number_range.remove(prime)
        primes_list.append(prime)
        multiples = set(range(prime * 2, n + 1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
```

Now we can just pass the function the upper bound of our search and it will do the rest!

Let's go for something large, say a million...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```

