# **Project Introduction: Student Grade Management System**


## **1. Objective**

The objective of this project is to design a **console-based application** that allows a teacher or school administrator to efficiently manage student and teacher data. The system focuses on **OOP concepts**, such as **inheritance, encapsulation, abstraction, and polymorphism**, while also implementing **file handling** for data persistence.

Specifically, the system allows:

* Entering and storing **student names, roll numbers, and marks** for multiple subjects.
* Automatic calculation of **total marks, average percentage, and grades** based on predefined criteria.
* Viewing **individual student report cards** or a **list of all students**.
* Editing student marks and updating grades dynamically.
* Adding and viewing **teacher records**, including subject and salary.
* Persistent storage of data between program executions using **text files**.

## **2. Problem Statement**

Managing student grades and teacher data manually can be time-consuming, error-prone, and inefficient, especially in institutions with many students. Teachers need a **reliable, automated system** to:

* Store and retrieve data
* Calculate grades automatically
* Generate report cards
* Update records without affecting other data

This project provides a **simple, yet effective solution** using **C++ and Object-Oriented Programming** principles.

## **3. Technologies and Concepts Used**

1. **Programming Language:** C++
2. **OOP Concepts:**

   * **Encapsulation** – keeping data private and exposing only necessary operations
   * **Inheritance** – `Student` and `Teacher` classes inherit from a common `Person` base class
   * **Polymorphism** – `showDetails()` function behaves differently for students and teachers
   * **Abstraction** – users interact with functions like `addStudent()`, `editStudent()` without knowing the underlying implementation
3. **File Handling:** Using text files to save and retrieve student and teacher data between sessions
4. **Data Structures:** Arrays for storing marks; no STL vectors used (simplicity and OOP focus)
5. **Console-Based UI:** Simple text menus for user interaction

## **4. Features**

* **Student Management:**

  * Add new students
  * View all students in a formatted table
  * Search for a student by ID
  * Edit student marks → automatic recalculation of total, average, and grade
  * Generate detailed report cards

* **Teacher Management:**

  * Add new teachers
  * View all teachers

* **Polymorphism Demonstration:**

  * Same function `showDetails()` displays different outputs for students and teachers

* **Data Persistence:**

  * Student and teacher data stored in `students.txt` and `teachers.txt`
  * Data persists between program runs

## **5. Benefits of the System**

* **Time-saving:** Automatic grade calculation and report generation
* **Accuracy:** Reduces manual errors in grade computation
* **Reusability:** OOP design allows easy extension of the system (more subjects, new user types)
* **User-Friendly:** Clear menus and formatted display
* **Professional Design:** Demonstrates all major OOP concepts for academic purposes
