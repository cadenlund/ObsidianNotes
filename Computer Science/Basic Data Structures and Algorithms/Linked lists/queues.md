
## queues

* fifo, like a stack. We can push elements called enqueue 
* we can pop with dequeue
* still called push and pop with queue. 



```cpp
#include <iostream>
#include <queue>  // Include the queue header

using namespace std;

int main() {
    // Create an empty queue of integers.
    queue<int> q;

    // Check if the queue is empty.
    if (q.empty()) {
        cout << "The queue is empty initially." << endl;
    }

    // Push some elements into the queue.
    q.push(10);
    q.push(20);
    q.push(30);

    // Display the size of the queue.
    cout << "After pushing 10, 20, 30, the queue size is: " << q.size() << endl;

    // Access and display the front and back elements.
    cout << "Front element: " << q.front() << endl;  // Should be 10.
    cout << "Back element: " << q.back() << endl;    // Should be 30.

    // Pop an element from the front of the queue.
    q.pop();
    cout << "\nAfter one pop:" << endl;
    cout << "New front element: " << q.front() << endl;  // Now should be 20.
    cout << "Queue size: " << q.size() << endl;          // Size should now be 2.

    // Continue popping all elements from the queue.
    cout << "\nPopping all elements:" << endl;
    while (!q.empty()) {
        cout << "Popping element: " << q.front() << endl;
        q.pop();
    }

    // Confirm the queue is empty.
    cout << "\nQueue empty? " << boolalpha << q.empty() << endl;

    return 0;
}

``
(int) 0
```output
```
`
```output
The queue is empty initially.
After pushing 10, 20, 30, the queue size is: 3
Front element: 1Back element: 30

After one pop:
New front element: 20
Queue size: 2

Popping all elements:
Popping element: 20
Popping element: 30

Queue empty? true
0
```
