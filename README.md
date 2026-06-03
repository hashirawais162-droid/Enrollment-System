# Enrollment-System
# Student Enrollment System (Java OOP + Queue)

## Problem

The problem that we want AI to write a solution to is creating a student enrollment system for a course with limited capacity.

When a course is full, additional students should be placed in a waiting list. The system should automatically move students from the waitlist into the course when space becomes available.

This program will use Object-Oriented Programming (OOP) and a Queue data structure to simulate this real-world scenario.
import java.util.*;

import java.util.*;

// Student class
class Student {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    // Improves printing
    @Override
    public String toString() {
        return name;
    }
}

// Course class
class Course {
    private String courseName;
    private int capacity;
    private List<Student> enrolledStudents;
    private Queue<Student> waitlist;

    public Course(String courseName, int capacity) {
        this.courseName = courseName;
        this.capacity = capacity;
        enrolledStudents = new ArrayList<>();
        waitlist = new LinkedList<>();
    }

    // Enroll student
    public void enroll(Student student) {
        if (enrolledStudents.size() < capacity) {
            enrolledStudents.add(student);
            System.out.println(student.getName() + " enrolled in " + courseName);
        } else {
            waitlist.add(student);
            System.out.println(student.getName() + " added to waitlist for " + courseName);
        }
    }

    //  FIXED: Drop student by name (not index)
    public void dropStudent(String name) {
        Iterator<Student> it = enrolledStudents.iterator();

        while (it.hasNext()) {
            Student s = it.next();
            if (s.getName().equalsIgnoreCase(name)) {
                it.remove();
                System.out.println(name + " dropped from " + courseName);

                // Move from waitlist
                if (!waitlist.isEmpty()) {
                    Student next = waitlist.poll();
                    enrolledStudents.add(next);
                    System.out.println(next.getName() + " moved from waitlist to enrolled");
                }
                return;
            }
        }

        // Check waitlist
        Iterator<Student> waitIt = waitlist.iterator();
        while (waitIt.hasNext()) {
            Student s = waitIt.next();
            if (s.getName().equalsIgnoreCase(name)) {
                waitIt.remove();
                System.out.println(name + " removed from waitlist");
                return;
            }
        }

        System.out.println("Student not found.");
    }

    //  Improved display formatting
    public void displayStudents() {
        System.out.println("\n====================================");
        System.out.println("        COURSE DETAILS");
        System.out.println("====================================");
        System.out.println("Course Name : " + courseName);
        System.out.println("Capacity    : " + capacity);
        System.out.println("Enrolled    : " + enrolledStudents.size());
        System.out.println("Waitlisted  : " + waitlist.size());

        System.out.println("\n----------- ENROLLED STUDENTS -----------");
        if (enrolledStudents.isEmpty()) {
            System.out.println("No students enrolled.");
        } else {
            int i = 1;
            for (Student s : enrolledStudents) {
                System.out.printf("%-3d | %-20s\n", i++, s);
            }
        }

        System.out.println("\n----------- WAITLISTED STUDENTS ----------");
        if (waitlist.isEmpty()) {
            System.out.println("No students in waitlist.");
        } else {
            int i = 1;
            for (Student s : waitlist) {
                System.out.printf("%-3d | %-20s\n", i++, s);
            }
        }

        System.out.println("====================================\n");
    }
}

// Main class
public class EnrollmentSystem {
    public static void main(String[] args) {

        Course course = new Course("Math 101", 2);

        // Test data
        course.enroll(new Student("Alice"));
        course.enroll(new Student("Bob"));
        course.enroll(new Student("Charlie"));
        course.enroll(new Student("David"));

        course.displayStudents();

        System.out.println("\nDropping Alice...\n");
        course.dropStudent("Alice");

        course.displayStudents();
    }
}

Alice enrolled in Math 101
Bob enrolled in Math 101
Charlie added to waitlist for Math 101
David added to waitlist for Math 101

====================================
        COURSE DETAILS
====================================
Course Name : Math 101
Capacity    : 2
Enrolled    : 2
Waitlisted  : 2

----------- ENROLLED STUDENTS -----------
1   | Alice               
2   | Bob                 

----------- WAITLISTED STUDENTS ----------
1   | Charlie             
2   | David               
====================================


Dropping Alice...

Alice dropped from Math 101
Charlie moved from waitlist to enrolled

====================================
        COURSE DETAILS
====================================
Course Name : Math 101
Capacity    : 2
Enrolled    : 2
Waitlisted  : 1

----------- ENROLLED STUDENTS -----------
1   | Bob                 
2   | Charlie             

----------- WAITLISTED STUDENTS ----------
1   | David               
====================================


 ----jGRASP: Operation complete.
