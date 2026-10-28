KEC R2024 Student Academic & Continuous Evaluation System (Java JDBC)
==========================================================================

REQUIREMENTS
------------
- JDK 17 or later
- MySQL Server 8.0 or compatible version
- MySQL Connector/J JAR
- VS Code with Java Extension Pack, or IntelliJ IDEA
- MySQL Workbench (optional, for viewing and managing the database)

TECHNOLOGIES USED
-----------------
- Java: application logic and menu-driven interface
- JDBC: connects the Java application to MySQL
- MySQL: stores the project data
- MySQL Workbench: helps view and manage the database
- MySQL Connector/J: JDBC driver used by Java

DATABASE
--------
Database name: kec_r2024

Tables:
1. students
   Stores roll number, student name, department, current semester, and
   record creation time.

2. courses
   Stores course code, title, credits, and course type:
   THEORY, PRACTICAL, or EMBEDDED.

3. enrollments
   Stores the student-course relationship, semester, classes held,
   classes attended, medical condonation approval, and creation time.

4. assessment_marks
   Stores CIA1, CIA2, assignment/quiz marks, lab evaluation, ESE marks,
   ESE absence status, and the last update time.

PROJECT FILES
-------------
- kec_schema.sql: SQL schema script for creating the database tables.
- src/DBConnection.java: manages the JDBC connection.
- src/Student.java: represents student details.
- src/Course.java: represents course details.
- src/Enrollment.java: represents enrollment details.
- src/AcademicOperations.java (or similarly named academic operations
  source file): contains academic operations.
- src/Main.java: starts the menu-driven Java application.
- lib/mysql-connector-j-26.10.0.jar: MySQL JDBC driver, if this is the
  exact JAR filename in your lib folder.
- .class files: compiled Java bytecode files.

SETUP
-----
1. Start the MySQL Server service.
2. Open MySQL Workbench or the MySQL command-line client.
3. Run kec_schema.sql to create the database and tables.
4. Select the database and verify the tables:

   USE kec_r2024;
   SHOW TABLES;

5. Open src/DBConnection.java.
6. Check the JDBC URL, username, and password. The URL should point to
   the database, for example:

   jdbc:mysql://localhost:3306/kec_r2024

7. Set the username and password to match your local MySQL account.
   Do not share your database password publicly.
8. Make sure the MySQL Connector/J JAR is inside the lib folder and is
   included in the Java classpath.

COMPILE/RUN (WINDOWS CMD)
-------------------------
Run commands from the project root folder. The visible source tree shows
Main.java, so these commands use Main as the entry point. If your source
uses a package declaration or the actual main class is different, adjust
the commands accordingly.

   javac -cp "lib\mysql-connector-j-26.10.0.jar" -d out src\*.java
   java -cp "out;lib\mysql-connector-j-26.10.0.jar" Main

Replace the JAR filename if it differs from the file in your lib folder.
In VS Code, you can also add the connector JAR under Java Projects >
Referenced Libraries.

FUNCTIONAL REQUIREMENTS
-----------------------
- Register students with roll number, name, department, and semester.
- Add courses with course code, title, credits, and course type.
- Enroll students into courses for a semester.
- Record classes held and classes attended.
- Enter CIA Test 1 and CIA Test 2 marks incrementally.
- Record assignment/quiz/lab marks where supported by the application.
- Enter ESE marks or mark a student absent for ESE.
- Check examination eligibility using attendance.
- Generate student academic information and reports supported by the
  application.

DEMO ORDER
----------
Use demo data only. First check that each roll number and course code
does not already exist.

1. Add Student:
   Roll Number: DEMOIT001
   Name: Demo Student
   Department: IT
   Semester: 3

2. Add Course:
   Course Code: DEMOIT31
   Title: Java Programming
   Credits: 4
   Course Type: THEORY

3. Enroll the student in the course for Semester 3.

4. Record Attendance:
   Classes Held: 40
   Classes Attended: 36
   Attendance: 90%

5. Enter assessment marks where the menu supports them:
   CIA1: 80
   CIA2: 85
   Assignment/Quiz: 90

6. Enter ESE marks: 75, or demonstrate the ESE absence option.

7. Open the academic report/mark sheet for DEMOIT001.

IMPORTANT: Follow the prompts shown by your actual application. The
database schema shared for this project has enrollment IDs, and your
application may ask for an enrollment ID instead of a roll number during
attendance or marks entry.

INCREMENTAL MARKS ENTRY
-----------------------
The project requirement is to support marks entry in phases as
assessments occur during the semester. The assessment_marks table allows
some marks to remain NULL until entered. Whether blank input retains a
previous value depends on the Java code, so verify this behavior in the
running application before demonstrating it.

ATTENDANCE RULES IN THE PROJECT SPECIFICATION
---------------------------------------------
- Attendance >= 75%: eligible for ESE.
- Attendance >= 65% and below 75%: requires approved medical condonation.
- Attendance < 65%: shortage of attendance (SA) and barred from ESE.

Attendance percentage:
   (Classes Attended / Classes Held) * 100

EVALUATION RULES IN THE PROJECT SPECIFICATION
---------------------------------------------
THEORY:
- CIA weightage: 40%
- ESE weightage: 60%
- ESE minimum threshold: 45% (27/60)
- Overall pass minimum: 50 marks

PRACTICAL:
- CIA weightage: 60%
- ESE weightage: 40%
- ESE minimum threshold: 50% (20/40)
- Overall pass minimum: 50 marks

EMBEDDED:
- CIA weightage: 50%
- ESE weightage: 50%
- ESE minimum threshold: 45% (22.5/50)
- Overall pass minimum: 50 marks

Grade bands in the supplied project specification:
- O: 91-100, grade point 10
- A+: 81-90, grade point 9
- A: 71-80, grade point 8
- B+: 61-70, grade point 7
- B: 50-60, grade point 6
- RA: re-appearance/fail if aggregate is below 50 or the ESE minimum
  is not met
- SA: shortage of attendance, grade point 0
- AB: absent in ESE, grade point 0

SGPA formula in the project specification:
   SGPA = Sum(Course Credits * Grade Points Earned)
          / Sum(Total Enrolled Credits)

Equivalent Percentage = SGPA * 10

IMPLEMENTATION NOTES
--------------------
- The database schema and records have been inspected using MySQL.
- Sample records previously shown include students itr01 and itr02,
  course DBMS01, and an enrollment with 10 classes held and 9 attended.
- The project specification defines the attendance, evaluation, grading,
  SGPA, and reporting rules. Verify the actual Java implementation and
  test outputs before claiming every calculation is complete.
- A previously shared console output stated that official grades and
  SGPA were not calculated in that version. Confirm the current source
  code before presenting grade sheets as fully implemented.
- The specification asks for a department-wise pass/fail summary and
  class average SGPA. Verify whether this report exists in your current
  code before describing it as completed.

EXPECTED REPORTS IN THE PROJECT SPECIFICATION
---------------------------------------------
- Student semester mark sheet with internal/external marks, total,
  letter grade, pass/fail status, SGPA, and equivalent percentage.
- Attendance defaulter report for students below 65% attendance.
- Department-wise pass/fail count and class average SGPA.

TROUBLESHOOTING
---------------
- Access denied for user 'root':
  Check that the username and password in DBConnection.java match your
  local MySQL account.

- 'mysql' is not recognized:
  Run mysql.exe using its full path, or add the MySQL Server bin folder
  to the Windows PATH.

- ClassNotFoundException:
  Run from the project root, confirm compilation succeeded, and check
  the class name and classpath.

- Duplicate entry for PRIMARY:
  The roll number or course code already exists. Use a unique demo value
  or view the existing record.

- No suitable driver:
  Check that the Connector/J JAR filename and classpath are correct.

CONCLUSION
----------
This project uses Java and JDBC to access a MySQL database for student
records, courses, enrollment, attendance, and assessment marks. It is
intended to support examination eligibility and academic reporting under
the supplied KEC R2024 project requirements. Test each feature and
calculation in the current application before the final presentation.
