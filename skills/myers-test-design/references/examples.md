# Examples & Method Cheatsheet — Myers Test Design

Load this file when you need a worked pattern or forget a method recipe.

## Example A — function with dependency (PATH A)

`placeOrder(itemId, qty, inventory)` → ok | out_of_stock | invalid

| ID | Inputs | Stub inventory | Expected | Tech | Pri |
|----|--------|----------------|----------|------|-----|
| U01 | id=1, qty=0 | — | invalid | BVA | P0 |
| U02 | id=1, qty=-1 | — | invalid | BVA | P0 |
| U03 | id=1, qty=1 | stock=0 | out_of_stock | EP-val | P0 |
| U04 | id=1, qty=1 | stock=1 | ok + stock decremented | EP-val | P0 |
| U05 | id=1, qty=1 | throws | error surfaced, no partial order | ERR | P0 |
| U06 | id=null, qty=1 | — | invalid | EP-inv | P0 |

## Example B — two modules (PATH B)

`API.createOrder` → `OrderService` → `Inventory`

Seams: API→Service, Service→Inventory.
Strategy: bottom-up (Inventory real/unit done → Service with Inventory stub → API with Service stub/real).
Must include: bad JSON to API; Service gets Inventory fail; thread T01 success checkout; T02 inventory fail → 409/rollback.

## Method cheatsheet

| Method | Use | Recipe |
|--------|-----|--------|
| **EP** | cut domain | valid/invalid classes → 1 rep each |
| **BVA** | edges | min-1, min, min+1, mid, max-1, max, max+1 |
| **Cause-effect** | interacting rules | causes → decision table → columns as cases |
| **White-box** | code present | add cases for unhit **branches** |
| **Driver** | unit of callee | fake caller |
| **Stub** | unit of caller | fake callee |

## Principles worth one line each

- Psychology: independent eyes beat author-only testing.
- Economics: cluster effort on hot modules; refresh aging suites (pesticide paradox).
- Static inspection first when reviewing code: off-by-one, null, interface mismatch, I/O checklist.
