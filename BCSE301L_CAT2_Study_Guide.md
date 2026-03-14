# 🎯 BCSE301L — Software Engineering CAT-2 Study Guide
### Modules 3, 4 & 5 | Based on PYQs (2023-24, 2024-25) & Faculty Materials

---

# 📊 Exam Strategy & Topic Priority

> **Exam Format:** 5 questions × 10 marks each = 50 marks | Duration: 90 min
> **Coverage:** Module 3 (Requirements & System Modeling), Module 4 (Design Concepts), Module 5 (Testing)

## PYQ Frequency Heat Map

| Topic | C1 '23-24 | C2 '23-24 | G1 '24-25 | G2 '24-25 | Frequency | Tier |
|-------|:---------:|:---------:|:---------:|:---------:|:---------:|:----:|
| **Cyclomatic Complexity + CFG** | ✅ | ✅ | ✅ | ✅ | **4/4** | 🔴 |
| **Cohesion (types & identification)** | ✅ | ✅ | ✅ | ✅ | **4/4** | 🔴 |
| **Coupling (types & identification)** | ✅ | ✅ | ✅ | ✅ | **4/4** | 🔴 |
| **Basis Path Testing** | ✅ | ✅ | ✅ | ✅ | **4/4** | 🔴 |
| **BVA & Equivalence Class Partitioning** | ✅ | ✅ | ✅ | ✅ | **4/4** | 🔴 |
| **Architectural Design (style identification)** | ✅ | ✅ | — | ✅ | **3/4** | 🔴 |
| **UML Diagrams (Class, Use Case, Sequence)** | ✅ | ✅ | — | — | **2/4** | 🔴 |
| **DFD (Level 0 & Level 1)** | ✅ | ✅ | — | ✅ | **3/4** | 🔴 |
| **Requirements Model Elements** | — | — | ✅ | — | **1/4** | 🟡 |
| **Requirement Validation Techniques** | — | — | ✅ | — | **1/4** | 🟡 |
| **Abstraction vs Refinement** | — | — | — | ✅ | **1/4** | 🟡 |
| **Transform vs Transaction Flow** | — | — | — | ✅ | **1/4** | 🟡 |
| **UI Design Principles** | — | — | ✅ | — | **1/4** | 🟡 |
| **Web App / E-commerce Testing** | ✅ | — | — | — | **1/4** | 🟡 |
| **Volatile Requirements** | — | — | ✅ | — | **1/4** | 🟡 |
| **Test Case Design for Functions** | — | ✅ | — | — | **1/4** | 🟡 |
| **ER Diagrams** | ✅ | — | — | — | **1/4** | 🟢 |
| **Structure Charts (DFD mapping)** | — | ✅ | — | — | **1/4** | 🟢 |
| **Requirement Management in Agile** | — | — | — | — | **0/4** | 🟢 |
| **Mutation Testing / Mobile App Testing** | — | — | — | — | **0/4** | 🟢 |
| **DevOps Testing / Cloud & Big Data Testing** | — | — | — | — | **0/4** | 🟢 |

## ⚡ Recommended Study Order (Time-Boxed)

| Priority | Time Allocation | Topics |
|----------|:--------------:|--------|
| **1st** | ~40% | Cyclomatic Complexity, CFG, Basis Path Testing, Independent Paths |
| **2nd** | ~25% | Cohesion & Coupling (all types with examples) |
| **3rd** | ~15% | BVA, Equivalence Class Partitioning, Test Case Design |
| **4th** | ~10% | Architectural Design, DFD, UML Diagrams |
| **5th** | ~10% | Requirements Model, Abstraction/Refinement, UI Design |

---

# 📚 Core Concepts (Summarized)

---

## 🔴 TIER 1 — Must-Know Topics

---

### 1. Cyclomatic Complexity & Control Flow Graph (CFG)

> **Key Takeaway:** This is the single most important topic — appears in EVERY paper, always worth 4–5 marks.

**What is Cyclomatic Complexity?**
A software metric that measures the number of **linearly independent paths** through a program's source code. Higher complexity = more test cases needed = harder to maintain.

**Three Methods to Calculate:**

```
Method 1: V(G) = E - N + 2P
  where E = edges, N = nodes, P = connected components (usually 1)

Method 2: V(G) = Number of regions in the CFG
  (count all enclosed regions + 1 for the outer region)

Method 3: V(G) = Number of predicate (decision) nodes + 1
  (predicate node = node with more than one outgoing edge)
```

**How to Draw a CFG:**
1. Number each statement in the code
2. Each statement becomes a **node**
3. Draw **edges** showing control flow between nodes
4. Decision points (if, for, while) have **two outgoing edges** (true/false)
5. Merge points where control flow rejoins become single nodes

**Quick Check Table:**

| Decision Constructs | Adds to Complexity |
|---|---|
| `if` statement | +1 |
| `else if` | +1 |
| `for` loop | +1 |
| `while` loop | +1 |
| `switch case` (each case) | +1 per case |
| `&&` or `\|\|` in condition | +1 each |

**Example — Factorial Function (from G1 '24-25 PYQ):**
```python
def factorial(n):        # node 1
    if n == 0:           # node 2 (decision)
        return 1         # node 3
    else:
        result = 1       # node 4
        for i in range(1, n+1):  # node 5 (decision)
            result *= i  # node 6
        return result    # node 7
```
- Decision nodes: `if n == 0` and `for` loop = **2 predicate nodes**
- **V(G) = 2 + 1 = 3**

---

### 2. Basis Path Testing

> **Key Takeaway:** Always asked alongside cyclomatic complexity. You MUST identify independent paths AND design test cases for each.

**Step-by-Step Process:**
1. **Draw the CFG** from the code
2. **Calculate Cyclomatic Complexity** V(G) — this tells you the number of independent paths
3. **Identify Basis Set of Paths** — each path must traverse at least one new (untested) edge
4. **Generate Test Cases** — one test case per independent path

**Rules for Basis Paths:**
- Total number of basis paths = V(G)
- Each path must include at least **one new edge** not covered by previous paths
- Some paths may not be testable standalone (need to be tested as part of another path)

**Graph (Connection) Matrix Method:**
- Square matrix with rows/columns = nodes
- Cell value = 1 if edge exists, 0 otherwise
- For each row: sum column values − 1
- Sum all row values + 1 = Cyclomatic Complexity

---

### 3. Cohesion (Types, from Best to Worst)

> **Key Takeaway:** You will be given code snippets and asked to IDENTIFY the type of cohesion. Always justify whether it should be high or low.

| Level | Type | Description | Quality |
|:-----:|------|-------------|:-------:|
| 7 | **Functional** | All elements contribute to a **single, well-defined task** | **Best** ✅ |
| 6 | **Sequential** | Output of one element is the **input to the next** (pipeline) | Good |
| 5 | **Communicational** | Elements operate on the **same data** | Good |
| 4 | **Procedural** | Elements are related by **order of execution** | Medium |
| 3 | **Temporal** | Elements are executed at the **same time** (e.g., initialization) | Medium |
| 2 | **Logical** | Elements are logically related but do **different things** (e.g., all I/O routines) | Poor |
| 1 | **Coincidental** | Elements have **no logical relationship** — grouped arbitrarily | **Worst** ❌ |

**Exam-Critical Examples (from PYQs):**

```python
# FUNCTIONAL COHESION (High, Best) — Single well-defined task
def calculate_average(numbers):
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)
```

```python
# COINCIDENTAL COHESION (Low, Worst) — No logical relationship
class Utility:
    def print_message(self, message): ...
    def calculate_square(self, number): ...
    def read_file(self, filename): ...
```

---

### 4. Coupling (Types, from Best to Worst)

> **Key Takeaway:** Coupling is about the **inter-dependency between modules**. Low coupling is ALWAYS desirable.

| Level | Type | Description | Quality |
|:-----:|------|-------------|:-------:|
| 1 | **Data Coupling** | Modules communicate via **simple data parameters** only | **Best** ✅ |
| 2 | **Stamp Coupling** | Modules pass **structured data** (e.g., objects, records) but may not use all fields | Good |
| 3 | **Control Coupling** | One module passes a **flag/control parameter** that directs another module's logic | Medium |
| 4 | **Common/Global Coupling** | Modules share **global data** | Poor |
| 5 | **Content Coupling** | One module directly **accesses/modifies internal data** of another | **Worst** ❌ |

**Exam-Critical Examples (from G2 '24-25 — Library System):**

| Scenario | Coupling Type |
|----------|--------------|
| User Management passes `book_id` to Book Management | **Data Coupling** (simple parameter) |
| Borrowing Module passes user contact details, book title, due date to Notification Module | **Stamp Coupling** (structured data) |
| Borrowing Module directly accesses database tables of Book Management | **Content Coupling** (worst!) |
| Report Generation retrieves JSON records from other modules | **Data Coupling** |

---

### 5. Black-Box Testing: BVA & Equivalence Class Partitioning

> **Key Takeaway:** Almost always a 6-mark question. You must show both BVA and ECP with clear test case tables.

**Boundary Value Analysis (BVA):**
Test at the **edges** of equivalence classes:
- Minimum value
- Just above minimum
- Nominal value
- Just below maximum
- Maximum value
- Just beyond boundaries (invalid)

**Equivalence Class Partitioning (ECP):**
Divide input domain into **partitions** where all values in a partition are expected to behave the same.
- **Valid** classes → expected to produce correct results
- **Invalid** classes → expected to produce errors

**Template for Answer:**
```
Step 1: Identify input parameters and their ranges
Step 2: Define equivalence classes (valid + invalid)
Step 3: For BVA — pick boundary values from each class
Step 4: Create test case table with:
  - Test Case ID
  - Input Values
  - Expected Output
  - Rationale
```

---

### 6. Architectural Design Styles

> **Key Takeaway:** Given a scenario, IDENTIFY the style and JUSTIFY it. Most common: Layered, Event-Driven, Client-Server.

| Style | When to Use | Key Characteristics |
|-------|-------------|---------------------|
| **Layered** | Enterprise apps, separation of concerns needed | Presentation → Business Logic → Data Access |
| **Event-Driven (Interrupt-Driven)** | IoT, real-time systems, asynchronous events | Event handlers, callbacks, minimal CPU overhead |
| **Client-Server** | Web apps, multi-user systems | Server provides services, clients consume |
| **Pipe & Filter** | Data processing pipelines | Data flows through sequential filters |
| **Repository** | Systems with shared data (databases, IDEs) | Central data store accessed by multiple components |
| **MVC** | Web apps with UI | Model, View, Controller separation |

---

### 7. UML Diagrams & System Modeling

> **Key Takeaway:** Be ready to draw Use Case, Sequence, Class, Activity, and State diagrams for any scenario.

**Use Case Diagram Elements:**
- **Actors** (stick figures) — external entities interacting with the system
- **Use Cases** (ovals) — functionalities the system provides
- **Relationships:** Include, Extend, Generalization
- System boundary box

**Sequence Diagram Elements:**
- **Lifelines** — vertical dashed lines for each object
- **Messages** — horizontal arrows (solid = sync, dashed = return)
- **Activation bars** — rectangles on lifelines showing active periods

**Class Diagram Elements:**
- **Classes** — rectangles with 3 compartments (name, attributes, methods)
- **Relationships:** Association, Aggregation (hollow diamond), Composition (filled diamond)
- **Multiplicity:** 1, 0..1, *, 1..*

**Key UML Summary (from faculty material):**

| Diagram | Purpose | When Asked |
|---------|---------|------------|
| **Use Case** | High-level system functionality, actor interactions | "Model the interaction between users and system" |
| **Sequence** | Chronological event flow through a use case | "Model the interaction among objects using lifeline" |
| **Class** | Static structure - classes, attributes, relationships | "Draw a static structure diagram" |
| **Activity** | Workflow / business process | "Model the workflow" |
| **State** | Object states and transitions | "Model dynamic behavior" |
| **Component** | Physical architecture | "Show system components" |
| **Deployment** | Hardware-software mapping | "Show deployment topology" |

---

### 8. Data Flow Diagrams (DFD)

> **Key Takeaway:** Context Diagram (Level 0) + Level 1 DFD — a 6-7 mark question.

**Level 0 (Context Diagram):**
- Single process (circle) = the entire system
- External entities (rectangles) around it
- Data flows (arrows) showing input/output

**Level 1 DFD:**
- Break the system into sub-processes
- Show data stores (parallel lines)
- Show detailed data flows between processes

**Elements:**
```
Process         → Circle/Rounded Rectangle
External Entity → Rectangle
Data Store      → Open-ended rectangle (parallel lines)
Data Flow       → Arrow with label
```

---

## 🟡 TIER 2 — Important Topics

---

### 9. Requirements Model Elements

> Three key elements (from G1 '24-25 PYQ, 10 marks):

1. **Scenario-based Elements** → Use Case Diagrams
2. **Behavioral Elements** → State Transition Diagrams
3. **Flow-oriented Model** → DFD / Sequence / Activity Diagrams

For a given scenario, you should be able to draw ONE diagram for each element.

---

### 10. Abstraction vs Refinement

> From G2 '24-25 PYQ (5 marks):

| Concept | Definition | Example |
|---------|-----------|---------|
| **Abstraction** | Creating a general class/interface, hiding implementation details | General `Payment` class → subclasses `CreditCard`, `PayPal`, `UPI` |
| **Refinement** | Starting with a single module, progressively breaking it down into detailed sub-modules | `PaymentProcessor` module → later split into handlers for each method |

**Why Abstraction is Better:**
- Follows **Open/Closed Principle** — open for extension, closed for modification
- Leverages **polymorphism**
- Adding new types doesn't modify existing code
- Lower risk of bugs in existing functionality

---

### 11. Transform Flow vs Transaction Flow

> From G2 '24-25 PYQ (5 marks):

| Type | Description | Example |
|------|------------|---------|
| **Transform Flow** | Data enters → transformed through sequential processing → exits | Data processing pipeline |
| **Transaction Flow** | Single input triggers one of several action paths based on its type | Food delivery: order → verify payment → prepare → assign delivery → deliver |

---

### 12. UI Design Principles (from G1 '24-25)

**Key Principles:** Clarity, Consistency, Familiarity, Efficiency, Responsiveness, Aesthetics, Forgiveness, Accessibility

**Design Process:** User Research → Information Architecture → Wireframing → Prototyping → Visual Design → Testing

**Key Elements:** Layout, Typography, Color, Icons, Buttons, Forms, Navigation

**Design Issues (from faculty material):**
- System Response Time (minimize variability)
- Help Facilities
- Error Handling (clear, constructive, nonjudgmental)
- Menu and Command Labelling
- Application Accessibility
- Internationalization

---

### 13. Volatile Requirements & Requirement Validation

**Volatile Requirements Types:**
1. **Mutable** — change due to environment changes
2. **Emergent** — arise as understanding of system grows
3. **Consequential** — result from introducing the system
4. **Compatibility** — depend on other systems/processes

**Requirement Validation Techniques:**
1. Requirements Reviews
2. Prototyping
3. Test-case Generation
4. Automated Consistency Analysis

---

## 🟢 TIER 3 — Skim Topics

- ER Diagrams (appeared once)
- Structure Charts / DFD-to-Structure-Chart mapping
- Requirement Management in Agile
- Mutation Testing
- Mobile App Testing
- DevOps Testing / Cloud & Big Data Testing
- Inspection and Auditing

---

# ✅ Solved PYQs (Grouped by Topic)

---

## Topic 1: Cyclomatic Complexity + Basis Path Testing

---

### 📝 PYQ 1: Selection Sort (C1 '23-24, 10 marks)

**Code:**
```java
void SelectionSort(int array1[]) {
    int size = array1.length;           // 1
    for (int i = 0; i < size; i++) {    // 2 (decision)
        int minIndex = i;               // 3
        for (int j = i+1; j < size; j++) { // 4 (decision)
            if (array1[j] < array1[minIndex]) // 5 (decision)
                minIndex = j;           // 6
        }
        int temp = array1[minIndex];    // 7
        array1[minIndex] = array1[i];   // 8
        array1[i] = temp;              // 9
    }
}
```

**a) CFG & Cyclomatic Complexity:**

```
Nodes: 1 → 2 → 3 → 4 → 5 → 6 → 7,8,9

CFG Edges:
  1 → 2
  2 → 3  (true: i < size)
  2 → EXIT (false: i >= size)
  3 → 4
  4 → 5  (true: j < size)
  4 → 7  (false: j >= size)
  5 → 6  (true: array1[j] < array1[minIndex])
  5 → 4  (false)
  6 → 4
  7 → 8 → 9 → 2
```

**Predicate (decision) nodes:** node 2 (for i), node 4 (for j), node 5 (if)
```
V(G) = 3 predicate nodes + 1 = 4
```

**b) Path Coverage Test Suite:**

| Path | Description | Test Input | Expected Output |
|------|-------------|-----------|-----------------|
| Path 1 | Empty array — outer loop never executes | `[]` | `[]` |
| Path 2 | Single element — outer loop runs once, inner doesn't | `[5]` | `[5]` |
| Path 3 | Already sorted — inner loop runs but no swap needed | `[1, 2, 3]` | `[1, 2, 3]` |
| Path 4 | Reverse sorted — all paths including swap | `[3, 2, 1]` | `[1, 2, 3]` |

---

### 📝 PYQ 2: Factorial Function (G1 '24-25, 10 marks)

**Code:**
```python
def factorial(n):        # 1
    if n == 0:           # 2 (decision)
        return 1         # 3
    else:
        result = 1       # 4
        for i in range(1, n+1):  # 5 (decision)
            result *= i  # 6
        return result    # 7
```

**a) Cyclomatic Complexity:**
```
Decision points: if (node 2) + for loop (node 5) = 2
V(G) = 2 + 1 = 3

Using E-N+2P: E = 10, N = 9
V(G) = 10 - 9 + 2 = 3
```

**b) Boundary Value Analysis Test Cases:**

| TC | Input | Expected Output | Rationale |
|----|:-----:|:---------------:|-----------|
| 1 | `n = 0` | `1` | Minimum valid input, base case |
| 2 | `n = 1` | `1` | Just above minimum |
| 3 | `n = 5` | `120` | Typical valid input |
| 4 | `n = 10` | `3628800` | Maximum practical input |
| 5 | `n = -1` | Error/Exception | Invalid negative input |

---

### 📝 PYQ 3: Discount Calculator (G2 '24-25, 10 marks)

**Code:**
```c
float calculate_discount(float total_amount, int is_member, char coupon_code[]) {
    if (total_amount <= 0) { return 0; }
    float discount = 0;
    if (is_member) { discount += total_amount * 0.1; }
    if (strcmp(coupon_code, "SAVE20") == 0) {
        discount += total_amount * 0.2;
    } else if (strcmp(coupon_code, "SAVE10") == 0) {
        discount += total_amount * 0.1;
    }
    float max_discount = total_amount * 0.3;
    if (discount > max_discount) { discount = max_discount; }
    return total_amount - discount;
}
```

**i) Cyclomatic Complexity:**
```
Decision points:
  1. if (total_amount <= 0)
  2. if (is_member)
  3. if (coupon == "SAVE20")
  4. else if (coupon == "SAVE10")
  5. if (discount > max_discount)

V(G) = 5 + 1 = 6
```

**ii) Independent Paths:**
```
Path 1: total_amount ≤ 0 → Return 0
Path 2: total_amount > 0, not member, no valid coupon → No discount
Path 3: total_amount > 0, member, no valid coupon → 10% member discount
Path 4: total_amount > 0, member, SAVE20 → 10% + 20% discount
Path 5: total_amount > 0, member, SAVE10 → 10% + 10% discount
Path 6: total_amount > 0, discount > 30% cap → Capped at 30%
```

**iii) Test Cases:**

| TC | total_amount | is_member | coupon_code | Expected | Rationale |
|----|:-----------:|:---------:|:-----------:|:--------:|-----------|
| 1 | `0` | `0` | `"NONE"` | `0` | Zero amount case |
| 2 | `100` | `0` | `"NONE"` | `100` | No discount applied |
| 3 | `100` | `1` | `"NONE"` | `90` | Member discount only (10%) |
| 4 | `100` | `1` | `"SAVE20"` | `70` | Member (10%) + Coupon (20%) |
| 5 | `100` | `1` | `"SAVE10"` | `80` | Member (10%) + Coupon (10%) |
| 6 | `200` | `1` | `"SAVE20"` | `140` | Cap applied (30% max) |

---

### 📝 PYQ 4: Leap Year — Basis Path + ECP (C2 '23-24, 10 marks)

**Pseudocode:**
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

**Basis Paths & Test Cases:**

| Path | Condition | Test Input | Expected |
|------|-----------|:----------:|:--------:|
| Path 1 | Divisible by 4, not by 100 | `2020` | `True` |
| Path 2 | Divisible by 4, by 100, by 400 | `2000` | `True` |
| Path 3 | Divisible by 4, by 100, not by 400 | `1900` | `False` |
| Path 4 | Not divisible by 4 | `1999` | `False` |

**Equivalence Class Partitioning:**

| Class | Description | Example | Expected |
|-------|-------------|:-------:|:--------:|
| Valid Class 1 | Divisible by 4, not by 100 | `2004` | `True` |
| Valid Class 2 | Divisible by 400 | `2000` | `True` |
| Invalid Class 3 | Not divisible by 4 | `2001` | `False` |
| Invalid Class 4 | Divisible by 100, not by 400 | `1900` | `False` |

---

## Topic 2: Cohesion & Coupling (Scenario-Based)

---

### 📝 PYQ 5: Cloud-Based Document Editor (C2 '23-24, 10 marks)

**Scenario:** Design a cloud-based document editing and collaboration tool with real-time editing, change tracking, and integrated chat. Must be scalable with future AI expansion.

**Architecture:** Layered Architecture
```
┌─────────────────────────────────────┐
│      Presentation Layer             │ → Editor UI, Chat UI
├─────────────────────────────────────┤
│      Business Logic Layer           │ → Change tracking, versioning, AI
├─────────────────────────────────────┤
│      Data Access Layer              │ → Cloud storage, document retrieval
└─────────────────────────────────────┘
```

**Three Types of Coupling (for enhanced modularity):**

| Type | Example |
|------|---------|
| **Data Coupling** | Document module passes edited content to version control via standardized format |
| **Stamp Coupling** | Chat module exchanges messages using predefined data structures |
| **External/Event Coupling** | User editing triggers event notification to collaborators |

**Two Types of Cohesion:**

| Type | Example |
|------|---------|
| **Functional** | Document Editing Module handles ONLY editing (add, delete, format text) |
| **Communicational** | Editing & Collaboration modules share document data |

---

### 📝 PYQ 6: Library Management System (G2 '24-25, 10 marks)

**Coupling Identification from scenarios:**

| Scenario | Coupling Type | Justification |
|----------|--------------|---------------|
| User Mgmt passes `book_id` to Book Mgmt | **Data Coupling** | Simple data parameter |
| Borrowing passes user details, title, due date to Notification | **Stamp Coupling** | Structured data record |
| Borrowing directly accesses Book Mgmt's DB tables | **Content Coupling** | Violates encapsulation |
| Report Generation retrieves JSON from modules | **Data Coupling** | Structured query results |

**Cohesion Identification:**

| Module | Cohesion Type | Justification |
|--------|--------------|---------------|
| User Management | **Functional** | Single responsibility: verify credentials, check fines |
| Borrowing Module | **Sequential** | Steps must be executed in order |
| Notification Module | **Logical** | Selects function (email/SMS/push) based on conditions |

---

### 📝 PYQ 7: Code Snippet Cohesion ID (G1 '24-25, 4 marks)

**Snippet (i) — `calculate_average`:**
> **Functional Cohesion (High)** — All elements contribute to a single, well-defined task.

**Snippet (ii) — `Utility` class with `print_message`, `calculate_square`, `read_file`:**
> **Coincidental Cohesion (Low)** — Elements are grouped arbitrarily with no logical relationship. This is the WORST type.

---

## Topic 3: UML Diagrams & DFD

---

### 📝 PYQ 8: Ticket Reservation System (C2 '23-24, 10 marks)

**a) Use Case Diagram:**
```
Actors:
  - Passenger (primary)
  - Payment Gateway (secondary)
  - Admin (secondary)

Use Cases:
  - Search Route
  - Check Seat Availability
  - Select Ticket Type (adult/child/senior)
  - Calculate Fare (includes peak hour pricing)
  - Process Payment
  - Generate Ticket
  - Cancel Booking
```

**b) Sequence Diagram (Ticket Booking):**
```
Passenger → System: Search route
System → Database: Query availability
Database → System: Return seat data
System → Passenger: Display available seats
Passenger → System: Select seat + ticket type
System → System: Calculate fare (distance × type × peak multiplier)
System → Passenger: Display fare
Passenger → System: Confirm booking
System → Payment Gateway: Process payment
Payment Gateway → System: Payment confirmation
System → Passenger: Issue ticket
```

---

### 📝 PYQ 9: Academic Record Management DFD (C1 '23-24, 10 marks)

**Level 0 (Context Diagram):**
```
External Entities: Student, Faculty, Admin
Single Process: Academic Record Management System
Data Flows:
  Student → System: Registration details, Course selection
  Faculty → System: Marks
  System → Student: Report card, SWA, VC List status
```

**Level 1 DFD Processes:**
1. Manage Courses → Course DB
2. Admit Students → Student DB
3. Key in Marks → Marks DB
4. Calculate SWA → uses Marks DB
5. Calculate Weighted Average → uses Previous Marks + Current
6. Format & Print → generates Report
7. Check VC List (SWA ≥ 85)
8. Check Conditional Standing (SWA < 50)

---

## Topic 4: Architectural Design & Design Concepts

---

### 📝 PYQ 10: IoT Smart Home Architecture (C1 '23-24, 5 marks)

**Scenario:** Firmware for IoT device monitoring temperature, humidity, air quality. Sends periodic updates to server.

**Answer:** **Event-Driven Architecture (Interrupt-Driven)**

**Justification:**
- Device needs to handle **asynchronous events** (sensor readings at different intervals)
- **Interrupt-driven** approach minimizes CPU overhead — processor sleeps until an event occurs
- Each sensor can trigger an interrupt independently
- Ideal for real-time monitoring with minimal power consumption
- Supports concurrent handling of multiple sensor types
- Periodic updates can be timer-interrupt driven

---

### 📝 PYQ 11: Abstraction vs Refinement — Payment System (G2 '24-25, 5 marks)

**Method 1 — Abstraction:**
```
Payment (General Class)
  ├── CreditCard
  ├── PayPal
  └── UPI
```
- Hides implementation details
- Provides a high-level **interface**
- Uses **inheritance and polymorphism**

**Method 2 — Refinement:**
```
PaymentProcessor (Single Module)
  → Later split into:
    ├── CreditCardHandler
    ├── PayPalHandler
    └── UPIHandler
```
- Starts general, progressively broken down
- Top-down decomposition

**Which is Better?** → **Abstraction**
- Follows **Open/Closed Principle**: open for extension, closed for modification
- Adding `BitcoinPayment` subclass doesn't change existing code
- Polymorphism provides a unified interface
- Lower risk of introducing bugs

---

### 📝 PYQ 12: Food Delivery — Transform vs Transaction Flow (G2 '24-25, 5 marks)

**Scenario workflow:**
```
Customer places order → Payment verified → Order confirmed →
Sent to restaurant → Prepared → Delivery agent assigned →
Agent picks up → Delivers to customer
```

**Answer:** **Transaction Flow**
- Processes **discrete, independent transactions** (each order)
- Each transaction follows a specific sequence of steps
- Control flow **branches** based on transaction details
- System can handle **multiple transactions concurrently**
- DFD should be drawn showing the data flow through these processes

---

## Topic 5: Testing Techniques

---

### 📝 PYQ 13: Web App Testing — E-Commerce (C1 '23-24, 4 marks)

**Testing Strategy for Checkout Process:**

1. **Functional Testing:** Verify each step — add to cart, shipping info, payment, order confirmation
2. **Cross-browser/device Testing:** Desktop, mobile, tablets, multiple browsers
3. **Payment Testing:** Test all payment methods (credit/debit, PayPal, etc.)
4. **Form Validation:** Invalid emails, addresses, card numbers — verify error messages
5. **Order Confirmation:** Verify order recorded correctly, confirmation email sent
6. **Load Testing:** Simulate heavy traffic during peak times
7. **Security Testing:** HTTPS, SSL, encrypted data storage
8. **User Acceptance Testing:** Real user feedback sessions

**Alternative Theory Answer (from textbook):**
1. Content model reviewed for errors
2. Interface model reviewed for use case coverage
3. Design model reviewed for navigation errors
4. UI tested for presentation/navigation errors
5. Each functional component unit tested
6. Navigation architecture tested
7. Environmental configuration testing
8. Security testing
9. Performance testing
10. End-user controlled testing

---

### 📝 PYQ 14: Banking Transfer — Black Box Testing (C1 '23-24, 6 marks)

**Boundary Value Analysis:**

| Parameter | Boundary | Test Value |
|-----------|----------|:----------:|
| Transfer Amount | Minimum | `$1` |
| Transfer Amount | Just above min | `$2` |
| Transfer Amount | Maximum | `$10,000` |
| Transfer Amount | Just below max | `$9,999` |
| Transfer Amount | Above max (invalid) | `$10,001` |
| Account Balance | Zero balance | `$0` |

**Equivalence Class Partitioning:**

| Class | Type | Scenario | Expected |
|-------|------|----------|----------|
| EC1 | Valid | Savings → Checking (own accounts) | Success |
| EC2 | Valid | To another user's savings | Success |
| EC3 | Valid | To another user's checking | Success |
| EC4 | Invalid | Amount < minimum ($0) | Error |
| EC5 | Invalid | Amount > maximum ($10,001) | Error |
| EC6 | Invalid | Insufficient funds | Error |
| EC7 | Invalid | Invalid account number format | Error |

---

### 📝 PYQ 15: Subscription Cost — Test Case Design (C2 '23-24, 10 marks)

**Function:**
```javascript
function calculateYearlySubscriptionCost(numberOfUsers, planType) {
    let costPerUser = planType === 'basic' ? 100 : 150;
    let discount = numberOfUsers > 100
        ? (planType === 'basic' ? 0.1 : 0.15) : 0;
    return numberOfUsers * costPerUser * (1 - discount);
}
```

**Test Cases:**

| TC | numberOfUsers | planType | Expected | Rationale |
|----|:------------:|:--------:|:--------:|-----------|
| 1 | `50` | `'basic'` | `5000` | Basic cost without discount |
| 2 | `101` | `'premium'` | `12877.5` | Premium discount just above threshold |
| 3 | `100` | `'basic'` | `10000` | Edge case: exactly at discount threshold (no discount) |
| 4 | `-1` | `'premium'` | `Error/0` | Invalid negative user count |
| 5 | `50` | `null` | `Error` | Missing/invalid planType |

---

## Topic 6: Requirements Engineering

---

### 📝 PYQ 16: Antivirus Software — Requirements Model (G1 '24-25, 10 marks)

**Given Requirements:**
1. Core Detection (signature, heuristic, ML)
2. Real-time monitoring (system + web)
3. Automatic malware removal
4. Low resource consumption
5. Daily updates from server
6. Attractive UI
7. Code security
8. Legal considerations

**Answer (Three Model Elements):**

**1. Scenario-based Elements (Use Case Diagram):**
```
Actors: User, Server, Admin
Use Cases:
  - Run Scan (Full/Quick)
  - Real-time Monitoring
  - Update Definitions
  - Remove Malware
  - View Reports
  - Configure Settings
```

**2. Behavioral Elements (State Transition Diagram):**
```
States:
  Idle → Scanning → Threat Detected → Quarantining → Cleaned → Idle
  Idle → Updating → Updated → Idle
  Idle → Monitoring → Threat Blocked → Idle
```

**3. Flow-oriented Model (DFD/Activity Diagram):**
```
File Input → Signature Check → Heuristic Analysis → ML Detection
    → Threat? → Yes → Quarantine → Remove
              → No → Allow
```

---

# 🧠 Quick Revision — Formulas & Mnemonics

## Cyclomatic Complexity (memorize all 3 methods)
```
V(G) = E - N + 2        (edges - nodes + 2)
V(G) = Regions           (enclosed regions + outer)
V(G) = Decisions + 1     (count if/for/while/case)
```

## Cohesion Mnemonic (Best → Worst): "**F**ancy **S**hoes **C**an **P**rovide **T**rue **L**uxury **C**omfort"
```
F - Functional     (BEST)
S - Sequential
C - Communicational
P - Procedural
T - Temporal
L - Logical
C - Coincidental   (WORST)
```

## Coupling Mnemonic (Best → Worst): "**D**ata **S**tamp **C**ontrol **C**ommon **C**ontent"
```
D - Data           (BEST - simple parameters)
S - Stamp          (structured data)
C - Control        (flags/control parameters)
C - Common/Global  (shared global data)
C - Content        (direct internal access - WORST)
```

## BVA Quick Check
```
Always test: min, min+1, nominal, max-1, max, min-1, max+1
```

---

> **💡 Final Exam Tip:** In a 90-minute exam with 5 questions, you have ~18 minutes per question. Start with the question you're most confident about. Draw diagrams neatly — marks are often given for diagram clarity. Always label your diagrams. For cyclomatic complexity, show your working using AT LEAST one method, preferably two for verification.
