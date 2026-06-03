# Enrollment-System
# Student Enrollment System (Java OOP + Queue)

## Problem

The problem that we want AI to write a solution to is creating a student enrollment system for a course with limited capacity.

When a course is full, additional students should be placed in a waiting list. The system should automatically move students from the waitlist into the course when space becomes available.

This program will use Object-Oriented Programming (OOP) and a Queue data structure to simulate this real-world scenario.
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

    // Drop student
   public void dropStudent(String name) {
    // Try to remove from enrolled students
    Iterator<Student> it = enrolledStudents.iterator();

    while (it.hasNext()) {
        Student s = it.next();
        if (s.getName().equalsIgnoreCase(name)) {
            it.remove();
            System.out.println(name + " dropped from " + courseName);

            // Move next waitlisted student into enrolled
            if (!waitlist.isEmpty()) {
                Student next = waitlist.poll();
                enrolledStudents.add(next);
                System.out.println(next.getName() + " moved from waitlist to enrolled");
            }
            return;
        }
    }

    // If not found in enrolled, check waitlist
    Iterator<Student> waitIt = waitlist.iterator();
    while (waitIt.hasNext()) {
        Student s = waitIt.next();
        if (s.getName().equalsIgnoreCase(name)) {
            waitIt.remove();
            System.out.println(name + " removed from waitlist");
            return;
        }
    }

    // If student not found anywhere
    System.out.println("Student not found.");
}
public void displayStudents() {
    System.out.println("\n=================================");
    System.out.println("        COURSE DETAILS");
    System.out.println("=================================");
    System.out.println("Course: " + courseName);
    System.out.println("Capacity: " + enrolledStudents.size() + "/" + capacity);

    System.out.println("\n--- Enrolled Students ---");
    if (enrolledStudents.isEmpty()) {
        System.out.println("No students enrolled.");
    } else {
        int i = 1;
        for (Student s : enrolledStudents) {
            System.out.println(i + ". " + s.getName());
            i++;
        }
    }

    System.out.println("\n--- Waitlisted Students ---");
    if (waitlist.isEmpty()) {
        System.out.println("No students in waitlist.");
    } else {
        int i = 1;
        for (Student s : waitlist) {
            System.out.println(i + ". " + s.getName());
            i++;
        }
    }

    System.out.println("=================================\n");
}
// Main class
public class EnrollmentSystem {
    public static void main(String[] args) {

        Course course = new Course("Math 101", 2);

        Student s1 = new Student("Alice");
        Student s2 = new Student("Bob");
        Student s3 = new Student("Charlie");
        Student s4 = new Student("David");

        course.enroll(s1);
        course.enroll(s2);
        course.enroll(s3); // goes to waitlist
        course.enroll(s4); // goes to waitlist

        course.viewStudents();

        System.out.println("\nDropping a student...\n");
        course.dropStudent();

        course.displayStudents();
    }
}
