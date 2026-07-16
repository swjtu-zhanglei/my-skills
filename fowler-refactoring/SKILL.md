---
name: fowler-refactoring
description: "Knowledge base from Fowler's Refactoring, 2nd Edition: core frameworks, 12 chapter digests, technique index, and smell→technique map. Use when applying refactorings, identifying code smells, or improving design without changing behavior."
---

# Refactoring: Improving the Design of Existing Code (2nd Edition)
**Author**: Martin Fowler | **12 Chapters** | **Source**: book-refactoring2.ifmicro.com

## Core Frameworks

### 1. Definition & Two Hats (Ch1, Ch2)
**Refactoring** = restructuring internal design **without changing observable behavior**.  
As a noun: a named transform (e.g. Extract Function). As a verb: a sequence of small transforms that improve structure.

- **Two Hats**: either add features (don't restructure) or refactor (don't change behavior). Know which hat you wear; never mix them in one step.
- **Boy Scout Rule**: leave the codebase healthier than you found it.

### 2. When to Refactor (Ch2)

| Situation | What to do |
|-----------|------------|
| Preparatory refactoring | Clean up *before* adding a feature so the change stays small |
| Comprehension | If you don't understand it, refactor until you do (code as living comment) |
| Litter-pickup | Opportunistic small cleanups; don't demand perfection |
| **Rule of Three** | First time: just do it. Second: wince. Third: refactor |
| **Do not refactor** | Code that should be rewritten; near ship with no tests / high risk window |

### 3. Safety Protocol (Ch1, Ch4, Ch5)
1. **Self-testing first**: never refactor on a red bar  
2. **Baby steps**: tiny steps; compile/test after each  
3. **Commit cadence**: small steps stay reversible  
4. **Catalog format** (Ch5): Name → Sketch → Motivation → Mechanics → Example  

### 4. Encapsulation & Moving Features (Ch7, Ch8)
- **Encapsulate Record / Collection**: never let clients mutate internals; getters return copies or read-only views  
- **Extract Class / Inline Class**: split oversized classes; merge thin ones  
- **Hide Delegate / Remove Middle Man**: hide long delegation chains; remove pure forwarders  
- **Move Function / Field**: follow the data; module boundaries follow understanding  
- **Split Loop / Replace Loop with Pipeline**: one concern per loop; prefer readable pipelines when clearer  

### 5. Conditionals & APIs (Ch10, Ch11)
- **Guard clauses** instead of deep nesting; surface the happy path  
- **Polymorphism** instead of repeated type switches (when variants are complex—not the default for every `if`)  
- **Introduce Special Case** to collapse null/special branches  
- **Separate Query from Modifier (CQS)**: side-effect-free queries are freely composable  
- **Remove Flag Argument**: split into intent-clear functions  
- **Factory Function** instead of complex constructors  

### 6. Inheritance vs Delegation (Ch12)
- Inheritance fits a **single dimension of variation** when: subclass is a true subtype, and all superclass methods apply  
- **Pull Up / Push Down** methods and fields to match real sharing  
- When inheritance hurts: **Replace Subclass/Superclass with Delegate**  
- **Replace Type Code with Subclasses** when type drives behavior  
- Fowler does *not* preach "inheritance is evil"; "prefer composition" reacts to *abuse*  

### 7. Performance & YAGNI (Ch2)
- Write clear, changeable code first; **profile the ~10% hotspots** (90% rule) before optimizing  
- **YAGNI**: don't add abstractions for imaginary futures; refactor for real needs  
- Performance guesses are usually wrong—**measure, don't speculate**  

---

## Chapter Summaries

### Ch1 — A First Example
**Core idea**: Refactoring is economic, not aesthetic. Good code is code people can change easily.
- Baby-step loop: compile → test → commit  
- Temps breed long functions—remove them, then Extract Function  
- **Split Phase**: intermediate data structure separates calculation from rendering (text/HTML share the same calc)  
- **Replace Conditional with Polymorphism**: when the same type code is switched in many places, introduce inheritance + factory so branches live in one place  
- Key techniques: Extract Function, Split Phase, Replace Conditional with Polymorphism  

### Ch2 — Principles of Refactoring
**Core idea**: The only purpose of refactoring is to make development go faster—economic, not moral/aesthetic.
- Two Hats: add features vs restructure; switch often, wear only one at a time  
- Rule of Three, preparatory refactoring, litter-pickup, Boy Scout Rule  
- YAGNI: high quality for *current* needs, not speculative flexibility  
- CI / trunk-based: branch lifetime measured in days  
- Key techniques: Parameterize Function, expand–contract (parallel change)  

### Ch3 — Bad Smells in Code
**Core idea**: Smells are *hints*, not metrics—they point where a refactoring may help; judgment is trained intuition.
- **Mysterious Name** is the most common trigger; renaming often forces deeper design cleanup  
- **Divergent Change** vs **Shotgun Surgery** are mirrors: one module changes for many reasons vs one change scattered across modules  
- Only *repeated* switches (not every switch) deserve polymorphism  
- Comments are a *fragrance* until used as deodorant—then they signal a smell  
- 24 smells, each with preferred techniques (see Smell→Technique map below)  

### Ch4 — Building Tests
**Core idea**: Solid self-testing code is a prerequisite for refactoring. "Find the bug" shrinks from hours to minutes.
- Fully automated + self-checking tests; red bar → no refactor  
- Fixture isolation: `beforeEach` builds fresh fixtures; never share mutable fixtures  
- Risk-driven: test what can break; skip trivial accessors  
- On a bug: write a failing repro test first; coverage finds untested code—it is not a quality score  
- Key technique: Introduce Assertion  

### Ch5 — Toward a Catalog of Refactorings
**Core idea**: The catalog is a reference, not a tutorial. Each entry has five parts: Name, Sketch, Motivation, Mechanics, Example.
- Mechanics = baby steps + executable checklist  
- Sketch is a memory hook; read the example on first use  
- Catalog is not complete—only the techniques worth recording  
- Automated tools reduce migration mechanics; understanding still matters  

### Ch6 — A First Set of Refactorings
**Core idea**: Eleven highest-leverage techniques from low-level to high-level: intent/implementation → naming/data → combining modules and splitting phases.
- **Extract Function**: criterion is "intent vs implementation," not length or reuse  
- **Inline Function**: remove useless indirection; don't inline polymorphic methods  
- **Encapsulate Variable**: turn "reorganize data" into "reorganize functions"  
- **Introduce Parameter Object**: real value is a new domain concept  
- **Split Phase**: intermediate structure so each phase has one subject  
- Combine Functions into Class (when source data updates) vs Transform (read-only)  
- Also: Extract/Inline Variable, Change Function Declaration, Rename Variable  

### Ch7 — Encapsulation
**Core idea**: Encapsulation layers: data structures → primitives → temps → classes → associations → functions. Immutability often beats encapsulation alone.
- Encapsulate Record/Collection: copies or read-only views  
- **Replace Primitive with Object**: small wrappers grow into useful value objects  
- **Replace Temp with Query**: only for temps computed once and never mutated  
- Extract/Inline Class: class-level extract/inline following move field + move function  
- Also: Hide Delegate, Remove Middle Man, Substitute Algorithm  

### Ch8 — Moving Features
**Core idea**: Moving features encodes modular understanding into the code. Functions, fields, statements, and loops all move.
- **Move Function**: frequent references to another context → move closer  
- **Move Field**: data always used together, or changing one requires changing the other  
- **Split Loop**: one job per loop; refactor first, optimize later  
- **Replace Loop with Pipeline**: map/filter often clearer than loops  
- Also: Move Statements into Function/Callers, Slide Statements, Remove Dead Code  

### Ch9 — Organizing Data
**Core idea**: Data structures are the skeleton of behavior. One responsibility per variable; prefer queries for derived data; names are the entry to understanding.
- **Split Variable**: except loop counters and accumulators, multi-assignment variables should split  
- **Rename Field**: data names matter more than flowchart elegance  
- **Replace Derived Variable with Query**: assert the derivation, then remove the field  
- Reference vs value: immutable + concurrent → value; shared mutation → reference + repository  
- Key pattern: Repository  

### Ch10 — Simplifying Conditional Logic
**Core idea**: Give conditionals structure so intent shows through. Complex conditionals are high-value refactoring targets.
- **Decompose Conditional**: name the condition and each branch  
- **Replace Nested Conditional with Guard Clauses**: single-exit dogma is outdated  
- **Replace Conditional with Polymorphism**: many functions switching on the same type code—not simple if/else  
- **Introduce Special Case**: object behavior instead of scattered null checks; special case must be immutable  
- Also: Consolidate Conditional Expression, Introduce Assertion  

### Ch11 — Refactoring APIs
**Core idea**: APIs are the joints of software. Optimize for callers' ease of use and minimal function dependencies.
- **Separate Query from Modifier** (CQS)  
- **Remove Flag Argument**: boolean literals are especially bad  
- **Preserve Whole Object**: shorter lists + cohesive data  
- Replace Parameter with Query / Replace Query with Parameter: responsibility and referential transparency  
- Also: Parameterize Function, Remove Setting Method, Replace Constructor with Factory Function, Replace Function with Command / Replace Command with Function  

### Ch12 — Dealing with Inheritance
**Core idea**: Inheritance is concise reuse when semantics fit; switch to delegation when it doesn't. "Prefer composition" corrects abuse, not inheritance itself.
- Pull Up / Push Down methods and fields by true scope  
- **Replace Type Code with Subclasses** (direct vs indirect inheritance depends on mutability)  
- **Extract Superclass**: inheritance need not be planned up front—it often emerges  
- **Replace Subclass/Superclass with Delegate**: inheritance is single-shot and tight; delegation is multi-direction and clearer  
- Also: Remove Subclass, Collapse Hierarchy, Pull Up Constructor Body  

---

## Technique Quick Index

| Technique | Chapter |
|-----------|---------|
| Extract Function | Ch6 |
| Inline Function | Ch6 |
| Extract Variable | Ch6 |
| Inline Variable | Ch6 |
| Change Function Declaration | Ch6 |
| Encapsulate Variable | Ch6 |
| Rename Variable | Ch6 |
| Introduce Parameter Object | Ch6 |
| Combine Functions into Class | Ch6 |
| Combine Functions into Transform | Ch6 |
| Split Phase | Ch6 |
| Encapsulate Record | Ch7 |
| Encapsulate Collection | Ch7 |
| Replace Primitive with Object | Ch7 |
| Replace Temp with Query | Ch7 |
| Extract Class | Ch7 |
| Inline Class | Ch7 |
| Hide Delegate | Ch7 |
| Remove Middle Man | Ch7 |
| Substitute Algorithm | Ch7 |
| Move Function | Ch8 |
| Move Field | Ch8 |
| Move Statements into Function / Callers | Ch8 |
| Replace Inline Code with Function Call | Ch8 |
| Slide Statements | Ch8 |
| Split Loop | Ch8 |
| Replace Loop with Pipeline | Ch8 |
| Remove Dead Code | Ch8 |
| Split Variable | Ch9 |
| Rename Field | Ch9 |
| Replace Derived Variable with Query | Ch9 |
| Change Reference to Value | Ch9 |
| Change Value to Reference | Ch9 |
| Decompose Conditional | Ch10 |
| Consolidate Conditional Expression | Ch10 |
| Replace Nested Conditional with Guard Clauses | Ch10 |
| Replace Conditional with Polymorphism | Ch10 |
| Introduce Special Case | Ch10 |
| Introduce Assertion | Ch10 |
| Separate Query from Modifier | Ch11 |
| Parameterize Function | Ch11 |
| Remove Flag Argument | Ch11 |
| Preserve Whole Object | Ch11 |
| Replace Parameter with Query | Ch11 |
| Replace Query with Parameter | Ch11 |
| Remove Setting Method | Ch11 |
| Replace Constructor with Factory Function | Ch11 |
| Replace Function with Command | Ch11 |
| Replace Command with Function | Ch11 |
| Pull Up Method | Ch12 |
| Pull Up Field | Ch12 |
| Pull Up Constructor Body | Ch12 |
| Push Down Method | Ch12 |
| Push Down Field | Ch12 |
| Replace Type Code with Subclasses | Ch12 |
| Remove Subclass | Ch12 |
| Extract Superclass | Ch12 |
| Collapse Hierarchy | Ch12 |
| Replace Subclass with Delegate | Ch12 |
| Replace Superclass with Delegate | Ch12 |

---

## Smell → Technique Map

| Smell | Preferred techniques | Chapters |
|-------|----------------------|----------|
| Mysterious Name | Change Function Declaration / Rename Variable / Rename Field | Ch6, Ch9 |
| Duplicated Code | Extract Function + Slide Statements + Pull Up Method | Ch6, Ch8, Ch12 |
| Long Function | Extract Function, Split Phase, Replace Loop with Pipeline, Replace Function with Command | Ch6, Ch8, Ch11 |
| Long Parameter List | Introduce Parameter Object, Preserve Whole Object, Replace Parameter with Query | Ch6, Ch11 |
| Global Data | Encapsulate Variable | Ch6 |
| Mutable Data | Encapsulate Variable, Split Variable, Separate Query from Modifier | Ch6, Ch9, Ch11 |
| Divergent Change | Extract Class, Split Phase, Move Function | Ch6, Ch7, Ch8 |
| Shotgun Surgery | Move Function/Field, Combine Functions into Class, Inline Class | Ch6, Ch7, Ch8 |
| Feature Envy | Move Function + Extract Function | Ch6, Ch8 |
| Data Clumps | Introduce Parameter Object, Extract Class, Preserve Whole Object | Ch6, Ch7, Ch11 |
| Primitive Obsession | Replace Primitive with Object, Extract Class | Ch7 |
| Repeated Switches | Replace Conditional with Polymorphism | Ch10 |
| Loops | Replace Loop with Pipeline | Ch8 |
| Lazy Element | Inline Function / Inline Class / Collapse Hierarchy | Ch6, Ch7, Ch12 |
| Speculative Generality | Collapse Hierarchy, Inline, Remove Dead Code | Ch7, Ch8, Ch12 |
| Temporary Field | Extract Class, Move Function, Introduce Special Case | Ch7, Ch8, Ch10 |
| Message Chains | Hide Delegate + Extract/Move Function | Ch7 |
| Middle Man | Remove Middle Man, Replace Superclass/Subclass with Delegate | Ch7, Ch12 |
| Insider Trading | Move Function/Field, Hide Delegate | Ch7, Ch8 |
| Large Class | Extract Class, Extract Superclass, Replace Type Code with Subclasses | Ch7, Ch12 |
| Alternative Classes with Different Interfaces | Change Function Declaration, Move Function, Extract Superclass | Ch6, Ch8, Ch12 |
| Data Class | Encapsulate Record, Remove Setting Method, Move Function | Ch7, Ch8, Ch11 |
| Refused Bequest | Push Down Method/Field, Collapse Hierarchy | Ch12 |
| Comments | Extract Function + Change Function Declaration + Introduce Assertion | Ch6, Ch10 |

---

## Scope & Limits

- Covers principles, 24 smells, and ~60 catalogued techniques from *Refactoring* 2e.  
- No full code walkthroughs; for complete mechanics, use the book chapter.  
- Book examples are mostly JavaScript; techniques transfer to any language.
