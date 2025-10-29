# Welcome to the Quality Management System (QMS)

This repository hosts the complete Quality Management System (QMS) and Regulatory Documentation Framework for our organization, developed in alignment with **ISO 13485:2016**, **EU MDR 2017/745**, and related standards for medical device software. It serves as both the documentation source and the operational platform for managing controlled processes, procedures, and records.

---

## Purpose

This repository provides a unified environment for authoring, reviewing, approving, and maintaining all QMS and regulatory documentation. It ensures:

* Full traceability of changes to documents and records.
* Compliance with ISO 13485 document and record control requirements.
* Controlled management of the regulatory strategy, technical documentation, and software lifecycle.
* Transparent collaboration between regulatory, quality, and development functions.

---

## Structure

* **/Canvases/** — Process definitions and flowcharts.
* **/Assets/** — All documents of the system such as Standard Operating Procedures (SOPs) and Work instructions.
* **/Templates/** — Controlled forms and document templates including metadata templates.
* **/Media/** — Images, PDFs and other media used in the Assets files.
* /Requirements/ - Requirements from applied regulations and standards to be referenced in Assets and Canvases.

---

## How to Contribute

Changes to the QMS follow the **CI → CR → CIA → CCR** control flow defined in the [QMS Change Control Procedure](./Assets/QMS%20Change%20Control%20Procedure.md):

1. **Change Initiation (CI)** — Create a branch that clearly identifies the intended change.
2. **Change Request (CR)** — Open a Pull Request to propose the change.
3. **Change Impact Assessment (CIA)** — Complete the PR template with impact analysis and justification.
4. **Change Control Record (CCR)** — Once approved and merged, the PR serves as the official record.

Developers may follow either of the two supported workflows:

* [QMS Git Work Instruction — Developer CLI](./Assets/QMS%20Git%20Work%20Instruction%20%E2%80%94%20Developer%20CLI.md)
* [QMS Git Work Instruction — GitHub Web Interface](./Assets/QMS%20Git%20Work%20Instruction%20%E2%80%94%20GitHub%20Web%20Interface.md)

---

## Getting Started

1. Authenticate to GitHub and clone the repository.
2. Read the [Document and Record Control Procedure](./Assets/Document%20and%20Record%20Control.md) to understand control rules.
3. Follow the relevant Git Work Instruction for your role (CLI or Web Interface).
4. Use descriptive branch names (e.g., `procedure/update-training-policy`).
5. Complete all required CIA sections before requesting review.

---

## Scope of this Repository

This repository includes and governs:

* The **QMS** for the organization.
* The **Regulatory Strategy** and **Compliance Plan** (CE-marking roadmap).
* The **Technical Documentation Framework**.

It is the authoritative source of all controlled content related to regulatory compliance and QMS operations.

---

## Contact

For questions regarding this repository or QMS processes:
📧 [jakob.dumky@noncomplicity.com](mailto:jakob.dumky@noncomplicity.com)

---

> *This repository represents a living management system. All updates must follow the established change control process to ensure continued regulatory compliance and data integrity.*
