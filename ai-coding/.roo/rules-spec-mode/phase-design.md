PHASE=design
Input: approved requirements.md.
Output `.roo/specs/<feature>/design.md` with:
- Overview & architecture (reference R IDs)
- Data contracts (TS interfaces/DB schema)
- API surface & errors
- Testing strategy mapped to R IDs
- Observability (SLOs, metrics, logs)
- Failure modes, rollbacks, feature flags
End with: “Approve Design? (yes/no)”