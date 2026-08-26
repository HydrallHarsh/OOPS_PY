Beekeeping & Apiary Management

# Beekeeping & Apiary Management

*Plain-English guide to the terminology, processes, and hidden rules behind the  
Beekeeping product backlog.*

## Contents

Contents ........................................................................................................ 1  
1. Domain Overview ......................................................................................... 2  
2. Glossary of Terms ........................................................................................ 3  
3. Key Processes Explained .............................................................................. 5  
&nbsp;&nbsp;&nbsp;&nbsp;The Inspection Cycle .................................................................................... 6  
&nbsp;&nbsp;&nbsp;&nbsp;Treatment → Withdrawal → Harvest ................................................................. 6  
&nbsp;&nbsp;&nbsp;&nbsp;Queen Lineage Tracking .................................................................................. 6  
&nbsp;&nbsp;&nbsp;&nbsp;Swarm Capture & Intake .................................................................................. 6  
&nbsp;&nbsp;&nbsp;&nbsp;Annual Health Dashboard ................................................................................ 6  
4. Hidden Rules & Gotchas .................................................................................. 6  
5. Story-by-Story Cheat Sheet ............................................................................. 7  
&nbsp;&nbsp;&nbsp;&nbsp;UI Sheet ....................................................................................................... 8  
&nbsp;&nbsp;&nbsp;&nbsp;Database Sheet .............................................................................................. 8  
&nbsp;&nbsp;&nbsp;&nbsp;Programming Sheet ........................................................................................ 9  
6. Likely Trainee Questions (FAQ) ..................................................................... 10  
7. Quick-Reference Appendix ............................................................................. 12  
&nbsp;&nbsp;&nbsp;&nbsp;Common Bee Diseases & Pests Quick-Reference .................................................. 13  
&nbsp;&nbsp;&nbsp;&nbsp;Typical Withdrawal Periods (illustrative — check jurisdiction) ............................ 13

Beekeeping & Apiary Management

## 1. Domain Overview

An apiary is simply a location where honey-bee colonies are kept. A working beekeeper (the
'apiarist') typically manages anywhere from a handful to several hundred individual hives, often
spread across multiple sites. Each hive is a small business in its own right: it produces honey,
beeswax, propolis, pollen and — most importantly — pollination services that growers pay for.
The software in this backlog is built for a commercial or semi-commercial beekeeper who has
outgrown the paper notebook stage and now needs a system of record.

The day-in-the-life of a beekeeper revolves around inspections. Roughly every 7 to 14 days
during the active season the keeper opens each hive, lifts out frames, looks at brood patterns,
counts queen cells, estimates honey stores, scans for pests like Varroa mites, and watches for
diseases such as American Foulbrood (AFB) or European Foulbrood (EFB). Everything
observed has to be logged because regulators, honey buyers, and the keeper's own memory all
depend on it.

On top of inspections the keeper schedules treatments (anti-Varroa chemicals, antibiotics in
some jurisdictions), captures swarms, requeens failing colonies, feeds sugar syrup in lean
months, and harvests honey when the supers are capped. Each of these activities has rules
attached — most importantly the 'withdrawal period' after any veterinary treatment, during which
honey cannot legally be harvested or sold.

Software helps by enforcing those rules automatically, by giving the keeper a map of where
every hive is, by tracking the lineage of every queen (a good queen line is worth real money),
and by producing the paperwork required for organic certification, honey labelling and export.

## 2. Glossary of Terms

Every term used in the backlog, defined in plain English. Skim once, then keep this section open
during training sessions.

| Term | Definition |
|---|---|
| Apiary | A site holding one or more hives. A beekeeper usually has several apiaries spread out so the bees do not over-graze any single area. |
| Hive | The physical box (or stack of boxes) that houses one colony of bees. In the database every hive has its own ID and history. |
| Colony | The living superorganism inside a hive: one queen, thousands of workers, and seasonally some drones. |
| Queen | The single egg-laying female. Her genetics drive the colony's temperament and productivity, which is why the backlog tracks queen lineage like a pedigree. |
| Drone | Male bee. Does no work; exists only to mate with virgin queens. |

Page 3

| Term | Definition |
|---|---|
| Worker | Sterile female bee. Does everything: nursing, foraging, guarding, cleaning. |
| Brood | Eggs, larvae and pupae — the next generation. Inspectors judge a queen by her brood pattern (solid = good, spotty = failing). |
| Frame | A removable wooden rectangle holding the wax comb. A standard hive box holds 8–10 frames. |
| Super | An upper hive box used only for honey storage (never brood). Adding a super is giving the bees more room to store nectar. |
| Queen Excluder | A metal or plastic grid that lets workers through but blocks the larger queen, so brood does not end up on supers. |
| Foundation | A pre-stamped sheet of wax inside an empty frame, giving bees a guide to draw comb. |
| Capped Honey | Honey the bees have sealed with a wax cap, meaning moisture is low enough to harvest without fermenting. |
| Extraction | Spinning honey out of the comb in a centrifuge after uncapping. |
| Requeening | Replacing the existing queen with a new one — done when the old queen is failing, aggressive, or carrying poor genetics. |
| Swarm | Roughly half a colony leaving with the old queen to start a new home. Capturing swarms is a free source of new colonies. |
| Swarm Cell | An elongated queen cell on the bottom edge of a frame — early warning that the colony is preparing to swarm. |
| Supersedure Cell | A queen cell built mid-frame, meaning the colony intends to replace its current queen rather than swarm. |
| Varroa (Varroa destructor) | A parasitic mite that weakens bees and spreads viruses. The single biggest pest in modern beekeeping. |
| Varroa Wash | An alcohol or sugar test that counts mites per 100 bees. Triggers the decision to treat. |
| AFB — American Foulbrood | A bacterial disease (Paenibacillus larvae) that is fatal to colonies and, in many jurisdictions, legally notifiable; infected hives are usually burned. |
| EFB — European Foulbrood | A less severe bacterial brood disease, often treatable, but still triggers quarantine in the system. |
| Nosema | A gut fungus weakening adult bees, usually treated with fumagillin where legal. |
| Small Hive Beetle | An invasive pest that fouls honey and comb. |

Page 4

## 3. Key Processes Explained

### The Inspection Cycle

Every 7–14 days in season the keeper opens each hive, frame by frame. They record: queen
seen/yes/no, eggs seen, brood pattern, frames of brood, frames of honey, queen cells
observed, pest counts, temperature, and any actions taken (added super, removed frame,
applied treatment). The UI form mirrors this exact flow so the keeper can fill it on a phone in the
field. Each inspection writes a row to the inspections table and may trigger downstream actions:
a high Varroa count opens a Treatment task, a swarm cell observation opens a 'split or requeen'
task, a missing queen flips the hive status to 'queenless' and starts a 21-day clock.

### Treatment → Withdrawal → Harvest

When a treatment is applied the system stamps a withdrawal-end date based on the product.
Any attempt to log a honey harvest from that hive before the date must be rejected
(this is the WithdrawalPeriodViolation exception in the Programming sheet). This single rule is
the most important compliance constraint in the whole backlog — getting it wrong can mean
contaminated honey reaching consumers and the keeper losing their licence.

### Queen Lineage Tracking

Queens are the genetic heart of an operation. Each queen record links to her mother queen
(self-FK), her introduction date, her source (bred in-house, bought from breeder X, captured
swarm), and her outcome (superseded, swarmed, killed, replaced). The lineage graph lets a
keeper answer questions like 'all my best-producing hives descend from queen Q-2021-014'
and breed accordingly.

### Swarm Capture & Intake

When the keeper (or a member of the public) reports a swarm hanging in a tree, it is captured
into a temporary box, transported, and installed in a hive. The system records the capture
location, date, estimated size, and assigns a new hive ID. Captured swarms are flagged
'unknown lineage' until a queen is identified.

### Annual Health Dashboard

At year end the dashboard aggregates per-hive metrics: total honey harvested (kg), number of
inspections, disease events, treatments applied, queen changes, and overwinter survival. This
drives next-season decisions about which lines to expand and which hives to cull.

## 4. Hidden Rules & Gotchas

These are the non-obvious constraints baked into the backlog. Trainees almost always miss
them on a first read.

- Honey CANNOT be harvested while a hive is inside a withdrawal period. The system
  blocks the harvest form, not just warns.
- A hive flagged for EFB or AFB quarantine cannot transfer frames, queens or equipment
  to other hives — cross-contamination is the main spread vector.
- Queen replacement is blocked while a quarantine is active because moving queens is a
  known disease vector.
- A 'queenless' status that lasts more than 21 days means the colony cannot produce a
  new queen on its own (laying-worker stage) and must be merged or culled.
- Geo-coordinates of apiaries are sensitive — hive theft is real, and the dashboard should
  mask exact coordinates for non-admin users.
- Treatment products differ by jurisdiction — the system stores the withdrawal period per
  product, not hard-coded.
- Swarm captures of unknown origin must NOT be merged with healthy production hives
  until at least one disease-free inspection.
- Honey yield must be recorded per super, not per hive, because supers are routinely
  swapped between hives.

## 5. Story-by-Story Cheat Sheet

Below is a plain-English summary of each backlog item, the question a trainee is most likely to
ask, and a model answer.

### UI Sheet

| Story | Plain-English summary & likely Q&A |
|---|---|
| UI-001 Hive Registration | Creates a new hive record with location, type, source colony. Trainee: 'Why capture GPS?' Answer: hives are mobile — they get moved to pollination contracts and stolen — so a centre coordinate is essential. |
| UI-002 Inspection Log Entry | The most-used screen. Trainee: 'Why so many fields?' Answer: each field maps to a regulatory or health metric the keeper needs to defend later. |
| UI-003 Queen Replacement Form | Records old queen out, new queen in, source, lineage link. Trainee: 'Why block this during quarantine?' Answer: queens are a disease vector. |
| UI-004 Honey Harvest Entry | Captures kg per super on a date. Trainee: 'Why does it sometimes refuse to save?' Answer: the hive is inside a treatment withdrawal period. |
| UI-005 Disease / Pest Report | Logs observation + severity. Triggers quarantine workflow for notifiable diseases like AFB. |
| UI-006 Apiary Map View | Plots all apiaries and hives on a map. Trainee: 'Why mask coordinates?' Answer: hive theft. |
| UI-007 Swarm Capture Intake | Records a newly captured swarm. Flagged 'unknown lineage' until proven healthy. |
| UI-008 Treatment Scheduler | Plans and logs treatments; auto-computes withdrawal-end date. |
| UI-009 Feeding / Supplement Log | Records sugar syrup or pollen patties given. Important for organic certification — organic honey cannot come from heavily supplemented hives during flow. |
| UI-010 Annual Health Dashboard | Year-end roll-up. Trainee: 'Why overwinter survival?' Answer: it is the single best indicator of operation health. |

### Database Sheet

| Story | Plain-English summary & likely Q&A |
|---|---|
| DB-001 hives table | PK hive_id, FK apiary_id, status enum (active, queenless, quarantine, dead). Indexed on status for dashboard speed. |

Beekeeping & Apiary Management

| DB-002 inspections table | PK inspection_id, FK hive_id, timestamp, observer, JSON metrics column for flexible field additions. |
| DB-003 queens table | Self-referencing FK mother_queen_id enables lineage tree. Unique on (hive_id, introduced_at) to prevent duplicate active queens. |
| DB-004 harvests table | Stores kg per super per date. CHECK constraint references treatments to refuse rows inside withdrawal period. |
| DB-005 treatments table | FK to hive, product_id, applied_date, withdrawal_days, computed withdrawal_end_date. Indexed for fast lookup at harvest time. |
| DB-006 diseases_observed | Links inspection to disease enum. AFB rows flip hive status to quarantine via trigger. |
| DB-007 apiaries table | Geo lat/long, owner contact, max-hive capacity. Used by map view. |
| DB-008 swarm_captures | Capture date, location, estimated size, assigned_hive_id once installed. |
| DB-009 feedings table | Date, hive, feed type, quantity. Joined with harvests to flag non-organic honey. |
| DB-010 audit_log | Append-only table of every status change. Essential for regulator audits. |

### Programming Sheet

| Story | Plain-English summary & likely Q&A |
|---|---|
| PROG-001 Hive class hierarchy | Base Hive with subclasses ProductionHive, NucHive, BreedingHive. Demonstrates polymorphism — each computes 'is harvestable' differently. |
| PROG-002 InspectionService | Takes raw form data, validates, persists, and fires events (high Varroa → treatment task). |
| PROG-003 QueenLineageTracker | Graph data structure (adjacency map) supporting ancestor and descendant queries. |
| PROG-004 HarvestCalculator | Sums kg per super, applies moisture deduction, returns saleable kg. |
| PROG-005 TreatmentValidator | Throws WithdrawalPeriodViolation when harvest date < withdrawal_end_date. |
| PROG-006 DiseaseAlertEngine | Subscribes to inspection events, escalates AFB/EFB to quarantine workflow. |

Page 9

Beekeeping & Apiary Management

| PROG-007 ApiaryGeoUtils | Haversine distance between apiaries; used to enforce minimum spacing between disease-quarantined and clean sites. |
| PROG-008 CSV import/export | Bulk import inspections from offline mobile use; export for accountant. |
| PROG-009 JSON inspection archive | Long-term cold storage of inspection blobs older than N years. |
| PROG-010 Custom exceptions | QueenAbsentException, WithdrawalPeriodViolation, QuarantineActiveException — each maps to a user-friendly error in the UI. |

## 6. Likely Trainee Questions (FAQ)

**Q. What is a 'super' versus a 'brood box'?**  
**A.** Brood boxes are the lower boxes where the queen lays eggs; supers sit on top, separated by a queen excluder, and hold honey only.

**Q. Why can't the keeper just harvest whenever there's honey?**  
**A.** Because of withdrawal periods after treatments, moisture content rules, and seasonal forage windows.

**Q. What's the difference between AFB and EFB?**  
**A.** AFB is bacterial, fatal, and usually requires destruction of the hive. EFB is also bacterial but often treatable; both trigger quarantine in our system.

**Q. Why track queen lineage?**  
**A.** Genetics determine temperament, disease resistance, and honey yield. Good lines are worth breeding from.

**Q. What is a swarm cell vs supersedure cell?**  
**A.** Swarm cells hang off the bottom of frames (colony will split); supersedure cells sit mid-frame (colony will replace its queen quietly).

**Q. Why is Varroa such a big deal?**  
**A.** It weakens bees and vectors viruses; uncontrolled Varroa is the leading cause of colony collapse globally.

**Q. What does 'queenless' mean?**  
**A.** No laying queen present. Beyond ~21 days the colony develops laying workers and is effectively doomed.

**Q. Why does the harvest screen sometimes block me?**  
**A.** An active treatment withdrawal period covers that hive; the system refuses to let contaminated honey enter inventory.

**Q. What is a nuc?**  
**A.** A small 4–5 frame colony used for starting new hives, holding spare queens, or selling to other beekeepers.

**Q. Why GPS hives?**  
**A.** Hives are moved for pollination contracts and are commonly stolen; GPS is operational, not decorative.

**Q. What's a pollination contract?**  
**A.** Growers (e.g. almond orchards) pay beekeepers to place hives in their fields during bloom.

**Q. What does 'capped' honey mean?**  
**A.** Bees seal cells with wax when moisture is low enough (<18%) to prevent fermentation. Capped = ready to harvest.

**Q. Why is the queens table self-referencing?**

Page 11

Beekeeping & Apiary Management

**A.** Because each queen has a mother queen who is also in the same table — that's a tree, modelled with a FK pointing back to the same table's PK.

**Q. Is feeding sugar syrup cheating?**  
**A.** It's normal husbandry, but honey produced while feeding cannot be labelled organic or pure; that's why the feedings table exists.

**Q. What's the role of the audit log table?**  
**A.** Regulators can demand a full history of any hive's status changes — append-only logs make this provable.

**Q. What is propolis?**  
**A.** Plant resin bees collect to seal the hive; sold as a health supplement, occasionally tracked in inventory.

**Q. What does requeening cost?**  
**A.** A mated queen from a reputable breeder typically costs $30–60; breeder queens cost much more.

**Q. What is moisture content and why deduct it?**  
**A.** Buyers pay for honey at a standard moisture (usually 18%); wetter honey is dried or down-priced, hence the deduction in HarvestCalculator.

Beekeeping & Apiary Management

## 7. Quick-Reference Appendix

### Common Bee Diseases & Pests Quick-Reference

| Name | Type | Effect | System Action |
|---|---|---|---|
| AFB (American Foulbrood) | Bacterial | Fatal, highly contagious | Auto-quarantine, notifiable |
| EFB (European Foulbrood) | Bacterial | Brood death, treatable | Quarantine, antibiotic option |
| Nosema | Fungal/microsporidian | Adult bee dysentery, weakening | Treatment record |
| Varroa destructor | Mite | Weakens bees, vectors viruses | Periodic wash, treatment |
| Small Hive Beetle | Insect | Slimy damage, honey fouling | Trap, inspection note |
| Wax Moth | Insect | Comb destruction in weak hives | Strengthen or cull |
| Chalkbrood | Fungal | Mummified larvae | Inspection note, requeen |

### Typical Withdrawal Periods (illustrative — check jurisdiction)

| Product | Treats | Withdrawal (days) |
|---|---|---|
| Oxalic acid (vaporised) | Varroa | 0 (no honey supers on) |
| Formic acid | Varroa | 0–7 |
| Amitraz strips | Varroa | 14–42 |
| Oxytetracycline | EFB (where legal) | 42+ |
| Fumagillin | Nosema | Off-season only |
