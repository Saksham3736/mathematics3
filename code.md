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
        cout << "Enter your choice: ";
        cin >> choice;
		while(true){
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