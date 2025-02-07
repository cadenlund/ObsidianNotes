x! = n times n-1 times n-2


```cpp
	#include <iostream>
using namespace std;

// Recursive factorial function
unsigned long long factorial(unsigned int n) {
    // Base case: factorial of 0 and 1 is 1.
    if(n == 0 || n == 1) {
        return 1;
    }
    // Recursive case: n * factorial of (n-1)
    return n * factorial(n - 1);
}

int main() {
    unsigned int num;
    cout << "Enter a non-negative integer: ";
    cin >> num;
    cout << "Factorial of " << num << " is: " << factorial(num) << endl;
    return 0;
}

```