
## Doubly linked lists
* Has two pointers prev and next
* Now we can traverse forwards and backwards;



```cpp
#include <iostream>

class Node {
public:
    int data;
    Node* prev;
    Node* next;

    Node(int value) {
        data = value;
        prev = nullptr;
        next = nullptr;
    }
};

class DoublyLinkedList {
private:
    Node* head;
    Node* tail;

public:
    DoublyLinkedList() {
        head = nullptr;
        tail = nullptr;
    }

    // Insert at the beginning
    void insertAtBeginning(int value) {
        Node* newNode = new Node(value);
        if (head == nullptr) {
            head = tail = newNode;
        } else {
            newNode->next = head;
            head->prev = newNode;
            head = newNode;
        }
    }

    // Insert at the end
    void insertAtEnd(int value) {
        Node* newNode = new Node(value);
        if (tail == nullptr) {
            head = tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
    }

    // Delete a node with a specific value
    void deleteNode(int value) {
        Node* temp = head;
        while (temp != nullptr && temp->data != value) {
            temp = temp->next;
        }

        if (temp == nullptr) return;  // Not found

        if (temp == head) {
            head = head->next;
            if (head) head->prev = nullptr;
        } else if (temp == tail) {
            tail = tail->prev;
            if (tail) tail->next = nullptr;
        } else {
            temp->prev->next = temp->next;
            temp->next->prev = temp->prev;
        }

        delete temp;
    }

    // Display the list forward
    void displayForward() {
        Node* temp = head;
        while (temp != nullptr) {
            std::cout << temp->data << " ";
            temp = temp->next;
        }
        std::cout << std::endl;
    }

    // Display the list backward
    void displayBackward() {
        Node* temp = tail;
        while (temp != nullptr) {
            std::cout << temp->data << " ";
            temp = temp->prev;
        }
        std::cout << std::endl;
    }

    ~DoublyLinkedList() {
        Node* temp = head;
        while (temp != nullptr) {
            Node* next = temp->next;
            delete temp;
            temp = next;
        }
    }
};

int main() {
    DoublyLinkedList dll;

    dll.insertAtBeginning(10);
    dll.insertAtBeginning(5);
    dll.insertAtEnd(20);
    dll.insertAtEnd(25);

    std::cout << "Forward: ";
    dll.displayForward(); // Output: 5 10 20 25

    std::cout << "Backward: ";
    dll.displayBackward(); // Output: 25 20 10 5

    dll.deleteNode(10);

    std::cout << "After deleting 10, Forward: ";
    dll.displayForward(); // Output: 5 20 25

    return 0;
}

```