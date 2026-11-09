# PERSONAL.WIKI — T06: Experimental Project & Reasoning Notebook

An **offline research notebook**, with searchable TiddlyWiki-style posts, portable JSON, and a **separate, local SQLite history of each post's versions**. No cloud service, shard catalog, or cross-database search.

> **Experimental / single-user software.** This repository is a public research and
> personal productivity prototype, not a supported production service. It was
> developed and tested primarily on Linux; the graphical interface and PVS
> integration have additional dependencies. See **Known limits** below before
> using it with important information. Keep separate backups.

**License:** Original code, documentation, and demonstration material that the
publisher owns are released under **CC0 1.0 Universal** (see [`LICENSE`](LICENSE)).
CC0 is a public-domain dedication, to the extent legally possible; it does not
change the rights or licenses of separately installed dependencies or third-party
material. The earlier `app/LICENSE` Unlicense notice has been aligned to CC0
for this proposed release, subject to the publisher confirming they hold the
necessary rights. See [`ORIGINS.md`](ORIGINS.md) for the project's lineage.

## Overview

**PERSONAL.WIKI** is an experimental, single-user desktop wiki designed to keep
project work and the *reasons behind it* together. It combines local notes and
search with a version history that requires a reason for changes, project and
task tracking, checked field contracts, reusable documentation forms, and an
optional read-only bridge to a separate project versioning system (PVS-01).

This is a personal research and productivity prototype rather than a hosted
service, a complete TiddlyWiki implementation, or production-grade security
software. Notes stay on the user's computer in SQLite databases.

### Main capabilities

- **Write and find notes:** TiddlyWiki-compatible JSON posts, tags, links,
  search, and local backup/restore.
- **Record the reasoning:** reason-required edit history, restore-as-new-version,
  and Toulmin argument fields (claim, grounds, warrant, backing, qualifier,
  rebuttal) inside structured forms.
- **Follow project work:** projects, goals, milestones, tasks, a dashboard,
  Kanban views, and Waterfall plans.
- **Evolve structure:** field contracts for tagged posts can become more
  precise; changes that loosen a contract or break existing posts request
  explicit acknowledgement.
- **Work alongside PVS-01:** browse pinned snapshots and compare local status
  edits with external PVS state. The wiki never writes directly to PVS.

![Personal Wiki project dashboard](docs/images/02_dashboard.png)

## Related personal projects

The author developed this experiment using work from earlier personal
repositories, especially:

| Related project | How it relates | GitHub repository URL |
|---|---|---|
| **Personal Wiki** | Earlier wiki implementation and local notebook concepts | `ADD_PERSONAL_WIKI_GITHUB_URL` |
| **Addendum of the Desiderata** | Earlier personal work informing the reasoning and documentation experiments | `ADD_DESIDERATA_GITHUB_URL` |

**Replace these two URL markers with the exact GitHub links before publishing.**
They are intentionally *not* links to guessed repositories. The archive did not
identify the author's GitHub account or the upstream repository URLs. See
[ORIGINS.md](ORIGINS.md) for further details and a distinction between reported
influences and verified file provenance.

### Quick start

Requires Python **3.11+** and **PySide6** for the graphical interface.

```sh
python3 -m venv .venv
# Activate the virtual environment for your operating system, then:
python -m pip install PySide6
python start_personal_wiki.py --check
python start_personal_wiki.py
```

Windows users can run `Start PERSONAL.WIKI.bat`; macOS users can use
`Start PERSONAL.WIKI.command`; Linux users can use `Start PERSONAL.WIKI.sh`.
See the [manual](docs/USER_MANUAL.md) for platform-specific instructions and
`WIKI_PREVIEW_MODE=qt` if the WebEngine preview is unavailable.

To run tests without a desktop session:

```sh
python -m unittest discover -s tests -v
```

GUI tests require PySide6. PVS tests need a compatible external source checkout
identified by `PVS_SRC`. Local notebooks and demo data generated on your
machine should not be committed into the source repository.

---

## What's new in T06

T06 adds project management on top of T05.1. **SQLite content schema v2 and
history schema v1 are unchanged**; T05.1 notebooks open as they are.

- **Typed fields (contracts).** A tag can have a field contract: a JSON-schema
  post titled `$:/schema/<tag>`. Posts carrying that tag cannot be saved unless
  their custom fields satisfy it. Contracts are ordinary posts, so every change
  is versioned and needs a reason. **Tightening a contract is accepted
  directly; loosening it, deleting/renaming it, or tightening it while existing
  posts would break asks you to acknowledge**, and the history reason is marked
  `[contract change acknowledged by owner]`. The tightening check is the
  Function–Data OOP ratchet (`app/field_schema.py`), with a fix so that dropping
  a type from a union is recognised as a tightening. Field values stay strings
  for TiddlyWiki compatibility; `format` types them as `date`, `integer` or
  `number`.
- **Project tracking.** *View → Install Project Kit* adds contracts for
  `project`, `goal`, `milestone` and `task` (fields `status`, `parent`, `due`,
  `priority`) and a guide post, *10 — Managing Projects*. *Tiddler → New
  Project Item…* (Ctrl+Shift+P) creates one under the post you are reading.
  *View → Project Dashboard* (Ctrl+D) shows each project's tree with progress,
  plus overdue, due-this-week, blocked, contract violations, missing parents
  and parent cycles. Project posts show a fields panel and their children.
- **Editable fields.** The editor now has a *Fields* table, shows the contract
  for the post's tags, and adds rows for required fields. Removing a row removes
  the field. Non-string values from imported JSON are kept unless you change them.
- **Restore a version.** *View Post History…* has *Restore This Version…*.
  Restoring writes a **new** version (history is never rewritten). A deleted
  post's history is closed, so restoring from it creates a new post whose reason
  names the source. Protected posts cannot be restored over; restored content
  must satisfy today's contracts.
- **Forms with arguments.** *View → Install Forms Kit* adds nine forms —
  5W1H, 5 Whys, A3 report, decision record (ADR), PDCA cycle, daily log, Kanban
  board, Waterfall plan and a standalone Toulmin argument — each a template
  (`$:/template/<form>`) plus a contract. *Tiddler → New from Template…*
  (Ctrl+T) starts one. **Every form requires a Toulmin argument** (claim,
  grounds, warrant; optional backing, qualifier, rebuttal), entered in the
  editor's Argument section and shown as a box on the post; the parts are
  plain strings shaped like PVS-01 arguments. Kanban posts draw a live board
  of a project's items with a WIP limit; Waterfall posts draw a phase strip
  whose phase changes record the gate reason. Forms linked to a project are
  listed on its page under *Documents*. Templates are editable, versioned posts.
- **Two-way PVS workflow.** *File → Refresh from PVS…* (Ctrl+Shift+R) reads a
  PVS-01 workspace into the open notebook at one consistent pin. **The wiki
  never writes to PVS**; you enter changes there yourself. On each PVS post,
  **PVS owns** `parent` and every `pvs_*` field (shown in a *From PVS* panel,
  hidden in the editor, refused if edited by hand), and **you own** the text
  (seeded once, never overwritten) and your tags. **Status moves both ways**:
  a three-way comparison against PVS's value at the last refresh takes PVS's
  change, keeps yours (listed *to enter in PVS* with the PVS operation), marks
  equal changes in sync, or flags a **conflict** you resolve in *File → PVS
  Sync…*. Arguments aimed at PVS (`pvs_target`, `pvs_side`) stay *to enter*
  until a refresh finds the same claim and warrant in PVS. *Export PVS Archive
  Notebook…* still makes a protected read-only copy.
- **PVS read out of process.** The wiki never imports PVS: a refresh runs
  `tools/pvs_export.py` in a separate process, which writes a versioned JSON
  snapshot (`pvs-snapshot` v1) of PVS's own data; *Refresh from PVS Snapshot
  File…* reads one exported elsewhere. A notebook recognises its workspace by
  **identity** (the objects' genesis digests), so a moved workspace is
  accepted after you point at it, and a different one is refused. The
  exporter can later move into PVS unchanged.
- **Notebook bundles.** *File → Export / Restore Notebook Bundle…* and
  `tools/notebook_bundle.py export|inspect|restore` write and restore one
  `.pwbundle` file (the pair, a checksummed manifest, the change-log and PVS
  link state). Restore verifies checksums, history, protected posts and the
  change-log chain before creating a NEW pair; nothing is ever overwritten.
- **Change log.** A chain of posts tagged `changelog`: one entry per day of
  edits and one per PVS refresh (written in the refresh's own transaction).
  Each entry links the previous one and stores its SHA-256; earlier entries
  become protected, and *Verify Post History* walks the chain.

**User manual:** [`docs/USER_MANUAL.md`](docs/USER_MANUAL.md), or the
self-contained `docs/USER_MANUAL.html` (images embedded; rebuild with
`python3 tools/build_manual_html.py`).

**Test data:** `python3 tools/make_test_data.py` creates `TEST_DATA/` — a
notebook with real history that fills every dashboard section, five JSON import
scenarios, and (with `PVS_SRC` set) a PVS workspace plus its snapshot notebook.
The manual's chapter 12 walks through it step by step with expected results.

**Browse panel.** The list has a *Show:* picker — **Posts** (default; hides
`$:/` system posts), **Projects**, **Forms**, **Tags**, **System ($:/)** and
**All** — and search (Title / Text / Tag) works inside the chosen view.

**Fixed:** quitting the app could crash Python on exit (PySide teardown order;
present since T05.1, harmless to data). Both entry points now destroy the
window before the application.

**Tests:** `python3 -m unittest discover -s tests -v` (86 tests in `tests/`,
one file per area, including the manual's walkthrough; set `PVS_SRC` to run
the PVS tests; GUI tests run when PySide6 is installed).

**Known limits (deferred security work):** event reasons and timestamps are
not covered by a hash chain, and the complete history of a deleted post can be
removed without `verify_history` noticing. Contract acknowledgement records
your decision but does not authenticate you. See *Compatibility and limits*.

---

*The rest of this README describes the T05.1 storage base, which T06 keeps.*


## What's fixed in T05.1

This is a small database-only correction to T05. The existing desktop reader,
renderer, demo content, launchers and user-facing wiki workflow are retained.
The SQLite content schema remains **v2** and history schema remains **v1**.

- **Read-only history:** opening a wiki read-only also opens its existing companion
  history database read-only. Version inspection and history verification work
  without enabling edits or creating a new history file.
- **Verified paired backups:** before publishing a snapshot, the application
  checks that the live content and history agree, copies both under a writer
  lock, and verifies the copied pair. A mismatch refuses backup creation.
- **Safe new-wiki creation:** both database files are initialized and verified
  privately before publication. If an ordinary creation error occurs, partial
  output is removed without overwriting anything already present.

**Keep the two database files together.** An unexpected power failure during
publication of two separate filenames can leave an incomplete pair; the wiki
will refuse it rather than silently invent history. A paired SQLite backup is
not a replacement for external/offline copies. Historical hashes inside the
writable history database detect inconsistencies, not malicious coordinated
replacement of both files.

## Architecture

The **interface and wiki mechanics have a clear boundary**. The PySide6 window
calls one framework-independent API (`app/wiki_core.py`) for all reading,
searching, browsing, writing, importing, exporting, history, contracts,
projects, forms, validation, backup and rendering. The same API is used by the
command-line starter and the test-data tool.

| Layer | Module | Holds |
|---|---|---|
| Desktop UI (PySide6) | `main.py` | entry point; re-exports the UI classes |
| | `ui_window.py` | main window: browse panel, preview, menus, actions |
| | `ui_dialogs.py` | post editor (fields, Toulmin argument), project-item and template choosers |
| | `ui_preview.py` | WebEngine page that only loads app-generated HTML |
| Core API (no Qt) | `wiki_core.py` | `WikiCore` — the only entry point for UIs and tools |
| Views (no Qt) | `views.py` | all generated HTML: post panels, dashboard, contracts page, kanban, waterfall |
| | `renderer.py` | TiddlyWiki markup → HTML |
| Rules (no Qt) | `field_schema.py` | contract schemas and the tightening ratchet |
| | `projects.py` | project kit, overview, due dates, children |
| | `forms.py` | forms kit, templates, board and phase data |
| | `pvs_bridge.py` | runs the exporter / reads a `pvs-snapshot` JSON → records (never imports PVS) |
| | `bundle.py` | notebook bundles: verified export, checked restore |
| | `pvs_sync.py` | refresh into the notebook, ownership, three-way status reconcile, reasoning tracking |
| | `changelog.py` | daily and refresh change-log entries, digest chain, verification |
| Tools | `tools/pvs_export.py` | the only code that loads PVS: workspace → JSON snapshot |
| | `tools/notebook_bundle.py` | bundle export / inspect / restore from a terminal |
| | `tools/make_test_data.py` | test data, including a two-refresh PVS loop |
| Storage | `history_db.py` | every write, with contract gate and history, in one transaction |
| | `db.py` | SQLite storage and reads; **no writers** (they could bypass history) |
| | `schema_migrations.py` | explicit side-by-side schema migration |

The content format, original reader/markup renderer, 15 demonstration posts,
SQLite content schema v2 and companion history schema v1 remain unchanged.
There is no server, plugin framework, new database, shard catalog or hidden
cross-wiki indexing. Each folder still contains one independent wiki pair.

**For future interfaces:** `app/wiki_core.py` is the supported entry point.
The storage modules are implementation details; do not access their SQLite
connections directly from a UI.

A second desktop UI, terminal UI or other client can call the same core using
ordinary Python, without importing PySide6:

```python
import sys
sys.path.insert(0, "app")  # when running from the extracted project root
from wiki_core import WikiCore

with WikiCore("MY_WIKI_DATA/personal_wiki.db") as wiki:
    print(wiki.search("research", mode="Text"))
    print(wiki.verify_history())
```

Opening another database means closing this `WikiCore` instance and opening
another; **operations never span multiple wiki folders**. Use a separate
instance in each background thread and close it when finished.

## Run the demo

1. Extract this folder to a **writable location**. Do not overwrite your existing T03 folder or another working database.
2. Install **Python 3.11+**, SQLite with FTS5/JSON (normally included), and PySide6. For example, on macOS/Linux: `python3 -m venv .venv` followed by `.venv/bin/python -m pip install PySide6`. On Windows: `py -3 -m venv .venv` and `.venv\Scripts\python -m pip install PySide6`.
3. Launch **Start PERSONAL.WIKI.command** on Mac, **Start PERSONAL.WIKI.bat** on Windows, or **Start PERSONAL.WIKI.sh** on Linux. Alternatively, run `python3 start_personal_wiki.py` (Windows: `py -3 start_personal_wiki.py`). On Linux without Qt WebEngine, you can use `WIKI_PREVIEW_MODE=qt python3 start_personal_wiki.py`.
4. The first launch creates **15 editable demo posts**, including [[09 — Post History and Integrity]], in `MY_WIKI_DATA/`. Start reading at **Welcome to PERSONAL.WIKI**.

If launch fails, run `python3 start_personal_wiki.py --check` then `python3 start_personal_wiki.py --prepare` to check or initialize storage without loading the GUI.

## One wiki folder, two independent SQLite files

```text
MY_WIKI_DATA/
  personal_wiki.db                # current posts, SQLite schema v2
  personal_wiki.history.sqlite    # full post history, SQLite schema v1
```

You choose the active wiki using **File → Open Database…**. Title/text/tag search and all create/edit/delete operations affect **only the currently open content database**. There are no shard references or extra fields in the content schema. If you want another independent wiki, use a separate folder, or **File → New Database…** in the folder of your choice; the application creates its matching history file. The starter launcher always opens its default `MY_WIKI_DATA` database on the next run, so a separately created database needs to be reopened through the File menu.

**Keep both SQLite files together.** Never restore the content DB alone or swap histories between similarly named databases. If the history file is missing, ordinary editing is refused. Opening a pre-T04 database prompts you to initialize a first history baseline. That initial snapshot captures the **present** records, not changes from before you enabled this feature.

## Post histories

- **Create:** the entire initial post is saved as `first_version`, with a UTC timestamp, reason, and SHA-256 digest. This includes imported posts and all TiddlyWiki custom fields.
- **Edit or rename:** you must supply a reason. The history records the complete **before** and **after** post objects and both digests, together with the time.
- **Delete:** you must supply a reason. The deleted post's final content remains in history. If you create a different post later using the same title, it starts a new history sequence.
- **Import JSON:** new posts have a first version. Importing with explicit overwrite requires a reason; the entire batch is rolled back if one operation fails. Protected historical posts remain protected.
- **Tiddler → View Post History…:** inspect versions, reasons, timestamps, source objects and hashes, including deleted posts. Since T06, *Restore This Version…* writes the chosen version back as a new version.
- **File → Verify Post History:** compare hashes and version sequences with the active content database. The application also checks consistency before opening a database for editing.

Post hashing uses SHA-256 of the complete JSON object with sorted keys and canonical compact JSON serialization (UTF-8). Hashes help detect accidents and mismatches; **they do not make a writable SQLite history file tamper-proof**. Anyone able to replace both a post and its history could forge a matching hash. For stronger protection, keep **independent backups** of the pair, preferably on external/offline media.

## Backup, JSON, and SQLite durability

**File → Snapshot Database…** now saves a *pair* of verified SQLite copies, named `your_backup.db` and `your_backup.history.sqlite`. Both are required for a complete restoration. The backup command refuses to overwrite an existing backup. Finish an import before backing up.

**File → Export JSON…** exports the current TiddlyWiki records and their unknown fields, but **does not export version history**. Export is not a replacement for the paired database backup. Do not copy just an open `.db` file, because SQLite journaling may be active.

To commit changes to both files in a single SQLite transaction, this edition uses temporary `ATTACH` connections with **DELETE rollback-journal mode** (rather than WAL) and `synchronous=EXTRA`. This changes SQLite's journal setting but **does not change the content schema v2**. An unexpected disk or OS failure is still a reason to verify and, if necessary, restore your last verified backup. Store active databases on a normal local disk, not a cloud-sync or network-mounted folder.

## Compatibility and limits

The original reader, editor, TiddlyWiki JSON format, and supported markup renderer remain in place. This is **not a complete TiddlyWiki widget/macro runtime**, encrypted vault, or multi-user server. Don't open the same content database for writing in an older app that does not know about its history file; this edition will refuse later mismatches rather than silently rewriting the history.

The separate authoritative research archive, historical C5 fixtures and prior engineering checkpoints are **not included**. T05.1 has passed focused headless storage and failure-injection checks on Linux, including a 10,455-object C5 import, history, JSON-equivalence and paired-backup rehearsal. These tests are not shipped in the lean application ZIP. **The actual desktop GUI on Windows, macOS and Raspberry Pi has not been acceptance-tested in this environment.**

See [`LICENSE`](LICENSE) for licensing.
