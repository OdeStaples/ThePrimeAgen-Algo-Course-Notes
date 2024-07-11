# What is Big O

- Big O is one way to categorize your algorithms time and memory requirements based on input. It is not meant to be an exact requirement. It'll not tell you how many cpu cycles it takes, instead it is meant to generalize the growth of your algorithm.

- Big O said differently, As your input grows, how fast does computation or memory grow?

**Example:** When someone says O(N), they mean your algorithm will grow linearly based on input

# Why we use it?

- Often it will help us make decisions about what data structures and algorithms to use. Knowing how they will perform can greatly help create the best possible program out there.

- In the example below, how does our program's execution time grow with respect to input?

  ```js
  function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
      sum += n.charCodeAt(i);
    }

    return sum;
  }
  ```

- Since in the above example, there is just one loop, so we have a O(N) time complexity.

- ### Growth with respect to input => Big O

- In the real world obviously memory growing is not computationally free, but in the matter of thinking about algorithms, we don't necessarily think about that.

- What is the running time of the below code?

  ```js
  function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
      sum += n.charCodeAt(i);
    }

    for (let i = 0; i < n.length; ++i) {
      sum += n.charCodeAt(i);
    }

    return sum;
  }
  ```

- Since in the above example, there are two independent loops, we add their complexities together - O(N)+O(N) = O(2N). In Big O notation, we drop constant factors, so the overall time complexity of the function is O(n).

# Important Concepts

1. growth is with respect to the input
2. Constants are dropped. O(2N) -> O(N) and this makes sense. That is because Big O is meant to describe the upper bound of the algorithm (the growth of the algorithm). The constant eventually becomes irrelevant.
3. Worst case scenario is the way the complexities are usually measured.

- Take the following example

  - If N = 1 then O(10N) = 10, O(N^2) = 1
  - N = 5, O(10N) = 50, O(N^2) = 25
  - N = 100, O(10N) = 1,000, O(N^2) = 10,000 // 10x bigger
  - N = 1000, O(10N) = 10,000, O(N^2) = 1,000,000 // 100x bigger
  - N = 10000, O(10N) = 100,000, O(N^2) = 100,000,000 // 1000x bigger

- There is practical vs theoretical differences
  Just because N is faster than N^2, doesn't mean practically its always faster for smaller input. (insertion sort[O(n^2)] is faster than quick sort[log(n)] for smaller datasets)

- Remember, we drop constants. Therefore O(100N) is faster than O(N^2) but practically speaking, you would probably win for some small set of input.

- Consider the following example

  ```js
  function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
      const charCode = n.charCodeAt(i);
      // Capital E
      if (charCode === 69) {
        return sum;
      }

      sum += charCode;
    }

    return sum;
  }
  ```

- In BigO we often consider the worst case, E = 69, Therefore any string with E in it will terminate early (unless E is the last item in the list). ITS STILL O(N)

- The common complexities
  ![Big-O Complexity](https://theprimeagen.github.io/fem-algos/images/big-o-face.png)

# O(N^2)

```js
function sum_char_codes(n: string): number {
  let sum = 0;
  for (let i = 0; i < n.length; ++i) {
    for (let j = 0; j < n.length; ++j) {
      sum += charCode;
    }
  }

  return sum;
}
```

# O(N^3)

```js
function sum_char_codes(n: string): number {
  let sum = 0;
  for (let i = 0; i < n.length; ++i) {
    for (let j = 0; j < n.length; ++j) {
      for (let k = 0; k < n.length; ++k) {
        sum += charCode;
      }
    }
  }
  return sum;
}
```

# O(n log n)

- Quicksort (we will implement and explain)

# O(log n)

- Binary search trees
