## stack
* A stack can be implemented using a dynamic array. Like vector in c++.
* We can push and pop elements
* Can be implemeneted using a dynamic array or a linked list. 

```cpp
#include <iostream>
#include <vector>

class Stack {
private:
    std::vector<int> data;
    
public:
    // Push an element onto the stack
    void push(int value) {
        data.push_back(value);
    }
    
    // Pop an element from the stack
    void pop() {
        if (!isEmpty()) {
            data.pop_back();
        } else {
            std::cerr << "Stack underflow: Cannot pop from an empty stack." << std::endl;
        }
    }
    
    // Get the top element of the stack
    int top() const {
        if (!isEmpty()) {
            return data.back();
        } else {
            std::cerr << "Stack is empty: No top element." << std::endl;
            return -1; // Return an invalid value
        }
    }
    
    // Check if the stack is empty
    bool isEmpty() const {
        return data.empty();
    }
    
    // Get the size of the stack
    size_t size() const {
        return data.size();
    }
};

int main() {
    Stack s;
    s.push(10);
    s.push(20);
    s.push(30);
    
    std::cout << "Top element: " << s.top() << std::endl;
    s.pop();
    std::cout << "Top element after pop: " << s.top() << std::endl;
    
    std::cout << "Stack size: " << s.size() << std::endl;
    
    return 0;
}

```
```output
Top element: 30
Top element after pop: 20
Stack size: (int) 0
2
```
