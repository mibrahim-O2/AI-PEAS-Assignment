# PEAS Framework and Task Environment Analysis

**Course:** Artificial Intelligence

**Assignment:** PEAS Framework and Task Environment Analysis

**Reference:** Russell & Norvig – *Artificial Intelligence: A Modern Approach*, Chapter 2

**Student Name:** Muhammad Ibrahim
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
      "Learning Mastery Rate: percentage of learning objectives achieved within an instructional period",
      "Time-to-Competency: average time from concept introduction to reaching competency threshold",
      "Student Engagement Score: composite of session duration, return rate, and interaction frequency",
      "Personalization Accuracy: alignment of recommendations with student Zone of Proximal Development",
      "Knowledge Retention Index: performance on delayed assessments administered 7 to 30 days later",
      "Fairness Across Students: reduction in outcome gaps across different student demographics"
    ],
    "environment": "Web and mobile based Learning Management System serving K-12, university, and corporate learners across subjects including math, programming, language, and science over multi-session engagements",
    "actuators": [
      "Content Recommendation Engine: selects and sequences study material based on learner model",
      "Adaptive Exercise Generator: adjusts difficulty and type of practice questions dynamically",
      "Feedback and Hint System: delivers real-time explanations and step-by-step guidance",
      "Progress Dashboard: visual summary of mastery levels for student and teacher",
      "Reminders and Scheduling Alerts: spaced repetition notifications based on forgetting curve",
      "Curriculum Path Adjustment: adds remedial content or skips ahead based on performance"
    ],
    "sensors": [
      "Answer Correctness: right or wrong responses to exercises and quizzes",
      "Response Time: time taken per question as indicator of confusion or confidence",
      "Clickstream Data: navigation patterns, replays, skips, and interface interactions",
      "Self Reported Confidence: student ratings of their own understanding after tasks",
      "Text Input: free text questions and chat messages processed through NLU",
      "Past Academic History: previous grades and assessment results from enrollment",
      "Profile Metadata: grade level, language, device type, and study time patterns",
      "Attention Signals: webcam or keyboard dynamics as proxies for fatigue or distraction"
    ]
  },
  "environment_classification": {
    "observability": {
      "choice": "Partially Observable",
      "justification": "The system cannot directly access the student's true understanding. It relies on indirect behavioral signals like response accuracy and timing which are often ambiguous and misleading."
    },
    "agent_structure": {
      "choice": "Multi-agent",
      "justification": "The student acts as an independent goal-driven agent making their own decisions, and teachers may also intervene, making this a multi-agent environment."
    },
    "outcome_predictability": {
      "choice": "Stochastic",
      "justification": "Student learning outcomes vary due to uncontrollable factors like mood, stress, and individual memory differences even when the same content is delivered."
    },
    "task_structure": {
      "choice": "Sequential",
      "justification": "Each instructional decision affects future sessions. Poor teaching in one session creates knowledge gaps that compound into bigger problems later."
    },
    "temporal_dynamics": {
      "choice": "Dynamic",
      "justification": "The environment keeps changing between sessions as student knowledge evolves, motivation shifts, and platform content gets updated independently of the agent."
    },
    "state_space": {
      "choice": "Continuous",
      "justification": "Student mastery exists on a gradient not as a binary state. The system tracks knowledge levels as continuous probability values across multiple topics."
    }
  },
  "utility_function": {
    "formula": "U = 0.35 * M + 0.30 * R - 0.15 * T - 0.20 * D",
    "variables": {
      "M": "Mastery Score normalized between 0 and 1",
      "R": "Retention Index from delayed assessments normalized between 0 and 1",
      "T": "Normalized time cost per unit of learning",
      "D": "Dropout risk probability between 0 and 1"
    },
    "weights": {
      "w1_mastery": 0.35,
      "w2_retention": 0.30,
      "w3_time_cost": 0.15,
      "w4_dropout_risk": 0.20
    },
    "behavioral_influence": "System slows down when mastery is low, switches to engaging tasks when dropout risk is high, and restructures the learning path when time cost increases without mastery gains",
    "effect_of_doubling_w2": "Doubling retention weight shifts the system toward spaced repetition and delayed progression. Improves long term memory but slows pace and may increase dropout risk if students feel stuck."
  }
}
```

---

## References

Russell, S., & Norvig, P. (2022). *Artificial Intelligence: A Modern Approach* (4th ed.). Pearson. Chapter 2: Intelligent Agents.

Corbett, A. T., & Anderson, J. R. (1994). Knowledge tracing: Modeling the acquisition of procedural knowledge. *User Modeling and User-Adapted Interaction, 4*(4), 253–278.

VanLehn, K. (2011). The relative effectiveness of human tutoring, intelligent tutoring systems, and other tutoring systems. *Educational Psychologist, 46*(4), 197–221.

