---
title: Hash Tables - Data Structures
date: 2023-12-19 +0300
categories: [Software Engineering, Data structures]
tags: [software engineering, data structures, caching] # TAG names should always be lowercase
img_path: /assets/img/posts/HASHTABLES
---

TLDR; A hash table is a [data structure](https://en.wikipedia.org/wiki/Data_structure) that manages a collection of key-value pairs, they are widely used in DB indexing and caching. Hash tables support operations such as insertion, deletion, and lookup(search), with the requirement that keys must be unique. In the implementation of hash tables, an [array](<https://en.wikipedia.org/wiki/Array_(data_structure)>) `A` of size `m` is utilized, where `m` >= `n`. Each value `x` is placed inside a bucket at an array index determined by the hash function `h(x)`, with the constraint that `h(x)` < `m`.

In software engineering or computing, a hash table is a data structure which impelements a dictionary, you might ask what a dictionary is, simply put, a dictionary is another data structure that stores a collection of key-value pairs, some migth refer to it as a `map` or an `associative array` though the term dictionary is commonly accepted.

![hash-tables-illustration](hash-tables-illustration.png){: width="400" height="400" style="background: white; border-radius: 6px;" title="An illustration of hash tables" .normal}

#### Why Hash Tables?

Good question, it's very common to deal with _related_ data when it comes to computing, for that, we use arrays. An array is a data structure used to store and organize data in a way that allows for efficient and random access, it consists of a collection of elements/items, each identified by an index. These elements are stored in contiguous memory locations and can be accessed directly by their index. The index is typically an integer that represents the position of an element in the array. You can think of an array as a bookshelf that has a fixed size, say 100 shelves (array size), each shelf contains something of sometype, it can be another shelf or just one item like a spoon for example.

Now, consider a simple one-dimensional array of names [anna, lucas and ali], to find a name in this array, you can implement a bruteforce approach where you iterate through the array till you find the one you are looking for, this is technically refered to as `linear search`, the array [anna, lucas and ali] is relatively small but for a big array, it could take a considerable amount of time to find a name, now suppose you knew the position of the name in the array, it would take you less time to retrieve said name, regardless of the size of the array or the position of the name, but how do you know which position in the array contains the value you are looking for?, that's the catch!, you do so by calculating the position depending on the value itself, this introduces relatedness between an element and it's position in the array(the index) and that's the gist of hash tables. By using this technique, they reduce the average time complexity from `O(n)/linear time` to `O(1)/constant time` 🎉

#### Components

1. Array of Buckets:
   - Buckets, also known as slots or bins are containers within the array that make up the hash table.
   - Each bucket holds one or more key-value pairs that share the same index, as determined by the hash function.
2. Hash function:

   - This is an integral part of hash tables, it's a `one-way` mathematical function that computes the appropriate bucket index to store the key-pairs. In an ideal world, hash functions should be...

     1. easy to compute.
     2. deterministic, meaning for a given key, it always produces the same output.

        ```bash
         # An example of a hash function that
         # illustrates their deterministic nature
         echo "hello" | sha256sum
         # always produces this hash code ->
         # 5891b5b522d5df086d0ff0b110fbd9d21bb4fc7163af34d08286a2e846f6be03
        ```

     3. able avoid (atleast most of the times) collisions. This happens when a hash function computes the same index `i` when given different keys. Collisions are inevitable in hash tables due to the finite range of space and an infinite number of possible keys. You might ask why we are not using the keys themselves as identifiers for their assciated values instead of passing them through a hash function, This helps in keeping _all_ the ops on the data structure with an average time complexity of O(1), Otherwise you'd have to keep a sorted list of keys, which is much slower to store and retrieve mappings from, or worse, have an unsorted array that results in an average time complexity of O(n).

     There are several strategies to handle collisions and two common approaches are:

     - Separate Chaining:

     In this approach, each bucket in the hash table contains a linked list (or another data structure) that holds all key-value pairs that hash to the same index. When a collision occurs, the new key-value pair is simply added to the existing list at that index.

     - Open Addressing:

     In open addressing, when a collision occurs, the algorithm searches for the next available (unoccupied) slot in the array. This can involve various techniques such as linear probing (checking the next slot), quadratic probing, or double hashing.

#### Using a Hash Table

Hash tables are already implemented and optimized for you in many programming languages, In Typescript/Javascript for example, you can use a hash table by instanciating the built in `Map` class.

```ts
const map1 = new Map();

map1.set("a", 1);
map1.set("b", 2);
map1.set("c", 3);

console.log(map1.get("a"));
// Expected output: 1

map1.set("a", 97);

console.log(map1.get("a"));
// Expected output: 97

console.log(map1.size);
// Expected output: 3

map1.delete("b");

console.log(map1.size);
// Expected output: 2
```

#### Summary

- Hash tables are used to index large amounts of data, whether be it for caching or DB indexing
- The position/address/index of each key is calculated using the key itself by employing a hashing technique which should ideally minimize collisions. Should they occur, they are resolved by employing various techniques such as open addressing or separate chaining.
- Operations done on a hash table have an average time complexity of O(1)/constant time, meaning, regardless of the size of the store, it will take the same amount of time to either insert, search or delete an element/value, this degrades from O(1) to O(n) if there are many collisions.
- The performance of a hash table is directly proportional to the chosen hash function's ability to disperse the indices. However, construction of such a hash function is practically impossible, that being so, implementations depend on case-specific collision resolution techniques in achieving higher performance.

#### Recommended Resources

- [Use Maps more and Objects less](https://www.youtube.com/watch?v=hubQQ3F337A) - Steve from (Builder.io)

I hope you enjoyed reading this.\
Take good care 🤝
