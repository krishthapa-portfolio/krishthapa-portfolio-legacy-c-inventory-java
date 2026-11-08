# legacy-c-inventory-java

Demo legacy system for [REPLIC8](https://github.com/jaiom-rola-sp/REPLIC8): the old inventory and shipping software of "Subsidiary C", an industrial logistics and manufacturing company. Written in Java, J2SE 1.4 style (no generics; `Vector`, `Hashtable`). All data is made up. Replaces `legacy-c-inventory-fortran` as C's import repo.

- `src/com/subc/pil/`: `Inventory.java` (2003 code and the command dispatcher), `PilRecs.java` (shared helpers), `Sourcing.java`, `WhseRpt.java`, `Purch.java`.
- `data/`: fixed-width data (`stock.dat`, `lanes.dat`, `catalog.dat`, `suppliers.dat`, `purchase_orders.dat`).
- 9 read-only operations: `checkStock`, `getShippingOptions`, `getWarehouse`, `listLowStock`, `getSupplier`, `getPartSourcing`, `listPoLines`, `getCheapestShipping`, `quoteDelivery`.
- Line protocol: the first stdin line is the command (matched case-insensitively), then `KEY=VALUE` lines, then `END`. Output is `KEY=VALUE` lines; `BEGIN-ITEMS` starts a list and `--` closes each item.
- Shared keys with the maintenance system: part ID (for example `PMP-2210`) and site region (for example `PERMIAN`).
- Written to look like code several people changed over the years: logic split across classes that extend a shared helper class, copy-pasted helpers, a cache, a deprecated method and removed commands left in comments. `checkStock` and `getShippingOptions` give the same answers as the original version.

Demo: `quoteDelivery PART-ID=CBL-0500 DESTINATION=PERMIAN QUANTITY=2` ships from ODESSA by BASINEXPRESS in 5 days for $20,325.50.

## Build and run

```
mkdir classes
javac -d classes src/com/subc/pil/*.java
printf 'checkStock\nPART-ID=PMP-2210\nEND\n' | java -cp classes com.subc.pil.Inventory
```
