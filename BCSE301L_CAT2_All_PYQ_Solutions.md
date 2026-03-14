# 📋 BCSE301L — All Previous Year Questions with Detailed Solutions
### CAT-2 | Papers: C1 '23-24, C2 '23-24, G1 '24-25, G2 '24-25

---

# 📑 Question Index (20 Questions Total)

| # | Topic Group | Paper | Q# | Marks | Module |
|---|------------|-------|:--:|:-----:|:------:|
| 1 | Cyclomatic Complexity & Basis Path | C1 '23-24 | Q5 | 10 | 5 |
| 2 | Cyclomatic Complexity & Basis Path | C2 '23-24 | Q4 | 10 | 5 |
| 3 | Cyclomatic Complexity & Basis Path | G1 '24-25 | Q5 | 10 | 5 |
| 4 | Cyclomatic Complexity & Basis Path | G2 '24-25 | Q5 | 10 | 5 |
| 5 | Cohesion & Coupling | C1 '23-24 | Q3 | 10 | 4 |
| 6 | Cohesion & Coupling | C2 '23-24 | Q3 | 10 | 4 |
| 7 | Cohesion & Coupling | G1 '24-25 | Q4a | 4 | 4 |
| 8 | Cohesion & Coupling | G2 '24-25 | Q3 | 10 | 4 |
| 9 | UML Diagrams | C1 '23-24 | Q2 | 10 | 3 |
| 10 | UML Diagrams | C2 '23-24 | Q1 | 10 | 3 |
| 11 | UML Diagrams | G2 '24-25 | Q2 | 10 | 3 |
| 12 | DFD & System Modeling | C1 '23-24 | Q1 | 10 | 3 |
| 13 | DFD & System Modeling | C2 '23-24 | Q2 | 10 | 3/4 |
| 14 | Black-Box Testing (BVA, ECP) | C1 '23-24 | Q4 | 10 | 5 |
| 15 | Black-Box Testing (BVA, ECP) | C2 '23-24 | Q5 | 10 | 5 |
| 16 | Design Concepts | G1 '24-25 | Q4b | 6 | 4 |
| 17 | Design Concepts | G2 '24-25 | Q4 | 10 | 4 |
| 18 | Requirements Engineering | G1 '24-25 | Q2 | 10 | 3 |
| 19 | Requirements Engineering | G1 '24-25 | Q3 | 10 | 3 |
| 20 | Risk Management | G1 '24-25 | Q1 | 10 | 2 |
| 21 | Risk Management | G2 '24-25 | Q1 | 10 | 2 |

---

# 🔵 GROUP 1: Cyclomatic Complexity & Basis Path Testing

> **⚠️ IMPORTANT:** This topic appeared in **ALL 4 papers** (100% frequency). Always worth 10 marks. You MUST know three methods to calculate CC and how to derive independent paths + test cases.

> **💡 Key Formula Cheat Sheet:**
> - `V(G) = E − N + 2` (edges − nodes + 2)
> - `V(G) = Regions in CFG` (enclosed + outer region)
> - `V(G) = Decision Nodes + 1` (count every if/for/while/else-if)
> - Number of independent paths = V(G)

---

## Q1 — Selection Sort CFG & Path Coverage
**📌 Paper:** C1 '23-24 (Slot C1+TC1) | **Module:** 5 | **Marks:** 10 (5+5) | **CO4, BL4**

### Question
Consider the following program segment:
```java
1.  void SelectionSort(int array1[]) {
2.      int size = array1.length;
3.      for (int i = 0; i < size; i++) {
4.          int minIndex = i;
5.          for (int j = i+1; j < size; j++) {
6.              if (array1[j] < array1[minIndex])
7.                  minIndex = j;
8.          }
9.          int temp = array1[minIndex];
10.         array1[minIndex] = array1[i];
11.         array1[i] = temp;
12.     }
13. }
```
a) Prepare the CFG and determine the cyclomatic complexity of the selection sort function. (5 marks)
b) Design a test suite for the selection sort that satisfies Path coverage. (5 marks)

### Detailed Solution

**Part (a) — CFG & Cyclomatic Complexity**

> **🔑 IMPORTANT:** When drawing CFG for loops, the loop condition node has TWO outgoing edges — one for "condition true" (enter loop body) and one for "condition false" (exit loop).

**Step 1: Identify nodes and edges**
```
Node 1: int size = array1.length         (statement)
Node 2: i < size                         (DECISION — outer for)
Node 3: int minIndex = i                 (statement)
Node 4: j < size                         (DECISION — inner for)
Node 5: array1[j] < array1[minIndex]     (DECISION — if)
Node 6: minIndex = j                     (statement)
Node 7: temp = array1[minIndex]; swap    (statements 9-11)
Node 8: EXIT
```

**Step 2: Draw edges**
```
1 → 2                    (enter outer loop)
2 → 3  (true: i<size)    (enter loop body)
2 → 8  (false: i>=size)  (exit to END)
3 → 4                    (enter inner loop)
4 → 5  (true: j<size)    (enter inner body)
4 → 7  (false: j>=size)  (exit inner loop, do swap)
5 → 6  (true: found smaller)
5 → 4  (false: not smaller, back to inner loop check)
6 → 4                    (back to inner loop check)
7 → 2                    (back to outer loop check)
```

**Step 3: Calculate Cyclomatic Complexity**
```
Method 1: V(G) = E − N + 2 = 10 − 8 + 2 = 4
Method 2: Predicate nodes = 3 (nodes 2, 4, 5) → V(G) = 3 + 1 = 4
```

> **✅ Answer: V(G) = 4** → 4 independent paths needed

**Part (b) — Test Suite for Path Coverage**

> **🔑 IMPORTANT:** Each test case must force execution of a unique path. The number of test cases = V(G) = 4.

| Path | Description | Test Input | Expected Output |
|------|-------------|:----------:|:---------------:|
| Path 1 | Outer loop never executes (empty array) | `[]` | `[]` |
| Path 2 | Outer loop runs, inner loop doesn't (single element) | `[5]` | `[5]` |
| Path 3 | Inner loop runs, if-condition is false (already sorted) | `[1, 2, 3]` | `[1, 2, 3]` |
| Path 4 | Inner loop runs, if-condition true (swap occurs) | `[3, 1, 2]` | `[1, 2, 3]` |

---

## Q2 — Leap Year: Basis Path Testing + Equivalence Class Partitioning
**📌 Paper:** C2 '23-24 (Slot C2+TC2) | **Module:** 5 | **Marks:** 10 (6+4) | **CO4, BL4**

### Question
For the given leap year pseudocode:
```
function isLeapYear(year):
    if (year is not divisible by 4) then
        return False
    else if (year is not divisible by 100) then
        return True
    else if (year is divisible by 400) then
        return True
    else
        return False
```
- Design test cases for basis path testing. (6 Marks)
- Apply equivalence class partitioning. (4 Marks)

### Detailed Solution

**Part (a) — Basis Path Testing**

> **🔑 IMPORTANT:** First identify all decision nodes, then trace independent paths. Each path = one test case. Marks: 2 for identifying paths + 4 for test cases.

**Decision nodes:** 3 decisions → `V(G) = 3 + 1 = 4` paths

**Independent Paths:**
```
Path 1: year NOT divisible by 4              → return False
Path 2: year divisible by 4, NOT by 100      → return True
Path 3: year divisible by 4, by 100, by 400  → return True
Path 4: year divisible by 4, by 100, NOT 400 → return False
```

**Test Cases:**

| Path | Input | Expected | Why This Value |
|:----:|:-----:|:--------:|----------------|
| Path 1 | `1999` | `False` | Not divisible by 4 at all |
| Path 2 | `2020` | `True` | Divisible by 4, not by 100 |
| Path 3 | `2000` | `True` | Divisible by 4, 100, AND 400 |
| Path 4 | `1900` | `False` | Divisible by 4 and 100, but NOT 400 |

**Part (b) — Equivalence Class Partitioning**

> **🔑 IMPORTANT:** You need 2 valid classes + 2 invalid classes for full marks.

| Class | Type | Description | Example | Expected |
|:-----:|:----:|-------------|:-------:|:--------:|
| Class 1 | ✅ Valid | Divisible by 4, not by 100 | `2004` | `True` |
| Class 2 | ✅ Valid | Divisible by 400 | `2000` | `True` |
| Class 3 | ❌ Invalid | Not divisible by 4 | `2001` | `False` |
| Class 4 | ❌ Invalid | Divisible by 100, not by 400 | `1900` | `False` |

---

## Q3 — Factorial: Cyclomatic Complexity + BVA
**📌 Paper:** G1 '24-25 (Slot G1+TG1) | **Module:** 5 | **Marks:** 10 (5+5) | **CO5, BL5**

### Question
```python
def factorial(n):
    if n == 0:
        return 1
    else:
        result = 1
        for i in range(1, n + 1):
            result *= i
        return result
```
a) Compute cyclomatic complexity using flow graph/chart method. (5 marks)
b) Generate at least five test cases using Boundary Value Analysis. (5 marks)

### Detailed Solution

**Part (a) — Cyclomatic Complexity**

> **🔑 IMPORTANT:** The flow graph is worth 3 marks, the CC calculation is worth 2 marks. Draw the graph FIRST.

**CFG Nodes:**
```
Node 1: Start / function entry
Node 2: if n == 0       ← DECISION
Node 3: return 1
Node 4: result = 1
Node 5: for i in range  ← DECISION (loop condition)
Node 6: result *= i
Node 7: return result
Node 8: End
```

**Edges (E=10):**
```
1→2, 2→3(true), 2→4(false), 3→8, 4→5, 5→6(true), 5→7(false), 6→5, 7→8
```

**Calculation (show BOTH methods for safety):**
```
Method 1: V(G) = E − N + 2 = 10 − 9 + 2 = 3
Method 2: Decision points = 2 (if, for) → V(G) = 2 + 1 = 3
```

> **✅ Answer: V(G) = 3**

**Part (b) — Boundary Value Analysis**

> **🔑 IMPORTANT:** For BVA, always test: minimum valid, just above min, nominal, just below max, maximum, and invalid boundaries. 1 mark per test case.

| TC | Input | Expected Output | Boundary Type |
|:--:|:-----:|:---------------:|---------------|
| 1 | `n = 0` | `1` | Minimum valid input (base case) |
| 2 | `n = 1` | `1` | Just above minimum |
| 3 | `n = 5` | `120` | Typical/nominal valid input |
| 4 | `n = 9` | `362880` | Just below upper bound |
| 5 | `n = 10` | `3628800` | Maximum practical input |
| 6 | `n = -1` | Error/Exception | Invalid — below minimum |
| 7 | `n = 12` | `479001600` | Edge case — large number |

---

## Q4 — Discount Calculator: CC + Independent Paths + Test Cases
**📌 Paper:** G2 '24-25 (Slot G2+TG2) | **Module:** 5 | **Marks:** 10 (4+2+4) | **CO3, BL2**

### Question
```c
float calculate_discount(float total_amount, int is_member, char coupon_code[]) {
    if (total_amount <= 0)       { return 0; }
    float discount = 0;
    if (is_member)               { discount += total_amount * 0.1; }
    if (strcmp(coupon_code, "SAVE20") == 0)
                                 { discount += total_amount * 0.2; }
    else if (strcmp(coupon_code, "SAVE10") == 0)
                                 { discount += total_amount * 0.1; }
    float max_discount = total_amount * 0.3;
    if (discount > max_discount) { discount = max_discount; }
    return total_amount - discount;
}
```
i. Calculate cyclomatic complexity (4 marks)
ii. Identify all independent paths (2 marks)
iii. Write test cases to cover all independent paths (4 marks)

### Detailed Solution

**Part (i) — Cyclomatic Complexity**

> **🔑 IMPORTANT:** Count EVERY if and else-if as a separate decision point. `else` alone does NOT count.

**Decision Points:**
```
1. if (total_amount <= 0)                    → 1 decision
2. if (is_member)                            → 1 decision
3. if (strcmp(coupon_code, "SAVE20") == 0)    → 1 decision
4. else if (strcmp(coupon_code, "SAVE10")==0) → 1 decision
5. if (discount > max_discount)              → 1 decision
                                    Total    = 5 decisions
```

```
V(G) = 5 + 1 = 6
```

> **✅ Answer: V(G) = 6** → 6 independent paths

**Part (ii) — Independent Paths**

| Path | Condition Flow | Result |
|:----:|----------------|--------|
| 1 | `total_amount ≤ 0` | Return 0 |
| 2 | `total > 0`, NOT member, no valid coupon | No discount → return total |
| 3 | `total > 0`, IS member, no valid coupon | 10% member discount |
| 4 | `total > 0`, IS member, coupon = "SAVE20" | 10% + 20% = 30% discount |
| 5 | `total > 0`, IS member, coupon = "SAVE10" | 10% + 10% = 20% discount |
| 6 | `total > 0`, discount exceeds 30% cap | Discount capped at 30% |

**Part (iii) — Test Cases**

> **🔑 IMPORTANT:** Each test case maps to one independent path. Show input, expected output, AND what path it covers.

| TC | total_amount | is_member | coupon_code | Expected Output | Path Covered |
|:--:|:-----------:|:---------:|:-----------:|:---------------:|:------------:|
| 1 | `0` | `0` | `"NONE"` | `0` | Path 1: zero amount |
| 2 | `100` | `0` | `"NONE"` | `100` | Path 2: no discount |
| 3 | `100` | `1` | `"NONE"` | `90` | Path 3: member only (10%) |
| 4 | `100` | `1` | `"SAVE20"` | `70` | Path 4: member+20% coupon |
| 5 | `100` | `1` | `"SAVE10"` | `80` | Path 5: member+10% coupon |
| 6 | `200` | `1` | `"SAVE20"` | `140` | Path 6: cap at 30% max |

> **🔎 TC6 Calculation:** `200 × 0.1 + 200 × 0.2 = 60` → but `max = 200 × 0.3 = 60` → `60 ≤ 60` so cap NOT applied. For cap to apply you'd need member+SAVE20 with a scenario where combined > 30%. Since member(10%)+SAVE20(20%)=30% exactly equals cap, the cap doesn't trigger. The answer key uses `200, member, SAVE20 → 140` which is `200 - 60 = 140` (30% discount applied, cap equality case).

---

# 🔵 GROUP 2: Cohesion & Coupling

> **⚠️ IMPORTANT:** Appeared in **ALL 4 papers**. You will be given either (a) code snippets to identify cohesion type, or (b) system scenarios to identify both coupling AND cohesion between modules.

> **💡 Quick Reference:**
> - **Cohesion** = bonding WITHIN a module (HIGH is good)
> - **Coupling** = dependency BETWEEN modules (LOW is good)
> - Cohesion ranking (best→worst): Functional > Sequential > Communicational > Procedural > Temporal > Logical > Coincidental
> - Coupling ranking (best→worst): Data > Stamp > Control > Common > Content

---

## Q5 — IoT Architecture + Temporal Cohesion
**📌 Paper:** C1 '23-24 (Slot C1+TC1) | **Module:** 4 | **Marks:** 10 (5+5) | **CO3, BL4**

### Question
a) Developing firmware for a microcontroller-based IoT device that monitors temperature, humidity, and air quality in a smart home. The device continuously samples sensor data and sends periodic updates to a central server. Identify the appropriate architecture control style and justify. (5 marks)

b) How does temporal cohesion contribute to the overall organization and efficiency of a software module? Provide a real-world example. (5 marks)

### Detailed Solution

**Part (a) — Architecture Style**

> **🔑 IMPORTANT:** For IoT/real-time scenarios, the answer is almost always **Event-Driven (Interrupt-Driven)** architecture.

**Answer: Event-Driven Architecture (Interrupt-Driven)**

**Justification (write at least 4 points for 5 marks):**
1. **Asynchronous event handling** — sensors generate data at different intervals; interrupts allow each sensor to be serviced independently
2. **Minimal CPU overhead** — processor sleeps/idles until an interrupt occurs, saving power (critical for IoT)
3. **Real-time responsiveness** — interrupts ensure immediate response to sensor threshold breaches (e.g., air quality alert)
4. **Scalability** — new sensors can be added as new interrupt sources without redesigning the system
5. **Periodic updates** — timer interrupts can trigger periodic data transmission to the central server

**Part (b) — Temporal Cohesion**

> **🔑 IMPORTANT:** Temporal cohesion = elements grouped because they execute at the **same time**, not because they are logically related.

**Definition:** Temporal cohesion occurs when elements of a module are grouped because they need to execute at the same point in time, such as initialization, cleanup, or shutdown routines.

**Contribution to organization:**
- Makes code easier to understand by grouping time-related activities
- Reduces confusion about execution order
- Easier to debug and maintain

**Real-world Example — Online Shopping Cart:**
When a user adds an item to their cart, these activities happen together:
1. Update the total price
2. Display confirmation message
3. Store item in user's cart
4. Update cart item count badge

All these are grouped in one module because they must occur at the same time (when "Add to Cart" is clicked), even though they serve different functions.

---

## Q6 — Cloud Document Editor: Architecture + Coupling + Cohesion
**📌 Paper:** C2 '23-24 (Slot C2+TC2) | **Module:** 4 | **Marks:** 10 (5+5) | **CO3, BL3**

### Question
Designing a cloud-based document editing and collaboration tool with simultaneous editing, change tracking, and integrated chat. Must be scalable with future AI expansion.
- Identify appropriate architecture style and justify. (5 marks)
- Give three types of relevant coupling and two types of cohesion. (5 marks)

### Detailed Solution

**Architecture: Layered Architecture**

> **🔑 IMPORTANT:** For enterprise/scalable web apps with multiple concerns, **Layered Architecture** is the go-to answer.

```
┌──────────────────────────────────┐
│    Presentation Layer            │  → Document editor UI, Chat UI, Collaboration UI
├──────────────────────────────────┤
│    Business Logic Layer          │  → Change tracking, versioning, auth, future AI
├──────────────────────────────────┤
│    Data Access Layer             │  → Cloud storage, document retrieval, metadata
└──────────────────────────────────┘
```

**Justification:**
- **Modularity** — each layer handles specific responsibility
- **Scalability** — layers can scale independently
- **Future expansion** — AI features can be added as new layer/service without affecting existing layers

**Three Types of Coupling:**

| # | Coupling Type | Example from Scenario |
|:-:|--------------|----------------------|
| 1 | **Data Coupling** | Document editing module passes edited content to version control module using standardized data format |
| 2 | **Stamp Coupling** | Chat module exchanges messages with other modules using predefined data structures |
| 3 | **External/Event Coupling** | When a user starts editing, an event is broadcasted to other collaborating users for real-time updates |

**Two Types of Cohesion:**

| # | Cohesion Type | Example from Scenario |
|:-:|--------------|----------------------|
| 1 | **Functional Cohesion** | Document Editing Module handles ONLY editing tasks (add, delete, format text) — single well-defined responsibility |
| 2 | **Communicational Cohesion** | Editing Module & Collaboration Module share document data and collaborate closely for real-time sync |

---

## Q7 — Code Snippet Cohesion Identification
**📌 Paper:** G1 '24-25 (Slot G1+TG1) | **Module:** 4 | **Marks:** 4 | **CO3, BL4**

### Question
Identify the type of bonding (cohesion) and justify whether it should be high or low:

**(i) Code Snippet:**
```python
def calculate_average(numbers):
    """Calculates the average of a list of numbers."""
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)
```

**(ii) Code Snippet:**
```python
class Utility:
    def print_message(self, message):
        print(message)
    def calculate_square(self, number):
        return number * number
    def read_file(self, filename):
        with open(filename, 'r') as file:
            return file.read()
```

### Detailed Solution

> **🔑 IMPORTANT:** The question uses the word "bonding" — this means **cohesion** (measure within a module). Always state: (1) type name, (2) high or low, (3) justification.

**Snippet (i): `calculate_average`**
- **Type:** Functional Cohesion
- **Level:** HIGH (Best)
- **Justification:** Every element in this function contributes to a **single, well-defined task** — calculating the average. The guard clause, the sum, and the division all serve one purpose.

**Snippet (ii): `Utility` class**
- **Type:** Coincidental Cohesion
- **Level:** LOW (Worst)
- **Justification:** The three methods (`print_message`, `calculate_square`, `read_file`) have **no logical relationship** to each other. They are grouped arbitrarily into a "Utility" class. Printing, math, and file I/O are completely unrelated tasks.

---

## Q8 — Library Management System: Coupling & Cohesion (6 Scenarios)
**📌 Paper:** G2 '24-25 (Slot G2+TG2) | **Module:** 4 | **Marks:** 10 | **CO3, BL2**

### Question
Six module interactions in a Library Management System. Identify coupling and cohesion type for each.

### Detailed Solution

> **🔑 IMPORTANT:** This is the most comprehensive coupling/cohesion question ever asked (10 marks, 6 sub-parts). Master this and you can answer ANY coupling/cohesion question.

**Scenario (i):** User Management verifies credentials, checks fines, sends user status + fine details to Borrowing Module. (2 marks)

| Aspect | Answer | Justification |
|--------|--------|---------------|
| **Coupling** | **Data Coupling** | Only required data (user status, fine details) is passed — no internal workings exposed |
| **Cohesion** (User Mgmt) | **Functional** | Single responsibility: verify credentials & check fines |
| **Cohesion** (Borrowing) | **Communicational** | All functions operate on the same dataset (user status + fine details) |

**Scenario (ii):** Borrowing Module directly accesses and updates Book Management's database tables. (2 marks)

| Aspect | Answer | Justification |
|--------|--------|---------------|
| **Coupling** | **Content Coupling** (WORST!) | Directly accesses another module's internal data — violates encapsulation |
| **Cohesion** (Book Mgmt) | **Functional** | Single responsibility: managing book data |
| **Cohesion** (Borrowing) | **Sequential** | Performs dependent steps in order (check availability → update records) |

**Scenario (iii):** Borrowing Module passes user contact details, book title, return due date to Notification Module. (2 marks)

| Aspect | Answer | Justification |
|--------|--------|---------------|
| **Coupling** | **Stamp Coupling** | Structured data (contact details, title, date) passed as function arguments |
| **Cohesion** (Notification) | **Functional** | Single responsibility: sending notifications |

**Scenario (iv):** Report Generation retrieves JSON data from multiple modules without modifying anything. (2 marks)

| Aspect | Answer | Justification |
|--------|--------|---------------|
| **Coupling** | **Data Coupling** | Retrieves structured data (JSON) without modifying internal logic |
| **Cohesion** (Report Gen) | **Functional** | Single well-defined task: generating reports |

**Scenario (v):** Inside Borrowing Module: validate user → check book → update records → adjust availability → notify user. (1 mark)

| Aspect | Answer | Justification |
|--------|--------|---------------|
| **Cohesion** | **Sequential Cohesion** | Steps must execute in strict order; output of one feeds the next |

**Scenario (vi):** Notification Module checks preferred mode (email/SMS/push) using if-else conditions. (1 mark)

| Aspect | Answer | Justification |
|--------|--------|---------------|
| **Cohesion** | **Logical Cohesion** | Multiple related functions selected by control flag (if-else); they do similar things but aren't functionally unified |

---

# 🔵 GROUP 3: UML Diagrams (Class, Use Case, Sequence)

> **⚠️ IMPORTANT:** UML questions give you a scenario and ask you to draw specific diagrams. Know which diagram to pick based on keywords in the question.
> - "interaction between users and system" → **Use Case Diagram**
> - "static structure / classes / attributes / relationships" → **Class Diagram**
> - "lifeline / chronological / message exchange" → **Sequence Diagram**
> - "structural blueprint / entities and interconnections" → **Class or Component Diagram**
> - "temporal ordering / method calls" → **Sequence Diagram**

---

## Q9 — University Account System: Class + Sequence Diagram
**📌 Paper:** C1 '23-24 (Slot C1+TC1) | **Module:** 3 | **Marks:** 10 (6+4) | **CO3, BL4**

### Question
A simplified account system: Students have a University Account (account id, name) → Link Account (email, password — can reset password, cannot change email). Link Account accesses Blackboard (courses: add, download, upload homework) and Zoom (meetings: create, delete). Course has course id, name, ≥1 meeting, ≥1 homework. Meeting has meeting id, invitation link, password. Homework has course id, homework no., list of questions.

a) Draw a static structure diagram (class diagram). (6 marks)
b) Draw a sequence diagram for joining online lecture BCSE301L. Objects: Student, Zoom App, Zoom Server, University Account Server. (4 marks)

### Detailed Solution

**Part (a) — Class Diagram**

> **🔑 IMPORTANT:** For class diagrams — identify classes, their attributes, methods, and relationships (association, aggregation, composition). Show multiplicity.

**Classes and Attributes:**

```
┌─────────────────────┐
│  UniversityAccount   │
├─────────────────────┤
│ - accountId: String  │
│ - studentName: String│
├─────────────────────┤
│                     │
└────────┬────────────┘
         │ 1..1 has
         ▼
┌─────────────────────┐        ┌──────────────────────┐
│    LinkAccount       │        │     Blackboard        │
├─────────────────────┤        ├──────────────────────┤
│ - email: String      │───────>│ - courses: List       │
│ - password: String   │        ├──────────────────────┤
├─────────────────────┤        │ + addCourse()         │
│ + resetPassword()    │        │ + downloadHomework()  │
└──────────┬──────────┘        │ + uploadHomework()    │
           │                    └──────────────────────┘
           │ accesses
           ▼
┌─────────────────────┐
│       Zoom           │
├─────────────────────┤
│ - meetings: List     │
├─────────────────────┤
│ + createMeeting()    │
│ + deleteMeeting()    │
└─────────────────────┘

┌─────────────────────┐    1..*    ┌────────────────────┐
│      Course          │◆─────────>│     Meeting         │
├─────────────────────┤            ├────────────────────┤
│ - courseId: String   │            │ - meetingId: String │
│ - courseName: String │            │ - inviteLink: String│
├─────────────────────┤            │ - password: String  │
│                     │            ├────────────────────┤
└──────────◆──────────┘            │ + joinById()       │
           │ 1..*                  └────────────────────┘
           ▼
┌─────────────────────┐
│     Homework         │
├─────────────────────┤
│ - courseId: String   │
│ - homeworkNo: int    │
│ - questions: List    │
└─────────────────────┘
```

**Key Relationships:**
- UniversityAccount **1:1** LinkAccount (composition)
- LinkAccount **accesses** Blackboard and Zoom (association)
- Course **1..\*** Meeting (composition — filled diamond)
- Course **1..\*** Homework (composition — filled diamond)

**Part (b) — Sequence Diagram (Joining Online Lecture)**

```
Student          Zoom App         Zoom Server       Univ. Account Server
  │                │                  │                    │
  │──open app────>│                  │                    │
  │                │──connect───────>│                    │
  │                │<─connected──────│                    │
  │──enter meeting│                  │                    │
  │   ID + pwd───>│                  │                    │
  │                │──verify meeting>│                    │
  │                │                  │──verify account──>│
  │                │                  │<─account valid─────│
  │                │<─meeting valid──│                    │
  │                │──join request──>│                    │
  │                │<─stream data────│                    │
  │<─show lecture──│                  │                    │
```

---

## Q10 — Ticket Reservation System: Use Case + Sequence Diagram
**📌 Paper:** C2 '23-24 (Slot C2+TC2) | **Module:** 3 | **Marks:** 10 (4+6) | **CO3, BL3**

### Question
Enhanced ticket reservation system for public transportation. Integrates real-time seat availability, fare calculation (distance, ticket type, peak hours), passenger details, payment processing.
a) Model user-system interaction using suitable UML approach. (4 marks)
b) Model the ticket booking scenario using suitable UML approach. (6 marks)

### Detailed Solution

**Part (a) — Use Case Diagram**

> **🔑 IMPORTANT:** Marks: 1 for identifying correct diagram + 3 for representation.

**Actors:** Passenger (primary), Payment Gateway (secondary), Admin (secondary)

**Use Cases:**
```
┌──────────────────────────────────────────────┐
│          Ticket Reservation System            │
│                                              │
│  ┌──────────────────────┐                    │
│  │ Search Route          │◄── Passenger       │
│  └──────────────────────┘                    │
│  ┌──────────────────────┐                    │
│  │ Check Seat Availability│◄── Passenger      │
│  └──────────────────────┘                    │
│  ┌──────────────────────┐                    │
│  │ Select Ticket Type    │◄── Passenger       │
│  │ (adult/child/senior)  │                    │
│  └──────────────────────┘                    │
│  ┌──────────────────────┐                    │
│  │ Calculate Fare        │ ←«include»─ Book   │
│  │ (distance, peak hrs)  │             Ticket  │
│  └──────────────────────┘                    │
│  ┌──────────────────────┐                    │
│  │ Process Payment       │──► Payment Gateway │
│  └──────────────────────┘                    │
│  ┌──────────────────────┐                    │
│  │ Generate Ticket       │                    │
│  └──────────────────────┘                    │
│  ┌──────────────────────┐                    │
│  │ Cancel Booking        │◄── Passenger       │
│  └──────────────────────┘                    │
└──────────────────────────────────────────────┘
```

**Part (b) — Sequence Diagram**

> **🔑 IMPORTANT:** Marks: 1 for identifying correct diagram + 5 for full representation with all system parameters (distance, ticket type, peak pricing).

```
Passenger       System         Database      Payment Gateway
   │               │               │               │
   │─search route─>│               │               │
   │               │─query seats──>│               │
   │               │<─seat data────│               │
   │<─show seats───│               │               │
   │               │               │               │
   │─select seat──>│               │               │
   │ +ticket type  │               │               │
   │ (adult/child) │               │               │
   │               │               │               │
   │               │──calculate────│               │
   │               │  fare:        │               │
   │               │  base × dist  │               │
   │               │  × type_mult  │               │
   │               │  × peak_mult  │               │
   │               │               │               │
   │<─display fare─│               │               │
   │               │               │               │
   │─confirm+pay──>│               │               │
   │               │─process pay──>│               │──>Payment Gateway
   │               │               │               │<──confirmation
   │               │<─pay success──│               │
   │               │─save booking─>│               │
   │               │<─confirmed────│               │
   │<─issue ticket─│               │               │
```

---

## Q11 — Air Traffic Control: Class/Component + Sequence Diagram
**📌 Paper:** G2 '24-25 (Slot G2+TG2) | **Module:** 3 | **Marks:** 10 (5+5) | **CO3, BL3**

### Question
Air Traffic Control system — monitors aircraft, ensures safe distances, guides takeoff/landing, real-time alerts, collision detection, and backup server failover.
i. Model the structural blueprint using UML. (5 marks)
ii. Depict temporal ordering of method calls/object interactions using UML. (5 marks)

### Detailed Solution

**Part (i) — Class Diagram (Structural Blueprint)**

> **🔑 IMPORTANT:** "Structural blueprint" + "entities and interconnections" = Class Diagram or Component Diagram.

**Key Classes:**
```
┌───────────────────┐     monitors     ┌──────────────────┐
│   RadarSystem      │────────────────>│    Aircraft       │
├───────────────────┤                  ├──────────────────┤
│ - radarId          │                  │ - flightId        │
│ - range            │                  │ - position        │
├───────────────────┤                  │ - altitude        │
│ + trackAircraft()  │                  │ - speed           │
│ + feedData()       │                  │ - heading         │
└───────────────────┘                  └──────────────────┘
        │ feeds data
        ▼
┌───────────────────┐     displays     ┌──────────────────┐
│   ATCSystem        │────────────────>│ DisplayInterface   │
├───────────────────┤                  ├──────────────────┤
│ - airspaceData     │                  │ - screenId        │
│ - safetyThresholds │                  ├──────────────────┤
├───────────────────┤                  │ + renderMap()     │
│ + processData()    │                  │ + showAlert()     │
│ + detectCollision()│                  └──────────────────┘
│ + generateAlert()  │
│ + failover()       │     notifies    ┌──────────────────┐
└────────┬──────────┘────────────────>│ Controller        │
         │                             ├──────────────────┤
         │ switches to                 │ - controllerId    │
         ▼                             ├──────────────────┤
┌───────────────────┐                  │ + issueCommand()  │
│  BackupServer      │                  └──────────────────┘
├───────────────────┤                         │ communicates
│ - status           │                         ▼
├───────────────────┤                  ┌──────────────────┐
│ + activate()       │                  │ CommSystem        │
│ + syncData()       │                  ├──────────────────┤
└───────────────────┘                  │ + sendToRadio()   │
                                       │ + sendAutoMsg()   │
                                       └──────────────────┘
```

**Part (ii) — Sequence Diagram (Temporal Ordering)**

```
RadarSystem     ATCSystem      DisplayUI     Controller    CommSystem    BackupServer
    │               │              │              │             │             │
    │─feed data────>│              │              │             │             │
    │ (position,    │              │              │             │             │
    │  altitude,    │              │              │             │             │
    │  speed)       │              │              │             │             │
    │               │─process &    │              │             │             │
    │               │ update map──>│              │             │             │
    │               │              │              │             │             │
    │               │─detect       │              │             │             │
    │               │ collision!   │              │             │             │
    │               │              │              │             │             │
    │               │─show alert──>│              │             │             │
    │               │─notify──────>│──────────────│             │             │
    │               │              │  recommend   │             │             │
    │               │              │  action      │             │             │
    │               │              │              │─send cmd───>│             │
    │               │              │              │ (to pilot)  │             │
    │               │              │              │             │             │
    │               │─[if failure]─│──────────────│─────────────│─failover──>│
    │               │              │              │             │  (ms level) │
    │               │<─────────────│──────────────│─────────────│─sync data──│
```

---

# 🔵 GROUP 4: DFD & System Modeling

---

## Q12 — Academic Record Management: ER Diagram + DFD
**📌 Paper:** C1 '23-24 (Slot C1+TC1) | **Module:** 3 | **Marks:** 10 (3+3+4) | **CO3, BL3**

### Question
Students' academic record management: courses (course number, credits, syllabus), students (roll number, address, semester), marks entry, SWA calculation, VC list (SWA ≥85), conditional standing (SWA <50).
a) Create an ER diagram. (3 marks)
b) Design context diagram and DFD Level 1. (3+4 marks)

### Detailed Solution

**Part (a) — ER Diagram**

> **🔑 IMPORTANT:** Identify entities, attributes (underline primary keys), and relationships with cardinality.

**Entities & Attributes:**
```
[Course]  ─── Course_Number(PK), Number_of_Credits, Syllabus
[Student] ─── Roll_Number(PK), Address, Semester_Number
[Subject] ─── Subject_Name(PK), Credit_Points, Marks
[Transcript] ─ Roll_Number(FK), Semester_Number, SWA
```

**Relationships:**
- Student **enrolls in** Course → Many-to-Many
- Student **has marks for** Subject → One-to-Many
- Student **has** Transcript → One-to-One per semester

**Part (b) — Context Diagram (Level 0)**

```
                    ┌─────────────┐
  Student ────────> │  Academic    │ ────────> Student
  (registration,    │  Record      │  (report card,
   course selection)│  Management  │   SWA, VC status)
                    │  System      │
  Faculty ────────> │              │
  (marks)           └─────────────┘
```

**DFD Level 1 — Processes:**
```
P1: Manage Courses      → reads/writes Course DB
P2: Admit Students      → reads/writes Student DB
P3: Key in Marks        → reads/writes Marks DB
P4: Calculate SWA       → reads Marks DB, writes SWA
P5: Calculate Weighted  → reads Previous Marks + Current, outputs Weighted Avg
     Average
P6: Format & Print      → reads SWA + Marks, generates Report
P7: Check VC List       → reads SWA (≥85?), outputs VC List Status
P8: Check Conditional   → reads SWA (<50?), outputs Conditional Standing
     Standing
```

> **🔑 IMPORTANT:** Each process must show input data flows, output data flows, and which data stores they interact with.

---

## Q13 — Online Shopping Platform: DFD + Structure Chart
**📌 Paper:** C2 '23-24 (Slot C2+TC2) | **Module:** 3/4 | **Marks:** 10 (6+4) | **CO3, BL2**

### Question
Online shopping platform: browse products, add to cart, place orders, manage accounts, process payments via gateways. Admins manage listings, users, orders. Data stored in dedicated databases.
- DFD Level 0 and Level 1. (6 marks)
- Convert DFD into structure chart. (4 marks)

### Detailed Solution

**DFD Level 0 (Context Diagram):**
```
  Customer ──────> ┌──────────────────┐ ──────> Customer
  (browse, order,  │  Online Shopping  │  (confirmations,
   payment info)   │     System        │   order status)
                   └──────────────────┘
  Admin ──────────>         │          ──────> Admin
  (product mgmt,            │           (reports)
   user mgmt)               │
                    Payment Gateway
```

**DFD Level 1:**
```
P1: Browse Products    ← Product DB
P2: Manage Cart        ← Cart DB
P3: Place Order        → Order DB
P4: Process Payment    ↔ Payment Gateway
P5: Manage Account     ↔ User DB
P6: Admin - Manage     → Product DB, User DB, Order DB
     Products/Users
```

> **🔑 IMPORTANT:** DFD Level 0 = 2 marks, Level 1 = 4 marks. Make sure every process has labeled data flows.

**Structure Chart (from DFD — Transform/Transaction Mapping):**

> **🔑 IMPORTANT:** Structure chart shows module hierarchy with control flow. Use transform flow for data processing centers and transaction flow for branching paths.

```
              Online Shopping System
              ┌──────┼──────────┐
              │      │          │
         Input    Transform    Output
         Branch   Center       Branch
              │      │          │
        ┌─────┼──┐   │    ┌────┼────┐
        │     │  │   │    │    │    │
    Browse  Cart Order Payment Confirm Notify
    Products Mgmt Process Process  Order  User
```

---

# 🔵 GROUP 5: Black-Box Testing

> **⚠️ IMPORTANT:** BVA and ECP appear in EVERY paper. Always structure your answer as a clear table with Input, Expected Output, and Rationale columns.

---

## Q14 — E-Commerce Checkout + Banking Transfer Testing
**📌 Paper:** C1 '23-24 (Slot C1+TC1) | **Module:** 5 | **Marks:** 10 (4+6) | **CO4, BL3**

### Question
a) Testing checkout process of an e-commerce website. (4 marks)
b) Black box testing (BVA + ECP) for a banking application's fund transfer functionality. (6 marks)

### Detailed Solution

**Part (a) — Testing Checkout Process (4 marks)**

> **🔑 IMPORTANT:** List at least 4-5 testing approaches. You can use EITHER the practical approach or the textbook theory approach.

**Practical Approach:**
1. **Functional testing** — verify each step: add to cart → shipping info → payment → order confirm
2. **Cross-browser/device testing** — test on desktop, mobile, tablets, multiple browsers
3. **Payment method testing** — credit/debit cards, PayPal, Apple Pay
4. **Form validation testing** — invalid emails, addresses, card numbers; verify error messages
5. **Load testing** — simulate heavy traffic during peak times
6. **Security testing** — HTTPS, SSL, encrypted data
7. **Order confirmation** — verify email sent, order recorded correctly
8. **User acceptance testing** — real user feedback

**Alternative Textbook Answer (10 steps for WebApp testing):**
1. Content model reviewed for errors
2. Interface model reviewed for use case coverage
3. Design model reviewed for navigation errors
4. UI tested for presentation/navigation errors
5. Each functional component unit tested
6. Navigation architecture tested
7. Environmental configuration testing
8. Security testing
9. Performance testing
10. Controlled end-user testing

**Part (b) — Banking Transfer: BVA + ECP (6 marks)**

> **🔑 IMPORTANT:** State your assumptions first, then show BVA table, then ECP table.

**Assumptions:**
- Transfer within same bank only
- Min transfer: $1, Max transfer: $10,000
- Account types: savings and checking
- Must have sufficient funds

**Boundary Value Analysis:**

| Parameter | Boundary | Test Value | Expected |
|-----------|----------|:----------:|----------|
| Transfer Amount | At minimum | `$1` | Success |
| Transfer Amount | Just above min | `$2` | Success |
| Transfer Amount | Nominal | `$500` | Success |
| Transfer Amount | Just below max | `$9,999` | Success |
| Transfer Amount | At maximum | `$10,000` | Success |
| Transfer Amount | Above max | `$10,001` | **Error** |
| Transfer Amount | Below min | `$0` | **Error** |
| Account Balance | Zero | `$0` transfer attempt | **Error** |

**Equivalence Class Partitioning:**

| Class | Type | Scenario | Expected |
|:-----:|:----:|----------|:--------:|
| EC1 | ✅ Valid | Own savings → own checking | Success |
| EC2 | ✅ Valid | Own checking → own savings | Success |
| EC3 | ✅ Valid | To another user's savings | Success |
| EC4 | ❌ Invalid | Amount < $1 | Error |
| EC5 | ❌ Invalid | Amount > $10,000 | Error |
| EC6 | ❌ Invalid | Insufficient funds | Error |
| EC7 | ❌ Invalid | Invalid account number format | Error |

---

## Q15 — Subscription Cost Calculator: Test Case Design
**📌 Paper:** C2 '23-24 (Slot C2+TC2) | **Module:** 5 | **Marks:** 10 | **CO4, BL5**

### Question
```javascript
function calculateYearlySubscriptionCost(numberOfUsers, planType) {
    let costPerUser = planType === 'basic' ? 100 : 150;
    let discount = numberOfUsers > 100 ? (planType === 'basic' ? 0.1 : 0.15) : 0;
    return numberOfUsers * costPerUser * (1 - discount);
}
```
Write five comprehensive test cases covering all scenarios including boundary and edge cases.

### Detailed Solution

> **🔑 IMPORTANT:** Marking rubric: 5 marks for scenario coverage (1 each), 3 marks for correct expected outputs (0.5 each), 2 marks for rationale + error handling. Cover BOTH plans, threshold boundary, and invalid inputs.

**Understanding the function logic:**
```
Plan 'basic':   $100/user/year,  10% discount if >100 users
Plan 'premium': $150/user/year,  15% discount if >100 users
Discount threshold: >100 users (NOT ≥100, so exactly 100 gets NO discount)
```

**Test Cases:**

| TC | numberOfUsers | planType | Calculation | Expected | Rationale |
|:--:|:------------:|:--------:|-------------|:--------:|-----------|
| 1 | `50` | `'basic'` | `50 × 100 × (1-0) = 5000` | `5000` | Basic plan, no discount (below threshold) |
| 2 | `101` | `'premium'` | `101 × 150 × (1-0.15) = 12877.5` | `12877.5` | Premium discount applied just above threshold |
| 3 | `100` | `'basic'` | `100 × 100 × (1-0) = 10000` | `10000` | **Edge case:** exactly AT threshold → NO discount |
| 4 | `-1` | `'premium'` | Invalid input | `Error/0` | Negative user count — tests error handling |
| 5 | `50` | `null` | Invalid planType | `Error` | Missing/invalid planType — tests robustness |

> **🔎 Key Insight for TC3:** The condition is `numberOfUsers > 100` (strictly greater than), so 100 users get NO discount. This is a classic **boundary value** trap — many students incorrectly apply discount at 100.

---

# 🔵 GROUP 6: Design Concepts (Abstraction, Refinement, Transform/Transaction Flow, UI)

---

## Q16 — UI Design Principles
**📌 Paper:** G1 '24-25 (Slot G1+TG1) | **Module:** 4 | **Marks:** 6 | **CO3, BL4**

### Question
List out any four key aspects (principles, design process and elements) of UI Design.

### Detailed Solution

> **🔑 IMPORTANT:** They want aspects from ALL THREE categories. Give 2 principles + 2 design process + 2 elements = 6 aspects for full marks.

**Principles (any 2 — 2 marks):**
| Principle | Description |
|-----------|-------------|
| **Clarity** | Interface should be easy to understand and navigate |
| **Consistency** | Use consistent colors, fonts, icons throughout |
| **Efficiency** | Enable users to accomplish tasks quickly |
| **Forgiveness** | Allow easy recovery from errors |
| **Accessibility** | Usable by people with disabilities |

**Design Process (any 2 — 2 marks):**
| Step | Description |
|------|-------------|
| **User Research** | Understanding target audience and needs |
| **Wireframing** | Creating low-fidelity sketches of layout |
| **Prototyping** | Developing interactive mockups |
| **Visual Design** | Applying visual elements to final design |
| **Usability Testing** | Evaluating the interface with real users |

**Key Elements (any 2 — 2 marks):**
| Element | Description |
|---------|-------------|
| **Layout** | Arrangement of elements on screen |
| **Typography** | Choice and use of fonts |
| **Color** | Selection and application of colors |
| **Navigation** | System for moving between parts of the interface |

---

## Q17 — Food Delivery (Transaction Flow) + Payment System (Abstraction vs Refinement)
**📌 Paper:** G2 '24-25 (Slot G2+TG2) | **Module:** 4 | **Marks:** 10 (5+5) | **CO3, BL3**

### Question
a) Food Delivery workflow: customer orders → payment verified → order confirmed → sent to restaurant → prepared → delivery agent assigned → picked up → delivered. Transaction flow or transform flow? Justify. (5 marks)

b) Payment system design: Method 1 creates general Payment class with CreditCard, PayPal, UPI subclasses. Method 2 starts with single PaymentProcessor module, later divided into handlers. Which uses abstraction? Which uses refinement? Which is better? (5 marks)

### Detailed Solution

**Part (a) — Transaction Flow (5 marks: 3 for DFD + 2 for justification)**

> **🔑 IMPORTANT:** Transaction flow = single input triggers one of several action paths. Transform flow = input → processing center → output.

**Answer: Transaction Flow**

**DFD for the workflow:**
```
Customer → [Place Order] → [Verify Payment] → [Confirm Order]
                                                      │
                                            ┌─────────┴─────────┐
                                            ▼                   ▼
                                    [Send to Restaurant]  [Reject Order]
                                            │
                                            ▼
                                    [Prepare Order]
                                            │
                                            ▼
                                    [Assign Delivery Agent]
                                            │
                                            ▼
                                    [Pick Up & Deliver]
                                            │
                                            ▼
                                        Customer
```

**Justification (write all 3 for 2 marks):**
1. Processes **discrete, independent transactions** (each order is independent)
2. Each transaction follows a specific sequence but the system handles **multiple transactions concurrently**
3. Control flow **branches** based on transaction details (e.g., payment valid/invalid, restaurant availability)

**Part (b) — Abstraction vs Refinement (5 marks: 1+1+1+2)**

| Aspect | Method 1 | Method 2 |
|--------|----------|----------|
| **Concept** | **Abstraction** (1 mark) | **Refinement** (1 mark) |
| **How it works** | General `Payment` class hides implementation; subclasses (`CreditCard`, `PayPal`, `UPI`) provide specifics | Single `PaymentProcessor` module progressively decomposed into specialized handlers |
| **Better for adding new payments?** | ✅ **YES** (1 mark) | ❌ No |

**Justification for Abstraction being better (2 marks):**
1. Follows **Open/Closed Principle** — system is open for extension (add `BitcoinPayment` subclass) but closed for modification (existing code unchanged)
2. Leverages **polymorphism** — unified `Payment` interface, each subclass implements its own `process()` method
3. **Minimizes bug risk** — adding new payment methods doesn't touch existing working code

---

# 🔵 GROUP 7: Requirements Engineering & Risk Management

---

## Q18 — E-Commerce: Volatile Requirements + Requirement Validation
**📌 Paper:** G1 '24-25 (Slot G1+TG1) | **Module:** 3 | **Marks:** 10 (7+3) | **CO3, BL4**

### Question
Style Sphere e-commerce platform facing challenges with customer acquisition, building trust, supply chain. Identify sources of changing requirements with factors and explain types of volatile requirements. (7 marks) Do we need to implement requirement validation techniques? Justify. (3 marks)

### Detailed Solution

**Part 1 — Sources of Changing Requirements (3 marks)**

> **🔑 IMPORTANT:** Link each requirement change directly to the scenario's business challenges.

| # | Requirement Change | Factor |
|:-:|-------------------|--------|
| 1 | **Niche Focus & Differentiation** | Need to stand out from competitors; add unique features like sustainability ratings, carbon footprint tracking |
| 2 | **Content Marketing & Storytelling** | Build trust through transparency; add supplier stories, fabric sourcing details |
| 3 | **Data-Driven Optimization** | Customer acquisition requires analytics; add recommendation engine, A/B testing |
| 4 | **Strategic Partnerships** | Supply chain issues need partnerships; integrate with ethical suppliers, logistics providers |

**Types of Volatile Requirements (4 marks — 1 each):**

| Type | Definition | Example from Scenario |
|------|-----------|----------------------|
| **Mutable** | Requirements that change due to evolving environment | Customer preferences for sustainable fashion change with trends |
| **Emergent** | Requirements that emerge as understanding grows | Need for supply chain transparency feature discovered during development |
| **Consequential** | Requirements arising from introducing the system | After launching e-commerce, need for customer support chat emerges |
| **Compatibility** | Requirements depending on other systems/processes | Integration with new payment gateways or shipping APIs |

**Part 2 — Requirement Validation (3 marks)**

**Answer: YES, requirement validation is necessary.**

> **🔑 IMPORTANT:** Validation ensures requirements are complete, consistent, and correct BEFORE development begins.

**Why validation is needed:**
- All requirements have been stated **unambiguously**
- **Inconsistencies, omissions, and errors** have been detected and corrected
- Work products conform to **established standards**

**Validation Techniques:**
1. **Requirements Reviews** — stakeholders review requirements document
2. **Prototyping** — build quick prototype to validate understanding
3. **Test-case Generation** — write test cases early to verify requirements are testable
4. **Automated Consistency Analysis** — tool-based checks for contradictions

---

## Q19 — Antivirus Software: Requirements Model Elements
**📌 Paper:** G1 '24-25 (Slot G1+TG1) | **Module:** 3 | **Marks:** 10 | **CO3, BL6**

### Question
ABC Company needs antivirus software with: 1) Core Detection (signature, heuristic, ML), 2) Real-time monitoring, 3) Auto malware removal, 4) Low resource consumption, 5) Daily updates, 6) Attractive UI, 7) Code security, 8) Legal considerations. Build the software model based on elements of the requirements model.

### Detailed Solution

> **🔑 IMPORTANT:** The "requirements model" has THREE elements. You need to draw ONE diagram for each: scenario-based (3m), behavioral (3m), flow-oriented (4m).

**1. Scenario-based Elements — Use Case Diagram (3 marks)**

```
┌────────────────────────────────────────────────┐
│            Antivirus System                     │
│                                                │
│  ┌──────────────────┐  ┌───────────────────┐    │
│  │ Run Full Scan    │  │ Run Quick Scan     │    │
│  └──────────────────┘  └───────────────────┘    │
│  ┌──────────────────┐  ┌───────────────────┐    │
│  │ Real-time Monitor│  │ Update Definitions │    │ ←── Update Server
│  └──────────────────┘  └───────────────────┘    │
│  ┌──────────────────┐  ┌───────────────────┐    │
│  │ Remove Malware   │  │ View Reports       │    │
│  └──────────────────┘  └───────────────────┘    │
│  ┌──────────────────┐                          │
User ──>│ Configure Settings│                          │
│  └──────────────────┘                          │
└────────────────────────────────────────────────┘
```

**2. Behavioral Elements — State Transition Diagram (3 marks)**

```
         ┌──────┐
         │ Idle │◄──────────────────────────────┐
         └──┬───┘                               │
            │ [user clicks scan / scheduled]    │
            ▼                                   │
       ┌─────────┐                              │
       │Scanning │──[signature check]──►        │
       │         │──[heuristic check]──►        │
       │         │──[ML detection]──►           │
       └────┬────┘                              │
            │                                   │
    ┌───────┴────────┐                          │
    ▼                ▼                          │
[No Threat]   [Threat Detected]                 │
    │                │                          │
    │                ▼                          │
    │         ┌─────────────┐                   │
    │         │Quarantining │                   │
    │         └──────┬──────┘                   │
    │                ▼                          │
    │         ┌─────────────┐                   │
    │         │  Cleaned    │───────────────────┘
    │         └─────────────┘
    └───────────────────────────────────────────┘
```

**3. Flow-oriented Model — Data Flow Diagram / Activity (4 marks)**

```
[File Input] → [Signature-Based Detection]
                       │
                  match found?
                 /           \
               Yes            No
                │              ▼
                │     [Heuristic Analysis]
                │              │
                │         suspicious?
                │        /          \
                │      Yes           No
                │       │             ▼
                │       │     [ML-Based Detection]
                │       │             │
                │       │        threat?
                │       │       /      \
                │       │     Yes      No
                ▼       ▼      ▼        ▼
           [Quarantine File]      [Allow File]
                │
                ▼
           [Auto Remove Malware]
                │
                ▼
           [Log & Report]
```

---

## Q20 — GFC Risk Management Plan
**📌 Paper:** G1 '24-25 (Slot G1+TG1) | **Module:** 2 | **Marks:** 10 | **CO2, BL6**

### Question
Global Finance Corp. experienced a cyberattack — data breach, system disruption, financial loss, reputation damage. As Risk Management Team Head, classify risks and prepare a Risk Management plan.

### Detailed Solution

> **🔑 IMPORTANT:** Risk Management has 4 phases. 2 marks for the framework + 2 marks each for the 4 sub-topics = 10 marks.

**Risk Management Plan:**

**1. Risk Identification (2 marks)**
| Risk Category | Specific Risks |
|--------------|----------------|
| **Data Risk** | Customer financial data exposed, personal information leaked |
| **Service Risk** | Transaction processing systems compromised, service downtime |
| **Network Risk** | Firewall vulnerability exploited, lateral movement through network |

**2. Risk Analysis (2 marks)**
| Area | Analysis |
|------|----------|
| **Vulnerability Management** | Identify and patch firewall vulnerability that was exploited |
| **Access Controls** | Assess how attackers moved laterally — review access permissions |
| **Incident Response** | Evaluate response time and effectiveness of current incident response |
| **Data Security** | Assess encryption levels and data protection mechanisms |

**3. Risk Planning (2 marks)**
| Strategy | Details |
|----------|---------|
| **Invest in Security** | Allocate budget for advanced firewall, intrusion detection systems |
| **Proactive Monitoring** | Implement 24/7 network monitoring with anomaly detection |
| **Contingency Planning** | Create disaster recovery plan, backup systems, and business continuity plan |

**4. Risk Monitoring (2 marks)**
| Action | Implementation |
|--------|---------------|
| **Strengthen Data Security** | Encrypt all sensitive data at rest and in transit; implement multi-factor authentication |
| **Improve Incident Response** | Regular penetration testing, incident response drills, updated response playbook |

---

## Q21 — E-Learning Platform: Risk Identification + Risk Information Sheet
**📌 Paper:** G2 '24-25 (Slot G2+TG2) | **Module:** 2 | **Marks:** 10 (5+5) | **CO2, BL4**

### Question
E-learning platform with interactive courses, live classes, automated tests, progress tracking. Supports 10,000 concurrent users. Tight deadline, needs high performance, scalability, security.
i. Identify five potential risks, their type, and likelihood. (5 marks)
ii. Create a detailed Risk Information Sheet for one risk. (5 marks)

### Detailed Solution

**Part (i) — Five Risks (1 mark each)**

| # | Risk | Type | Likelihood |
|:-:|------|:----:|:----------:|
| 1 | Performance bottlenecks due to high concurrency | Technical | High |
| 2 | Security vulnerabilities (data breaches) | Technical | High |
| 3 | Underestimating dev time for third-party API integrations | Estimation | Medium |
| 4 | Changing client requirements after development starts | Requirement | High |
| 5 | Timeline delays due to unforeseen complexities | Project | High |

**Part (ii) — Risk Information Sheet (5 marks)**

> **🔑 IMPORTANT:** This is a structured template. Include ALL fields for full marks.

| Field | Details |
|-------|---------|
| **Risk ID** | R-0001 |
| **Risk Name** | Performance Bottlenecks Due to High Concurrency |
| **Category/Type** | Technical |
| **Description** | Platform must support 10,000 concurrent users. Without optimized architecture, this could cause slow response times, server crashes, or degraded UX during live classes and assessments. |
| **Likelihood** | High |
| **Impact** | High |
| **Risk Triggers** | • Sudden user spikes during peak hours • Inefficient database queries • Poorly optimized backend logic • Network congestion |
| **Mitigation Strategies** | • Implement load balancing across multiple servers • Use horizontal scaling with cloud auto-scaling • Optimize database queries + implement caching • Conduct stress testing during development • Use CDN for reduced latency |
| **Contingency Plan** | • Deploy failover servers • Real-time monitoring with alerting • Dedicated support team during peak hours |

---

# 🧠 Quick Reference: What to Expect per Question Slot

> Based on all 4 papers, here's the typical pattern:

| Q# | Typical Topic | Module | Marks |
|:--:|--------------|:------:|:-----:|
| Q1 | Risk Management OR UML/DFD (scenario) | 2/3 | 10 |
| Q2 | Requirements (volatile, validation) OR UML/DFD | 3 | 10 |
| Q3 | Architecture + Coupling + Cohesion (scenario) | 4 | 10 |
| Q4 | Design Concepts OR Cohesion ID + UI Design | 4 | 10 |
| Q5 | **Cyclomatic Complexity + Basis Path + Test Cases** | 5 | 10 |

> **💡 Q5 is ALWAYS testing (CC + basis path).** Prepare this first — it's the most predictable and formulaic question.

---

*Generated from analysis of BCSE301L CAT-2 papers: C1 '23-24, C2 '23-24, G1 '24-25, G2 '24-25*
