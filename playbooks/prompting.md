# Playbook: Prompting & Context Hygiene

**Status:** scaffold. MVP for accepted practices; micro-opt ROI is a REVIEW PR.

## MVP — widely accepted, no research needed
- Start a fresh conversation with `/clear` when the subject changes.
- Use `/handoff` to transfer context deliberately between sessions.
- Skip pleasantries; write terse, direct prompts.
- Prefer LeanCTX/map mode over raw files for large sources.

## REVIEW PR — needs measurement before asserting
- Micro-optimization ROI: line breaks, punctuation, capitalization, em dashes
  vs hyphens, double spaces, abbreviations — per-token cost and whether worth it.
