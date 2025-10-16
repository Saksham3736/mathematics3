# **Student Grade Management System – Complete Workflow**

## **1️⃣ Program Structure Overview**

The project is built using **Object-Oriented Programming** (OOP) concepts:

```
                 Person (Base Class)
                /                   \
          Student (Derived)       Teacher (Derived)
```

### Key Features

1. Add / View / Edit **Student records**
2. Add / View **Teacher records**
3. **Report cards** with total, average, and grade
4. **File handling** for persistence
5. **Polymorphism** via `showDetails()`

## **2️⃣ Classes and Their Responsibilities**

### **2.1 Person (Base Class)**

* **Data Members**: `id`, `name`
* **Methods**:

  * `input()` – enter basic details
  * `display()` – display basic info in a table
  * `showDetails()` – pure virtual function (demonstrates **polymorphism**)
* **Purpose**: Encapsulates common attributes for both students and teachers


### **2.2 Student (Derived Class)**

* **Data Members**:

  * `marks[3]`, `total`, `average`, `grade`

* **Methods**:

  * `input()` – enter marks and calculate grade
  * `display()` – display summary in table
  * `showDetails()` – detailed report card (polymorphic)
  * `editMarks()` – modify marks and recalculate grade
  * `save()` / `load()` – file operations (CSV format)

* **Workflow**:

  1. Teacher adds a student → data entered → grade calculated automatically
  2. Student data is saved to `students.txt`
  3. Student can be viewed individually or as a full list
  4. Editing automatically updates the file and recalculates grade


### **2.3 Teacher (Derived Class)**

* **Data Members**:

  * `subject`, `salary`

* **Methods**:

  * `input()` – enter teacher details
  * `display()` – summary in table
  * `showDetails()` – detailed info (polymorphic)
  * `save()` / `load()` – file operations

* **Workflow**:

  1. Teacher added → data stored in `teachers.txt`
  2. Can be displayed individually or as a full list


## **3️⃣ File Handling Workflow**

### **3.1 Student File – `students.txt`**

* **Format (CSV)**:

```
ID,Name,Mark1,Mark2,Mark3
101,Rahul,85,90,80
102,Riya,78,88,92
```

* **Workflow**:

  1. **Add** → append to file
  2. **View all** → read file line by line, parse CSV, display table
  3. **Edit** → read each line, modify if ID matches, write to temp file, replace original


### **3.2 Teacher File – `teachers.txt`**

* **Format (CSV)**:

```
ID,Name,Subject,Salary
201,Meena,Physics,55000
202,Ankit,Chemistry,50000
```

* **Workflow**: Similar to students, but no editing required


## **4️⃣ Grade Calculation Workflow (Student)**

```cpp
total = marks[0] + marks[1] + marks[2];
average = total / 3.0;
if (average >= 90) grade = 'A';
else if (average >= 75) grade = 'B';
else if (average >= 60) grade = 'C';
else if (average >= 45) grade = 'D';
else grade = 'F';
```

* Automatically happens during **input()** or **editMarks()**
* Ensures grade is always correct when marks are updated


## **5️⃣ Polymorphism Workflow**

* **Person** has `virtual void showDetails() = 0` → makes Person **abstract**
* **Student** overrides it → shows detailed report card
* **Teacher** overrides it → shows detailed teacher info

**Example:**

```cpp
Person* p;
Student s; Teacher t;

p = &s;
p->showDetails(); // calls Student version

p = &t;
p->showDetails(); // calls Teacher version
```

✅ Same function call behaves differently depending on object type → **runtime polymorphism**


## **6️⃣ Menu System Workflow**

1. **Main Menu**:

   ```
   1. Add Student
   2. View All Students
   3. View Individual Student Report
   4. Edit Student Record
   5. Add Teacher
   6. View All Teachers
   7. Exit
   ```

2. **User Choice Handling**:

   * 1 → `addStudent()` → input → save to file
   * 2 → `showAllStudents()` → read file → display table
   * 3 → `findStudent()` → search by ID → call `showDetails()`
   * 4 → `editStudent()` → search by ID → modify marks → save
   * 5 → `addTeacher()` → input → save to file
   * 6 → `showAllTeachers()` → read file → display table
   * 7 → Exit program

3. **Looping**:

   * `while(true)` keeps menu active until user exits

4. **Pause Between Actions**:

   * `pause()` function lets user read output before returning to menu


## **7️⃣ Sample Workflow Example**

**Scenario: Teacher Adds a Student and a Teacher**

1. User selects **Add Student**

   * Inputs ID: 101, Name: Rahul, Marks: 85 90 80
   * System calculates total = 255, average = 85, grade = B
   * Data saved in `students.txt`

2. User selects **Add Teacher**

   * Inputs ID: 201, Name: Meena, Subject: Physics, Salary: 55000
   * Data saved in `teachers.txt`

3. User selects **View All Students**

   ```
   ID        Name           Total     Avg       Grade
   101       Rahul          255       85.00     B
   ```

4. User selects **View Individual Student**

   ```
   --- Student Report ---
   ID: 101
   Name: Rahul
   Subject 1: 85
   Subject 2: 90
   Subject 3: 80
   Total: 255
   Average: 85
   Grade: B
   ```

5. User selects **Edit Student Record**

   * Updates marks to 90 92 88
   * Total recalculated → 270
   * Average → 90
   * Grade updated → A

6. **Polymorphism Example**

   * Call `showDetails()` on a `Person*` pointing to a student → shows detailed report card
   * Call `showDetails()` on a `Person*` pointing to a teacher → shows teacher details


## **8️⃣ Program Flow Diagram (Textual)**

```
[Start] → [Display Menu]
    ↓
[User Choice]
    ├─ 1 → addStudent() → input() → calculateGrade() → save()
    ├─ 2 → showAllStudents() → read file → display()
    ├─ 3 → findStudent() → read file → if ID match → showDetails()
    ├─ 4 → editStudent() → read file → editMarks() → save()
    ├─ 5 → addTeacher() → input() → save()
    ├─ 6 → showAllTeachers() → read file → display()
    └─ 7 → Exit
```


## **9️⃣ Key OOP Concepts Highlighted in Workflow**

| Concept       | Where it Appears                            |
| ------------- | ------------------------------------------- |
| Encapsulation | Private members (`marks`, `salary`)         |
| Inheritance   | `Student` & `Teacher` inherit `Person`      |
| Polymorphism  | `showDetails()` behaves differently         |
| Abstraction   | Users interact via input/display functions  |
| File Handling | `save()` & `load()` methods for persistence |
| Modularity    | Each functionality is a separate function   |

## ✅ Summary

Your **workflow covers everything**:

1. Adding, editing, and viewing **students** and **teachers**
2. **Grade calculation & report card generation**
3. **File persistence** for saving/loading records
4. **Polymorphism demonstration**
5. **Menu-based interaction**

