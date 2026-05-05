# PEAS Framework and Task Environment Analysis

**Course:** Artificial Intelligence

**Assignment:** PEAS Framework and Task Environment Analysis

**Reference:** Russell & Norvig – *Artificial Intelligence: A Modern Approach*, Chapter 2

**Student Name:** Muhammad Ibrahim
**Roll No:** 2k23/CSE/94 
**Date:** May 5, 2026

---

## Table of Contents

1. [PEAS Framework](#task-1-peas-framework)
2. [Environment Classification](#task-2-environment-classification)
3. [Critical Analysis](#task-3-critical-analysis)
4. [Structured JSON Representation](#task-4-structured-json-representation)
5. [References](#references)

---

## Introduction

For this assignment I picked the **AI Tutor system**  basically a smart learning app that figures out what a student knows, what they are struggling with, and then adjusts the study material accordingly. I chose this because honestly it is something I have actually used and I find it interesting how it makes decisions differently for every student.

The whole idea is that instead of showing everyone the same content, the system watches how you answer questions, how long you take, whether you skip stuff and then builds a picture of where you are at. Based on that it decides what to teach you next. This is a pretty good real world example of an intelligent agent because it has clear inputs, outputs, goals and it is working in a messy unpredictable environment which is basically a student trying to learn something.

---

## Task 1: PEAS Framework

The system I am analyzing is an **AI Tutor for Personalized Student Learning Recommendations** deployed inside a digital learning platform like an LMS (Learning Management System).

---

### Performance Measures

These are the things the system tries to do well. I came up with six that I think actually matter:

**1. Learning Mastery Rate**:
This measures what percentage of the topics a student actually masters. If I complete a chapter but failed the quiz three times before passing, that is not great mastery. The system should be pushing content in a way that the student genuinely understands it, not just guesses through it. Higher mastery rate means the tutor is doing its job properly.

**2. Time-to-Competency**:
How long does it take a student to go from zero knowledge of a topic to actually getting it? This matters because a good tutor should not waste your time repeating stuff you already know or going too fast on stuff you do not understand yet. Shorter time with proper understanding means the system is efficiently personalizing.

**3. Student Engagement Score**:
If students keep coming back, finishing sessions and actually interacting with the material that means the system is keeping them motivated. This is measured using things like how long they stay in a session, how often they return on their own, and whether they quit halfway through exercises. A boring tutor loses students fast.

**4. Personalization Accuracy**:
This checks whether the content the system recommends actually matches where the student is at. If it keeps giving me super easy stuff when I already know it, or throws hard problems at me when I have not learned the basics yet that is bad personalization. The goal is to always keep the student in what researchers call the Zone of Proximal Development (mean not too easy, not too hard).

**5. Knowledge Retention Index**:
Short term performance can be misleading. A student might score well right after studying but forget everything in a week. So this measure checks performance on delayed tests like a quiz given 7 or 30 days later. If retention is low, the system needs to use better strategies like spaced repetition.

**6. Fairness Across Students**:
The system should work equally well for different types of students or different backgrounds, languages, learning speeds. If it works great for some students but keeps failing others from a certain group, that is a problem. This measure tracks whether outcome gaps between student groups are shrinking over time.

---

### Environment

The AI Tutor runs inside a **web or mobile based learning platform**. It is used by students in schools, universities or even companies doing employee training. The subjects can range from math and programming to languages and science. Sessions can happen over days, weeks or even a full semester so the system needs to remember things about you across multiple logins.

There are also other people involved not just the student. Teachers can sometimes see what the system recommended and step in. Platform admins control what content is available. So the environment is not just one student and the AI it is a whole ecosystem.

---

### Actuators

These are the ways the AI Tutor actually does things its outputs and actions:

- **Content Recommendation:** Picks what reading, video or exercise to show next based on the student's current level
- **Exercise Generator:** Creates or selects practice questions at the right difficulty not too easy, not too hard
- **Feedback System:** When you get something wrong it does not just say "wrong answer" it explains why, gives hints, and walks you through the correct thinking
- **Progress Dashboard:** Shows the student and teacher a visual summary of what topics are mastered, what needs work, and what is coming next
- **Reminders and Alerts:** Sends notifications when it is a good time to review something based on your forgetting curve
- **Path Adjustment:** If you are struggling it can add extra foundational lessons. If you are flying through material it can skip ahead and save your time

---

### Sensors

These are all the inputs the system uses to understand the student:

- **Answer Correctness:** Whether you got a question right or wrong and by how much
- **Response Time:** How long you took to answer taking too long usually means you are confused or guessing
- **Clickstream Data:** Where you clicked, what you replayed, what you skipped your full navigation history inside the app
- **Self Reported Confidence:** Some platforms ask "how confident are you about this?" after exercises that data is useful
- **Text Input:** If the student types a question or uses a chat feature, the system reads and understands it
- **Past Academic History:** Your previous grades and performance when you first sign up
- **Profile Info:** Grade level, what language you study in, what device you are using, what time of day you usually study
- **Attention Signals (advanced):** Some systems use camera data or typing patterns to detect if you are zoning out or getting tired

---

## Task 2: Environment Classification

| Dimension | Classification | Justification |
|-----------|---------------|---------------|
| Fully Observable vs. Partially Observable | Partially Observable | The system cannot directly see the student's actual understanding it can only observe indirect signals like response accuracy and time taken, which are often misleading. |
| Single-agent vs. Multi-agent | Multi-agent | The student is not just a passive user, they are an active agent making their own decisions, and teachers can also step in and change things. |
| Deterministic vs. Stochastic | Stochastic | Even with the same content and same student profile, results will vary because of things like mood, stress, and distractions that the system has no control over. |
| Episodic vs. Sequential | Sequential | What the tutor decides in session one directly affects session two and beyond a bad recommendation early on can cause problems that build up over time. |
| Static vs. Dynamic | Dynamic | The student's knowledge, motivation, and external situation keep changing between sessions, so the environment never stays the same even when the system is idle. |
| Discrete vs. Continuous | Continuous | Student mastery is not simply "knows it" or "does not" it exists on a scale, and the system has to track it as a continuous value across multiple topics at once. |

---

## Task 3: Critical Analysis

### 3.1 Most Technically Challenging Dimension

In my opinion the hardest part of building this system is that it can never directly see what is actually going on inside the student's mind. Everything it knows is based on indirect signals, and those signals are often misleading.
Out of all six dimensions, Partial Observability is the hardest to implement technically. The real challenge is that the system needs to measure something it cannot directly access the student's actual understanding. Engineers have to rely on indirect signals like response accuracy and response time, but these are unreliable. A student could guess correctly or make a silly mistake, and the system cannot distinguish between the two. To work around this, engineers build probabilistic models like Bayesian Knowledge Tracing which track a confidence probability for each concept per student and update it after every interaction. This requires significant computational power, large amounts of training data to calibrate properly, and even then the model can still misread a student — making it the most technically demanding dimension to solve.

### 3.2 Utility Function Speed vs Accuracy Trade-off

For an AI Tutor the core trade-off is between delivering content quickly and making sure the student actually learns it properly. The proposed utility function is:

```
U = 0.4 × A − 0.6 × S

```
Where A is Accuracy how well the student is mastering the content and S is Speed Cost how much time is being spent per topic. The higher weight on S means the system tries to keep sessions efficient. If the weight of A is doubled from 0.4 to 0.8, the agent shifts its priority completely toward learning quality. It will slow down, repeat concepts more, add extra practice questions, and refuse to move forward until the student demonstrates solid understanding even if it takes much longer. The system becomes a thorough tutor but a slow one.

This is why the system needs probabilistic models like Bayesian Knowledge Tracing which do not just say "student knows this" but instead maintain a probability like "there is a 70% chance this student has mastered this concept" and update that probability after every single interaction based on new evidence like correct answers, wrong answers, and response time.

---

## Task 4: Structured JSON Representation

```json
{
  "system_name": "AI Tutor for Personalized Student Learning Recommendations",
  "peas": {
    "performance_measures": [
      "Learning Mastery Rate: percentage of topics a student actually masters within an instructional period",
      "Time-to-Competency: average time from zero knowledge of a topic to reaching proper understanding",
      "Student Engagement Score: composite of session duration, return rate, and task completion frequency",
      "Personalization Accuracy: alignment of recommendations with student Zone of Proximal Development",
      "Knowledge Retention Index: performance on delayed tests administered 7 to 30 days after instruction",
      "Fairness Across Students: reduction in outcome gaps across different backgrounds, languages, and learning speeds"
    ],
    "environment": "Web and mobile based Learning Management System used by students in schools, universities, and corporate training across subjects like math, programming, language, and science over multi-session engagements involving students, teachers, and platform admins",
    "actuators": [
      "Content Recommendation: picks reading, video, or exercise based on student current level",
      "Exercise Generator: creates or selects practice questions at the right difficulty",
      "Feedback System: explains wrong answers, gives hints, and walks through correct thinking",
      "Progress Dashboard: visual summary of mastered topics, areas needing work, and upcoming content",
      "Reminders and Alerts: sends review notifications based on forgetting curve",
      "Path Adjustment: adds foundational lessons when struggling or skips ahead when performing well"
    ],
    "sensors": [
      "Answer Correctness: whether student got a question right or wrong and by how much",
      "Response Time: how long student took to answer as indicator of confusion or confidence",
      "Clickstream Data: clicks, replays, skips, and full navigation history inside the app",
      "Self Reported Confidence: student ratings of their own understanding after exercises",
      "Text Input: questions or chat messages typed by student processed through NLU",
      "Past Academic History: previous grades and performance from enrollment",
      "Profile Info: grade level, language, device type, and time of day study patterns",
      "Attention Signals: camera data or typing patterns to detect fatigue or distraction"
    ]
  },
  "environment_classification": {
    "fully_observable_vs_partially_observable": {
      "choice": "Partially Observable",
      "justification": "The system cannot directly see the student's actual understanding, it can only observe indirect signals like response accuracy and time taken, which are often misleading."
    },
    "single_agent_vs_multi_agent": {
      "choice": "Multi-agent",
      "justification": "The student is not just a passive user, they are an active agent making their own decisions, and teachers can also step in and change things."
    },
    "deterministic_vs_stochastic": {
      "choice": "Stochastic",
      "justification": "Even with the same content and same student profile, results will vary because of things like mood, stress, and distractions that the system has no control over."
    },
    "episodic_vs_sequential": {
      "choice": "Sequential",
      "justification": "What the tutor decides in session one directly affects session two and beyond, a bad recommendation early on can cause problems that build up over time."
    },
    "static_vs_dynamic": {
      "choice": "Dynamic",
      "justification": "The student's knowledge, motivation, and external situation keep changing between sessions, so the environment never stays the same even when the system is idle."
    },
    "discrete_vs_continuous": {
      "choice": "Continuous",
      "justification": "Student mastery is not simply knows it or does not, it exists on a scale and the system has to track it as a continuous value across multiple topics at once."
    }
  },
  "utility_function": "U = 0.4 * A - 0.6 * S"
}

```

---

## References

Russell, S., & Norvig, P. (2022). *Artificial Intelligence: A Modern Approach* (4th ed.). Pearson. Chapter 2: Intelligent Agents.

Corbett, A. T., & Anderson, J. R. (1994). Knowledge tracing: Modeling the acquisition of procedural knowledge. *User Modeling and User-Adapted Interaction, 4*(4), 253–278.

VanLehn, K. (2011). The relative effectiveness of human tutoring, intelligent tutoring systems, and other tutoring systems. *Educational Psychologist, 46*(4), 197–221.

