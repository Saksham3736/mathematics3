# DS practical 
```cpp
//practical 10
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* next;
};
Node* header = new Node();
void insertNode(int value) {
	Node* newNode = new Node();
	newNode->data = value;
	Node* curr = header->next;
	Node* prev = header;
	while (curr != header && curr->data < value) {
		prev = curr;
		curr = curr->next;
	}
	newNode->next = curr;
	prev->next = newNode;
	cout << "Inserted " << value << " successfully."<<endl;
}
void deleteNode(int value) {
	if (header->next == header) {
		cout << "List is empty."<<endl;
		return;
	}
	Node* curr = header->next;
	Node* prev = header;
	while (curr != header) {
		if (curr->data == value) {
			prev->next = curr->next;
			delete curr;
			cout << "Deleted " << value << " successfully."<<endl;
			return;
		}
		prev = curr;
		curr = curr->next;
	}
	cout << "Element " << value << " not found."<<endl;
}
void traverseList() {
	if (header->next == header) {
		cout << "List is empty."<<endl;
		return;
	}
	Node* temp = header->next;
	cout << "List elements: ";
	while (temp != header) {
		cout << temp->data << " ";
		temp = temp->next;
	}
	cout << endl;
}
int main() {
	header->next = header;
	int choice, value;
	cout << "=== Sorted Singly Circular Linked List (SCLL) ===\n";
		cout << "\nMenu:"<<endl;
		cout << "1. Insert"<<endl;
		cout << "2. Delete"<<endl;
		cout << "3. Traverse"<<endl;
		cout << "4. Exit"<<endl;
	while (true) {
		cout << "Enter your choice: ";
		cin >> choice;
		switch (choice) {
			case 1:
				cout << "Enter value to insert: ";
				cin >> value;
				insertNode(value);
				break;
			case 2:
				cout << "Enter value to delete: ";
				cin >> value;
				deleteNode(value);
				break;
			case 3:
				traverseList();
				break;
			case 4:
				cout << "Exiting program..."<<endl;
				return 0;
			default:
				cout << "Invalid choice! Try again."<<endl;
		}
	}
}
```
```cpp
//DS practical 11
#include <iostream>
using namespace std;
struct Employee {
	string ssn, name, dept, designation;
	double salary;
	string phone;
	Employee* prev;
	Employee* next;
};
class DoublyLinkedList {
private:
	Employee* head;
public:
	DoublyLinkedList() {
		head = NULL;
	}
	Employee* createNode() {
		Employee* newNode = new Employee;
		cout << "Enter SSN: ";
		cin >> newNode->ssn;
		cout << "Enter Name (single word): ";
		cin >> newNode->name;
		cout << "Enter Department: ";
		cin >> newNode->dept;
		cout << "Enter Designation: ";
		cin >> newNode->designation;
		cout << "Enter Salary: ";
		cin >> newNode->salary;
		cout << "Enter Phone Number: ";
		cin >> newNode->phone;
		newNode->prev = NULL;
		newNode->next = NULL;
		return newNode;
	}
    // Insert at the beginning
    void insertFront() {
        Employee* newNode = createNode();
        if (head == NULL) {
            head = newNode;
        } else {
            newNode->next = head;
            head->prev = newNode;
            head = newNode;
        }
        cout << "Inserted at beginning."<<endl;
    }
    void insertEnd() {
        Employee* newNode = createNode();
        if (head == NULL) {
            head = newNode;
        } else {
            Employee* temp = head;
            while (temp->next != NULL)
                temp = temp->next;
            temp->next = newNode;
            newNode->prev = temp;
        }
        cout << "Inserted at end."<<endl;
    }
    void insertAfterSSN() {
        if (head == NULL) {
            cout << "List is empty. Adding at beginning."<<endl;
            head = createNode();
            return;
        }
        string targetSSN;
        cout << "Enter SSN after which to insert: "<<endl;
        cin >> targetSSN;
        Employee* temp = head;
        while (temp != NULL && temp->ssn != targetSSN)
            temp = temp->next;
        if (temp == NULL) {
            cout << "SSN not found."<<endl;
            return;
        }
        Employee* newNode = createNode();
        newNode->next = temp->next;
        newNode->prev = temp;
        if (temp->next != NULL)
            temp->next->prev = newNode;
        temp->next = newNode;
        cout << "Inserted after SSN " << targetSSN << "."<<endl;
    }
    void display() {
        if (head == NULL) {
            cout << "List is empty."<<endl;
            return;
        }
        Employee* temp = head;
        int count = 0;
        while (temp != NULL) {
            count++;
            cout << "\nEmployee " << count <<endl;
            cout << "SSN: " << temp->ssn <<endl;
            cout << "Name: " << temp->name <<endl;
            cout << "Department: " << temp->dept <<endl;
            cout << "Designation: " << temp->designation <<endl;
            cout << "Salary: " << temp->salary <<endl;
            cout << "Phone: " << temp->phone <<endl;
            temp = temp->next;
        }
        cout << "\nTotal number of employees: " << count <<endl;
    }
    void deleteFront() {
        if (head == NULL) {
            cout << "List is empty."<<endl;
            return;
        }
        Employee* temp = head;
        head = head->next;
        if (head != NULL)
            head->prev = NULL;
        delete temp;
        cout << "Deleted first employee."<<endl;
    }
    void deleteEnd() {
        if (head == NULL) {
            cout << "List is empty."<<endl;
            return;
        }
        if (head->next == NULL) {
            delete head;
            head = NULL;
            cout << "Deleted last employee. List is now empty."<<endl;
            return;
        }

        Employee* temp = head;
        while (temp->next != NULL)
            temp = temp->next;

        temp->prev->next = NULL;
        delete temp;
        cout << "Deleted last employee."<<endl;
    }
    ~DoublyLinkedList() {
        while (head != NULL) {
            Employee* temp = head;
            head = head->next;
            delete temp;
        }
    }
};
int main() {
    DoublyLinkedList dll;
    int choice;
        cout << "\n--- Employee DLL Menu ---"<<endl;
        cout << "1. Insert at Beginning"<<endl;
        cout << "2. Insert at End"<<endl;
        cout << "3. Insert After SSN"<<endl;
        cout << "4. Display and Count"<<endl;
        cout << "5. Delete First"<<endl;
        cout << "6. Delete Last"<<endl;
        cout << "7. Exit"<<endl;
		while(true){
        cout << "Enter your choice: ";
        cin >> choice;
			switch (choice) {
            case 1: dll.insertFront(); break;
            case 2: dll.insertEnd(); break;
            case 3: dll.insertAfterSSN(); break;
            case 4: dll.display(); break;
            case 5: dll.deleteFront(); break;
            case 6: dll.deleteEnd(); break;
            case 7: cout << "Exiting."<<endl; return 0;
            default: cout << "Invalid choice."<<endl;
        }
    }

    return 0;
}

```
```cpp
//DS practical 12
#include <bits/stdc++.h>
using namespace std;
struct Term {
    int coeff;
    int x, y, z;
    Term* next;
};
Term* poly1 = nullptr;
Term* poly2 = nullptr;
Term* polysum = nullptr;
Term* createTerm(int c, int px, int py, int pz) {
    Term* newTerm = new Term();
    newTerm->coeff = c;
    newTerm->x = px;
    newTerm->y = py;
    newTerm->z = pz;
    newTerm->next = nullptr;
    return newTerm;
}
void insertTerm(Term** head, int c, int px, int py, int pz) {
    Term* newTerm = createTerm(c, px, py, pz);
    if (*head == nullptr) {
        *head = newTerm;
        return;
    }
    Term* temp = *head;
    while (temp->next != nullptr)
        temp = temp->next;
    temp->next = newTerm;
}
void displayPoly(Term* head) {
    if (head == nullptr) {
        cout << "Polynomial is empty.\n";
        return;
    }
    Term* temp = head;
    while (temp != nullptr) {
        cout << temp->coeff << "x^" << temp->x << "y^" << temp->y << "z^" << temp->z;
        if (temp->next != nullptr && temp->next->coeff >= 0)
            cout << " + ";
        else if (temp->next != nullptr)
            cout << " ";
        temp = temp->next;
    }
    cout << endl;
}
void createPolynomial(Term** head, int n) {
    for (int i = 0; i < n; i++) {
        int c, x, y, z;
        cout << "Enter coefficient, powers of x, y, z: ";
        cin >> c >> x >> y >> z;
        insertTerm(head, c, x, y, z);
    }
}
void addPolynomials(Term* p1, Term* p2, Term** sum) {
    while (p1 != nullptr && p2 != nullptr) {
        if (p1->x == p2->x && p1->y == p2->y && p1->z == p2->z) {
            int c = p1->coeff + p2->coeff;
            if (c != 0)
                insertTerm(sum, c, p1->x, p1->y, p1->z);
            p1 = p1->next;
            p2 = p2->next;
        } else if ((p1->x > p2->x) || 
                  (p1->x == p2->x && p1->y > p2->y) ||
                  (p1->x == p2->x && p1->y == p2->y && p1->z > p2->z)) {
            insertTerm(sum, p1->coeff, p1->x, p1->y, p1->z);
            p1 = p1->next;
        } else {
            insertTerm(sum, p2->coeff, p2->x, p2->y, p2->z);
            p2 = p2->next;
        }
    }
    while (p1 != nullptr) {
        insertTerm(sum, p1->coeff, p1->x, p1->y, p1->z);
        p1 = p1->next;
    }
    while (p2 != nullptr) {
        insertTerm(sum, p2->coeff, p2->x, p2->y, p2->z);
        p2 = p2->next;
    }
}
int main() {
    int n1, n2, choice;
    cout << "=== Polynomial Addition Program (3 variables: x, y, z) ===\n";
    cout << "\nMenu:\n";
    cout << "1. Create POLY1(x,y,z)\n";
    cout << "2. Create POLY2(x,y,z)\n";
    cout << "3. Display POLY1, POLY2\n";
    cout << "4. Add POLY1 and POLY2 → POLYSUM\n";
    cout << "5. Display POLYSUM\n";
    cout << "6. Exit\n";
    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter number of terms in POLY1: ";
                cin >> n1;
                poly1 = nullptr;
                createPolynomial(&poly1, n1);
                cout << "POLY1 created successfully.\n";
                break;
            case 2:
                cout << "Enter number of terms in POLY2: ";
                cin >> n2;
                poly2 = nullptr;
                createPolynomial(&poly2, n2);
                cout << "POLY2 created successfully.\n";
                break;
            case 3:
                cout << "\nPOLY1(x,y,z): ";
                displayPoly(poly1);
                cout << "POLY2(x,y,z): ";
                displayPoly(poly2);
                break;
            case 4:
                polysum = nullptr;
                addPolynomials(poly1, poly2, &polysum);
                cout << "Polynomials added successfully.\n";
                break;
            case 5:
                cout << "\nPOLYSUM(x,y,z): ";
                displayPoly(polysum);
                break;
            case 6:
                cout << "Exiting program...\n";
                return 0;
            default:
                cout << "Invalid choice! Try again.\n";
        }
    }
}
```
```cpp
//DS practical 13
#include <bits/stdc++.h>
using namespace std;
#define MAX 100
int stackArr[MAX];
int top = -1;
void push(int value) {
    if (top == MAX - 1) {
        cout << "Stack Overflow! Cannot push " << value << "." << endl;
        return;
    }
    stackArr[++top] = value;
    cout << "Pushed " << value << " onto stack successfully." << endl;
}
void pop() {
    if (top == -1) {
        cout << "Stack Underflow! Stack is empty." << endl;
        return;
    }
    cout << "Popped element: " << stackArr[top--] << endl;
}
void display() {
    if (top == -1) {
        cout << "Stack is empty." << endl;
        return;
    }
    cout << "Stack elements (top to bottom): ";
    for (int i = top; i >= 0; i--)
        cout << stackArr[i] << " ";
    cout << endl;
}
int main() {
    int choice, value;
    cout << "=== Stack Implementation using Array ===" << endl;
    cout << "\nMenu:" << endl;
    cout << "1. Push" << endl;
    cout << "2. Pop" << endl;
    cout << "3. Display" << endl;
    cout << "4. Exit" << endl;
    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter value to push: ";
                cin >> value;
                push(value);
                break;
            case 2:
                pop();
                break;
            case 3:
                display();
                break;
            case 4:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}

```
```cpp
//DS practical 14
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* next;
};
Node* topNode = nullptr;
void push(int value) {
    Node* newNode = new Node();
    newNode->data = value;
    newNode->next = topNode;
    topNode = newNode;
    cout << "Pushed " << value << " onto stack successfully." << endl;
}
void pop() {
    if (topNode == nullptr) {
        cout << "Stack Underflow! Stack is empty." << endl;
        return;
    }
    Node* temp = topNode;
    cout << "Popped element: " << temp->data << endl;
    topNode = topNode->next;
    delete temp;
}
void display() {
    if (topNode == nullptr) {
        cout << "Stack is empty." << endl;
        return;
    }
    Node* temp = topNode;
    cout << "Stack elements (top to bottom): ";
    while (temp != nullptr) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << endl;
}
int main() {
    int choice, value;
    cout << "=== Stack Implementation using Linked List ===" << endl;
    cout << "\nMenu:" << endl;
    cout << "1. Push" << endl;
    cout << "2. Pop" << endl;
    cout << "3. Display" << endl;
    cout << "4. Exit" << endl;

    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter value to push: ";
                cin >> value;
                push(value);
                break;
            case 2:
                pop();
                break;
            case 3:
                display();
                break;
            case 4:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
//DS practical 15
#include <bits/stdc++.h>
using namespace std;
#define MAX 100
int stackArr[MAX];
int top = -1;
void push(int digit) {
    if (top == MAX - 1) {
        cout << "Stack Overflow!" << endl;
        return;
    }
    stackArr[++top] = digit;
}
int pop() {
    if (top == -1) {
        cout << "Stack Underflow!" << endl;
        return -1;
    }
    return stackArr[top--];
}
void checkPalindrome(int num) {
    int original = num;
    top = -1; // reset stack
    while (num > 0) {
        int digit = num % 10;
        push(digit);
        num /= 10;
    }
    int reversed = 0;
    int place = 1;
    while (top != -1) {
        reversed = reversed * 10 + pop();
    }
    cout << "Original number: " << original << endl;
    cout << "Reversed number: " << reversed << endl;
    if (original == reversed)
        cout << "The number is a Palindrome." << endl;
    else
        cout << "The number is NOT a Palindrome." << endl;
}
int main() {
    int choice, num;
    cout << "=== Palindrome Check using Stack ===" << endl;
    cout << "\nMenu:" << endl;
    cout << "1. Check Palindrome" << endl;
    cout << "2. Exit" << endl;

    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter a number: ";
                cin >> num;
                checkPalindrome(num);
                break;
            case 2:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}

```
```cpp
//DS practical 16
#include <bits/stdc++.h>
using namespace std;
#define MAX 100
int stackArr[MAX];
int top = -1;
int val1, val2, result; // global variables for operations
void push(int value) {
    if (top == MAX - 1) {
        cout << "Stack Overflow!" << endl;
        return;
    }
    stackArr[++top] = value;
}
void pop() {
    if (top == -1) {
        cout << "Stack Underflow!" << endl;
        return;
    }
    top--;
}
void evaluatePostfix(string expr) {
    top = -1; // reset stack
    for (char ch : expr) {
        if (isdigit(ch)) {
            push(ch - '0'); // push operand
        } 
        else if (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '%' || ch == '^') {
            // Pop last two values manually (using globals)
            if (top < 1) {
                cout << "Invalid Expression!" << endl;
                return;
            }
            val2 = stackArr[top]; pop();
            val1 = stackArr[top]; pop();
            switch (ch) {
                case '+': result = val1 + val2; break;
                case '-': result = val1 - val2; break;
                case '*': result = val1 * val2; break;
                case '/': result = val1 / val2; break;
                case '%': result = val1 % val2; break;
                case '^': result = pow(val1, val2); break;
            }
            push(result);
        }
        else if (ch == ' ' || ch == '\t') {
            continue;
        }
        else {
            cout << "Invalid character in expression: " << ch << endl;
            return;
        }
    }
    if (top == 0)
        cout << "Result = " << stackArr[top] << endl;
    else
        cout << "Invalid Expression!" << endl;
}
int main(){
    int choice;
    string expr;
    while (true) {
        cout << "\n=== Postfix Expression Evaluation ===" << endl;
        cout << "1. Evaluate Postfix Expression" << endl;
        cout << "2. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();
        switch (choice) {
            case 1:
                cout << "Enter postfix expression (single-digit operands only): ";
                getline(cin, expr);
                evaluatePostfix(expr);
                break;
            case 2:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice!" << endl;
        }
    }
	return 0;
}

```
```cpp
//DS practical 17
#include <bits/stdc++.h>
using namespace std;
#define MAX 100
char stackArr[MAX];
int top = -1;
string infix, postfix;
void push(char c) {
    if (top == MAX - 1) {
        cout << "Stack Overflow!" << endl;
        return;
    }
    stackArr[++top] = c;
}
void pop() {
    if (top == -1) {
        cout << "Stack Underflow!" << endl;
        return;
    }
    top--;
}
char peek() {
    if (top == -1)
        return '\0';
    return stackArr[top];
}
int precedence(char op) {
    if (op == '^')
        return 3;
    if (op == '*' || op == '/' || op == '%')
        return 2;
    if (op == '+' || op == '-')
        return 1;
    return 0;
}
bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '%' || c == '^');
}
void infixToPostfix(string expr) {
    postfix = "";
    top = -1; // reset stack
    for (char c : expr) {
        if (isalnum(c)) {  // operand
            postfix += c;
        } 
        else if (c == '(') {
            push(c);
        } 
        else if (c == ')') {
            while (top != -1 && peek() != '(') {
                postfix += peek();
                pop();
            }
            if (top != -1 && peek() == '(')
                pop(); // remove '('
        } 
        else if (isOperator(c)) {
            while (top != -1 && precedence(peek()) >= precedence(c) && c != '^') {
                postfix += peek();
                pop();
            }
            push(c);
        }
    }
    while (top != -1) {
        postfix += peek();
        pop();
    }
    cout << "Postfix Expression: " << postfix << endl;
}
int main() {
    int choice;
        cout << "\n=== INFIX TO POSTFIX CONVERSION ===" << endl;
        cout << "1. Convert Infix to Postfix" << endl;
        cout << "2. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

    while (true) {
        switch (choice) {
            case 1:
                cout << "Enter Infix Expression: ";
                getline(cin, infix);
                infixToPostfix(infix);
                break;
            case 2:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
return 0;
}
```
```cpp
//DS practical 18
#include <bits/stdc++.h>
using namespace std;
int moveCount = 0;
void towerOfHanoi(int n, char source, char auxiliary, char destination) {
    if (n == 1) {
        cout << "Move disk 1 from " << source << " to " << destination << endl;
        moveCount++;
        return;
    }
    towerOfHanoi(n - 1, source, destination, auxiliary);
    cout << "Move disk " << n << " from " << source << " to " << destination << endl;
    moveCount++;
    towerOfHanoi(n - 1, auxiliary, source, destination);
}
int main() {
    int choice, n;
    cout << "=== Tower of Hanoi Problem ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Solve Tower of Hanoi\n";
        cout << "2. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter number of disks: ";
                cin >> n;
                moveCount = 0;
                cout << "\nMoves to solve Tower of Hanoi:\n";
                towerOfHanoi(n, 'A', 'B', 'C'); // A=source, B=auxiliary, C=destination
                cout << "\nTotal moves required: " << moveCount << endl;
                break;
            case 2:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
//DS practical 19
#include <bits/stdc++.h>
using namespace std;
void swap(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}
void partition(int arr[], int low, int high, int &pivotIndex) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    pivotIndex = i + 1;
}
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi;
        partition(arr, low, high, pi);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
void display(int arr[], int n) {
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    cout << endl;
}
int main() {
    int choice, n;
    cout << "=== QuickSort Program ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Sort a List using QuickSort\n";
        cout << "2. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:{
                cout << "Enter number of elements: ";
                cin >> n;
                int arr[n];
                cout << "Enter elements:\n";
                for (int i = 0; i < n; i++)
                    cin >> arr[i];
                quickSort(arr, 0, n - 1);
                cout << "Sorted list: ";
                display(arr, n);
                break;}
            case 2:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
	return 0;
}
```
```cpp
//DS practical 20
#include <bits/stdc++.h>
using namespace std;
#define MAX 100
int queueArr[MAX];
int frontIndex = -1, rearIndex = -1;
void enqueue(int value) {
    if (rearIndex == MAX - 1) {
        cout << "Queue Overflow! Cannot insert " << value << "." << endl;
        return;
    }
    if (frontIndex == -1) // first element
        frontIndex = 0;
    rearIndex++;
    queueArr[rearIndex] = value;
    cout << "Inserted " << value << " into queue successfully." << endl;
}
void dequeue() {
    if (frontIndex == -1 || frontIndex > rearIndex) {
        cout << "Queue Underflow! Queue is empty." << endl;
        return;
    }
    cout << "Deleted element: " << queueArr[frontIndex] << endl;
    frontIndex++;
}
void displayQueue() {
    if (frontIndex == -1 || frontIndex > rearIndex) {
        cout << "Queue is empty." << endl;
        return;
    }
    cout << "Queue elements: ";
    for (int i = frontIndex; i <= rearIndex; i++)
        cout << queueArr[i] << " ";
    cout << endl;
}
int main() {
    int choice, value;
    cout << "=== Linear Queue Implementation (Array) ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Insert (Enqueue)\n";
        cout << "2. Delete (Dequeue)\n";
        cout << "3. Display Queue\n";
        cout << "4. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                enqueue(value);
                break;
            case 2:
                dequeue();
                break;
            case 3:
                displayQueue();
                break;
            case 4:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
// DS practical 21
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* next;
};
Node* frontNode = nullptr;
Node* rearNode = nullptr;
void enqueue(int value) {
    Node* newNode = new Node();
    newNode->data = value;
    newNode->next = nullptr;
    if (rearNode == nullptr) { // first element
        frontNode = rearNode = newNode;
    } else {
        rearNode->next = newNode;
        rearNode = newNode;
    }
    cout << "Inserted " << value << " into queue successfully." << endl;
}
void dequeue() {
    if (frontNode == nullptr) {
        cout << "Queue Underflow! Queue is empty." << endl;
        return;
    }
    Node* temp = frontNode;
    cout << "Deleted element: " << temp->data << endl;
    frontNode = frontNode->next;
    if (frontNode == nullptr) // queue became empty
        rearNode = nullptr;
    delete temp;
}
void displayQueue() {
    if (frontNode == nullptr) {
        cout << "Queue is empty." << endl;
        return;
    }
    Node* temp = frontNode;
    cout << "Queue elements: ";
    while (temp != nullptr) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << endl;
}
int main() {
    int choice, value;
    cout << "=== Queue Implementation (Linked List) ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Insert (Enqueue)\n";
        cout << "2. Delete (Dequeue)\n";
        cout << "3. Display Queue\n";
        cout << "4. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                enqueue(value);
                break;
            case 2:
                dequeue();
                break;
            case 3:
                displayQueue();
                break;
            case 4:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
//DS practical 22
#include <bits/stdc++.h>
using namespace std;
#define MAX 5
int queueArr[MAX];
int frontIndex = -1, rearIndex = -1;
void checkOverflow() {
    if ((frontIndex == 0 && rearIndex == MAX - 1) || (rearIndex + 1) % MAX == frontIndex) {
        cout << "Queue Overflow! Circular Queue is full." << endl;
    } else {
        cout << "No overflow. Space available in Circular Queue." << endl;
    }
}
void checkUnderflow() {
    if (frontIndex == -1) {
        cout << "Queue Underflow! Circular Queue is empty." << endl;
    } else {
        cout << "No underflow. Circular Queue has elements." << endl;
    }
}
void enqueue(int value) {
    if ((frontIndex == 0 && rearIndex == MAX - 1) || (rearIndex + 1) % MAX == frontIndex) {
        cout << "Queue Overflow! Cannot insert " << value << "." << endl;
        return;
    }
    if (frontIndex == -1) // first element
        frontIndex = rearIndex = 0;
    else
        rearIndex = (rearIndex + 1) % MAX;
    queueArr[rearIndex] = value;
    cout << "Inserted " << value << " into Circular Queue successfully." << endl;
}
void dequeue() {
    if (frontIndex == -1) {
        cout << "Queue Underflow! Circular Queue is empty." << endl;
        return;
    }
    cout << "Deleted element: " << queueArr[frontIndex] << endl;
    if (frontIndex == rearIndex) // only one element
        frontIndex = rearIndex = -1;
    else
        frontIndex = (frontIndex + 1) % MAX;
}
void displayQueue() {
    if (frontIndex == -1) {
        cout << "Circular Queue is empty." << endl;
        return;
    }
    cout << "Circular Queue elements: ";
    int i = frontIndex;
    while (true) {
        cout << queueArr[i] << " ";
        if (i == rearIndex)
            break;
        i = (i + 1) % MAX;
    }
    cout << endl;
}
int main() {
    int choice, value;
    cout << "=== Circular Queue Implementation (Array) ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Check Overflow\n";
        cout << "2. Check Underflow\n";
        cout << "3. Insert (Enqueue)\n";
        cout << "4. Delete (Dequeue)\n";
        cout << "5. Display Circular Queue\n";
        cout << "6. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                checkOverflow();
                break;
            case 2:
                checkUnderflow();
                break;
            case 3:
                cout << "Enter value to insert: ";
                cin >> value;
                enqueue(value);
                break;
            case 4:
                dequeue();
                break;
            case 5:
                displayQueue();
                break;
            case 6:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
//DS practical 23
#include <bits/stdc++.h>
using namespace std;
#define MAX 5
int dequeArr[MAX];
int frontIndex = -1, rearIndex = -1;
void checkOverflow() {
    if ((frontIndex == 0 && rearIndex == MAX - 1) || (frontIndex == rearIndex + 1)) {
        cout << "Deque Overflow! No space available." << endl;
    } else {
        cout << "No overflow. Space is available." << endl;
    }
}
void checkUnderflow() {
    if (frontIndex == -1) {
        cout << "Deque Underflow! Deque is empty." << endl;
    } else {
        cout << "Deque has elements." << endl;
    }
}
void insertFront(int value) {
    if ((frontIndex == 0 && rearIndex == MAX - 1) || (frontIndex == rearIndex + 1)) {
        cout << "Deque Overflow! Cannot insert " << value << " at front." << endl;
        return;
    }
    if (frontIndex == -1) { // empty deque
        frontIndex = rearIndex = 0;
    } else if (frontIndex == 0) {
        frontIndex = MAX - 1;
    } else {
        frontIndex--;
    }
    dequeArr[frontIndex] = value;
    cout << "Inserted " << value << " at front successfully." << endl;
}
void insertRear(int value) {
    if ((frontIndex == 0 && rearIndex == MAX - 1) || (frontIndex == rearIndex + 1)) {
        cout << "Deque Overflow! Cannot insert " << value << " at rear." << endl;
        return;
    }
    if (frontIndex == -1) { // empty deque
        frontIndex = rearIndex = 0;
    } else if (rearIndex == MAX - 1) {
        rearIndex = 0;
    } else {
        rearIndex++;
    }
    dequeArr[rearIndex] = value;
    cout << "Inserted " << value << " at rear successfully." << endl;
}
void deleteFront() {
    if (frontIndex == -1) {
        cout << "Deque Underflow! Cannot delete from front." << endl;
        return;
    }
    cout << "Deleted element from front: " << dequeArr[frontIndex] << endl;
    if (frontIndex == rearIndex) { // only one element
        frontIndex = rearIndex = -1;
    } else if (frontIndex == MAX - 1) {
        frontIndex = 0;
    } else {
        frontIndex++;
    }
}
void deleteRear() {
    if (frontIndex == -1) {
        cout << "Deque Underflow! Cannot delete from rear." << endl;
        return;
    }
    cout << "Deleted element from rear: " << dequeArr[rearIndex] << endl;
    if (frontIndex == rearIndex) { // only one element
        frontIndex = rearIndex = -1;
    } else if (rearIndex == 0) {
        rearIndex = MAX - 1;
    } else {
        rearIndex--;
    }
}
void displayDeque() {
    if (frontIndex == -1) {
        cout << "Deque is empty." << endl;
        return;
    }
    cout << "Deque elements: ";
    int i = frontIndex;
    while (true) {
        cout << dequeArr[i] << " ";
        if (i == rearIndex) break;
        i = (i + 1) % MAX;
    }
    cout << endl;
}
int main() {
    int choice, value;
    cout << "=== Deque (Array Implementation) ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Check Overflow\n";
        cout << "2. Check Underflow\n";
        cout << "3. Insert at Front\n";
        cout << "4. Insert at Rear\n";
        cout << "5. Delete from Front\n";
        cout << "6. Delete from Rear\n";
        cout << "7. Display Deque\n";
        cout << "8. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1: checkOverflow(); break;
            case 2: checkUnderflow(); break;
            case 3:
                cout << "Enter value to insert at front: ";
                cin >> value;
                insertFront(value);
                break;
            case 4:
                cout << "Enter value to insert at rear: ";
                cin >> value;
                insertRear(value);
                break;
            case 5: deleteFront(); break;
            case 6: deleteRear(); break;
            case 7: displayDeque(); break;
            case 8:
                cout << "Exiting program..." << endl;
                return 0;
            default: cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
// DS practical 24
#include <bits/stdc++.h>
using namespace std;
#define MAX 100
struct Element {
    int data;
    int priority;
};
Element pq[MAX];
int size = 0;
void insertPQ(int value, int prio) {
    if (size == MAX) {
        cout << "Priority Queue Overflow! Cannot insert." << endl;
        return;
    }
    // insert in sorted order based on priority (higher priority first)
    int i = size - 1;
    while (i >= 0 && pq[i].priority < prio) {
        pq[i + 1] = pq[i];
        i--;
    }
    pq[i + 1].data = value;
    pq[i + 1].priority = prio;
    size++;
    cout << "Inserted " << value << " with priority " << prio << " successfully." << endl;
}
void deletePQ() {
    if (size == 0) {
        cout << "Priority Queue Underflow! Queue is empty." << endl;
        return;
    }
    cout << "Deleted element: " << pq[0].data << " with priority " << pq[0].priority << endl;
    for (int i = 1; i < size; i++) {
        pq[i - 1] = pq[i];
    }
    size--;
}
void displayPQ() {
    if (size == 0) {
        cout << "Priority Queue is empty." << endl;
        return;
    }
    cout << "Priority Queue elements (Data:Priority): ";
    for (int i = 0; i < size; i++) {
        cout << pq[i].data << ":" << pq[i].priority << " ";
    }
    cout << endl;
}
int main() {
    int choice, value, prio;
    cout << "=== Priority Queue (Array Implementation) ===" << endl;
        cout << "\nMenu:\n";
        cout << "1. Insert into Priority Queue\n";
        cout << "2. Delete from Priority Queue\n";
        cout << "3. Display Priority Queue\n";
        cout << "4. Exit\n";
    while (true) {
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                cout << "Enter element value: ";
                cin >> value;
                cout << "Enter element priority (higher number = higher priority): ";
                cin >> prio;
                insertPQ(value, prio);
                break;
            case 2:
                deletePQ();
                break;
            case 3:
                displayPQ();
                break;
            case 4:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
// DS practical 25
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* left;
    Node* right;
};
// Create a new node
Node* createNode(int value) {
    Node* newNode = new Node();
    newNode->data = value;
    newNode->left = newNode->right = NULL;
    return newNode;
}
// a) Insert a node in BST
void insertNode(Node*& root, int value) {
    if (root == NULL) {
        root = createNode(value);
        cout << "Inserted " << value << " successfully." << endl;
        return;
    }
    if (value < root->data)
        insertNode(root->left, value);
    else if (value > root->data)
        insertNode(root->right, value);
    else
        cout << "Duplicate values not allowed in BST!" << endl;
}
// b) Traverse BST - Inorder
void inorder(Node* root) {
    if (root == NULL) return;
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}
// Preorder
void preorder(Node* root) {
    if (root == NULL) return;
    cout << root->data << " ";
    preorder(root->left);
    preorder(root->right);
}

// Postorder
void postorder(Node* root) {
    if (root == NULL) return;
    postorder(root->left);
    postorder(root->right);
    cout << root->data << " ";
}
// c) Search for an element
void searchNode(Node* root, int key) {
    if (root == NULL) {
        cout << "Element not found in BST." << endl;
        return;
    }
    if (root->data == key)
        cout << "Element " << key << " found in BST." << endl;
    else if (key < root->data)
        searchNode(root->left, key);
    else
        searchNode(root->right, key);
}

// d) Delete a node from BST
Node* findMin(Node* root) {
    while (root->left != NULL)
        root = root->left;
    return root;
}
Node* deleteNode(Node* root, int key) {
    if (root == NULL) return root;

    if (key < root->data)
        root->left = deleteNode(root->left, key);
    else if (key > root->data)
        root->right = deleteNode(root->right, key);
    else {
        // Node found
        if (root->left == NULL && root->right == NULL) {
            delete root;
            return NULL;
        }
        else if (root->left == NULL) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        else if (root->right == NULL) {
            Node* temp = root->left;
            delete root;
            return temp;
        }
        else {
            Node* temp = findMin(root->right);
            root->data = temp->data;
            root->right = deleteNode(root->right, temp->data);
        }
    }
    return root;
}
// e) Display all traversals
void displayTraversals(Node* root) {
    if (root == NULL) {
        cout << "BST is empty." << endl;
        return;
    }
    cout << "Inorder Traversal: ";
    inorder(root);
    cout << "\nPreorder Traversal: ";
    preorder(root);
    cout << "\nPostorder Traversal: ";
    postorder(root);
    cout << endl;
}

// Main function with menu
int main() {
    Node* root = NULL;
    int choice, value;
    cout << "=== Binary Search Tree (BST) Implementation ===" << endl;
    while (true) {
        cout << "\nMenu:\n";
        cout << "1. Insert Element\n";
        cout << "2. Traverse BST (Inorder, Preorder, Postorder)\n";
        cout << "3. Search Element\n";
        cout << "4. Delete Element\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                insertNode(root, value);
                break;
            case 2:
                displayTraversals(root);
                break;
            case 3:
                cout << "Enter element to search: ";
                cin >> value;
                searchNode(root, value);
                break;
            case 4:
                cout << "Enter element to delete: ";
                cin >> value;
                root = deleteNode(root, value);
                cout << "If element existed, it has been deleted." << endl;
                break;
            case 5:
                cout << "Exiting program..." << endl;
                return 0;
            default:
                cout << "Invalid choice! Try again." << endl;
        }
    }
}
```
```cpp
//DS practical 26
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* left;
    Node* right;
    int height;
};
// Get height of node
int getHeight(Node* n) {
    return (n == NULL) ? 0 : n->height;
}
// Get balance factor
int getBalance(Node* n) {
    return (n == NULL) ? 0 : getHeight(n->left) - getHeight(n->right);
}
// Create new node
Node* createNode(int value) {
    Node* node = new Node();
    node->data = value;
    node->left = node->right = NULL;
    node->height = 1;
    return node;
}
// Right rotation
Node* rightRotate(Node* y) {
    Node* x = y->left;
    Node* T2 = x->right;
    x->right = y;
    y->left = T2;
    y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
    x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
    return x;
}
// Left rotation
Node* leftRotate(Node* x) {
    Node* y = x->right;
    Node* T2 = y->left;
    y->left = x;
    x->right = T2;
    x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
    y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
    return y;
}
// Insert node (void function)
void insertNode(Node*& root, int key) {
    if (root == NULL) {
        root = createNode(key);
        cout << "Inserted " << key << " into AVL Tree.\n";
        return;
    }

    if (key < root->data)
        insertNode(root->left, key);
    else if (key > root->data)
        insertNode(root->right, key);
    else {
        cout << "Duplicate values not allowed!\n";
        return;
    }

    root->height = 1 + max(getHeight(root->left), getHeight(root->right));
    int balance = getBalance(root);
    // Balancing rotations
    if (balance > 1 && key < root->left->data)
        root = rightRotate(root);
    else if (balance < -1 && key > root->right->data)
        root = leftRotate(root);
    else if (balance > 1 && key > root->left->data) {
        root->left = leftRotate(root->left);
        root = rightRotate(root);
    } else if (balance < -1 && key < root->right->data) {
        root->right = rightRotate(root->right);
        root = leftRotate(root);
    }
}
// Search in AVL
void searchNode(Node* root, int key) {
    if (root == NULL) {
        cout << "Element not found.\n";
        return;
    }
    if (root->data == key)
        cout << "Element " << key << " found in AVL Tree.\n";
    else if (key < root->data)
        searchNode(root->left, key);
    else
        searchNode(root->right, key);
}
// Find node with minimum value
Node* minValueNode(Node* node) {
    Node* current = node;
    while (current && current->left != NULL)
        current = current->left;
    return current;
}
// Delete node (void version)
void deleteNode(Node*& root, int key) {
    if (root == NULL) {
        cout << "Element not found.\n";
        return;
    }
    if (key < root->data)
        deleteNode(root->left, key);
    else if (key > root->data)
        deleteNode(root->right, key);
    else {
        if ((root->left == NULL) || (root->right == NULL)) {
            Node* temp = (root->left) ? root->left : root->right;
            if (temp == NULL) {
                delete root;
                root = NULL;
            } else {
                *root = *temp;
                delete temp;
            }
            cout << "Deleted " << key << " from AVL Tree.\n";
        } else {
            Node* temp = minValueNode(root->right);
            root->data = temp->data;
            deleteNode(root->right, temp->data);
        }
    }

    if (root == NULL)
        return;
    root->height = 1 + max(getHeight(root->left), getHeight(root->right));
    int balance = getBalance(root);
    // Balance rotations
    if (balance > 1 && getBalance(root->left) >= 0)
        root = rightRotate(root);
    else if (balance > 1 && getBalance(root->left) < 0) {
        root->left = leftRotate(root->left);
        root = rightRotate(root);
    } else if (balance < -1 && getBalance(root->right) <= 0)
        root = leftRotate(root);
    else if (balance < -1 && getBalance(root->right) > 0) {
        root->right = rightRotate(root->right);
        root = leftRotate(root);
    }
}
// Inorder traversal
void inorder(Node* root) {
    if (root != NULL) {
        inorder(root->left);
        cout << root->data << " ";
        inorder(root->right);
    }
}
int main() {
    Node* root = NULL;
    int choice, value;

    while (true) {
        cout << "\n--- AVL Tree Menu ---\n";
        cout << "1. Insert\n2. Search\n3. Delete\n4. Display (Inorder)\n5. Exit\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter value to insert: ";
            cin >> value;
            insertNode(root, value);
            break;
        case 2:
            cout << "Enter value to search: ";
            cin >> value;
            searchNode(root, value);
            break;
        case 3:
            cout << "Enter value to delete: ";
            cin >> value;
            deleteNode(root, value);
            break;
        case 4:
            cout << "Inorder Traversal: ";
            inorder(root);
            cout << endl;
            break;
        case 5:
            cout << "Exiting...\n";
            return 0;
        default:
            cout << "Invalid choice!\n";
        }
    }
}
```
```cpp
// DS pratical 27
#include <bits/stdc++.h>
using namespace std;
// Function to heapify a subtree rooted at index i
void heapify(int arr[], int n, int i) {
    int largest = i;       // Initialize largest as root
    int left = 2 * i + 1;  // left child index
    int right = 2 * i + 2; // right child index
    // If left child is larger than root
    if (left < n && arr[left] > arr[largest])
        largest = left;
    // If right child is larger than largest so far
    if (right < n && arr[right] > arr[largest])
        largest = right;
    // If largest is not root
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest); // Recursively heapify the affected subtree
    }
}
// Function to perform heapsort
void heapSort(int arr[], int n) {
    // Step 1: Build a max heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    // Step 2: Extract elements one by one from the heap
    for (int i = n - 1; i >= 0; i--) {
        swap(arr[0], arr[i]);   // Move current root to end
        heapify(arr, i, 0);     // Heapify the reduced heap
    }
}
// Function to display the array
void display(int arr[], int n) {
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    cout << endl;
}
// Main function
int main() {
    int n;
    cout << "Enter number of elements: ";
    cin >> n;
    int arr[n];

    cout << "Enter elements:\n";
    for (int i = 0; i < n; i++)
        cin >> arr[i];

    cout << "\nOriginal array: ";
    display(arr, n);

    heapSort(arr, n);

    cout << "Sorted array (Heapsort): ";
    display(arr, n);

    return 0;
}
```
```cpp
// DS practical 28
#include <bits/stdc++.h>
using namespace std;

#define MAX 20
int adj[MAX][MAX];   // Adjacency matrix
int n;               // Number of cities
bool visited[MAX];   // Visited array for DFS and BFS

// Function to create a graph using adjacency matrix
void createGraph() {
    cout << "Enter number of cities: ";
    cin >> n;

    cout << "Enter adjacency matrix (" << n << "x" << n << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> adj[i][j];
        }
    }
    cout << "Graph created successfully.\n";
}

// Function for DFS traversal
void DFS(int v) {
    visited[v] = true;
    cout << v << " ";

    for (int i = 0; i < n; i++) {
        if (adj[v][i] == 1 && !visited[i])
            DFS(i);
    }
}

void performDFS() {
    int start;
    cout << "Enter starting city (0 to " << n - 1 << "): ";
    cin >> start;

    for (int i = 0; i < n; i++)
        visited[i] = false;

    cout << "DFS Traversal: ";
    DFS(start);
    cout << endl;
}

// Function for BFS traversal
void performBFS() {
    int start;
    cout << "Enter starting city (0 to " << n - 1 << "): ";
    cin >> start;

    for (int i = 0; i < n; i++)
        visited[i] = false;

    queue<int> q;
    visited[start] = true;
    q.push(start);

    cout << "BFS Traversal: ";
    while (!q.empty()) {
        int v = q.front();
        q.pop();
        cout << v << " ";

        for (int i = 0; i < n; i++) {
            if (adj[v][i] == 1 && !visited[i]) {
                visited[i] = true;
                q.push(i);
            }
        }
    }
    cout << endl;
}

// Display adjacency matrix
void displayGraph() {
    cout << "\nAdjacency Matrix:\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++)
            cout << adj[i][j] << " ";
        cout << endl;
    }
}

// Main function with menu
int main() {
    int choice;
    cout << "=== Graph Representation and Traversal ===\n";
    cout << "Menu:\n";
    cout << "1. Create Graph (Adjacency Matrix)\n";
    cout << "2. Display Graph\n";
    cout << "3. DFS Traversal\n";
    cout << "4. BFS Traversal\n";
    cout << "5. Exit\n";

    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                createGraph();
                break;
            case 2:
                displayGraph();
                break;
            case 3:
                performDFS();
                break;
            case 4:
                performBFS();
                break;
            case 5:
                cout << "Exiting program...\n";
                return 0;
            default:
                cout << "Invalid choice! Try again.\n";
        }
    }
}
```
```cpp
// DS practical 29
#include <bits/stdc++.h>
using namespace std;

#define MAX 20

int n;                   // Number of vertices
int cost[MAX][MAX];      // Adjacency matrix for cost

// Function to create graph
void createGraph() {
    cout << "Enter number of vertices: ";
    cin >> n;

    cout << "Enter the cost adjacency matrix (" << n << "x" << n << "):\n";
    cout << "(Enter 0 if there is no edge between two vertices)\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> cost[i][j];
            if (cost[i][j] == 0)
                cost[i][j] = 999; // Represent no edge with high value
        }
    }
}

// --------- PRIM'S ALGORITHM ---------
void primsMST() {
    int selected[MAX];
    memset(selected, 0, sizeof(selected));
    int total = 0, edges = 0;

    selected[0] = 1; // Start from vertex 0

    cout << "\nEdges in the Minimum Spanning Tree (Prim's):\n";
    while (edges < n - 1) {
        int minCost = 999, x = 0, y = 0;
        for (int i = 0; i < n; i++) {
            if (selected[i]) {
                for (int j = 0; j < n; j++) {
                    if (!selected[j] && cost[i][j] < minCost) {
                        minCost = cost[i][j];
                        x = i;
                        y = j;
                    }
                }
            }
        }
        cout << x << " - " << y << " : " << cost[x][y] << endl;
        total += cost[x][y];
        selected[y] = 1;
        edges++;
    }
    cout << "Total cost of MST (Prim's) = " << total << endl;
}

// --------- KRUSKAL'S ALGORITHM ---------
int parent[MAX];

// Function to find parent (for Kruskal’s)
int findParent(int i) {
    while (parent[i] != i)
        i = parent[i];
    return i;
}

// Function to perform union operation
void unionSets(int i, int j) {
    int a = findParent(i);
    int b = findParent(j);
    parent[a] = b;
}

void kruskalsMST() {
    int minCost = 0, edgeCount = 0;

    // Initialize parent array
    for (int i = 0; i < n; i++)
        parent[i] = i;

    cout << "\nEdges in the Minimum Spanning Tree (Kruskal's):\n";
    while (edgeCount < n - 1) {
        int a = -1, b = -1, min = 999;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (findParent(i) != findParent(j) && cost[i][j] < min) {
                    min = cost[i][j];
                    a = i;
                    b = j;
                }
            }
        }

        unionSets(a, b);
        cout << a << " - " << b << " : " << min << endl;
        edgeCount++;
        minCost += min;
    }

    cout << "Total cost of MST (Kruskal's) = " << minCost << endl;
}

// Function to display cost adjacency matrix
void displayGraph() {
    cout << "\nCost Adjacency Matrix:\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++)
            cout << (cost[i][j] == 999 ? 0 : cost[i][j]) << " ";
        cout << endl;
    }
}

// Main function (menu-driven)
int main() {
    int choice;
    cout << "=== Minimum Spanning Tree Algorithms ===\n";
    cout << "Menu:\n";
    cout << "1. Create Graph (Cost Matrix)\n";
    cout << "2. Display Graph\n";
    cout << "3. Find MST using Prim's Algorithm\n";
    cout << "4. Find MST using Kruskal's Algorithm\n";
    cout << "5. Exit\n";

    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                createGraph();
                break;
            case 2:
                displayGraph();
                break;
            case 3:
                primsMST();
                break;
            case 4:
                kruskalsMST();
                break;
            case 5:
                cout << "Exiting program...\n";
                return 0;
            default:
                cout << "Invalid choice! Try again.\n";
        }
    }
}
```
```cpp
// DS practical 30
#include <bits/stdc++.h>
using namespace std;

#define MAX 20
#define INF 999

int n;                 // Number of vertices
int cost[MAX][MAX];    // Cost adjacency matrix
int dist[MAX];         // Shortest distance array
int visited[MAX];      // Visited array

// Function to create graph
void createGraph() {
    cout << "Enter number of vertices: ";
    cin >> n;
    cout << "Enter the cost adjacency matrix (" << n << "x" << n << "):\n";
    cout << "(Enter 0 if there is no edge between two vertices)\n";

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> cost[i][j];
            if (cost[i][j] == 0 && i != j)
                cost[i][j] = INF; // No edge represented by INF
        }
    }
    cout << "Graph created successfully.\n";
}

// Function to display cost matrix
void displayGraph() {
    cout << "\nCost Adjacency Matrix:\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (cost[i][j] == INF)
                cout << "∞ ";
            else
                cout << cost[i][j] << " ";
        }
        cout << endl;
    }
}

// Dijkstra’s Algorithm
void dijkstra() {
    int src;
    cout << "Enter the source vertex (0 to " << n - 1 << "): ";
    cin >> src;

    for (int i = 0; i < n; i++) {
        dist[i] = cost[src][i];
        visited[i] = 0;
    }
    dist[src] = 0;
    visited[src] = 1;

    for (int count = 1; count < n - 1; count++) {
        int minDist = INF, u = -1;

        // Find unvisited vertex with minimum distance
        for (int i = 0; i < n; i++) {
            if (!visited[i] && dist[i] < minDist) {
                minDist = dist[i];
                u = i;
            }
        }

        if (u == -1) break; // All reachable vertices processed

        visited[u] = 1;

        // Update distances of adjacent vertices
        for (int v = 0; v < n; v++) {
            if (!visited[v] && cost[u][v] != INF && dist[u] + cost[u][v] < dist[v])
                dist[v] = dist[u] + cost[u][v];
        }
    }

    cout << "\nShortest distances from source vertex " << src << ":\n";
    for (int i = 0; i < n; i++) {
        cout << "To vertex " << i << " : ";
        if (dist[i] == INF)
            cout << "No path\n";
        else
            cout << dist[i] << endl;
    }
}

// Main function
int main() {
    int choice;
    cout << "=== Dijkstra’s Shortest Path Algorithm ===\n";
    cout << "Menu:\n";
    cout << "1. Create Graph (Cost Matrix)\n";
    cout << "2. Display Graph\n";
    cout << "3. Find Shortest Path using Dijkstra’s Algorithm\n";
    cout << "4. Exit\n";

    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                createGraph();
                break;
            case 2:
                displayGraph();
                break;
            case 3:
                dijkstra();
                break;
            case 4:
                cout << "Exiting program...\n";
                return 0;
            default:
                cout << "Invalid choice! Try again.\n";
        }
    }
}
```
```cpp
// DS practical 31
#include <bits/stdc++.h>
using namespace std;

#define MAX 10   // Size of hash table
int hashTable[MAX];

// Function to initialize hash table
void initialize() {
    for (int i = 0; i < MAX; i++)
        hashTable[i] = -1; // -1 means empty slot
    cout << "Hash Table initialized successfully.\n";
}

// Hash function
int hashFunction(int key) {
    return key % MAX;
}

// Function to insert key using Linear Probing
void insertElement() {
    int key;
    cout << "Enter key to insert: ";
    cin >> key;

    int index = hashFunction(key);
    int originalIndex = index;
    int flag = 0;

    while (hashTable[index] != -1) {
        index = (index + 1) % MAX;
        if (index == originalIndex) {
            flag = 1;
            break;
        }
    }

    if (flag)
        cout << "Hash Table is full! Cannot insert key.\n";
    else {
        hashTable[index] = key;
        cout << "Key " << key << " inserted at index " << index << ".\n";
    }
}

// Function to search for a key
void searchElement() {
    int key;
    cout << "Enter key to search: ";
    cin >> key;

    int index = hashFunction(key);
    int originalIndex = index;
    int flag = 0;

    while (hashTable[index] != -1) {
        if (hashTable[index] == key) {
            cout << "Key " << key << " found at index " << index << ".\n";
            flag = 1;
            break;
        }
        index = (index + 1) % MAX;
        if (index == originalIndex)
            break;
    }

    if (!flag)
        cout << "Key " << key << " not found in the hash table.\n";
}

// Function to delete a key
void deleteElement() {
    int key;
    cout << "Enter key to delete: ";
    cin >> key;

    int index = hashFunction(key);
    int originalIndex = index;
    int flag = 0;

    while (hashTable[index] != -1) {
        if (hashTable[index] == key) {
            hashTable[index] = -1;
            cout << "Key " << key << " deleted from index " << index << ".\n";
            flag = 1;
            break;
        }
        index = (index + 1) % MAX;
        if (index == originalIndex)
            break;
    }

    if (!flag)
        cout << "Key " << key << " not found.\n";
}

// Function to display the hash table
void display() {
    cout << "\n--- Hash Table Status ---\n";
    for (int i = 0; i < MAX; i++) {
        cout << "Index " << i << " : ";
        if (hashTable[i] == -1)
            cout << "Empty\n";
        else
            cout << hashTable[i] << endl;
    }
}

// Main function
int main() {
    int choice;
    initialize();

    cout << "\n=== Hash Table Implementation (Linear Probing) ===\n";
    cout << "Menu:\n";
    cout << "1. Insert an Element\n";
    cout << "2. Search for an Element\n";
    cout << "3. Delete an Element\n";
    cout << "4. Display Hash Table\n";
    cout << "5. Exit\n";

    while (true) {
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: insertElement(); break;
            case 2: searchElement(); break;
            case 3: deleteElement(); break;
            case 4: display(); break;
            case 5: cout << "Exiting program...\n"; return 0;
            default: cout << "Invalid choice! Try again.\n";
        }
    }
}
```

