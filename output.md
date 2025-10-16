# PROGRAM:
```cpp
#include <iostream>
#include <fstream>
#include <iomanip>
#include <string>
#include <sstream>
using namespace std;
// =============================
// Base Class: Person
// =============================
class Person {
protected:
    int id;
    string name;
public:
    Person(int i = 0, string n = "") : id(i), name(n) {}
    virtual void input() {
        cout << "Enter ID: ";
        cin >> id;
        cout << "Enter Name: ";
        cin >> name;
    }
    virtual void display() const {
        cout << left << setw(10) << id << setw(15) << name;
    }
    virtual void showDetails() const = 0; // pure virtual → ensures polymorphism
    int getID() const { return id; }
    string getName() const { return name; }
    virtual ~Person() {}
};
// =============================
// Derived Class: Student
// =============================
class Student : public Person {
private:
    float marks[3];
    float total;
    float average;
    char grade;
    void calculateGrade() {
        total = marks[0] + marks[1] + marks[2];
        average = total / 3;
        if (average >= 90) grade = 'A';
        else if (average >= 75) grade = 'B';
        else if (average >= 60) grade = 'C';
        else if (average >= 45) grade = 'D';
        else grade = 'F';
    }
public:
    Student() : Person(), total(0), average(0), grade('F') {
        for (float &m : marks) m = 0;
    }
    void input() override {
        Person::input();
        cout << "Enter marks for 3 subjects: ";
        for (float &m : marks) cin >> m;
        calculateGrade();
    }
    void display() const override {
        Person::display();
        cout << setw(10) << total
             << setw(10) << fixed << setprecision(2) << average
             << setw(5) << grade << endl;
    }
    void showDetails() const override {
        cout << "\n--- Student Report ---\n";
        cout << "ID: " << id << "\nName: " << name << endl;
        for (int i = 0; i < 3; i++)
            cout << "Subject " << i + 1 << ": " << marks[i] << endl;
        cout << "Total: " << total
             << "\nAverage: " << average
             << "\nGrade: " << grade << endl;
    }
    void editMarks() {
        cout << "Enter new marks for 3 subjects: ";
        for (float &m : marks) cin >> m;
        calculateGrade();
    }
    void save(ofstream &out) const {
        out << id << "," << name << ","
            << marks[0] << "," << marks[1] << "," << marks[2] << "\n";
    }
    bool load(const string &line) {
        stringstream ss(line);
        string token;
        getline(ss, token, ','); id = stoi(token);
        getline(ss, name, ',');
        for (int i = 0; i < 3; i++) {
            getline(ss, token, ',');
            marks[i] = stof(token);
        }
        calculateGrade();
        return true;
    }
};
// =============================
// Derived Class: Teacher
// =============================
class Teacher : public Person {
private:
    string subject;
    float salary;

public:
    Teacher() : Person(), subject(""), salary(0) {}

    void input() override {
        Person::input();
        cout << "Enter Subject: ";
        cin >> subject;
        cout << "Enter Salary: ";
        cin >> salary;
    }
    void display() const override {
        Person::display();
        cout << setw(15) << subject << setw(10) << salary << endl;
    }
    void showDetails() const override {
        cout << "\n--- Teacher Details ---\n";
        cout << "ID: " << id << "\nName: " << name
             << "\nSubject: " << subject
             << "\nSalary: " << salary << endl;
    }
    void save(ofstream &out) const {
        out << id << "," << name << "," << subject << "," << salary << "\n";
    }
    bool load(const string &line) {
        stringstream ss(line);
        string token;
        getline(ss, token, ','); id = stoi(token);
        getline(ss, name, ',');
        getline(ss, subject, ',');
        getline(ss, token, ','); salary = stof(token);
        return true;
    }
};
// =============================
// Helper Functions
// =============================
bool fileExists(const string &filename) {
    ifstream fin(filename);
    return fin.good();
}
void pause() {
    cout << "\nPress Enter to continue...";
    cin.ignore();
    cin.get();
}
// =============================
// Student Functions
// =============================
void addStudent() {
    Student s;
    s.input();
    ofstream fout("students.txt", ios::app);
    s.save(fout);
    fout.close();
    cout << "Student added successfully!\n";
}
void showAllStudents() {
    if (!fileExists("students.txt")) { cout << "No student data found!\n"; return; }
    ifstream fin("students.txt");
    Student s; string line;

    cout << "\n--- All Students ---\n";
    cout << left << setw(10) << "ID" << setw(15) << "Name"
         << setw(10) << "Total" << setw(10) << "Avg" << setw(5) << "Grade" << endl;

    while (getline(fin, line)) if (!line.empty() && s.load(line)) s.display();
}
void findStudent() {
    if (!fileExists("students.txt")) { cout << "No student data found!\n"; return; }
    int id; cout << "Enter Student ID: "; cin >> id;

    ifstream fin("students.txt");
    Student s; string line; bool found = false;

    while (getline(fin, line)) { s.load(line); if (s.getID() == id) { s.showDetails(); found = true; break; } }
    if (!found) cout << "Student not found!\n";
}
void editStudent() {
    if (!fileExists("students.txt")) { cout << "No student data found!\n"; return; }
    int id; cout << "Enter Student ID to edit: "; cin >> id;

    ifstream fin("students.txt");
    ofstream fout("temp.txt");
    Student s; string line; bool found = false;

    while (getline(fin, line)) { s.load(line); if (s.getID() == id) { s.editMarks(); found = true; } s.save(fout); }

    fin.close(); fout.close();
    remove("students.txt"); rename("temp.txt", "students.txt");

    if (found) cout << "Record updated successfully!\n"; else cout << "Student not found!\n";
}
// =============================
// Teacher Functions
// =============================
void addTeacher() {
    Teacher t; t.input();
    ofstream fout("teachers.txt", ios::app); t.save(fout); fout.close();
    cout << "Teacher added successfully!\n";
}
void showAllTeachers() {
    if (!fileExists("teachers.txt")) { cout << "No teacher data found!\n"; return; }
    ifstream fin("teachers.txt"); Teacher t; string line;

    cout << "\n--- All Teachers ---\n";
    cout << left << setw(10) << "ID" << setw(15) << "Name"
         << setw(15) << "Subject" << setw(10) << "Salary" << endl;

    while (getline(fin, line)) if (!line.empty() && t.load(line)) t.display();
}
// =============================
// Main Menu
// =============================
int main() {
    int choice;
    while (true) {
        cout << "\n===== Student Grade Management System =====\n";
        cout << "1. Add Student\n2. View All Students\n3. View Individual Student Report\n4. Edit Student Record\n";
        cout << "5. Add Teacher\n6. View All Teachers\n7. Exit\nEnter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addStudent(); break;
            case 2: showAllStudents(); break;
            case 3: findStudent(); break;
            case 4: editStudent(); break;
            case 5: addTeacher(); break;
            case 6: showAllTeachers(); break;
            case 7: cout << "Exiting...\n"; return 0;
            default: cout << "Invalid choice!\n";
        }
        pause();
    }
}
```
# OUTPUT:
```
===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 1

Enter ID: 101
Enter Name: Rahul
Enter marks for 3 subjects: 85 90 80
Student added successfully!

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 5

Enter ID: 201
Enter Name: Meena
Enter Subject: Physics
Enter Salary: 55000
Teacher added successfully!

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 2

--- All Students ---
ID        Name           Total     Avg       Grade
101       Rahul          255       85.00     B

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 3

Enter Student ID: 101

--- Student Report ---
ID: 101
Name: Rahul
Subject 1: 85
Subject 2: 90
Subject 3: 80
Total: 255
Average: 85
Grade: B

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 6

--- All Teachers ---
ID        Name           Subject        Salary    
201       Meena          Physics        55000    

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 4

Enter Student ID to edit: 101
Enter new marks for 3 subjects: 90 92 88
Record updated successfully!

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 3

Enter Student ID: 101

--- Student Report ---
ID: 101
Name: Rahul
Subject 1: 90
Subject 2: 92
Subject 3: 88
Total: 270
Average: 90
Grade: A

Press Enter to continue...

===== Student Grade Management System =====
1. Add Student
2. View All Students
3. View Individual Student Report
4. Edit Student Record
5. Add Teacher
6. View All Teachers
7. Exit
Enter your choice: 7

Exiting...
```