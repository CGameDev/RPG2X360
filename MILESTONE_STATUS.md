# RPG2X360 Milestone Status

**Roadmap:** RMXP-First v2.0  
**Active milestone:** M00 — Repository Foundation & Safety Contract  
**Status:** NOT STARTED  
**Next milestone:** M01 — RMXP Detection, Game.ini & Source Snapshot  
**Do not advance to M01 until M00 acceptance is reported.**

## M00 required deliverables

- [ ] solution/project scaffold
- [ ] DI/configuration/logging
- [ ] workspace abstraction
- [ ] read-only `ISourceProject` boundary
- [ ] process-execution prohibition in importer layer
- [ ] safe temp/output service
- [ ] synthetic test fixture framework
- [ ] WPF navigation shell and theme foundation
- [ ] CLI shell
- [ ] CI-friendly test command
- [ ] repository documentation integrated into the implementation

## M00 acceptance gate

- [ ] solution builds
- [ ] tests prove source inputs are not written
- [ ] importer services expose no API that launches `Game.exe`
- [ ] converter/analyzer contains no path that evaluates source Ruby/RGSS during import/analysis
- [ ] UI opens and can create/select a workspace without conversion logic
- [ ] no proprietary RPG Maker or Xbox SDK/XDK content is committed
- [ ] M00 completion report recorded

## Required M00 evidence

When M00 is completed, record at minimum:

- repository path and branch;
- commit/checkpoint identifier;
- files created/changed;
- build result;
- test result;
- source-read-only safety evidence;
- executable/Ruby non-execution safety evidence;
- known limitations;
- exact next action for M01.

Suggested evidence location:

`evidence/M00/`

## Resume instruction

A new Codex session should read `AGENTS.md`, `CODEX_MASTER_IMPLEMENTATION_PROMPT.md`, the master milestone, this status file, and `commands/M00_BOOTSTRAP.txt` before beginning or resuming M00.
