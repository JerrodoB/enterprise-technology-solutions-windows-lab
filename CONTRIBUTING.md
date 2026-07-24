# Contributing Guidelines

Thank you for your interest in the **Enterprise Technology Solutions Windows Lab**.

This repository is maintained as a professional systems administration portfolio and follows enterprise documentation and infrastructure management practices.

---

# Project Philosophy

Every change to this repository should improve one or more of the following:

- Technical accuracy
- Documentation quality
- Infrastructure design
- Automation
- Maintainability
- Operational consistency

The objective is to demonstrate professional enterprise systems administration practices rather than simply complete technical exercises.

---

# Documentation Standards

All documentation should be:

- Clear
- Accurate
- Reproducible
- Professional
- Written using GitHub-Flavored Markdown (GFM)

Whenever practical, documentation should include:

- Objective
- Business or operational context
- Design considerations
- Implementation
- Validation
- Troubleshooting
- Lessons learned

---

# PowerShell Standards

PowerShell examples should:

- Use approved PowerShell verbs
- Be formatted for readability
- Include comments when appropriate
- Avoid hard-coded credentials
- Be safe to execute in a lab environment

Example:

```powershell
Import-Module ActiveDirectory

Get-ADUser -Identity "jsmith"
```

---

# Repository Organization

Documentation should remain within the appropriate directory.

| Directory | Purpose |
|-----------|---------|
| `docs/modules` | Module documentation |
| `docs/architecture` | Infrastructure design |
| `docs/standards` | Operational standards |
| `docs/decisions` | Architecture Decision Records (ADRs) |
| `docs/troubleshooting` | Known issues and resolutions |
| `docs/references` | Supporting technical references |
| `powershell` | Administrative scripts |
| `screenshots` | Configuration evidence |
| `diagrams` | Architecture diagrams |
| `reports` | Generated reports |

---

# Commit Guidelines

Commits should represent one logical unit of work.

Examples:

```text
Add enterprise README

Implement Organizational Unit hierarchy

Document Security Group standards

Add Active Directory PowerShell examples
```

Avoid combining unrelated changes into a single commit.

---

# Security

This repository must never contain:

- Passwords
- API keys
- Private keys
- Production credentials
- Confidential company information
- Personally identifiable production data

All examples should use fictional environments unless explicitly authorized for publication.

---

# Learning Philosophy

This project follows a structured learning methodology.

Every module should progress through the following stages:

```text
Learn
    ↓
Design
    ↓
Implement
    ↓
Validate
    ↓
Document
    ↓
Commit
```

Documentation is considered part of the implementation—not an optional activity.

---

# Version Control

Git is used to:

- Track infrastructure changes
- Maintain documentation history
- Record implementation milestones
- Support portfolio development

Each meaningful project milestone should be committed with a descriptive message.

---

# Goal

The long-term objective of this repository is to demonstrate the knowledge, documentation practices, and operational discipline expected of an Enterprise Windows Systems Administrator and, ultimately, an Enterprise Infrastructure Solutions Architect.