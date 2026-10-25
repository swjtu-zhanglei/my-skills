# Smell → Technique Map — Refactoring 2nd Edition

All 24 code smells from Ch3, each with its preferred refactoring techniques.
Load this file when diagnosing smells or choosing which technique to apply.

## How to read a smell

- A smell is a **hint**, not a metric. It points to where a refactoring *may* help.
- The most common trigger is **Mysterious Name** — renaming often forces deeper cleanup.
- **Divergent Change** (one module changes for many reasons) and **Shotgun Surgery**
  (one change scattered across modules) are mirror images.
- Only *repeated* switches on the same type code deserve polymorphism.
- Comments become a smell only when used as deodorant for bad code.

## The Map

| Smell | Symptom | Preferred techniques | Chapters |
|-------|---------|----------------------|----------|
| Mysterious Name | Name doesn't reveal intent | Change Function Declaration / Rename Variable / Rename Field | Ch6, Ch9 |
| Duplicated Code | Same structure in 2+ places | Extract Function + Slide Statements + Pull Up Method | Ch6, Ch8, Ch12 |
| Long Function | Doing too many things | Extract Function, Split Phase, Replace Loop with Pipeline, Replace Function with Command | Ch6, Ch8, Ch11 |
| Long Parameter List | Hard-to-call signature | Introduce Parameter Object, Preserve Whole Object, Replace Parameter with Query | Ch6, Ch11 |
| Global Data | Mutable from anywhere | Encapsulate Variable | Ch6 |
| Mutable Data | Shared state mutated freely | Encapsulate Variable, Split Variable, Separate Query from Modifier | Ch6, Ch9, Ch11 |
| Divergent Change | One module, many reasons to change | Extract Class, Split Phase, Move Function | Ch6, Ch7, Ch8 |
| Shotgun Surgery | One change, many modules touched | Move Function/Field, Combine Functions into Class, Inline Class | Ch6, Ch7, Ch8 |
| Feature Envy | Function obsessed with another module's data | Move Function + Extract Function | Ch6, Ch8 |
| Data Clumps | Same fields travel together | Introduce Parameter Object, Extract Class, Preserve Whole Object | Ch6, Ch7, Ch11 |
| Primitive Obsession | Domain concepts as raw strings/ints | Replace Primitive with Object, Extract Class | Ch7 |
| Repeated Switches | Same type-switch in many places | Replace Conditional with Polymorphism | Ch10 |
| Loops | Loop hides intent | Replace Loop with Pipeline | Ch8 |
| Lazy Element | Structure not paying its way | Inline Function / Inline Class / Collapse Hierarchy | Ch6, Ch7, Ch12 |
| Speculative Generality | Hooks for imaginary futures | Collapse Hierarchy, Inline, Remove Dead Code | Ch7, Ch8, Ch12 |
| Temporary Field | Field set only in some cases | Extract Class, Move Function, Introduce Special Case | Ch7, Ch8, Ch10 |
| Message Chains | `a.b().c().d()` navigation | Hide Delegate + Extract/Move Function | Ch7 |
| Middle Man | Class that only delegates | Remove Middle Man, Replace Superclass/Subclass with Delegate | Ch7, Ch12 |
| Insider Trading | Modules whispering via back channels | Move Function/Field, Hide Delegate | Ch7, Ch8 |
| Large Class | Too many fields/responsibilities | Extract Class, Extract Superclass, Replace Type Code with Subclasses | Ch7, Ch12 |
| Alternative Classes with Different Interfaces | Same job, different signatures | Change Function Declaration, Move Function, Extract Superclass | Ch6, Ch8, Ch12 |
| Data Class | Fields + getters/setters, no behavior | Encapsulate Record, Remove Setting Method, Move Function | Ch7, Ch8, Ch11 |
| Refused Bequest | Subclass ignores inherited API | Push Down Method/Field, Collapse Hierarchy | Ch12 |
| Comments | Deodorant for unclear code | Extract Function + Change Function Declaration + Introduce Assertion | Ch6, Ch10 |
