#data-structures
#Computer-Science
## Arrays
#### Definition 
* Always contiguous in system memory and are allocated by the OS. 
* The most simple data structure and they look the exact same in the [[RAM]]
* We can configure how many bytes each value in the array is able to store. 
* Can be accessed using indexing
* O(1) time complexity on accessing values
* Very Low overhead
* Arrays are fixed in size

| Address      | Data Stored (Value in Hex) | Description        |
| ------------ | -------------------------- | ------------------ |
| `0x00000000` | `0x12345678`               | Element 1 (Byte 1) |
| `0x00000001` | `0x34`                     | Element 1 (Byte 2) |
| `0x00000002` | `0x56`                     | Element 1 (Byte 3) |
| `0x00000003` | `0x78`                     | Element 1 (Byte 4) |
| `0x00000004` | `0x90ABCD12`               | Element 2 (Byte 1) |
| `0x00000005` | `0xAB`                     | Element 2 (Byte 2) |
| `0x00000006` | `0xCD`                     | Element 2 (Byte 3) |
| `0x00000007` | `0x12`                     | Element 2 (Byte 4) |

```cpp
#include <iostream>
#include <chrono>
#include <iomanip> // For formatted output

int main() {
    const int SIZE = 10; // Size of the array
    int array[SIZE];

    // Initialize the array
    for (int i = 0; i < SIZE; i++) {
        array[i] = i;
    }

    // Warm up the array (ensure all pages are mapped and cache is warmed)
    for (int i = 0; i < SIZE; i++) {
        volatile int temp = array[i];
    }

    // Warm up the timer to avoid first-call overhead
    auto dummyStart = std::chrono::high_resolution_clock::now();
    auto dummyEnd = std::chrono::high_resolution_clock::now();
    volatile auto dummyDuration = std::chrono::duration_cast<std::chrono::nanoseconds>(dummyEnd - dummyStart).count();

    // Measure access times
    std::cout << "Index | Address          | Time (nanoseconds)\n";
    std::cout << "---------------------------------------------\n";

    auto previousTime = std::chrono::high_resolution_clock::now();

    for (int i = 0; i < SIZE; i++) {
        // Access the array
        volatile int value = array[i];

        // Measure the time for this access
        auto currentTime = std::chrono::high_resolution_clock::now();
        auto accessTime = std::chrono::duration_cast<std::chrono::nanoseconds>(currentTime - previousTime).count();

        // Print index, address, and access time
        std::cout << std::setw(2) << i << "    | " << &array[i] << " | " << accessTime << " ns\n";

        // Update the previous time
        previousTime = currentTime;
    }

    return 0;
}


```
```output
Index | Address          | Time (nanoseconds)
---------------------------------------------
0     | 000000131A38E800 | 100 ns
1     | 000000131A38E804 | 25600 ns
2     | 000000131A38E808 | 4500 ns
3     | 000000131A38E80C | 4300 ns
4     | 000000131A38E810 | 4100 ns
5     | 000000131A38E814 | 4000 ns
6     | 000000131A38E818 | 3900 ns
7     | 000000131A38E81C | 4000 ns
8     | 000000131A38E820 | 3900 ns
9     | 000000131A38E824 | 3900 ns
(int) Index | Address          | Time (nanoseconds)
---------------------------------------------
 0    | 0000006201F8E4F0 | 100 ns
 1    | 0000006201F8E4F4 | 24200 ns
 2    | 0000006201F8E4F8 | 4400 ns
 3    | 0000006201F8E4FC | 4000 ns
 4    | 0000006201F8E500 | 4000 ns
 5    | 0000006201F8E504 | 3900 ns
 6    | 0000006201F8E508 | 4000 ns
 7    | 0000006201F8E50C | 4100 ns
 8    | 0000006201F8E510 | 4100 ns
 9    | 0000006201F8E514 | 4000 ns
(int) 0
Index | Address          | Time (nanoseconds)
---------------------------------------------
 0    | 0000000EDF18E2C0 | 0 ns
 1    | 0000000EDF18E2C4 | 25600 ns
 2    | 0000000EDF18E2C8 | 4500 ns
 3    | 0000000EDF18E2CC | 3900 ns
 4    | 0000000EDF18E2D0 | 4000 ns
 5    | 0000000EDF18E2D4 | 3800 ns
 6    | 0000000EDF18E2D8 | 3900 ns
 7    | 0000000EDF18E2DC | 4000 ns
 8    | 0000000EDF18E2E0 | 3800 ns
 9    | 0000000EDF18E2E4 | 3900 ns
(int) 0
Index | Address          | Time (nanoseconds)
---------------------------------------------
 0    | 000000A7E118E4B0 | 100 ns
 1    | 000000A7E118E4B4 | 28700 ns
 2    | 000000A7E118E4B8 | 5700 ns
 3    | 000000A7E118E4BC | 5000 ns
 4    | 000000A7E118E4C0 | 5000 ns
 5    | 000000A7E118E4C4 | 5000 ns
 6    | 000000A7E118E4C8 | 4800 ns
 7    | 000000A7E118E4CC | 4900 ns
 8    | 000000A7E118E4D0 | 4800 ns
 9    | 000000A7E118E4D4 | 4900 ns
(int) 0
Index | Address          | Time (nanoseconds)
---------------------------------------------
 0    | 000000797118E380 | 0 ns
 1    | 000000797118E384 | 26300 ns
 2    | 000000797118E388 | 4600 ns
 3    | 000000797118E38C | 4000 ns
 4    | 000000797118E390 | 4000 ns
 5    | 000000797118E394 | 3900 ns
 6    | 000000797118E398 | 3900 ns
 7    | 000000797118E39C | 3900 ns
 8    | 000000797118E3A0 | 3900 ns
 9    | 000000797118E3A4 | 3900 ns
(int) 0
0
```

##### Reasoning for the time to access array elements
* The first access is 0 due to the compiler probably loading the data of the array index into cache and the values of the timers already being calculated. 
* The second large delay could be a cache miss, the data about the array was not loaded into CPU cache and was not present in the current cache line(somewhere else in main memory). The CPU went to the location of the array and then figured out where the rest of its elements are in memory and then sent them to cache.
* The processor fetches a cache line (usually 64 bytes) into the cache which will contain index 0 and neighboring indices. 
* For sequential access, the prefetcher will analyze this pattern and start to load the next indices into the cache hence the speed up of the iterations due to the prefetcher loading these indices into lower cache levels. 
* The next code will monitor the before and after finding and printing 10 values and addresses in an array
* while the CPU reads the machine code of your executable it adds the objects into the heap and the stack and then comes back to 

| **Memory Component**       | **Description**                          | **Access Time (Approx.)** | **Cause of Delay**                                  |
| -------------------------- | ---------------------------------------- | ------------------------- | --------------------------------------------------- |
| **L1 Cache Hit**           | Fastest level of cache (closest to CPU). | ~1–4 ns                   | Data is already in the L1 cache.                    |
| **L2 Cache Hit**           | Next level of cache (larger, slower).    | ~4–15 ns                  | Data fetched from L2 after L1 cache miss.           |
| **L3 Cache Hit**           | Last level of cache before RAM.          | ~20–50 ns                 | Data fetched from L3 after L2 cache miss.           |
| **RAM Access (No Paging)** | Access to DRAM (main memory).            | ~50–100 ns                | Cache miss at all levels; fetch from RAM.           |
| **Minor Page Fault**       | Page is in RAM but not mapped.           | ~1,000–10,000 ns          | OS updates page table to map virtual memory to RAM. |
| **Major Page Fault**       | Page must be loaded from SSD (swap).     | ~100,000–1,000,000 ns     | OS reads page from SSD (or HDD) into RAM.           |

```cpp
#include <iostream>
#include <chrono>
#include <iomanip> // For formatted output

int main() {
    const int SIZE = 10; // Size of the array
    int array[SIZE];

    // Initialize the array
    for (int i = 0; i < SIZE; i++) {
        array[i] = i;
    }

    // Warm up the array (ensure all pages are mapped and cache is warmed)
    for (int i = 0; i < SIZE; i++) {
        volatile int temp = array[i];
    }

    // Measure the time to access the entire array
    auto start = std::chrono::high_resolution_clock::now();

    volatile int sum = 0; // Volatile to prevent optimization
    for (int i = 0; i < SIZE; i++) {
        sum += array[i]; // Access each element
    }

    auto end = std::chrono::high_resolution_clock::now();

    // Calculate the total and average time
    auto totalTime = std::chrono::duration_cast<std::chrono::nanoseconds>(end - start).count();
    double averageTime = static_cast<double>(totalTime) / SIZE;

    // Output results
    std::cout << "Total Access Time: " << totalTime << " ns\n";
    std::cout << "Average Time per Access: " << averageTime << " ns\n";

    return 0;
}

```
```output
Total Access Time: 100 ns
Average Time per Access: 10 ns
(int) 0
```

##### Why is it so much faster
* Calling the time function over and over makes the iterations very slow.
* The point of checking between iterations is to analyze what the cache is doing but its hard to say whether or not the data is accurate. 
* Cache locality is responsible for loading the array already into cache
#### Static vs. Dynamic arrays
* Static
	* Fixed at compile time
	* Memory is allocated on the stack, usually at the end portion of main memory
	* Faster due to contiguous memory and lower overhead (no- resizing)
	* Called array in C++
	* usually initialized to zero or null
	* You cant delete a value in a static array but you can change the value
* Dynamic 
	* size can grow and shrink 
	* Memory is allocated on the heap, requiring management
	* slower and you have to copy expanded arrays to new locations which can be time consuming. 
	* Called vector in C++
	* Usually in C++ each index address value lies in the heap because the stack does not allow dynamic  allocation. However, the actual metadata containing the information about the size and location of the index elements can be on the stack but it will point to the heap. 
	* This can be beneficial because static arrays on the stack have a better chance of being grabbed on a cache line -> [[Spatial Locality]]
	* Dynamic arrays are de-facto in js and python
	* Don't have to specify the size
	* Even though you would say that the time complexity of insertion or pushing a value to the end of an array is O(n), the [[Amortized Complexity]] is 0(1) because if we double the array size in memory every time we run out of physical size, we will rarely need to call this and re allocate the array into a new one which takes O(n). Rarely need to resize for a push. Amortized Complexity is the average time 
	* In practice the function runs in 0(1) time due to power series. 
	![[Pasted image 20250126203245.png]]
	* As we double, time it takes to make the large array is completely dominated by the last term because its always going to be greater than or equal to the sum of all the previous terms. 
	* we would say it takes O(2n). 2 because each time we have to allocate space and move the value to the new space. But we never care about constants in big O notation so the time it takes is O(n)
	* Its almost as fast as a static array but  obviously there is constants in play. 





```cpp
#include <iostream>
#include <vector>
#include <chrono> // For timing

int main() {
    const int SIZE = 10; // Size of the array and vector

    // Measure initialization time for static array
    auto startStatic = std::chrono::high_resolution_clock::now();
    int staticArray[SIZE];
    for (int i = 0; i < SIZE; i++) {
        staticArray[i] = i; // Initialize static array
    }
    auto endStatic = std::chrono::high_resolution_clock::now();

    // Measure initialization time for dynamic vector
    auto startDynamic = std::chrono::high_resolution_clock::now();
    std::vector<int> dynamicVector(SIZE);
    for (int i = 0; i < SIZE; i++) {
        dynamicVector[i] = i; // Initialize dynamic vector
    }
    auto endDynamic = std::chrono::high_resolution_clock::now();

    // Calculate time taken
    auto staticTime = std::chrono::duration_cast<std::chrono::nanoseconds>(endStatic - startStatic).count();
    auto dynamicTime = std::chrono::duration_cast<std::chrono::nanoseconds>(endDynamic - startDynamic).count();

    // Print results
    std::cout << "Initialization Time:\n";
    std::cout << "Static Array: " << staticTime << " nanoseconds\n";
    std::cout << "Dynamic Vector: " << dynamicTime << " nanoseconds\n";

    // Highlight the difference
    std::cout << "\nDifference: " << dynamicTime - staticTime << " nanoseconds\n";

    return 0;
}



```
```output
Initialization Time:
Static Array: 1652 microseconds
Dynamic Vector: 2849 microseconds

Difference: 1197 microseconds
(int) Initialization Time:
Static Array: 0 microseconds
Dynamic Vector: 1 microseconds

Difference: 1 microseconds
(int) 0
Initialization Time:
Static Array: 100 nanoseconds
Dynamic Vector: 1600 nanoseconds

Difference: 1500 nanoseconds
(int) 0
Initialization Time:
Static Array: 200 nanoseconds
Dynamic Vector: 1700 nanoseconds

Difference: 1500 nanoseconds
(int) 0
Initialization Time:
Static Array: 100 nanoseconds
Dynamic Vector: 1700 nanoseconds

Difference: 1600 nanoseconds
(int) 0
0
```

####  Dynamic array methods
* Accessing elements - O(1) - also known as reading
	* Can be accessed with the formula 
	* Address = Base Address + (index * Element Size)
	* element size is going to be the number of bits per element in the array and if we multiply the index, we get the distance in bits from the base address, like the head of the array, to the index position we want to access.
* Searching - O(n)
	* Linear search is O(n)
	* Binary Search is O(log(n))
* Insertion - O(1) | O(n)
	* At the end it is constant time because the element can be added directly to the extra allocated memory of the array in data
	* In the middle is 0(n) because all the elements need to be shifted to make room for the new element
* Deletion - O(1) | O(n)
	* At the end it is constant time because the element can be removed directly without having to shift the rest of the values
	* In the middle is 0(n) because all the elements need to be shifted to fill the deletion in the array

#### associated leet code problem
* Key takeaways Static arrays
	* Removing elements in an array in a iterating fashion does not require a dynamic array method on ever iteration. 
	* Instead we can loop through and if our condition is not! met (ie; detected a number that we need to remove), we use a second pointer to add it to the array. This second pointer only increments when the condition is not met meaning that the numbers we want to remove are skipped and eventually overridden leaving us with extra space at the end of the array with left over numbers. We return the logical size of the array so that it can be reshaped or interpreted after the fact; 
	* By trying to only use static arrays and avoid storing variables in the heap, we can significantly reduce the time to run. 
	* The CPU prefetcher and Cache line will grab the code and store it in cache making the computation blazingly fast. 

 [Remove Duplicates from an array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

[Remove Element](https://leetcode.com/problems/remove-element/description/ )

* Key takeaways Dynamic Arrays
	* Dynamic arrays run very fast if implemented correctly. 
	* The get Concatenation leet code was very easy.  
	* All it took was two create an answer array, size does not have to be declared and make two for loops pushing back each element of nums onto the answer array. Do that twice and its solved 
[Get Concatenation](https://leetcode.com/problems/concatenation-of-array/)

