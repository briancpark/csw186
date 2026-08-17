# csw186

Self-study of UC Berkeley **CS W186 — Introduction to Database Systems**
([course site](https://cs186berkeley.net/), [project specs](https://cs186.gitbook.io/project/)).

- `study/` — module notes
- `proj/` — the seven course projects, each a separate repository wired in as a submodule

## Projects

Each project lives in its own private repo. The upstream skeleton history is
preserved in every one and tagged **`skeleton`**, so the solution work is exactly
the commits after that tag:

```bash
git log --oneline skeleton..HEAD    # solution commits only
git diff skeleton..HEAD             # the entire solution as one diff
```

| | Project | Repo | Public tests |
|---|---|---|---|
| 0 | Setup / `StringDataBox` | [csw186-proj0](https://github.com/briancpark/csw186-proj0) | 1 |
| 1 | SQL (Lahman baseball DB) | [csw186-proj1](https://github.com/briancpark/csw186-proj1) | 17 |
| 2 | B+ Trees | [csw186-proj2](https://github.com/briancpark/csw186-proj2) | 20 |
| 3 | Joins & Query Optimization | [csw186-proj3](https://github.com/briancpark/csw186-proj3) | 31 |
| 4 | Concurrency (multigranularity 2PL) | [csw186-proj4](https://github.com/briancpark/csw186-proj4) | 76 |
| 5 | Recovery (ARIES) | [csw186-proj5](https://github.com/briancpark/csw186-proj5) | 19 |
| 6 | MongoDB / NoSQL | [csw186-proj6](https://github.com/briancpark/csw186-proj6) | 10 |

Projects 0 and 2–5 share the RookieDB Java codebase and build cumulatively:
proj3 and proj4 include proj2's B+tree, and proj5 includes everything before it.
`csw186-proj5` is therefore the complete database — B+tree indices, external
sorting and joins, a System R query optimizer, multigranularity locking, and
ARIES recovery.

Projects 1 and 6 are standalone query assignments.

## Tests and CI

Every repo runs its full test suite on GitHub Actions on each push and pull
request, as a matrix over the project test categories.

The Java projects:

```bash
mvn test -Dproj=<category> -P public     # course-provided tests
mvn test -Dproj=<category> -P student    # additional tests written here
```

Categories are `0`, `2`, `3Part1`, `3Part2`, `4Part1`, `4Part2`, `4Integration`, `5`.
Two traps worth knowing: the category interfaces are siblings rather than nested,
so `-Dproj=3` does **not** cover `3Part1`/`3Part2`; and a bare `mvn test` with no
`-P` activates a default `nonsystem` profile that can run zero tests while still
reporting `BUILD SUCCESS`. Always pass an explicit profile.

Each project also carries additional `@Category(StudentTests.class)` tests for
spec behavior the course tests leave uncovered.

### Reproducing the data-dependent projects

**proj1** needs `lahman.db`, which the course distributes via a Google Drive link
that requires sign-in and so cannot be fetched from CI. `tools/build_lahman.py`
rebuilds an equivalent database from the public Chadwick Bureau
`baseballdatabank` **v2020.1** release, and asserts the data vintage (Batting
2019 / HallOfFame 2018 / Salaries 2016) so a newer release cannot silently change
expected output. All 17 checks pass against a database built from scratch by that
script.

**proj6** needs the *legacy* `mongo` shell, which `test.py` invokes but which was
removed in MongoDB 6.0+ — MongoDB **4.4** is required. Its CI always validates
query syntax; the full run is gated behind `workflow_dispatch` plus a
`PROJ6_DATA_URL` secret, since the dataset is course-licensed.

## Working with the submodules

```bash
git clone --recurse-submodules https://github.com/briancpark/csw186.git
git submodule update --init --recursive   # if already cloned
```
