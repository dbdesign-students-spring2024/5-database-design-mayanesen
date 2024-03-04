# Data Normalization and Entity-Relationship Diagramming

#### by Maya Nesen

An assignment to normalize the structure of data and establish a set of Entity-Relationship Diagrams for the data.

## Tables

### Original Table: Why is it not 4NF Compliant?

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

This table in first normal form is very disorganized in terms of its content. While it has a fixed schema and only singular values (satisfying 1NF), below are the reasons why it does not satisfy second, third, and fourth normal form:

- Second Normal Form: Second normal form only works with composite keys. So, suppose we take `assignment_id` and `student_id` as our composite key. Then we have non-key fields, such as `classroom` or `assignment_topic`, which are not dependent on both or either key. So, the table violates 2NF.
- Third Normal Form: Non-key fields in 3NF must depend entirely on the primary/composite key. Once again, if we take `assignment_id` and `student_id` as our composite key, then we have several non-key fields which do not depend entirely on both id's.
- Fourth Normal Form: This table does not satisfy 2NF nor 3NF, which are minimum requirements for 4NF. Also, for 4NF, records must not contain more than one independent multi-valued fact about an entity. We see this issue for example in `professor`, `classroom`, `professor_email`. Both the classroom and the email depend on professor, but the classroom and the email of the professor are independent from each other.

To put this table in to fourth normal form, we must split it up into several tables. All of these tables are 4NF compliant because the non-key fields are entirely dependent on the composite key, so there are not more than one independent multi-values facts for each entity. In tables which contain several non-key fields, they are not independent, they both rely fully on the key fields.

### Table 1: Courses

This table contains the `course_id` (primary key) and its `course_name`. The course name depends entirely on what the course is.

| course_id | course_name                      |
| :-------- | :------------------------------- |
| CS-UA 101 | Intro to Computer Science        |
| CS-UA 102 | Data Structures                  |
| DS-UA 121 | Principles of Data Science       |
| DS-UA 122 | Causal Inference                 |
| CS-UA 340 | Fundamentals of Machine Learning |
| ...       | ...                              |

### Table 2: Course Sections

This table contains the composite key of `course_id`, `section_id`, and `prof_id`, and their corresponding `classroom`. The classroom depends fully on the course, its section, and the professor who teaches it.

| course_id | section_id | prof_id | classroom |
| :-------- | :--------- | :------ | :-------- |
| CS-UA 101 | 001        | 1       | WWH 101   |
| CS-UA 101 | 002        | 4       | PAUL 258  |
| CS-UA 101 | 003        | 4       | SILV 408  |
| CS-UA 102 | 002        | 2       | 60FA 314  |
| DS-UA 121 | 001        | 3       | WWH 201   |
| ...       | ...        | ...     | ...       |

### Table 3: Professors

Here, `prof_id` is the primary key, and `professor` and `professor_email` are non-key fields. The professor's name and email can be entirely retreived from the id.

| prof_id | professor | professor_email   |
| :------ | :-------- | :---------------- |
| 1       | Melvin    | l.melvin@foo.edu  |
| 2       | Logston   | e.logston@foo.edu |
| 3       | Nevarez   | i.nevarez@foo.edu |
| 4       | Smith     | j.smith@foo.edu   |
| 5       | Katz      | s.katz@foo.edu    |
| ...     | ...       | ...               |

### Table 4: Students

Here, we have student information. `student_id` is the primary key. The student's full name and email are entirely dependent on the id.

| student_id | first_name | last_name | email         |
| :--------- | :--------- | :-------- | :------------ |
| 1          | Emily      | Jones     | ej234@foo.edu |
| 2          | Bob        | Rooney    | br456@foo.edu |
| 3          | Sam        | Grey      | sg672@foo.edu |
| 4          | Joanna     | Larson    | jl904@foo.edu |
| 5          | Sara       | Crimson   | sc897@foo.edu |
| ...        | ...        | ...       | ...           |

### Table 5: Assignments by Section

`course_id`, `section_id`, and `assignment_id` form a composite key. The due_date is dependent on each of these things, since one same assignment can have a different due date depending on a course's specific section.

| course_id | section_id | assignment_id | due_date |
| :-------- | :--------- | :------------ | :------- |
| CS-UA 101 | 001        | 1             | 23.02.21 |
| CS-UA 101 | 001        | 2             | 18.11.21 |
| CS-UA 101 | 002        | 1             | 23.02.25 |
| DS-UA 122 | 003        | 3             | 05.05.21 |
| DS-UA 122 | 002        | 5             | 03.24.21 |
| CS-UA 340 | 002        | 4             | 04.07.21 |
| ...       | ...        | ...           | ...      |

### Table 6: Assignment Information

`assignment_id` is the primary key, and `assignment_topic` and `relevant_readings` depend uniquely on the specific assignment.

| assignment_id | assignment_topic                | relevant_readings   |
| :------------ | :------------------------------ | ------------------- |
| 1             | Data normalization              | Deumlich Chapter 3  |
| 2             | Single table queries            | Dümmlers Chapter 11 |
| 3             | Python and pandas               | Dümmlers Chapter 14 |
| 4             | Spreadsheet aggregate functions | Zehnder Page 87     |
| 5             | Logistic Regression             | Herstein Page 40    |
| ...           | ...                             | ...                 |

### Table 7: Grades

This holds the grades of students based on the `assignment_id` and `student_id`. But each student is also in a course and its specific section, and assignments vary based on these course sections.
Grades depend of course on the student, the assignment, and each assignment's course and section.

| grade_id | course_id | section_id | assignment_id | student_id | grade |
| :------- | :-------- | :--------- | ------------- | ---------- | ----- |
| 1        | CS-UA 101 | 001        | 1             | 1          | 80    |
| 2        | CS-UA 101 | 002        | 2             | 7          | 25    |
| 3        | CS-UA 101 | 003        | 1             | 4          | 75    |
| 4        | CS-UA 102 | 002        | 3             | 2          | 92    |
| 5        | DS-UA 121 | 001        | 4             | 2          | 65    |
| ...      | ...       | ...        | ...           | ...        | ...   |

## Entity-Relationship Diagram

Now that the tables satisfy fourth normal form, below is the Entity-Relationship Diagram (ERD).

![Entity-Relationship Diagram](https://github.com/dbdesign-students-spring2024/5-database-design-mayanesen/blob/main/erd-hw5.drawio.png)

Entities and their Attributes:

- Course: course_id (primary key), course_name
- Section: section_id (primary key), classroom
- Professor: prof_id (primary key), professor, professor_email
- Students: student_id (primary key), first_name, last_name, email
- Assignments: assignment_id (primary key), assignment_topic, relevant_reading
- Grades: grade_id (primary key), grade

Relationships (all of these are many-to-many relationships):

- courses (taught by) professors
- professors (teaches) sections
- courses (have) sections
- sections (assign) assignments
- students (completes) assignments
- students (receive) grades
- assignments (give) grades
