# Training Data Compliance Standard
## Spanforge Framework for Compliant AI Training Data

**Last Updated:** May 2, 2026  
**Effective Date:** May 2, 2026  
**Version:** 1.0  
**Status:** Open Standard (RFC-pending)

---

## Executive Summary

This standard defines what constitutes **compliant training data** for AI systems under:
- **EU AI Act** (Article 10 — High-Risk Systems)
- **GDPR** (Articles 5, 6, 13, 30)
- **DPDP Act** (India, 2023)
- **CCPA** (California)

It provides:
1. ✅ **PII detection framework** (30+ data types)
2. ✅ **Redaction standards** (techniques + verification)
3. ✅ **Data lineage requirements** (provenance tracking)
4. ✅ **Compliance audit checklist** (self-assessment)
5. ✅ **Tools & automation** (Spanforge CLI integration)

**Target audience:** Data engineers, ML practitioners, compliance officers, auditors.

---

## 1. Framework Overview

### 1.1 What This Standard Covers

This standard applies to **training datasets used to train or fine-tune AI models** that:
- Process personal data (GDPR scope)
- Are classified as "high-risk" (EU AI Act scope)
- Are deployed in regulated jurisdictions (CCPA, DPDP scope)
- May impact individuals' rights, freedoms, or safety

### 1.2 What This Standard Does NOT Cover

This standard does NOT address:
- ❌ **Model architecture** (how the model is built, not training data)
- ❌ **Training process** (optimization algorithms, hyperparameters)
- ❌ **Testing/validation data** (separate compliance framework needed)
- ❌ **Operational data** (data used for monitoring, not training)
- ✅ **Data claimed to be anonymized** — This standard DOES address it (see below)

**Important clarification on "anonymized" data:**

Under GDPR Article 4(1), "anonymized" data is data that **cannot identify individuals by any means, directly or indirectly**. In practice:
- ❌ Most datasets claimed "anonymized" are actually **pseudonymized** (still regulated personal data)
- ❌ Deleting names does NOT make data anonymous (other fields can re-identify)
- ✅ **True anonymization requires rigorous assessment** (k-anonymity ≥5, l-diversity, differential privacy)

**This standard addresses both:**
1. Datasets you claim are personal data (the majority)
2. Datasets claimed to be anonymized (requires verification per Section 5.4)

### 1.3 Why Training Data Compliance Matters

**The problem:**
- AI models trained on personal data can perpetuate or amplify biases
- Models trained on non-consensual data violate privacy regulations
- High-risk decisions require transparent, auditable training data

**The standard's goal:**
- Prove your training data is compliant
- Document data lineage and consent
- Detect and remediate PII before training
- Generate audit evidence for regulators

---

## 2. Training Data Compliance Checklist

### 2.1 Pre-Training (Before Training Data Is Used)

**Data Source Documentation:**
- [ ] Source identified (public dataset, customer data, synthetic, etc.)
- [ ] Source license documented (usage rights verified)
- [ ] Collection date recorded
- [ ] Collection method recorded (API, web scraping, survey, donation, etc.)
- [ ] Data collector identified (company, researcher, organization)

**Legal Basis (GDPR Article 6):**
- [ ] **Consent:** Individuals explicitly consented to data being used for training
  - Evidence: Consent form, email, checkbox with timestamp
  - Scope: Clearly stated "training AI models for [purpose]"
- **OR Legitimate Interest:** Using data for training serves legitimate interest without overriding individual rights
  - Legitimate Interest Assessment (LIA) completed
  - LIA shows balancing of interests
- **OR Contract:** Data subjects agreed to data training as part of service
  - Terms of Service reference training
  - Customer explicitly accepted

**Legal Basis (If EU AI Act applies):**
- [ ] **High-risk classification** determined
  - Model used for: hiring, lending, law enforcement, migration, education, etc.
  - Assessment: Does model fall under EU AI Act Article 6 or Annex III?
- [ ] **Documentation maintained**
  - Risk assessment (whether high-risk)
  - Justification (why data is necessary for this model)

**Data Source Verification:**
- [ ] No prohibited sources
  - ❌ Not from children without parental consent
  - ❌ Not from jail inmates
  - ❌ Not from protected health info (PHI) without HIPAA compliance
  - ❌ Not from financial data without proper access
- [ ] Source licensing allows intended use
  - Verify: Can I use this data to train AI?
  - Red flag: "For research only" — cannot use for commercial training

---

### 2.2 Data Preparation (As Data Is Prepared for Training)

**PII Detection:**
- [ ] Automated PII scan run (Spanforge or equivalent tool)
  - Using: 30+ PII detection patterns (see Section 3)
  - Coverage: All fields scanned for PII
  - Results: PII findings logged + remediated
- [ ] Manual spot-check performed
  - Random sample: 50–100 records reviewed by human
  - Check: Any PII missed by automated scan?
  - Documentation: Spot-check report signed off

**PII Remediation:**
- [ ] Identified PII redacted or removed
  - Method: [Redaction technique chosen — see Section 4]
  - Verification: Sample of redacted data reviewed
  - Evidence: Before/after comparison
- [ ] Redaction completeness verified
  - Residual data check: Can redacted values be reverse-engineered?
  - Quasi-identifier check: Can combination of fields re-identify?
  - Sign-off: Compliance officer confirms redaction sufficient

**Data Anonymization (if applicable):**
- [ ] **If claiming anonymization:** GDPR Article 4(1) standard met
  - Test: Can individual be identified? No → Anonymized ✅
  - Test: Can individual be identified with reasonable effort? No → Anonymized ✅
  - Common mistake: Deleting name doesn't anonymize (quasi-identifiers remain)
- [ ] **Anonymization technique documented**
  - Method: Aggregation, generalization, differential privacy, etc.
  - K-anonymity: Minimum group size? (typically k≥5)
  - L-diversity: Sensitive attributes diverse enough?
  - Evidence: Anonymization assessment report

**Sensitive Data Categories:**
- [ ] Identified & assessed (see Section 5)
  - Special categories per GDPR Article 9: Race, ethnicity, religion, unions, genetics, health, sex life
  - Quasi-identifiers: Age, ZIP code, income, job title + combinations
  - Biometric data: Facial recognition, fingerprints, voice
  - Behavioral data: Shopping history, browsing, location patterns

**Data Schema Validation:**
- [ ] Required fields verified
  - All critical fields present: [list your required fields]
  - No unexpected nulls
- [ ] Field types correct
  - Strings are strings, numbers are numbers, dates are dates
  - No corrupted or malformed data
- [ ] Field ranges reasonable
  - Ages between 0–120 (not 999999)
  - Dates in expected range (not year 1800 or 2500)
  - Prices positive (not negative, unless intentional)

---

### 2.3 Data Lineage & Documentation

**Provenance Documentation:**
- [ ] **Data lineage** (from source to training)
  - Original source → transformations → final training dataset
  - Each transformation step: What changed? Why?
  - Data loss tracking: Records dropped? Why?
- [ ] **Version control** (if dataset versioned)
  - Git repository for dataset schema
  - Data snapshots at key points
  - Reproducibility: Can this exact dataset be recreated?
- [ ] **Chain of custody** (who accessed the data?)
  - Employees who accessed data
  - Dates of access
  - Purpose of access

**Consent & Legal Basis Record:**
- [ ] **Consent records** (if consent is the basis)
  - Consent forms signed + dated
  - Email confirmations + timestamps
  - Opt-in confirmations (not opt-out)
  - Consent language clear: "Your data will be used to train AI models for [purpose]"
- [ ] **Legitimate Interest Assessment** (if using LIA)
  - Documented business purpose
  - Assessment of impact on individuals
  - Conclusion: Why legitimate interest isn't overridden
- [ ] **Contract proof** (if using contract basis)
  - ToS/contract referencing training
  - Customer signature/acceptance
  - Timestamp of acceptance

**Data Source Authority:**
- [ ] **Source authorized**
  - Data controller: Who owns this data? Are they authorized to share?
  - Rights verified: Did they have the right to consent on behalf of data subjects?
  - Licenses checked: Can we legally use this data for training?
- [ ] **Third-party obligations**
  - DPA signed (if relevant): Data processor agreement in place
  - Subprocessor management: If data went through third parties, are they approved?

---

### 2.4 Bias Analysis (Recommended, Especially for High-Risk Models)

**Why bias analysis matters for compliance:**

Under GDPR Article 22, EU AI Act Article 10, and CCPA, high-risk models must be assessed for:
- **Discrimination risk** — Does the model discriminate against protected groups?
- **Transparency** — Can you explain why the model made a decision for a specific person?
- **Human review** — Can humans override automated decisions for high-stakes outcomes?

**Bias analysis is not just a fairness best practice—it's a regulatory requirement.**

**For auditors:**
- Bias analysis proves due diligence in model training
- Documents awareness of discrimination risks
- Shows you took steps to detect and mitigate unfairness
- Required evidence in regulatory investigations

---

**Demographic Representation:**
- [ ] Distribution checked
  - Age: Represented across all age groups? (not just 25–35)
  - Gender: All genders represented? (not just male/female)
  - Geography: All regions/countries represented?
  - Income/socioeconomic status: Diversity present?
- [ ] Imbalance identified
  - Data points per group: Any group with <5% representation?
  - Underrepresentation documented: Which groups are missing?
  - Risk assessment: Could this cause bias in model predictions?

**Class Imbalance:**
- [ ] Target variable distribution checked
  - Positive class: X% of dataset
  - Negative class: Y% of dataset
  - Imbalance ratio: Is it severe (>10:1)?
- [ ] Mitigation plan (if severe imbalance)
  - Oversampling: Duplicate minority class examples
  - Undersampling: Remove majority class examples
  - Algorithmic: Use loss weighting or calibration
  - Synthetic data: Generate synthetic minority examples

**Fairness Assessment:**
- [ ] Fairness metrics calculated (if model predicts outcomes affecting people)
  - Demographic parity: Equal outcomes across groups?
  - Equalized odds: Equal true/false positive rates across groups?
  - Calibration: Model confidence accurate across groups?
- [ ] Disparate impact assessed
  - 80% rule: Outcome rate for protected group ≥80% of majority group?
  - Disparate impact ratio: If <80%, potential bias exists
  - Documentation: Is bias inherent to data or can it be mitigated?

---

### 2.5 Compliance Review & Sign-Off

**Internal Review:**
- [ ] **Data owner review**
  - Person: [Name], [Date]
  - Confirms: All data sources documented
  - Confirms: All PII removed/redacted
  - Confirms: Compliance checklist complete

- [ ] **Compliance officer review**
  - Person: [Name], [Date]
  - Confirms: Legal basis established
  - Confirms: No regulatory violations
  - Confirms: Ready for training

- [ ] **Security review**
  - Person: [Name], [Date]
  - Confirms: Data encrypted in transit/rest
  - Confirms: Access controls in place
  - Confirms: No unauthorized copies

**External Review (Optional, Recommended for High-Risk Models):**
- [ ] **Third-party audit**
  - Auditor: [Name/Firm]
  - Date: [Date]
  - Findings: [Summary]
  - Recommendation: Approved / Conditional / Rejected

**Final Approval:**
```
This training dataset is compliant with:
☐ GDPR (EU)
☐ CCPA (California)
☐ DPDP (India)
☐ EU AI Act (if applicable)

Approved for training by:
_____________________________  ___________
Compliance Officer Signature    Date

_____________________________  ___________
Data Owner Signature             Date

_____________________________  ___________
Model Owner Signature            Date
```

---

## 3. PII Detection Framework

### 3.1 PII Categories & Detection Patterns

**A. Identifiers (Can directly identify a person)**

| Category | Examples | Regex/Pattern | Confidence |
|---|---|---|---|
| Full Name | "John Smith", "María García" | `[A-Z][a-z]+ [A-Z][a-z]+` | High |
| Email Address | "john@example.com" | `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z\|a-z]{2,}\b` | High |
| Phone Number | "(555) 123-4567", "555-123-4567" | `\b\d{3}[-.]?\d{3}[-.]?\d{4}\b` | High |
| Postal Address | "123 Main St, Springfield, IL 62701" | [City name, ZIP code] | Medium |
| Social Security Number (US) | "123-45-6789" | `\b\d{3}-\d{2}-\d{4}\b` | High |
| Passport Number | "123456789" | `\b[A-Z]{1,2}\d{6,9}\b` | Medium |
| Driver License | "D1234567" | `\b[A-Z]\d{7,8}\b` | Medium |
| National ID | (varies by country) | [Country-specific] | Medium |
| Tax ID / VAT | "12-3456789" | `\b\d{2}-\d{7}\b` | Medium |

**B. Biometric Data**

| Category | Examples | Detection Method | Confidence |
|---|---|---|---|
| Facial Recognition | Image with face | Computer vision (facial detector) | High |
| Fingerprint | Image of fingerprint | Computer vision (ridge pattern) | High |
| Iris Scan | Image of iris | Computer vision (iris pattern) | High |
| Voice Print | Audio recording | Voice analysis (spectral characteristics) | Medium |
| DNA | Genetic sequence | Pattern matching (ATCG sequences) | High |
| Palm Print | Image of palm | Computer vision (ridge pattern) | Medium |

**C. Location Data**

| Category | Examples | Pattern | Confidence |
|---|---|---|---|
| GPS Coordinates | "40.7128° N, 74.0060° W" | `[-+]?\d{1,2}\.\d+,\s*[-+]?\d{1,2}\.\d+` | High |
| IP Address | "192.168.1.1" | `\b(?:\d{1,3}\.){3}\d{1,3}\b` | High |
| MAC Address | "00:1A:2B:3C:4D:5E" | `\b([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})\b` | High |
| WiFi SSID | "CoffeeShop_WiFi_5GHz" | [Known wifi networks] | Medium |
| Geofence Data | "User at Starbucks, 123 Main St" | [Location names + text] | Medium |

**D. Financial Data**

| Category | Examples | Pattern | Confidence |
|---|---|---|---|
| Credit Card | "4532-1111-2222-3333" | Luhn algorithm | High |
| Bank Account | "123456789" | Length + check digit validation | Medium |
| Routing Number | "021000021" | Known US bank routing numbers | Medium |
| IBAN | "DE89370400440532013000" | `\b[A-Z]{2}[0-9A-Z]{2}[0-9 ]{4}[0-9A-Z]{3}[0-9]{13}\b` | High |
| Cryptocurrency Address | "1A1z7agoat..." | Bitcoin/Ethereum address format | High |
| PayPal Account | "user@paypal.com" | Known PayPal domain | Medium |

**E. Health Data**

| Category | Examples | Detection Method | Confidence |
|---|---|---|---|
| Medical Record Number | "MR1234567" | Healthcare identifier patterns | Medium |
| Medication | "Metformin 500mg", "Lisinopril" | Drug name dictionary | Medium |
| Diagnosis | "Type 2 Diabetes", "Hypertension" | Medical condition vocabulary | Medium |
| Lab Results | "HbA1c: 7.2%", "HDL: 45 mg/dL" | Lab value patterns | Medium |
| Fitness Data | "Heart rate: 72 bpm", "VO2 max: 42" | Biometric measurement patterns | Low |

**F. Communication Data**

| Category | Examples | Detection Method | Confidence |
|---|---|---|---|
| Email Message | Full email text | Keyword patterns (Hi John, regards, etc.) | Medium |
| Chat Message | "Hey, how are you?" | Conversational patterns | Low |
| Phone Call | Audio of phone conversation | Speech analysis | Medium |
| SMS Message | "Hi, call me later" | Message length + patterns | Low |
| Slack/Teams | "@john Please review this" | @ mentions + internal context | Medium |

**G. Work/Education Data**

| Category | Examples | Pattern | Confidence |
|---|---|---|---|
| Employee ID | "E12345" | `\b[A-Z]\d{4,6}\b` | Medium |
| Job Title | "Software Engineer", "Manager" | Job title vocabulary | Low (could be pseudonymized) |
| Employer | "Google", "Microsoft" | Company name vocabulary | Low (could be pseudonymized) |
| School Name | "Stanford University", "MIT" | Education institution vocabulary | Low |
| Student ID | "S123456" | Student ID pattern | Medium |
| Grades/Transcripts | "A- in CS101", "GPA: 3.8" | Academic notation patterns | Low (with context) |

**H. Behavioral/Preference Data (Context-Dependent)**

| Category | Examples | Context | Risk Level |
|---|---|---|---|
| Shopping History | "Purchased: laptops, gaming gear" | Commercial purchasing only → Low PII risk. But if reveals health (diabetes supplies) → High | Medium |
| Browsing History | "Visited: healthcare sites, psychology blogs" | Aggregated, anonymized → Low. Linked to individual → High | Medium |
| Social Media Activity | "Posted about depression", "follows mental health accounts" | Reveals sensitive personal information (health, mental state) | 🔴 High |
| Location History | "Visited: mosque, gay bar, hospital" | Reveals religious beliefs, sexual orientation, health → Very sensitive | 🔴 High |
| Interests/Hobbies | "Interested in: LGBTQ+ topics, religion X" | Can reveal protected characteristics → Sensitive | Medium |

**⚠️ Context matters:** Behavioral data that reveals protected characteristics (religion, sexual orientation, health, union membership, politics) is high-risk, even if names are removed.

**I. Government/Legal Data**

| Category | Examples | Pattern | Confidence |
|---|---|---|---|
| Visa Number | "A123456789" | Visa number format | Medium |
| Refugee Status | "Refugee from Syria", "Asylum pending" | Legal status keywords | Medium |
| Criminal Record | "Convicted of shoplifting", "DUI arrest" | Criminal justice vocabulary | Medium |
| Court Records | "Case #2024-12345", "Judge Smith presiding" | Legal document patterns | Medium |

**J. Demographic Data (Quasi-Identifiers)**

| Category | Examples | Detection Method | Confidence |
|---|---|---|---|
| Exact Age | "Born 1985-03-15" | Date of birth patterns | High |
| Exact ZIP Code | "10001" | ZIP code pattern (5 digits US) | Medium |
| Race/Ethnicity | "Caucasian", "Hispanic", "Asian" | Protected category vocabulary | Medium |
| Gender/Sex | "Male", "Female", "Non-binary" | Gender classification vocabulary | Medium |
| Religion | "Christian", "Muslim", "Hindu", "Jewish" | Religion keywords | Medium |
| Sexual Orientation | "Gay", "Lesbian", "Bisexual", "Asexual" | Sexual orientation keywords | Medium |
| Union Membership | "Member of UAW", "AFSCME card holder" | Union name vocabulary | Medium |
| Political Affiliation | "Democrat", "Republican", "Libertarian" | Party affiliation keywords | Medium |

### 3.2 Detection Tool Integration

**⚠️ IMPORTANT: Limitations of Regex-Based Detection**

This section provides regex patterns for automated PII detection. **However, regex-based detection alone is insufficient for comprehensive PII identification.** You must supplement it with:

1. **Contextual analysis** — Understanding field names, data source, usage context
2. **Model-based detection** — NLP/ML models trained on actual PII examples
3. **Manual verification** — Human review of high-risk records
4. **Domain expertise** — Someone who understands the data source and sensitivity

**Why regex alone fails:**
- ✅ Can detect obvious patterns (email, SSN)
- ❌ Misses context-dependent PII ("John" in job title vs. as a person name)
- ❌ Prone to false positives ("123-45-6789" in a test file, not real SSN)
- ❌ Cannot detect implicit PII (shopping history revealing health conditions)
- ❌ Ineffective for non-English text, regional variations

**Better approach:**
```
Regex patterns (quick scan)
    ↓
NLP/ML model (contextual detection)
    ↓
Manual spot-check (human verification)
    ↓
Stakeholder review (domain expert approval)
```

**Spanforge Training Data Scanner CLI:**
```bash
spanforge validate --dataset training_data.jsonl \
  --dataset-scan \
  --check-pii-field-names \           # Regex-based
  --check-pii-values \                 # Regex-based
  --use-ml-model contextual_pii \     # ML-based detection (supplementary)
  --manual-review-sample 100 \         # Request human review
  --required-fields employee_id,name,email \
  --fail-on-violations \
  --format json
```

**Output interpretation:**
```json
{
  "pii_scan_method": "regex + ml_model + manual_review",
  "confidence_level": "medium",
  "findings": [...],
  "manual_review_required": true,
  "note": "Regex-based findings should be verified contextually"
}
```

---

## 4. Data Redaction & Anonymization

### 4.1 Redaction Techniques

**Technique 1: Complete Removal**
```
BEFORE: "John Smith, 40 years old, lives in Springfield, IL 62701"
AFTER:  "[REDACTED], [REDACTED] years old, lives in [REDACTED], [REDACTED]"

Use case: Not needed for training (e.g., name, address)
Risk: May lose contextual information
```

**Technique 2: Hashing (One-Way)**
```
BEFORE: john.smith@example.com
AFTER:  d4b1c63f28f2f85e3f9e7c5c4c5c5c5c (SHA-256 hash)

Use case: Preserve uniqueness without revealing identity
Risk: Can be reverse-engineered with rainbow tables
Mitigation: Add salt (random prefix)
```

**Technique 3: Generalization (Aggregation)**
```
BEFORE: Age 37, ZIP 94102, Income $125000, Job "Software Engineer"
AFTER:  Age 30-40, ZIP 941**, Income $100k-$150k, Job "Technology"

Use case: Preserve approximate value while reducing granularity
Risk: May reduce model utility
Benefit: Reduces re-identification risk
```

**Technique 4: Differential Privacy**
```
BEFORE: "John Smith, age 37, diagnosis diabetes"
AFTER:  "Anonymous Person, age ~37±5, diagnosis ~diabetes (noise added)"

Use case: Mathematical proof of privacy while allowing aggregate analysis
Risk: Computational overhead
Benefit: Strong privacy guarantees
```

**Technique 5: Synthetic Data Replacement**
```
BEFORE: Real patient data: ages, diagnoses, test results
AFTER:  Synthetically generated data maintaining statistical properties

Use case: Training models without real personal data
Risk: May not capture rare cases or edge cases
Benefit: Complete privacy guarantee + GDPR compliance
```

**Technique 6: Tokenization (Reversible)**
```
BEFORE: email = "john@example.com"
AFTER:  email = "TOKEN_12345" (with lookup table encrypted separately)

Use case: Preserve exact matching without exposing value
Risk: Lookup table must be protected
Benefit: Can restore original if needed for verification
```

### 4.2 Redaction Completeness Verification

**⚠️ CRITICAL: "Zero Findings" Does NOT Mean "Zero PII"**

After redacting PII, you will re-scan the dataset and expect 0 findings. **However, absence of detected PII does not guarantee the dataset contains no personal data.** This is a common misconception that creates legal risk.

**Why false negatives occur:**

1. **Detection limitations** (see Section 3.2)
   - Regex misses context-dependent PII
   - ML models have false negative rates (typically 10–20%)
   - New PII types not in detection patterns
   - Non-English text, abbreviations, variations

2. **Implicit PII** (cannot be pattern-detected)
   - Shopping history revealing health conditions
   - Browsing patterns revealing sexual orientation
   - Location history revealing religious affiliations
   - Behavioral data revealing mental health status

3. **Combination risks** (quasi-identifiers + context)
   - Age 35 + ZIP 94102 + "Software Engineer" = identifiable person
   - Exact job title + company + start date = identifiable
   - Rare diagnosis + exact age + location = identifiable

**Verification process must include:**

**Step 1: Automated Verification**
```
After redaction, re-scan for PII
→ Re-run scanner on redacted dataset
→ Expected result: 0 PII findings (from automated tools)
```

**Step 2: Manual Validation (REQUIRED)**
```
Sample 50–100 redacted records
→ Manual human review: Can this person be identified?
  - By themselves? (Age 35, "Software Engineer" alone = not identifiable)
  - By combination? (Age 35 + ZIP + job = identifiable?)
  - By external data? (Can this be linked to public records?)
→ Conclusion: Safe to train on OR needs further redaction
```

**Step 3: Quasi-Identifier Check (REQUIRED)**
```
Remaining fields checklist:
  ✅ Exact date of birth removed? (Generalize to year)
  ✅ Exact ZIP code removed? (Use ZIP prefix or city)
  ✅ Exact job title removed? (Generalize to job category)
  ✅ Combination: No field combination uniquely identifies
```

**Step 4: Domain Expert Review (REQUIRED)**
```
Someone familiar with the data source + business context reviews:
  ✅ Are there implicit PII I (shopping history, location, etc.)?
  ✅ Could rare diagnoses, sensitive attributes remain?
  ✅ Is any behavioral data that reveals protected characteristics?
→ Sign-off: "Safe to train with" OR "Needs further redaction"
```

**Compliance officer must document:**
- [ ] Who conducted manual review
- [ ] Sample size and selection method
- [ ] Findings from review
- [ ] Date of review
- [ ] Conclusion: Safe / Not Safe / Needs Further Review

---

## 5. Sensitive Data Categories (GDPR Article 9)

### 5.1 Special Categories

These require **explicit legal basis** (not just legitimate interest):

| Category | GDPR Article | Examples | Risk Level |
|---|---|---|---|
| Race/Ethnicity | Article 9 | "Latino", "Asian", "African-American" | 🔴 High |
| Political Opinions | Article 9 | "Democrat", "Support UBI", "Pro-choice" | 🔴 High |
| Religious Beliefs | Article 9 | "Christian", "Muslim", "Atheist" | 🔴 High |
| Trade Union Membership | Article 9 | "UAW member", "AFSCME card holder" | 🔴 High |
| Genetic Data | Article 9 | DNA sequences, genetic tests | 🔴 High |
| Biometric Data | Article 9 | Facial recognition, fingerprints, iris scans | 🔴 High |
| Health Data | Article 9 | Medical conditions, medications, lab results | 🔴 High |
| Sex Life / Sexual Orientation | Article 9 | "Gay", "Lesbian", dating app data | 🔴 High |

### 5.2 Quasi-Identifiers

These can indirectly identify people, especially in combination:

| Quasi-Identifier | Risk | Example Risk |
|---|---|---|
| Exact Age | Medium | Only 30,000 people age 37 in a city → Identifiable |
| Exact ZIP Code | Medium | Only 5 people in ZIP 10001, age 35, male → Identifiable |
| Exact Birthday | Medium | Birthday alone not unique, but + other data = identifiable |
| Job Title | Low-Medium | "CEO" is unique in small companies |
| Salary Range | Medium | "Income $500k+" is rare → Identifiable |
| Combination | 🔴 High | Age 37 + ZIP 94102 + "Software Engineer" = specific person |

### 5.3 Data Minimization

**Principle:** Collect only what's necessary for the model

```
Question: Does the model need the individual's exact birth date?
→ NO: Generalize to age range (30-40) or remove entirely
→ YES (if age needed): Use only birth year, not exact date

Question: Does the model need exact ZIP code?
→ NO: Use ZIP prefix (94*) or city name
→ YES (if location needed): Use first 3 digits only

Question: Does the model need exact job title?
→ NO: Categorize (Technology, Healthcare, Finance)
→ YES (if job needed): Generalize to industry, not exact title
```

---

## 6. Data Lineage & Provenance

### 6.1 Data Origin Documentation

**Required fields in data metadata:**

```json
{
  "dataset_name": "employee_training_v1",
  "collection_date": "2025-01-01",
  "collection_method": "Customer survey (opt-in)",
  "source_url": "https://example.com/survey",
  "source_license": "CC-BY 4.0",
  "source_organization": "Acme Corp",
  "data_owner_contact": "john@acme.com",
  "data_controller": "Acme Corp",
  "data_processor": "Spanforge Technologies",
  "processing_purpose": "Train ML model for HR automation",
  "legal_basis": "Explicit consent (per Article 6(1)(a) GDPR)",
  "consent_date": "2025-01-01",
  "consent_evidence": "survey_responses_signed.pdf",
  "geographic_scope": ["US", "EU", "India"],
  "retention_end_date": "2030-01-01",
  "estimated_subjects": 50000
}
```

### 6.2 Transformation Log

Track every change to the dataset:

```
TRANSFORMATION 1 (2025-01-15):
├── Input: Raw survey responses (53,000 rows)
├── Operation: Remove duplicates
├── Logic: Keep first response per email
├── Output: 52,500 rows (500 duplicates removed)
└── Justification: Prevent model overfitting to duplicate responses

TRANSFORMATION 2 (2025-01-16):
├── Input: Deduplicated responses (52,500 rows)
├── Operation: Scan and redact PII
├── Logic: Anonymize names, emails, phone, addresses
├── Output: 52,495 rows (5 rows with unrecoverable PII deleted)
└── Justification: GDPR compliance (Article 5(1)(e) data minimization)

TRANSFORMATION 3 (2025-01-17):
├── Input: PII-redacted data (52,495 rows)
├── Operation: Generalize quasi-identifiers
├── Logic: Age → age_range, ZIP → ZIP_prefix, Job → job_category
├── Output: 52,495 rows (no rows deleted, columns aggregated)
└── Justification: Further de-identification per k-anonymity standard (k≥5)

FINAL OUTPUT (2025-01-18):
├── Dataset name: employee_training_final_v1
├── Rows: 52,495
├── Columns: 18 (down from 25 original)
├── PII status: 0 findings on re-scan ✅
├── Anonymity status: k-anonymity k≥5 verified ✅
└── Approved for training: YES ✅
```

### 6.3 Reproducibility

Document how to recreate the exact dataset:

```bash
# Script: recreate_training_dataset.sh

# Step 1: Download original data
aws s3 cp s3://acme-data/survey_responses_20250101.csv survey_raw.csv

# Step 2: Run transformation pipeline
python transform.py \
  --input survey_raw.csv \
  --dedup-by email \
  --redact-pii yes \
  --pii-patterns patterns.yaml \
  --generalize age,zip,job \
  --output survey_final.csv

# Step 3: Verify
spanforge validate --dataset survey_final.csv \
  --dataset-scan \
  --fail-on-violations || exit 1

# Step 4: Archive
sha256sum survey_final.csv > survey_final.csv.sha256
git add survey_final.csv.sha256
git commit -m "Dataset v1.0 reproducible hash"
```

Result: Anyone can recreate the exact dataset from raw sources.

---

## 7. Compliance Checklist & Sign-Off

### 7.1 Self-Assessment Form

```markdown
# Training Data Compliance Self-Assessment

**Dataset Name:** [e.g., "employee_hr_model_training_v1"]
**Organization:** [Your company]
**Assessment Date:** [YYYY-MM-DD]
**Assessed By:** [Name, Title]

## Section A: Data Sourcing

### A1. Data Source
- [ ] Source identified and documented
- [ ] Source license verified (usage rights OK for training)
- [ ] Collection date recorded
- [ ] Collection method documented
- [ ] Data controller identified
- [ ] Data processor identified (if applicable)

### A2. Legal Basis (Choose one or more)
- [ ] Explicit Consent (GDPR Article 6(1)(a))
  - [ ] Consent forms signed
  - [ ] Consent language clear: "...for training AI..."
  - [ ] Timestamps recorded
  - Consent date: [DATE]
  
- [ ] Legitimate Interest (GDPR Article 6(1)(f))
  - [ ] Legitimate Interest Assessment (LIA) completed
  - [ ] LIA shows interests don't override rights
  - [ ] LIA approved by: [Name]
  
- [ ] Contract (GDPR Article 6(1)(b))
  - [ ] Training mentioned in contract
  - [ ] Contract signed + dated
  - Contract execution date: [DATE]

### A3. EU AI Act (if high-risk model)

**⚠️ IMPORTANT: High-Risk Classification Requires Legal Interpretation**

The EU AI Act's "high-risk" classification (Article 6 + Annex III) is not a simple checklist. Classification may require:
- Legal analysis of your specific use case
- Regulatory interpretation (guidelines still evolving)
- Expert assessment of risk level
- Consultation with legal counsel

**This checklist provides a starting point, not legal determination.**

- [ ] Model potentially high-risk? [YES/NO]
  - Does your model fall into one of these categories?
    - **Biometric identification/categorization** (facial recognition, etc.)
    - **Employment, benefits, education** (hiring, job allocation, student assessment)
    - **Law enforcement** (suspect detection, predictive policing)
    - **Migration, asylum, border control**
    - **Administration of justice** (court decisions, parole)
    - **Critical infrastructure** (power, water, transport)
  - **Note:** If your model isn't in these categories, likely NOT high-risk
  
  - If YES: Continue below. If NO: Skip to next section.
  
- [ ] **Legal review conducted?** [YES/NO]
  - Have you consulted with legal counsel on EU AI Act applicability?
  - Recommendation: Have a lawyer confirm classification
  - Legal opinion date: [DATE]
  - Lawyer name: [NAME]
  - Conclusion: High-risk / Not high-risk / Uncertain → needs further analysis
  
- [ ] **Risk assessment completed?** [YES/NO]
  - Risk assessment document: [FILE]
  - Assessment covered:
    - [ ] Severity of harms if model fails
    - [ ] Vulnerable populations affected
    - [ ] Discrimination/bias risks
    - [ ] Transparency requirements
  
- [ ] **Documentation maintained?** [YES/NO]
  - Risk assessment: [FILE]
  - Legal memo: [FILE]
  - Training data audit: [FILE]
  - Model card: [FILE]

**Important:** This standard provides tools for training data compliance, but does not determine whether your model is "high-risk" under the EU AI Act. Consult legal counsel for classification.

---

## Section B: Data Preparation

### B1. PII Detection
- [ ] Automated scan completed (tool: [e.g., "Spanforge"])
  - Date: [DATE]
  - Records scanned: [NUMBER]
  - PII detected: [NUMBER]
  
- [ ] Manual spot-check completed
  - Sample size: [e.g., "100 records"]
  - Findings: [e.g., "None" or "List issues"]
  - Signed by: [Name, Date]

### B2. PII Remediation
- [ ] Identified PII redacted/removed
  - Method used: [e.g., "Complete removal", "Hashing", "Generalization"]
  - Coverage: [e.g., "100% of detected PII"]
  
- [ ] Redaction completeness verified
  - Re-scan completed? [YES/NO]
  - Re-scan result: [e.g., "0 PII findings"]
  - Quasi-identifier check done? [YES/NO]
  
- [ ] Signed off by compliance officer
  - Name: [Name]
  - Date: [DATE]
  - Finding: [e.g., "Redaction sufficient for training"]

### B3. Data Anonymization (if applicable)
- [ ] Anonymization claimed? [YES/NO]
  - If NO: Skip this section
  - If YES: Continue below
  
- [ ] GDPR Article 4(1) standard met? [YES/NO]
  - Justification: [How is this truly anonymized?]
  
- [ ] Anonymization technique: [e.g., "k-anonymity with k=5"]
  - [ ] K-anonymity: k≥5
  - [ ] L-diversity: Sufficient diversity
  - [ ] Differential privacy parameters: [If applicable]

### B4. Sensitive Data Categories
- [ ] Sensitive data identified (GDPR Article 9)?
  - Identified categories: [e.g., "Health data, Race/ethnicity"]
  - [ ] Special legal basis established (not just consent)
  - Basis: [e.g., "Explicit consent + contract"]
  
- [ ] Quasi-identifiers identified?
  - Identified: [e.g., "Age, ZIP, Job title"]
  - [ ] Generalized/aggregated to reduce re-identification risk
  - K-anonymity achieved: k=[e.g., "≥5"]

### B5. Data Schema Validation
- [ ] Required fields present: [List]
  - [ ] All rows have required fields
  
- [ ] Field types correct: [List key fields]
  - [ ] No unexpected data types
  
- [ ] Value ranges reasonable: [Examples]
  - [ ] Ages: 0–120
  - [ ] Dates: 1900–2026
  - [ ] Prices: No negative values

---

## Section C: Bias & Fairness (Optional, Recommended)

### C1. Demographic Representation
- [ ] Demographics analyzed
  - Demographic breakdown: [e.g., "60% male, 40% female"]
  - Geographic: [e.g., "45% US, 35% EU, 20% Other"]
  
- [ ] Imbalance identified
  - Underrepresented groups: [e.g., "Non-binary <5%"]
  - Risk level: [Low/Medium/High]

### C2. Class Imbalance
- [ ] Target variable distribution checked
  - Positive class: [X%]
  - Negative class: [Y%]
  - Imbalance ratio: [Z:1]
  
- [ ] Mitigation applied (if imbalance severe)
  - Method: [e.g., "Oversampling minority class"]

### C3. Fairness Metrics
- [ ] Fairness assessed
  - Demographic parity: [YES/NO]
  - Equalized odds: [YES/NO]
  - Calibration: [YES/NO]
  
- [ ] Disparate impact assessed
  - 80% rule test: [PASS/FAIL]
  - If FAIL: Mitigation plan: [Describe]

---

## Section D: Documentation & Lineage

### D1. Provenance
- [ ] Data origin documented
  - Source: [URL or name]
  - Collection date: [DATE]
  - Collection method: [e.g., "Survey"]
  
- [ ] Processing history logged
  - Transformations: [Number: e.g., "3"]
  - Each transformation documented: [YES/NO]
  
- [ ] Reproducibility verified
  - Can dataset be recreated? [YES/NO]
  - Reproducibility script: [FILE]

### D2. Consent & Legal Basis
- [ ] Consent records maintained
  - Storage location: [e.g., "S3://bucket/consents/"]
  - Encryption: [e.g., "AES-256"]
  - Access log: [YES/NO]
  
- [ ] LIA (if applicable)
  - Document: [FILE]
  - Approval: [NAME, DATE]

### D3. Data Deletion & Retention
- [ ] Retention end date set: [DATE]
- [ ] Deletion procedure documented: [YES/NO]
- [ ] Deletion schedule: [E.g., "Delete 90 days after completion"]

---

## Section E: Final Sign-Offs

### E1. Data Team
- **Data Owner:**
  - Name: _____________________
  - Title: _____________________
  - Signature: _________________ Date: _______
  - Confirms: PII removed, legal basis established, ready for training

### E2. Compliance Team
- **Compliance Officer:**
  - Name: _____________________
  - Title: _____________________
  - Signature: _________________ Date: _______
  - Confirms: Compliant with GDPR, CCPA, DPDP, EU AI Act

### E3. Security Team
- **Security Officer:**
  - Name: _____________________
  - Title: _____________________
  - Signature: _________________ Date: _______
  - Confirms: Data encrypted, access controlled, audit log maintained

### E4. Model Team
- **Model Owner:**
  - Name: _____________________
  - Title: _____________________
  - Signature: _________________ Date: _______
  - Confirms: Data suitable for training, no foreseeable harms

---

## FINAL APPROVAL

✅ This training dataset is approved for use in training AI models.

Compliance Status: **APPROVED**
Compliance Date: [DATE]
Approved By: [All signatures above]

**⚠️ CRITICAL ACCOUNTABILITY NOTE:**

**Sign-off does not eliminate legal liability.** This approval documents that due diligence was performed and the dataset was deemed compliant at the time of review. However:

- ❌ Sign-off does NOT guarantee regulatory compliance
- ❌ Sign-off does NOT guarantee absence of PII or bias
- ❌ Sign-off does NOT protect signers from liability if issues emerge later
- ❌ Sign-off is NOT a legal opinion or certification

Signers remain responsible for:
1. **Accuracy of assessment** — Data was actually reviewed and checked
2. **Compliance with regulations** — Legal basis was actually established
3. **Ongoing monitoring** — The dataset is monitored for issues during training
4. **Response to issues** — If problems emerge, they are addressed promptly

**Liability:** If regulators later find the dataset non-compliant or the model causes harm due to training data issues:
- Signers may be held responsible for negligence or willful non-compliance
- "We followed the checklist" is not a legal defense
- "The tool said it was OK" is not a legal defense
- Only actual demonstrated compliance will protect signers

**Recommendation:** Maintain this compliance record and be prepared to explain your process to regulators if asked. Document any concerns raised during review.

Next Review Date: [DATE + 1 year]
```

---

## 8. Tools & Implementation

### 8.1 Spanforge CLI Commands

**⚠️ IMPORTANT: Tools Assist Compliance, But Don't Guarantee It**

The Spanforge Training Data Scanner is a **tool to support compliance**, not a replacement for human judgment, legal review, or manual validation. The scanner can:

✅ Detect obvious patterns (email, SSN, phone numbers)
✅ Generate audit reports and documentation
✅ Track compliance progress and findings
✅ Flag high-risk records for manual review

❌ **Cannot (and should not be relied upon solely to):**
❌ Guarantee absence of PII (false negatives are common)
❌ Determine legal basis validity or consent authenticity
❌ Assess complete regulatory compliance (requires lawyer + compliance officer)
❌ Evaluate whether redaction is sufficient (requires manual validation)
❌ Replace human judgment and domain expertise
❌ Generate legal opinions or certifications

**Tool output is advisory, not determinative. All critical findings require human review and stakeholder sign-off.**

---

**Example workflow:**

```bash
# Step 1: Scan dataset for PII (automated)
spanforge validate --dataset training_data.jsonl \
  --dataset-scan \
  --check-pii-field-names \
  --check-pii-values \
  --fail-on-violations \
  --format json > pii_report.json

# Step 2: Check required fields (automated)
spanforge validate --dataset training_data.jsonl \
  --required-fields employee_id,name,email \
  --format text

# Step 3: Generate compliance report (automated)
spanforge compliance report training_data.jsonl \
  --framework EU_AI_ACT \
  --format pdf > compliance_report.pdf

# Step 4: Manual review (HUMAN REQUIRED)
# → Open pii_report.json
# → Review all flagged records
# → Determine: Is this actually PII? Can it be re-identified?
# → Document decisions

# Step 5: Compliance review (LAWYER + COMPLIANCE OFFICER REQUIRED)
# → Legal basis established? ✅/❌
# → Consent valid? ✅/❌
# → Regulatory compliant? ✅/❌/⚠️ Needs work
# → Ready for training? YES/NO/CONDITIONAL
```

**Critical interpretation rules:**
- "0 PII findings" ≠ "Dataset is safe" (false negatives exist)
- Tool output is input to human review, not final decision
- Sign-off by compliance officer = formal determination, not tool output

### 8.2 Python Integration

```python
from spanforge_training_data import DatasetScanner

# Initialize scanner
scanner = DatasetScanner(
    pii_patterns='patterns.yaml',
    quasi_identifier_rules='quasi_id_rules.yaml'
)

# Scan dataset
results = scanner.scan_dataset(
    data_path='training_data.jsonl',
    sample_size=100  # Manual spot-check sample
)

# Get report
report = results.generate_report()
print(f"PII findings: {report.total_findings}")
print(f"Clean rows: {report.clean_rows}")

# Export for audit
report.export_to_pdf('audit_report.pdf')
report.export_for_compliance('compliance_bundle.zip')
```

---

## 9. FAQ & Common Questions

**Q: Does "anonymized" data need a legal basis?**
A: No. True anonymization removes regulatory requirements. But be careful — most data claimed "anonymized" is just "pseudonymized" (still personal data under GDPR). Verify with anonymization assessment.

**Q: Can I use publicly available data without consent?**
A: Not necessarily. "Public" doesn't mean "free to use for training." Check the license. Public GitHub code ≠ free for AI training.

**Q: What about synthetic data? Does it need the compliance checklist?**
A: Partially. If synthetic data is based on personal data, you still need to document the source data's compliance. Pure synthetic data (generated without personal data) has no PII risk.

**Q: Who is responsible — data team or compliance team?**
A: Shared responsibility. Data team handles PII removal + documentation. Compliance team verifies legal basis + audit trail. Model team ensures no foreseeable harms.

**Q: How often do I need to re-audit?**
A: Annually, or if the dataset is modified. If you add new data, re-scan for PII and re-verify legal basis.

---

## 10. References & Standards

**Regulations Cited:**
- GDPR Articles 4, 5, 6, 9, 13, 30, 37 (eu-lex.europa.eu)
- EU AI Act Article 10 (High-Risk Systems) — Recitals 71, 72
- CCPA §1798.100 et seq. (cppa.ca.gov)
- DPDP Act 2023 (India) — meity.gov.in
- HIPAA (if health data) — hhs.gov

**Standards Referenced:**
- ISO/IEC 27001 (Information Security Management)
- ISO/IEC 27701 (Privacy Information Management)
- ISO/IEC 12207 (AI Governance)
- Differential Privacy (Dwork & Roth, 2014)

**Tools Mentioned:**
- Spanforge Training Data Scanner (open-source)
- Great Expectations (Data Validation)
- Fairlearn (Fairness Metrics)
- Diffprivlib (Differential Privacy)

---

## Governance & Standard Evolution

### How This Standard is Maintained

This standard is published as an **open proposal** and evolves through community feedback and regulatory changes.

**Versioning:**
- **v1.0** (Current) — Initial release, May 2026
- **v1.1** (Planned Q3 2026) — Feedback incorporation, tool improvements
- **v2.0** (Planned 2027) — EU AI Act finalization, DPDP clarifications

**How to Propose Changes:**
1. **GitHub Issues** (https://github.com/spanforge/training-data-standard)
   - Feature requests: "Add support for [PII type]"
   - Bug reports: "Regex pattern doesn't catch [case]"
   - Questions: "How should we handle [scenario]?"

2. **RFC Process** (Request for Comments)
   - Major changes require RFC (formal proposal document)
   - RFC template: See GOVERNANCE.md
   - Discussion period: 30 days minimum
   - Adoption: Requires consensus from Spanforge + community feedback

3. **Contributions**
   - PII detection patterns (submit in YAML)
   - Bias analysis techniques
   - Compliance checklist improvements
   - Tool integrations
   - Real-world case studies

**Governance Committee:**
- **Spanforge Standards Team** (maintains specification)
- **Community Contributors** (propose improvements)
- **Regulatory Liaisons** (track GDPR/AI Act changes)
- **External Auditors** (verify credibility)

**Annual Review:**
- Every May: Review for regulatory changes
- Every May: Incorporate community feedback
- Every May: Release v1.x or v2.0 update

**Questions?** Contact: sriram@getspanforge.com

---

**[See Section 7.1 for complete compliance self-assessment template]**

---

## Appendix B: PII Detection Patterns (YAML)

```yaml
pii_patterns:
  email:
    pattern: '\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b'
    confidence: high
    action: redact
  
  ssn_us:
    pattern: '\b\d{3}-\d{2}-\d{4}\b'
    confidence: high
    action: redact
  
  phone_us:
    pattern: '\b(\d{3}[-.]?\d{3}[-.]?\d{4}|\d{10})\b'
    confidence: high
    action: redact
  
  passport:
    pattern: '\b[A-Z]{1,2}\d{6,9}\b'
    confidence: medium
    action: redact
  
  credit_card:
    pattern: '\b(\d{4}[-\s]?){3}\d{4}\b'
    confidence: high
    action: redact
    
  full_name:
    pattern: '[A-Z][a-z]+ [A-Z][a-z]+'
    confidence: medium
    action: redact
    context: full_name_field_only
```

---

## Important Disclaimer

**This standard is not exhaustive.** It provides a framework for training data compliance but should be applied in conjunction with:
- Applicable laws and regulations in your jurisdiction (GDPR, CCPA, DPDP, EU AI Act, etc.)
- Legal advice from qualified counsel
- Domain expertise from subject matter experts
- Best practices as they evolve in the field

**As regulations change and new guidance emerges**, this standard will be updated through the governance process outlined in the Governance section.

---

**Document Status:** ✅ Open Standard (v1.0)  
**Version:** 1.0 (Initial Release)  
**Next Planned Updates:** Q3 2026 (v1.1 feedback incorporation)  
**Next Major Review:** May 2, 2027

---

**Questions or feedback?** Contact: sriram@getspanforge.com  
**Propose improvements:** Submit GitHub issue to https://github.com/spanforge/training-data-standard
