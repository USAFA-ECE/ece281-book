# ECE 281: Digital Design and Computer Architecture

2026 Spring Syllabus

## Schedule

| Lesson | Topic                                    | Reading                  | Assigned     | Due                |
|--------|------------------------------------------|--------------------------|--------------|--------------------|
| 1      | Version Control                          |                          | HW 1         |                    |
| 2      | Logic Gates and Combinational Circuits   | 1.2, 1.5, 2.1            | HW 2         |                    |
| 3      | Boolean Equations and Algebra            | 2.2-2.3, ICE1            | HW 3         |                    |
| 4      | ICE1 - Breadboard Half Adder             |                          | HW 4         |                    |
| 5      | Multilevel Logic & VHDL                  | 2.4-2.6, ICE2, [VHDLoverview](https://usafa0.sharepoint.com/sites/ECE281/Class%20Materials/Reference/VHDLoverview.pdf?CT=1767475877405&OR=ItemsView&wdOrigin=TEAMSFILE.FILEBROWSER.DOCUMENTLIBRARY)            | HW 5         | ICE1               |
| 6      | ICE2 - VHDL Half Adder                   |                          | [HW 6](./HW/HW6.md) |             |
| 7      | Numbering Systems and Arithmetic         | 1.4                      | HW 7         | ICE2               |
| 8      | K-Maps                                   | 2.7, Lab 1               | Lab 1 Prelab |                    |
| 9      | Mux, Decoders, Combinational Timing      | 2.8-2.9                  | HW 9         |                    |
| 10     | Lab 1 - 31 Day Month                     |                          |              | Lab 1 Prelab       |
| 11     | Lab 1 - 31 Day Month                     |                          |              |                    |
| 12     | Arithmetic in Combinational Logic        | 5.1-5.2.3, 5.2.6-5.2.7, ICE3   | {ref}`ripple-adder-hw` |          |
| 13     | ICE3 - Ripple Adder                      |                          |              | Lab 1              |
| 14     | The Mighty ALU!                          | 5.2.4-5.2.5, Lab 2       | Lab 2 Prelab | ICE3               |
| 15     | GR #1                                    |                          |              |                    |
| 16     | Synchronous Circuits                     | 3.1-3.3                  | HW 16        |                    |
| 17     | Lab 2 - 7 Segment Display                |                          |              | Lab 2 Prelab       |
| 18     | Finite State Machine Intro               | 3.4.1-3.4.3              | HW 18        |                    |
| 19     | FSM Practice                             | 3.4.4-3.4.6, ICE4        | HW 19        |                    |
| 20     | ICE4 - Stoplight                         |                          | Lab 3 Prelab | Lab 2               |
| 21     | Memory Arrays                            | 5.4-5.5, Lab 3           | HW 21        | ICE4               |
| 22     | Lab 3 - T-bird Turn Signal               |                          |              | Lab 3 Prelab       |
| 23     | Lab 3 - T-bird Turn Signal               | ICE5                     |              |                    |
| 24     | ICE5 - Basic Elevator Controller         | ICE6                     | Lab 4 Prelab | Lab 3              |
| 25     | ICE6 - Time Division Multiplexing        | Lab 4                    |              | ICE5               |
| 26     | Lab 4 - Moore Elevator Controller        |                          |              | ICE6, Lab 4 Prelab |
| 27     | Lab 4 - Moore Elevator Controller        |                          | HW 27        |                    |
| 28     | GR #2                                    |                          |              |                    |
| 29     | RV32I Introduction                       | 6.1-6.2                  |              | Lab 4              |
| 30     | RV32I Registers, R-, I-Type Instructions | 6.4.1-6.4.2              | HW 30        |                    |
| 31     | RV32I U-, S-, B-Type Instructions        | 6.3.3, 6.4.3-6.4.5       |              |                    |
| 32     | RV32I Function Calls                     | 6.3.7-6.3.8              |              |                    |
| 33     | RV32I Memory Map                         | 6.5, 6.6.1               |              |                    |
| 34     | RV32I Single-Cycle Microarchitecture 1   | 7.1, 7.3-7.3.2           | Lab 5 Prelab |                    |
| 35     | RV32I Single-Cycle Microarchitecture 2   | 7.3.3-7.3.5, Lab 5       |              |                    |
| 36     | Lab 5 Prelab - CPU                       |                          |              |                    |
| 37     | Lab 5 - CPU                              |                          |              | Lab 5 Prelab       |
| 38     | Lab 5 - CPU                              | ICE7                     | HW 38        |                    |
| 39     | ICE7 - RV32I                             |                          |              |                    |
| 40     | Review                                   |                          |              | ICE7, Lab 5 Demo   |

Unless stated otherwise, assignments are submitted on [Gradescope](https://www.gradescope.com/). Check Gradescope for the due date/time.

## Course Information

### Instructors

#### [Lt Col Jason Wyche](https://www.usafa.edu/facultyprofile/?smid=80068)

- Course Director
- Office: 2F10

#### [Dr. George York](https://www.usafa.edu/facultyprofile/?smid=91372)

- Office: 2F6A

#### [Capt Donnelle January](https://www.usafa.edu/facultyprofile/?smid=91469)

- Office: 2E46B

### Course Goals

The goal of this course is for all cadets enrolled in the course to develop the ability to understand and design combinational and sequential circuits and construct, test, and debug these circuits using schematic diagrams and hardware description languages.  Cadets shall demonstrate an understanding of basic computer architecture and how a computer executes simple programs.

### Course Objectives

Cadets shall be able to:

- Demonstrate ability to design, analyze, and implement combinational and sequential circuits using schematic diagrams.
- Demonstrate ability to design, analyze, and implement combinational and sequential circuits using a hardware description language.
- Use contemporary software tools to debug a digital system design and verify that a digital system meets defined requirements.
- Demonstrate the ability to understand and analyze a microarchitecture (datapath and control unit) to implement a simple computer architecture.
- Demonstrate the ability to properly record and report laboratory work.

### Course Texts

*Digital Design and Computer Architecture: **RISC-V Edition*** by Sarah L. Harris and David Harris.

You **must** purchase an electronic or physical copy of this book. It will be one of the best textbooks of your career,
and you will continue to reference it in other courses.
**Sharing a digital copy is illegal and a violation of the honor code.**

- [Publisher link](https://shop.elsevier.com/books/digital-design-and-computer-architecture-risc-v-edition/harris/978-0-12-820064-3)
- [Supplemental materials](https://pages.hmc.edu/harris/ddca/ddcarv.html)

Readings should be completed *prior* to class.

### Policies

All course policies are in accordance with and subject to USAFA policies.

#### Collaboration & AI Policy

- For all assignments in this course, unless otherwise noted on the assignment, you may work with anyone. We expect all graded work, to include code, lab notebooks, and written reports, to be your own work. Copying another person’s work, with or without documentation, will result in NO academic credit.  Furthermore, copying without attribution is dishonorable and will be dealt with as an honor code violation.
- AI usage is limited to Level 3. Level 3 is defined as “Use of GenAI as a feedback tool on student-generated work. Cadets create their own work independently, then may use GenAI as a tool to get feedback about their draft, such as suggestions for improvement, clarification, alignment with the assignment instructions, or editing tips. Cadets are expected to make their own revisions based on this feedback; the submitted work should not include GenAI-generated text. This use of GenAI should be clearly communicated in their documentation statements.” This means you must write your own code but can ask an AI model for clarification, conceptual questions, and debugging help. You should not have the AI model write the solution for you and should put language in your prompt that asks the AI to not write the code for you. You must provide your instructor with a transcript of your conversation

#### Extra Instruction (EI)

Schedule EI with an instructor if you are having difficulty with the course material.  You must have read the assignment and attempted the homework before requesting EI.  You are responsible for material if you miss class. After you’ve read the assignment, attempted the homework, and checked with your classmates, you may then schedule EI if you have difficulty with the material. EI is not a remedial lecture.

#### Missing Class

 You must notify your instructor **via email** of any class absence as early as possible. The email should contain the following:  

- **SCA**
  - SCA number
  - Descriptive reason for the absence
  - Include date and lesson number  
  - Check your SCA to see if instructor “notification” or “permission” is required, and whether you are allowed to miss graded events.
- **Bed Rest**
  - Cc Squadron Commander on email

Whether excused or unexcused, you are still responsible for keeping up with all assignments.

## Grading

### Grade Weighting

```{mermaid}
pie title Prog Grade Weighting
    "GR 1" : 50
    "ICE/Labs" : 30
    "Lab Reports" : 10
    "HW/Pre-Labs" : 10
```

```{mermaid}
pie title Final Grade Weighting
    "GR 1" : 15
    "GR 2" : 15
    "ICE/Labs" : 25
    "Lab Reports" : 10
    "HW/Pre-Labs" : 10
    "Final": 25
```

Electrical and Computer Engineering courses are contract graded using the following 100 point scale.

|     Grade       |     Grade      |     Grade       |     Grade     |  
|:---------------:|:--------------:|:---------------:|:-------------:|
| 93 <= A <= 100  | 87 <= B+ < 90  | 77 <= C+ < 80   | 60 <= D < 70  |  
| 90 <= A- < 93   | 83 <= B < 87   | 73 <= C < 77    | 0 <= F < 60   |  
|                 | 80 <= B- < 83  | 70 <= C- < 73   |               |

You must complete all minimum functionalities on labs and in-class exercises in order to complete the course. Even if an assignment is so late that no credit will be received, the assignment must be completed to the satisfaction of the instructor to prevent a grade of “Incomplete.”

### Graded Assignments

**Homeworks** are designed both as a preview and review of content.

**In-Class Exercises (ICE)** require you to develop a technical solution in VHDL, RISC-V, or on a breadboard. They are designed to be completed during the 53-minute class period, but can be finished after class if more time is needed.

**Pre-Labs** are designed to help you complete the lab exercise, so must be submitted *before* the lab starts.

**Labs** include the code and demonstration for a lab exercise. The lab report is submitted/graded separately, see below.

**Lab Reports** must follow the format outlined in the ECE 281 [project report template](https://usafa0.sharepoint.com/:w:/r/sites/ECE281/Class%20Materials/ECE382_Project%20Report%20Template.docx?d=wfdbfc40c053e495a8b48dba315fe17ef&csf=1&web=1&e=CkZ0bH)

#### Late Assignments

Due date/time extensions for assignments will be granted at instructor discretion,
*provided you coordinate the extension **prior** to the day on which the assignment is due.*
It is your responsibility to plan ahead and communicate clearly.
These extensions will be reflected in [Gradescope](https://www.gradescope.com/) individually for you on that particular assignment.

### Exams

Graded Reviews and Final Exam are closed textbook, closed notes. A reference sheet will be provided - *also available in Teams*.
Testable material includes any concepts from the labs, lectures, exercises, homework, and assigned readings.
**Not all testable concepts will necessarily be covered in lectures.**

#### Missed Exams

- **Scheduled Absence:** If you know that you will be unable to take the GR during the scheduled GR period, you are required to inform your instructor as soon as possible before the GR and to schedule a make-up exam. The default make-up date/time will be the first DF Time period following the original exam date.
- **Unscheduled Absence:** If you miss the GR for reasons beyond your control (e.g. hospitalization, emergency leave, delayed field trip return, etc.), you must contact the course director within two working days to schedule a makeup. The default make-up date/time will be the first DF Time period following the original exam date.
- **Unexcused Absence (or Late):** As per *USAFA FOI 36-173, Academic Practices and Procedures*:
  - Cadets missing an examination due to an unexcused absence will receive a mandatory academic penalty of 25%.
  - Cadets arriving more than 15 minutes late (1 hour for final exams) are considered absent and will be scheduled to take the GR/Final Exam during the designated make-up date/time. They will receive a 25% academic penalty.
  - Cadets arriving less than 15 minutes late (1 hour for final exams) will immediately begin the exam and finish with the rest of the class. No academic penalty will be incurred.

### Final Exam Validation

The DFEC Department Head may exempt the top 5% of cadets in the course from the final exam. Candidates for validation of the final exam must:

- have at least a 90% average for the course
- have at least a 90% weighted average on all exams
- have completed minimum functionalities on all labs and in-class exercises

The Course Director will send **official** notification to everyone who validates the final NLT COB on T40. If you do not receive notification from the Course Director, you must take the final exam.

## Course Website Table of Contents

Course website: [https://usafa-ece.github.io/ece281-book](https://usafa-ece.github.io/ece281-book).

Students are encouraged to submit pull requests to [the github repository](https://github.com/usafa-ece/ece281-book)
for error corrections! **IP points will be awarded for pull requests fixing errors/issues/typos/etc.**

```{tableofcontents}
```

> **Disclaimer:** The contents of this website are for educational use only
> and do not necessarily reflect the official policy or position of the United States Air Force Academy,
> the Air Force, or the U.S. Government.

Department of Electrical and Computer Engineering, USAF Academy, CO 80840
