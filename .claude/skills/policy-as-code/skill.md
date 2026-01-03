# Policy as Code (OPA & CD Standards) Skill

## 🎯 Trigger
Activate this skill when the user asks to:
- "Verify our CD maturity."
- "Check for broken dependencies in the practices JSON."
- "Write a Rego policy to enforce a standard."
- "Audit a practice against the MinimumCD.org standards."

## 📖 Domain Context (cd-practices.json)
This repo contains a hierarchical data structure of Continuous Delivery practices.
- **Practices:** Definitive list of CD capabilities (e.g., "Trunk-based Development").
- **Dependencies:** A mapping of which practices must be in place before others can be achieved.
- **Maturity Levels:** Range from 0 (Foundational) to 3 (Expert/Root).

## ⚖️ Governance Rules
1. **Dependency Validation:** Every `depends_on_id` must have a matching `id` in the `practices` array.
2. **Maturity Check:** No high-level practice (Level 2 or 3) should be implemented without its Level 0/1 dependencies being met.
3. **Completeness:** Every practice must include 'benefits' and 'requirements' to be considered valid.

## 🛠️ Rego Logic Standards
When writing OPA policies, follow this structure:
```rego
package cd_policy

# Deny if a practice lacks required metadata
deny[msg] {
    some p in input.practices
    count(p.requirements) == 0
    msg := sprintf("Practice '%s' is missing requirements", [p.id])
}