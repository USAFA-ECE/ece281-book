# ECE 281: Digital Design and Computer Architecture

2025 Spring Syllabus

## Schedule

| Lesson | Topic                                    | Reading                  | Assigned     | Due                |
|--------|------------------------------------------|--------------------------|--------------|--------------------|
| 1      | Version Control                          | 1.2                      | HW 1         |                    |
| 2      | Logic Gates                              | 1.5, 2.1                 | HW 2         |                    |
| 3      | Boolean Equations and Algebra            | 2.2-2.3                  | HW 3         |                    |
| 4      | ICE1 - Breadboard Half Adder             |                          | HW 4         |                    |
| 5      | Multilevel Logic & VHDL                  | 2.4-2.6                  | HW 5         | ICE1               |
| 6      | ICE2 - VHDL Half Adder                   |                          | [HW 6](./HW/HW6.md) |             |
| 7      | Numbering Systems and Arithmetic         | 1.4                      | HW 7         | ICE2               |
| 8      | K-Maps                                   | 2.7                      | Lab 1 Prelab |                    |
| 9      | Mux, Decoders, Combinational Timing      | 2.8-2.9                  |              |                    |
| 10     | Lab 1 - 31 Day Month                     |                          |              | Lab 1 Prelab       |
| 11     | Lab 1 - 31 Day Month                     |                          |              |                    |
| 12     | Arithmetic in Combinational Logic        | 5.1-5.2.3, 5.2.6-5.2.8   | {ref}`ripple-adder-hw` |          |
| 13     | ICE3 - Ripple Adder                      |                          |              | Lab 1              |
| 14     | RV32I R- and I-Type Instructions         | 6.1-6.2.1, 6.3.2         | HW 14        | ICE3               |
| 15     | The Mighty ALU!                          | 5.2.4-5.2.5              | Lab 2 Prelab |                    |
| 16     | GR #1                                    |                          |              |                    |
| 17     | Lab 2 - 7 Segment Display                |                          |              | Lab 2 Prelab       |
| 18     | Synchronous Circuits                     | 3.1-3.3                  |              |                    |
| 19     | Finite State Machine Intro               | 3.4.1-3                  |              |                    |
| 20     | FSM Reverse Engineering                  | 3.4.4-6                  | HW 20        | Lab 2              |
| 21     | RV32I Registers                          | 6.2.2, 6.4.1-6.4.2       | HW 21        |                    |
| 22     | ICE4 - Stoplight                         |                          | Lab 3 Prelab |                    |
| 23     | Memory: RAM & ROM                        | 5.4-5.5                  |              | ICE4               |
| 24     | Lab 3 - T-bird Turn Signal               |                          |              | Lab 3 Prelab       |
| 25     | Lab 3 - T-bird Turn Signal               |                          | HW 25        |                    |
| 26     | RV32I U-, S-, B-Type Instructions        | 6.3.3-6.3.6, 6.4.3-      |              |                    |
| 27     | ICE5 - Basic Elevator Controller         |                          | Lab 4 Prelab | Lab 3              |
| 28     | ICE6 - Time Division Multiplexing        |                          |              | ICE5               |
| 29     | Lab 4 - Moore Elevator Controller        |                          |              | ICE6, Lab 4 Prelab |
| 30     | Lab 4 - Moore Elevator Controller        |                          | HW 30        |                    |
| 31     | GR #2                                    |                          |              |                    |
| 32     | RV32I Memory Map                         | 6.3.7-6.3.8,  6.5, 6.6.1 |              | Lab 4              |
| 33     | RV32I Function Calls                     | 6.4.4-6.4.7              |              |                    |
| 34     | RV32I Single-Cycle Microarchitecture 1   | 7.1, 7.3-7.3.2           | Lab 5 Prelab |                    |
| 35     | Lab 5 Prelab - CPU                       |                          |              |                    |
| 36     | RV32I Single-Cycle Microarchitecture 2   | 7.3.3-7.3.5              |              | Lab 5 Prelab       |
| 37     | Lab 5 - CPU                              |                          |              |                    |
| 38     | Lab 5 - CPU                              |                          | HW 38        | Lab 5 Demo         |
| 39     | ICE7 - RV32I                             |                          |              | ICE7               |
| 40     | Review                                   |                          |              |                    |

Unless stated otherwise, assignments are submitted on [Gradescope](https://www.gradescope.com/).

- Homework (HW) are always due at 0800 before the next lesson.
    - Extensions on HW are *only* granted in the case of medical or family emergency.
- All other assignments are due at 1159 on the T-day of that lesson.

## Course Information

### Instructors

#### [Capt Brian Yarbrough](https://www.usafa.edu/facultyprofile/?smid=54059)

- Course Director
- Office: 2F48

#### [Dr. George York](https://www.usafa.edu/facultyprofile/?smid=18247)

- Office: 2F6A

#### [Lt Col James Trimble](https://www.usafa.edu/facultyprofile/?smid=54055)

- Office: 2E46D

#### [Lt Col Jason Wyche](https://www.usafa.edu/facultyprofile/?smid=80068)

- Office: 2F10

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

#### Collaboration

For all assignments in this course, unless otherwise noted on the assignment, you may work with anyone. We expect all graded work, to include code, lab notebooks, and written reports, to be in your own work. Copying another person’s work, with or without documentation, will result in NO academic credit.  Furthermore, copying without attribution is dishonorable and will be dealt with as an honor code violation.

#### Extra Instruction (EI)

Schedule EI with an instructor if you are having difficulty with the course material.  You must have read the assignment and attempted the homework before requesting EI.  You are responsible for material if you miss class. After you’ve read the assignment, attempted the homework, and checked with your classmates, you may then schedule EI if you have difficulty with the material. EI is not a remedial lecture.

#### Missing Class

 You must notify your instructor **via email** of any class absence as early as possible.  You must provide a descriptive reason and SCA number, and you should describe what you will miss and how you would like to accomplish the missed work.  Be sure to check your SCA to see if instructor “notification” or “permission” is required, and whether you are allowed to miss graded events.

#### Late Assignments

With the exception of homeworks (which may only be submitted late in the case of family or medical emergency),
deadline extensions are liberally granted by instructor discretion,
*provided that you coordinate **prior** to the day on which the assignment is due.*
It is your responsibility to plan ahead and communicate clearly.
These extensions will be reflected ni Gradescope individually for you on that particular assignment.

## Grading

This course uses an alternative grading format.
The intent is that your grades will be fairer, more accurate, and more meaningful.
This grading format should also improve your learning and retention of concepts taught throughout the semester.

There are three broad categories of assignments:

- Pass/fail
- Lab reports
- Exams

The grade weights are as follows:

```{mermaid}
pie title Prog Grade Weighting
    "GR 1" : 25
    "HW" : 20
    "ICE" : 15
    "Prelab" : 10
    "Lab Code"  : 10
    "Lab Reports" : 20
```

```{mermaid}
pie title Final Grade Weighting
    "GR 1" : 15
    "GR 2" : 15
    "HW" : 10
    "ICE" : 10
    "Prelab" : 10
    "Lab Code"  : 10
    "Lab Reports" : 20
    "Final": 10
```

Grade breakdown follows the standard scale.

```{mermaid}
flowchart LR
    J["A\n100%"] --> I["A-\n93%"] --> H["B+\n90%"] --> G["B\n87%"] --> F["B-\n83%"] --> E["C+\n79%"] --> D["C\n77%"] --> C["C-\n73%"] --> B["D\n70%"] --> A["F\n≤60%"]
    style J fill:#006400,color:#000000
    style I fill:#006400,color:#000000
    style H fill:#90EE90,color:#000000
    style G fill:#90EE90,color:#000000
    style F fill:#90EE90,color:#000000
    style E fill:#FFA500,color:#000000
    style D fill:#FFA500,color:#000000
    style C fill:#FFA500,color:#000000
    style B fill:#FF4500,color:#000000
    style A fill:#FF0000,color:#000000
```

### Pass/Fail Assignments

All pas/fail assignments are submitted on Gradescope. Most of these assignments are auto-graded,
sometimes with immediate feedback and allowed resubmissions. The intent of these assignments is to help you
engage with the material and help you build context for how it all fits together.

To be considered a "pass" you must earn **60%** of the available points for the assignment.

- **Homeworks (HW)** are designed both as a preview and review of content.
    There are *no deadline extensions* on HW (including SCA), with the exception of medical or family emergency.
- **In-Class Exercises (ICE)** require you to develop a technical solution in VHDL, RISC-V, or on a breadboard.
    They are designed to be completed during the 53-minute class period, but can be finished afterwords if need be.
- **Pre-Labs** are designed to help you complete the lab exercise, so must be submitted *before* the lab starts.
- **Labs** include the code and demonstration for a lab exercise. The lab report is submitted separately, see below.

Your ultimate grade for a category will simply be the average of passed vs. total assignments for that category.
For example, if you complete 6/7 ICEs, you will earn $6/7=85\%$ for ICEs.

### Lab Reports

Each lab has a report (except Lab 5), submitted on Gradescope, that will be assessed using [The EMRN Rubric](https://rtalbert.org/emrn/)

![EMRN Rubric](https://rtalbert.org/content/images/2022/04/EMRN-rubric-2020.png)

For lab reports #1, #2, and #3, you may resubmit once per report for re-evaluation,
provided you earned an **M** or **R** on your original submission.
To prevent grading backlogs, the resubmission must take place **within ten days** of the lab report being returned.

**The total lab report score starts at 100:**

- Deduct 0% for each E
- Dduct 5% for each M
- Deduct 15% for each R
- Deduct 25% for each N
- Deduct 30% for each missing submissoin
- Add up to 2% for Lab 4 report

**Example:**

- A cadet earns an M on her first report.
- She then earns an R on her second report and resubmits, earning an E.
- She earns an M on her third report
- She earned a 100% on her fourth report

Her original grade was $100-(5+15+5)+2=87$.
But, after the Lab 2 Revisions, the ultimate grade is $100-(5+0+5)+2=92$.

### Exams

Graded Reviews and Final Exam are closed textbook, closed notes. A reference sheet - also available in Teams - will be provided.
Testable material includes any concepts from the labs, lectures, exercises, homework, and assigned readings.
**Not all testable concepts will necessarily be covered in lectures.**

#### Missed Exams

The following policies are outlined in USAFA FOI 537-3:

- **Scheduled Absence:** If you know that you will be unable to take the GR during the scheduled GR period, you are required to inform your instructor as soon as possible before the GR and to schedule a make-up exam.
- **Unscheduled Absence:** If you miss the GR for reasons beyond your control (e.g. hospitalization, emergency leave, delayed field trip return, etc.), you must contact the course director within two working days to schedule a makeup.  Exceptions can only be granted by the Department Head.

### Final Exam Validation

$$
V = GRLB
$$

where

- $V$ = validates the final exam
- $G$ = grade is >= 97%
- $R$ = has at least one Exemplary reports
- $L$ = passed all lab code assignments
- $B$ = returns their Basys3 board

The Course Director will send **official** notification to everyone who validates the final NLT COB on T40.
Unless you receive that email, you must take the final exam

## Course Website Table of Contents

Course website: [https://usafa-ece.github.io/ece281-book](https://usafa-ece.github.io/ece281-book).

Students are encouraged to submit pull requests to [the github repository](https://github.com/usafa-ece/ece281-book)
for error corrections!

```{tableofcontents}
```

> **Disclaimer:** The contents of this website are for educational use only
> and do not necessarily reflect the official policy or position of the United States Air Force Academy,
> the Air Force, or the U.S. Government.

Department of Electrical and Computer Engineering, USAF Academy, CO 80840
