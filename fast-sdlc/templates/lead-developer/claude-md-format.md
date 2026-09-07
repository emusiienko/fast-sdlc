<!-- Default format for CLAUDE.md — accept as-is or customize with lead-developer during Step 0 (Confirm Artifact Formats). Owned by whichever role's own folder this is — serves interactive Claude Code sessions started from that folder. Follow `sdlc` > Writing Discipline. Hard cap: 80 lines total — if an update would push past it, trim something else first, never let the file grow past this. -->

```markdown
# <Repo Name>

<One-line description of what this repo does and for whom.>

## Repository Structure
<Top-level folder map — one line per folder, a short phrase naming what it is, nothing more. Never narrate story/task history, ADR content, or implementation detail here — that's Technical Documentation/ADRs, already pointed to from Key Docs below. Update a folder's line in place when its purpose changes; never append to it.>

## Commands
| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server (port XXXX) |
| `npm run build` | Production build |
| `npm test` | Run test suite |

## Key Docs
<Pointers to where Architectural Design, Technical Documentation, Coding Conventions, and Stories actually live — resolve from fast-sdlc-project-setup.md, don't hardcode paths here>

## Gotchas
<Critical warnings — things NOT to do, known env quirks, security constraints>
```
