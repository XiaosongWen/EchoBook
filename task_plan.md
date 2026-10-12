# Task Plan: EchoBook Development Roadmap & Task 17 (Alignment Checkpoint & Resume)

## Overall Goal
Build EchoBook into a production-grade, highly resilient EPUB 3 Media Overlays narration generator that combines audiobooks with EPUB eBooks into synchronized read-aloud editions with zero manual intervention and robust interruption recovery.

---

## Current Roadmap Status

| Task ID | Task Name | Priority | Status | Spec Location |
|---|---|---|---|---|
| Tasks 01–16 | Core Pipeline, Decryptor, Alignment, SMIL, Gapless & Auto-Map | Core | **Complete** | `tasks/done/01-*.md` – `16-*.md` |
| **Task 17** | **Chapter-level Alignment Resume & Checkpointing** | **P1 (Resilience)** | **Next (In Progress)** | [tasks/17-alignment-checkpoint-and-resume.md](file:///Users/tomaswen/workspace/EchoPage/tasks/17-alignment-checkpoint-and-resume.md) |
| Task 18 | Workspace Lifecycle & Smart Pruning (`echobook clean`) | P1 (Storage) | Pending | [tasks/18-workspace-lifecycle-and-clean.md](file:///Users/tomaswen/workspace/EchoPage/tasks/18-workspace-lifecycle-and-clean.md) |
| Task 19 | Hardware-Aware Parallel Alignment & Concurrency Auto-Detection | P1 (Performance) | Pending | [tasks/19-parallel-alignment-and-vram-detection.md](file:///Users/tomaswen/workspace/EchoPage/tasks/19-parallel-alignment-and-vram-detection.md) |
| Task 20 | Power Management, Sleep Prevention & Auto-Sleep on Finish | P1 (Reliability) | Pending | [tasks/20-power-management-and-anti-sleep.md](file:///Users/tomaswen/workspace/EchoPage/tasks/20-power-management-and-anti-sleep.md) |

---

## Active Milestone: Task 17 - Chapter-level Alignment Resume & Checkpointing

### Current Phase: Phase 1 (Architecture & Requirements Analysis)

### Phase Breakdown

#### Phase 1: Requirements & Checkpoint Architecture
- [ ] Inspect existing `align_chapters` loop in `echobook/aligner.py` and workspace structure in `echobook/cli.py`.
- [ ] Design atomic checkpoint storage schema:
  - Per-chapter checkpoint files (`<work_dir>/checkpoints/ch_<spine_id>.json`) vs single append/atomic JSON.
  - Manifest / metadata header: model size, audio fingerprint / duration, alignment timestamp.
- [ ] Design resume detection & validation logic on build start.
- [ ] Define `--force` override behavior to bypass checkpoints when needed.
- **Status:** in_progress

#### Phase 2: Checkpoint Engine Implementation
- [ ] Implement atomic chapter flush (`save_chapter_checkpoint`) in `echobook/aligner.py`.
- [ ] Implement checkpoint discovery, integrity check, and resume loader (`load_checkpoints`).
- [ ] Wire checkpoint loop into `align()`: skip finished chapters, log clear progress banners (`Resuming 8/25 chapters completed...`).
- [ ] Add CLI flag `--force` to `echobook build` and `echobook align`.
- [ ] Add post-build checkpoint cleanup (prune per-chapter JSON files unless `--keep-temp` is set).
- **Status:** pending

#### Phase 3: Testing & Verification
- [ ] Unit tests for checkpoint atomic write, recovery, and corrupted file handling.
- [ ] Simulation test: interrupt simulated alignment midway, rerun with same `work_dir`, verify chapters 1..N-1 are skipped.
- [ ] Verification test: confirm resumed `alignment.json` matches uninterrupted build output.
- [ ] Verify `--force` ignores checkpoints and re-aligns all chapters.
- [ ] Run full project test suite to ensure zero regressions across existing 234 tests.
- **Status:** pending

#### Phase 4: Lifecycle Transition & Documentation
- [ ] Move `tasks/17-alignment-checkpoint-and-resume.md` to `tasks/done/17-alignment-checkpoint-and-resume.md`.
- [ ] Update `todo.md` and `task_plan.md` to advance to Task 18.
- [ ] Report completion using `/ponytail` concise style with exact verification command.
- **Status:** pending

---

## Key Decisions Made

| Decision | Rationale |
|---|---|
| Project renamed from EchoPage to EchoBook | Matches the core domain (audiobooks + ebooks) and unified CLI binary (`echobook`). |
| Global Task Management Rule | Standardized in `~/.gemini/config/rules/task_management.md` across all workspaces. |
| Per-chapter atomic JSON checkpoints | Avoids rewriting full multi-megabyte `alignment.json` every chapter; atomic write (`.tmp` + rename) prevents file corruption on sudden interruption. |
| Default `tasks/` + `tasks/done/` lifecycle | Minimizes context window usage; completed tasks are archived in `done/`, leaving only active backlog in `tasks/`. |

---

## Errors & Resolution History

| Error | Attempt | Resolution |
|---|---|---|
| `git rm` required for tracked rule file | 1 | `.agents/rules/task_management.md` was tracked in git commit history; removed using `git rm -f` to stage deletion. |
