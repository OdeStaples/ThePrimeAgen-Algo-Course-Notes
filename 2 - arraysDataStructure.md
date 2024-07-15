# What are Arrays?

- They are fixed size, continiguous memory chunks. That means you cannot grow it. There is no "insertAt" or push or pop. Doesn't mean you can't write those though.

### Note: The statement “arrays are fixed in size” is generally true for arrays in many programming languages, such as C or Java, where arrays have a fixed size once they are created. However, in JavaScript, arrays are more flexible and can dynamically grow or shrink in size.

# Array vs ArrayBuffer

- In JavaScript, `Array` and `ArrayBuffer` serve different purposes and have distinct characteristics. Here's a comparison :

### Array

- **Purpose**: General-purpose, high-level data structure for storing and manipulating lists of elements.
- **Flexibility**: Can store elements of any type (numbers, strings, objects, etc.).
- **Dynamic Size**: Can grow or shrink dynamically using methods like `push`, `pop`, `shift`, `unshift`, etc.
- **Methods**: Provides a wide range of built-in methods for array manipulation (e.g., `map`, `filter`, `reduce`, `forEach`).
- **Usage Example**:
  ```js
  let arr = [1, 2, 3, 4];
  arr.push(5);
  console.log(arr); // Output: [1, 2, 3, 4, 5]
  ```

### ArrayBuffer

- **Purpose**: Low-level binary data buffer used for handling raw binary data.
- **Fixed Size**: Once created, its size is fixed and cannot be changed.
- **Typed Arrays**: Often used with typed arrays (e.g., `Uint8Array`, `Float32Array`) to interpret and manipulate the binary data.
- **No Direct Methods**: Does not provide high-level methods for manipulation; instead, you use typed arrays to work with the data.
- **Usage Example**:
  ```js
  let buffer = new ArrayBuffer(8); // Create a buffer of 8 bytes
  let view = new Uint8Array(buffer);
  view[0] = 255;
  console.log(view); // Output: Uint8Array(8) [255, 0, 0, 0, 0, 0, 0, 0]
  ```

### Key Differences

- **Purpose**: Arrays are for general-purpose data storage and manipulation, while ArrayBuffers are for handling raw binary data.
- **Flexibility**: Arrays can store any type of data and change size dynamically, whereas ArrayBuffers have a fixed size and are used with typed arrays for specific data types.
- **Methods**: Arrays come with many built-in methods for manipulation, while ArrayBuffers require typed arrays for data access and manipulation.

In summary, use `Array` for general data storage and manipulation, and use `ArrayBuffer` when you need to work with raw binary data, such as in WebGL, file I/O, or network protocols.

- In this course, 'array' si often referred to ArrayBuffer as opposed to the regular array.

# So, What are Arrays?

- Arrays are a contiguous(unbreaking) memory space that contains a certain amount of bytes.

- How memory is interpreted depends on how you tell the compiler to interpret

- So when the compiler sees a chunk of memory - it treats those 4 bytes as a singular number (32bit number)

- Just because an array is a contigugous memory doesn't mean it'll be treated as such - it'll have meaning when you give it meaning.

- For example, in java when we say, int a[] = int[3] - it means you specify the compiler that you need 3 int types in contiguous/unbreakable space. When we do a[0], you're telling the compiler to go to the memory address of a, then add the offset of 0 multiplied by how big my type is (in this case 4bytes/32 bits), index 1 will be 4 bytes into the array or into the memory space and that's what creates an actual array

  ```js (node)
  const a = new ArrayBuffer(6); // Create an ArrayBuffer of 8 bytes
  console.log(a); // ArrayBuffer { [Uint8Contents]: <00 00 00 00 00 00>, byteLength: 6 } - Node.js uses Uint8Contents format to show the raw binary data in the buffer.

  const a8 = new Uint8Array(a);
  a8[0] = 45;
  a8[2] = 45;
  console.log(a); // ArrayBuffer {[Uint8Contents]: <2d 00 2d 00 00 00 00 00>,
  // byteLength: 8 }

  const a16 = new Uint16Array(a);
  a16[2] = 0x4545;
  console.log(a); // ArrayBuffer {[Uint8Contents]: <2d 00 2d 00 00 00 00 00>,byteLength: 8 }
  ```

This is what's happening in the above code snippet step-by-step:

### Step 1: Creating an ArrayBuffer

```javascript
const a = new ArrayBuffer(8);
```

- This line creates an `ArrayBuffer` named `a` with a size of 8 bytes. An `ArrayBuffer` is a low-level structure used to represent a generic, fixed-length binary data buffer. You can't directly manipulate the contents of an `ArrayBuffer`; instead, you use a view, such as a typed array or a `DataView`, to interact with the data.

### Step 2: Creating a Uint8Array View

```javascript
const a8 = new Uint8Array(a);
a8[1] = 45;
a8[3] = 45;
```

- Here, a `Uint8Array` named `a8` is created over the `ArrayBuffer` `a`. This means `a8` provides a byte-wise view of `a`, allowing you to manipulate the buffer's contents as unsigned 8-bit integers (bytes).
- The lines `a8[1] = 45;` and `a8[3] = 45;` modify the first and third bytes of the buffer to the value `45`.
- The `Uint8Array` allows you to store data worth 1 byte (8bits). The signed value range (-128 to 127) and unsigned value range (0 to 255).

### Step 3: Logging the ArrayBuffer

```javascript
console.log(a);
```

- Logging `a` directly won't show the modifications made through `a8` because `ArrayBuffer` objects don't have a human-readable representation of their content. Instead, it shows the object's identity and size, indicating it's still an 8-byte buffer (chunk of 1byte).

### Step 4: Creating a Uint16Array View

```javascript
const a16 = new Uint16Array(a);
a16[3] = 0x4545;
```

- A `Uint16Array` named `a16` is created over the same `ArrayBuffer` `a`. This provides a view where the buffer's contents are treated as unsigned 16-bit integers (2 bytes at a time).
- The line `a16[3] = 0x4545;` sets the third 16-bit integer in the buffer to the hexadecimal value `0x4545`. Since `Uint16Array` views the buffer in chunks of 2 bytes, this effectively modifies bytes 4 and 5 of the original buffer.
- The `Uint16Array` allows you to store data worth 2 bytes (16bits). The signed value range (-32,768 to 32,767) and unsigned value range (0 to 65,535).

### Step 5: Logging the ArrayBuffer Again

```javascript
console.log(a);
```

- Logging `a` again shows it's still an 8-byte buffer. However, the modifications made through both `a8` and `a16` have altered its contents. To see these changes, you would need to log the typed arrays (`a8` or `a16`) instead.

- Logging `a` after modifications through both `a8` and `a16` still shows it as an 8-byte buffer because `ArrayBuffer` objects themselves do not store or display the actual data they hold. Instead, they serve as a raw binary data buffer that can be accessed and manipulated through various views, such as `Uint8Array`, `Uint16Array`, etc. These views interpret the binary data in the buffer according to their element type and byte order.

- When you log an `ArrayBuffer` directly, JavaScript does not provide a mechanism to inspect the binary data within the buffer in a human-readable form. It only indicates that the object is indeed an `ArrayBuffer` and might show its size, but it does not reflect the changes made to the data through its views. This behavior is consistent across environments and is part of how `ArrayBuffer` and typed arrays are designed to work in JavaScript.

- To observe the changes made to the data within an `ArrayBuffer`, you must access it through one of its views (e.g., `Uint8Array`, `Uint16Array`). Each view presents the buffer's contents according to its own element type, allowing you to see and manipulate the data in a way that's meaningful for that type. For example, after modifying `a` through `a8` and `a16`, you could log `a8` or `a16` to see the effects of those modifications:

  ```javascript
  console.log(a8); // To see changes made by Uint8Array view
  console.log(a16); // To see changes made by Uint16Array view
  ```

- This design allows for efficient manipulation of binary data by providing multiple ways to interpret and access the same underlying buffer, without the overhead of copying data between different representations.

### Summary

This code demonstrates how multiple views (`Uint8Array` and `Uint16Array`) can be used to manipulate the same underlying `ArrayBuffer`. Each view interprets the buffer's binary data differently, allowing for flexible data manipulation. However, logging the `ArrayBuffer` itself only shows its size and identity, not the content changes made through its views.

### Inserting at a particular index means replacing the value at that index. Deleting a particular index is like replacing the value at that index with null

# Big-O for Arrays

- nameOfTheArray + contiguousWithInBits x offset(aka index)

- constant time aka O(1)

- since the operations are performed at indices, we do not need to loop over the array to get to a value, we know the length of the array, we know it's type and the index at which we need to perform operation.

- and since the operation is instantaneous no matter how big the index is, we don't do anything extra, we do a constant amount of thing no matter how big the input is, it doesn't grow with input at all - hence the Big-O is a constant time.
