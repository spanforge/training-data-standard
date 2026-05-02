# Training Data Compliance Standard

**By [Spanforge](https://getspanforge.com) · v1.0 · May 2026 · Open Standard**

A practical framework for building compliant AI training datasets under GDPR, the EU AI Act, CCPA, and India's DPDP Act.

---

## What This Is

Training data is where most AI compliance failures begin — not at deployment.

This standard gives data engineers, ML practitioners, and compliance teams a shared vocabulary and process for:

- Detecting and removing PII before training (30+ data types covered)
- Establishing and documenting legal basis (consent, legitimate interest, contract)
- Tracking data lineage from source to final dataset
- Running bias and fairness checks tied to regulatory requirements
- Generating audit-ready sign-off documentation

It is not a product. It is not automated. It is a **written standard** — a checklist, a set of definitions, and a sign-off process that teams can follow and auditors can verify.

---

## Files

| File | Description |
|---|---|
| [`SPANFORGE_Training_Data_Compliance_Standard_v1_0.md`](./SPANFORGE_Training_Data_Compliance_Standard_v1_0.md) | Full standard in Markdown |
| [`SPANFORGE_Training_Data_Compliance_Standard_v1.0.docx`](./SPANFORGE_Training_Data_Compliance_Standard_v1.0.docx) | Word document version (formatted, printable) |

---

## What's Covered

**10 sections:**

1. Framework Overview — scope, what this standard does and does not cover
2. Compliance Checklist — pre-training, data preparation, bias analysis, sign-off
3. PII Detection Framework — 30+ PII types across 10 categories (A–J) with detection patterns
4. Redaction & Anonymization — 6 techniques with completeness verification steps
5. Sensitive Data Categories — GDPR Article 9 special categories and quasi-identifiers
6. Data Lineage & Provenance — required metadata fields and transformation log template
7. Self-Assessment Form — Sections A–E with final sign-off block
8. Tools & Implementation — Spanforge CLI commands and Python integration
9. FAQ — common questions with direct answers
10. References — regulations, technical standards, and tools cited

---

## Regulations Referenced

| Regulation | Scope |
|---|---|
| GDPR (EU) | Articles 4, 5, 6, 9, 13, 30, 37 |
| EU AI Act | Article 10, Annex III, high-risk system requirements |
| CCPA (California) | Consumer data rights |
| DPDP Act (India, 2023) | Personal data collected in India |

---

## Important Notes

**This standard is not exhaustive.** It should be used alongside applicable laws, qualified legal counsel, and domain expertise.

**Zero PII detections ≠ safe.** Automated scans have false negative rates of 10–20%. Manual validation and domain expert review are required.

**Sign-off documents due diligence, not legal immunity.** Signers remain responsible for the accuracy of their assessment and compliance with applicable regulations.

**EU AI Act high-risk classification** requires legal interpretation — it cannot be determined by checklist alone. Consult legal counsel.

---

## Versioning

| Version | Date | Status |
|---|---|---|
| v1.0 | May 2026 | Current |
| v1.1 | Q3 2026 | Planned — community feedback + tool updates |
| v2.0 | 2027 | Planned — EU AI Act final rules + DPDP clarifications |

---

## Contributing

Contributions are welcome. This standard improves through real-world use.

**Ways to contribute:**
- Open an issue to report an error, ambiguity, or gap
- Submit a PR with improved PII detection patterns (YAML format, see Appendix B of the standard)
- Propose new sections or techniques via the RFC process (see `GOVERNANCE.md`)
- Share case studies of applying the standard in practice

**RFC process:** Major changes require a formal proposal with a 30-day community review period before adoption.

---

## License

[MIT](./LICENSE) — use freely, attribution appreciated.

---

## Contact

Questions or feedback: [sriram@getspanforge.com](mailto:sriram@getspanforge.com)

Built by [Spanforge](https://getspanforge.com) — AI governance and compliance infrastructure.
