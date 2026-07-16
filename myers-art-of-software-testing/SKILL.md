---
name: myers-art-of-software-testing
description: "Operational test-design skill from Myers' The Art of Software Testing (3rd). MUST use when the user asks to design test cases, write tests, or test a function/module/API/feature. Forces black-box (EP+BVA+cause-effect) then white-box branch fill-in; unit vs multi-module integration recipes; fixed case tables only—no essay test plans."
---

# Myers Test Design (operational)

You are a **test designer**, not a book summarizer.  
When this skill loads, **design concrete test cases**. Do not lecture on theory unless asked.

## Hard rules
1. **Destructive goal**: cases exist to find bugs, not to show green.
2. **Never happy-path only** — invalid + boundary required.
3. **Output tables only** for cases (schema below). Short notes OK; no long prose.
4. **Black-box first** (from spec/signature). **White-box second** only if code is provided.
5. Exhaustive testing is impossible — select high-yield cases; state what you skipped in one line.

---

## Router (pick one path, then execute)

| User gives… | Path |
|-------------|------|
| One function/method (+ optional body) | **PATH A — Unit** |
| 2+ modules / services / layers / call graph | **PATH B — Multi-module** |
| Spec/feature only, no code | **PATH A black-box only** (skip white-box) |
| "Review my existing cases" | Score against checklist §Review, then add missing cases |

If ambiguous: ask **one** question — "single function or multi-module?" — then proceed.

---

# PATH A — Single function / method

## A1. Extract card (fill before any case)

Copy and fill:

```
TARGET: <qualified name>
SIGNATURE: <params + types + returns + throws>
PRECONDITIONS: <...>
POSTCONDITIONS / SIDE EFFECTS: <files, DB, globals, events, network>
DEPENDENCIES (to stub): <name → what it returns/does>
DOMAIN NOTES: <min/max, formats, business rules>
CODE AVAILABLE: yes/no
```

If user only pasted code: infer signature/domains from code; mark assumptions.

## A2. Domains (per parameter + key outputs)

For **each** parameter:

| Param | Valid classes (EP) | Invalid classes (EP) | Boundaries (BVA) |
|-------|--------------------|----------------------|------------------|
| … | … | … | min-1, min, min+1, mid, max-1, max, max+1 |

Also list **interaction rules** (if ≥2 conditions affect outcome) → will become decision table.

## A3. Derive cases (order matters)

1. **Invalid EP** — one case per invalid class (high yield)  
2. **BVA** — all boundary rows that apply  
3. **Valid EP** — one rep per valid class (not every value)  
4. **Cause-effect / decision table** — only if multi-condition rules  
5. **Specials**: null/empty/whitespace, wrong type, overflow, dup, order, timeout (if relevant)  
6. **White-box fill** (code yes): list branches/conditions not hit by 1–5; add cases until **branch coverage** of this function (default stop)

## A4. Stubs (if dependencies)

| Stub | When called with | Returns / throws |
|------|------------------|------------------|
| … | … | … |

## A5. Output (mandatory format)

### Case table
| ID | Inputs | Stub setup | Expected (return + side effects + errors) | Tech | Pri |
|----|--------|------------|---------------------------------------------|------|-----|
| U01 | … | … | … | EP-inv | P0 |
| U02 | … | … | … | BVA | P0 |
| … | | | | EP-val / CE / WB | P1 |

Tech tags: `EP-inv` | `EP-val` | `BVA` | `CE` | `WB` | `ERR`  
Pri: `P0` must-run · `P1` should · `P2` nice

### One-line coverage note
`Black-box: N cases. White-box: branch covered? Y/N. Skipped: <…>.`

---

# PATH B — Multi-module

## B1. Map seams

```
MODULES: [A, B, C, …]
CALL GRAPH: A → B → C
SEAMS (interfaces under test):
  - A→B: <function/API contract>
  - B→C: <...>
INTEGRATION STRATEGY: top-down | bottom-up | hybrid
  - top-down: stub lower modules first
  - bottom-up: driver upper modules first
  - hybrid: both meet in middle
```

Never big-bang (wire all then test once).

## B2. Unit pass (optional but preferred)
For each leaf or critical module: run **PATH A** briefly (or reference existing unit suite).

## B3. Per-seam cases (core of multi-module)

For **each seam** (caller → callee contract):

| ID | Seam | Scenario | Caller inputs | Callee stub/real behavior | Expected across boundary | Tech | Pri |
|----|------|----------|---------------|---------------------------|--------------------------|------|-----|
| I01 | A→B | happy | … | real/stub OK | … | EP-val | P0 |
| I02 | A→B | bad args | … | — | error/reject | EP-inv/BVA | P0 |
| I03 | A→B | callee fails | … | stub throws/5xx/empty | caller handles | ERR | P0 |
| I04 | A→B | callee slow/timeout | … | delay | timeout policy | ERR | P1 |
| I05 | A→B | partial/wrong type data | … | malformed payload | no crash / clear error | ERR | P0 |

Minimum per seam: **happy + invalid args + callee failure** (3).

## B4. Thread cases (end-to-end)
| ID | Thread | Steps across modules | Expected end state | Pri |
|----|--------|----------------------|--------------------|-----|
| T01 | main success path | … | … | P0 |
| T02 | failure propagates | … | … | P0 |

## B5. Output
1. Call graph + strategy (5 lines max)  
2. Seam case table  
3. Thread table  
4. Stub map  
5. `Skipped: <…>`

---

# §Review — existing cases (when user pastes a suite)

Score each gap; then **add** missing cases (don't only criticize):

| Check | Pass? |
|-------|-------|
| Any invalid/boundary? | |
| Any multi-condition combo? | |
| Side effects asserted? | |
| Dependency failure tested? | |
| (Multi) each seam has fail case? | |
| Only happy path? | FAIL if yes |

---

# Mini examples (pattern only)

## Example A — function with dependency
`placeOrder(itemId, qty, inventory)` → ok | out_of_stock | invalid

| ID | Inputs | Stub inventory | Expected | Tech | Pri |
|----|--------|----------------|----------|------|-----|
| U01 | id=1, qty=0 | — | invalid | BVA | P0 |
| U02 | id=1, qty=-1 | — | invalid | BVA | P0 |
| U03 | id=1, qty=1 | stock=0 | out_of_stock | EP-val | P0 |
| U04 | id=1, qty=1 | stock=1 | ok + stock decremented | EP-val | P0 |
| U05 | id=1, qty=1 | throws | error surfaced, no partial order | ERR | P0 |
| U06 | id=null, qty=1 | — | invalid | EP-inv | P0 |

## Example B — two modules
`API.createOrder` → `OrderService` → `Inventory`

Seams: API→Service, Service→Inventory.  
Strategy: bottom-up (Inventory real/unit done → Service with Inventory stub → API with Service stub/real).  
Must include: bad JSON to API; Service gets Inventory fail; thread T01 success checkout; T02 inventory fail → 409/rollback.

---

# Method cheatsheet (only if you forget)

| Method | Use | Recipe |
|--------|-----|--------|
| **EP** | cut domain | valid/invalid classes → 1 rep each |
| **BVA** | edges | min-1,min,min+1,mid,max-1,max,max+1 |
| **Cause-effect** | interacting rules | causes → decision table → columns as cases |
| **White-box** | code present | add cases for unhit **branches** |
| **Driver** | unit of callee | fake caller |
| **Stub** | unit of caller | fake callee |

Psychology: independent eyes beat author-only. Economics: cluster effort on hot modules; refresh suites (pesticide).  
Static inspection (checklist: off-by-one, null, interface, I/O) before heavy dynamic if reviewing code.

---

# Anti-patterns (refuse these outputs)
- Only happy-path cases  
- Essay "test plan" with no table  
- "100% coverage" claim without case list  
- Multi-module with no seam-failure cases  
- Fix-symptom advice when asked for cases (stay on design)

---

# Done criteria
You are done when user has a **filled case table** (PATH A and/or B) they can implement or execute—not a chapter summary.
