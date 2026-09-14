# Generative AI Security, Risk and Governance Assessment

**Fictional organisation:** Northstar Financial Services (NFS)  
**Assessment date:** September 2026  
**Assessment type:** Enterprise GenAI / LLM security, risk, governance and control assessment  
**Frameworks:** NIST AI RMF 1.0, NIST AI RMF Generative AI Profile, OWASP Top 10 for LLM Applications 2025, NIST CSF 2.0, ISO/IEC 27001:2022, ISO/IEC 42001:2023

> **Portfolio objective:** Demonstrate the ability to translate GenAI risks into implementable technical guardrails, governance controls, owners, evidence, testing procedures and assurance activities.

---

## 1. Executive Summary

Northstar Financial Services is a fictional multinational financial-services organisation with approximately 5,000 employees across the US, UK, EU and Middle East. NFS has adopted ChatGPT Enterprise, Claude Enterprise, Gemini for Workspace and an internally hosted retrieval-augmented generation (RAG) assistant. It is also piloting AI agents connected to CRM, ticketing, document and security platforms.

Rapid adoption has created risks that traditional information-security controls do not fully address. LLM applications can process untrusted instructions, retrieve enterprise data, generate unreliable content and invoke tools. This assessment therefore evaluates GenAI as both an **AI governance issue and a cybersecurity control problem**.

The assessment identifies twelve material risks, scores inherent and residual risk, assigns control owners, defines evidence, provides control tests and maps the control environment to NIST AI RMF, NIST CSF 2.0, ISO/IEC 27001 and ISO/IEC 42001.

### Highest priorities

1. Prompt injection
2. Sensitive information disclosure / data leakage
3. Excessive agency and privileged tool use
4. Insecure plugins and third-party integrations
5. Hallucination / misinformation and weak human oversight
6. Shadow AI
7. Third-party AI dependency
8. Model/data manipulation and RAG integrity

The target operating model is:

**AI Use Case → Inventory → Risk Classification → Impact/Security Assessment → Controls → Testing → Approval → Monitoring → Incident/Exception Management → Reassessment**

---

# 2. Fictional Enterprise Scenario

| Attribute | Description |
|---|---|
| Organisation | Northstar Financial Services |
| Industry | Financial services |
| Employees | 5,000 |
| Regions | US, UK, EU, Middle East |
| Known AI use cases | 40 |
| Approved GenAI | ChatGPT Enterprise, Claude Enterprise, Gemini for Workspace |
| Internal AI | Northstar Knowledge Assistant (RAG) |
| Agent pilots | Security Operations Agent; Customer Service Agent |
| Cloud model | Hybrid cloud |
| Governance maturity | Developing |
| Primary concern | GenAI adoption is outpacing governance and security assurance |

### Business use cases

- Customer-service response drafting
- Internal knowledge search
- Software development assistance
- Cybersecurity investigation support
- Marketing content generation
- Legal-document summarisation
- HR communications
- Financial research
- Meeting summarisation
- Document classification
- Workflow automation

---

# 3. Reference GenAI Architecture

```text
Users
  |
  v
AI Applications / Chat Interfaces
  |
  +---- External LLM APIs
  +---- Internal LLM / RAG
  |
  v
Policy + Guardrail Layer
  |
  +---- DLP
  +---- Data classification
  +---- Prompt validation
  +---- Identity / RBAC
  |
  v
Agent / Tool Layer
  |
  +---- CRM
  +---- Ticketing
  +---- Email
  +---- Document repositories
  +---- Security APIs
  |
  v
Enterprise Data
  |
  +---- Vector database
  +---- Knowledge repositories
  +---- Logs / telemetry
```

**Security principle:** the LLM must not automatically inherit the full permissions of the human user or service account. Permissions must be enforced outside the model.

---

# 4. Assessment Methodology

## 4.1 Risk formula

`Risk Score = Likelihood × Impact`

| Score | Level | Treatment |
|---:|---|---|
| 1–4 | Low | Monitor |
| 5–9 | Moderate | Mitigate |
| 10–16 | High | Formal treatment and management oversight |
| 17–25 | Critical | Immediate treatment / escalation |

### Likelihood

1 Rare · 2 Unlikely · 3 Possible · 4 Likely · 5 Almost certain

### Impact

Impact considers confidentiality, integrity, availability, privacy, financial loss, regulatory exposure, customer harm, reputation and operational disruption.

---

# 5. GenAI Risk Taxonomy

This project uses OWASP's 2025 LLM risks as the principal technical taxonomy and extends it with enterprise governance risks. OWASP's 2025 list includes Prompt Injection, Sensitive Information Disclosure, Supply Chain, Data and Model Poisoning, Improper Output Handling, Excessive Agency, System Prompt Leakage, Vector and Embedding Weaknesses, Misinformation and Unbounded Consumption. citeturn0search1turn0search3

The enterprise taxonomy is:

1. Prompt and instruction risks
2. Data and privacy risks
3. Model/output reliability risks
4. Agent and tool risks
5. Identity and access risks
6. Application-security risks
7. Supply-chain risks
8. Governance and compliance risks
9. Human-factor risks
10. Availability and cost risks

---

# 6. Enterprise GenAI Risk Register

| ID | Risk | Category | L | I | Inherent | Key Control | Owner | Residual |
|---|---|---|---:|---:|---:|---|---|---:|
| GAI-001 | Prompt injection | Security | 4 | 5 | 20 Critical | Input validation + adversarial testing | AI Security | 10 High |
| GAI-002 | Sensitive information disclosure | Privacy/Security | 4 | 5 | 20 Critical | DLP + classification + access control | CISO | 10 High |
| GAI-003 | Hallucination / misinformation | Reliability | 4 | 4 | 16 High | Grounding + human review + evaluation | Business Owner | 6 Moderate |
| GAI-004 | Excessive agency | Security | 4 | 5 | 20 Critical | Least privilege + tool allowlisting | IAM / AI Security | 8 Moderate |
| GAI-005 | Insecure plugins/tools | Supply Chain | 4 | 4 | 16 High | Plugin review + allowlist | Security Architecture | 8 Moderate |
| GAI-006 | Model/data manipulation | Integrity | 3 | 5 | 15 High | Provenance + integrity checks | AI Engineering | 6 Moderate |
| GAI-007 | IP leakage | Privacy/IP | 4 | 4 | 16 High | DLP + acceptable-use controls | Legal/Security | 8 Moderate |
| GAI-008 | Shadow AI | Governance | 3 | 5 | 15 High | Approved-tool policy + discovery | GRC / IT | 6 Moderate |
| GAI-009 | Inadequate human oversight | Governance | 4 | 4 | 16 High | Human approval gates | Business Owner | 6 Moderate |
| GAI-010 | Third-party AI dependency | Supply Chain | 3 | 5 | 15 High | Vendor due diligence + contracts | Procurement/GRC | 8 Moderate |
| GAI-011 | System prompt leakage | Security | 3 | 4 | 12 High | Secret separation + testing | AI Engineering | 6 Moderate |
| GAI-012 | RAG/vector weakness | Security | 4 | 4 | 16 High | Access-aware retrieval + vector security | AI Engineering | 8 Moderate |

---

# 7. Detailed Risk and Control Assessments

## GAI-001 — Prompt Injection

**Risk statement:** Untrusted user or retrieved content alters model instructions, causing disclosure, manipulated output or unauthorised tool use.

OWASP identifies Prompt Injection as LLM01:2025 and notes impacts including sensitive-data disclosure, manipulated outputs, unauthorised function access and potentially arbitrary commands in connected systems. citeturn0search10

### Controls

- Treat external content as untrusted data.
- Separate system instructions from user/retrieved content.
- Validate high-risk inputs.
- Enforce tool policies outside the LLM.
- Conduct adversarial prompt-injection testing.
- Monitor suspicious prompt patterns.
- Use output validation and rate limiting.

### Evidence

Prompt-test reports, application-security results, tool-policy configuration, security logs and remediation tickets.

**Residual risk:** 10 — High.

---

## GAI-002 — Sensitive Information Disclosure / Data Leakage

**Risk statement:** Confidential, personal, financial, legal or security-sensitive information may enter or leave GenAI workflows without authorisation.

OWASP's LLM02:2025 covers sensitive information such as PII, financial details, health information, confidential business data, credentials and legal documents. citeturn0search8

### Controls

- AI data-classification policy
- DLP before external submission
- Data minimisation
- Masking/tokenisation
- Enterprise IAM
- Vendor retention controls
- Logging and monitoring
- Employee training

**Evidence:** DLP alerts, classification matrix, vendor terms, access reviews and training records.  
**Residual risk:** 10 — High.

---

## GAI-003 — Hallucination / Misinformation

### Risk

The LLM may produce plausible but incorrect information, fabricated citations or unsupported recommendations.

### Controls

- Retrieval grounding
- Authoritative source requirements
- Citation validation
- Output evaluation
- Human review
- Restricted use for high-impact decisions
- Benchmark datasets
- Feedback and escalation mechanisms

### Human-review rule

```text
LLM output → automated validation → qualified human review → approved output
                                      ↘ reject / revise
```

**Residual risk:** 6 — Moderate.

---

## GAI-004 — Excessive Agency

OWASP describes excessive agency as arising from excessive functionality, excessive permissions or excessive autonomy. citeturn0search7

### Controls

- Least privilege
- Tool/function allowlisting
- Function-level permissions
- Human approval for high-impact actions
- Transaction limits
- Segregation of duties
- Emergency kill switch
- Session and execution limits

### Example

**Bad:** `LLM Agent → Full CRM Administrator`

**Target:** `LLM Agent → read profile + draft case + recommend action; no delete/change/approve functions`

**Residual risk:** 8 — Moderate.

---

## GAI-005 — Insecure Plugins and Tools

### Controls

- Approved plugin catalogue
- Security architecture review
- Vendor assessment
- OAuth scope minimisation
- API authentication
- Secrets management
- Tool input/output validation
- Network segmentation
- Continuous monitoring
- Retirement process

**Evidence:** plugin approval, vendor assessment, API configuration, scope review and test results.  
**Residual risk:** 8 — Moderate.

---

## GAI-006 — Model Manipulation / Data Poisoning

### Controls

- Data provenance
- Source validation
- Dataset integrity checks
- Version control
- Change approval
- Model evaluation
- Data-quality monitoring
- Retrieval-source trust scoring

**Residual risk:** 6 — Moderate.

---

## GAI-007 — Intellectual Property Leakage

### Controls

- AI acceptable-use policy
- DLP
- Source-code scanning
- Data classification
- Approved AI tools
- Contractual restrictions
- Employee training
- Monitoring

**Residual risk:** 8 — Moderate.

---

## GAI-008 — Shadow AI

Employees may use personal ChatGPT/Claude accounts, public AI transcription tools, browser extensions or coding assistants outside governance controls.

### Controls

1. AI acceptable-use policy
2. Approved-tool catalogue
3. AI-service discovery
4. Endpoint/browser controls where lawful
5. Approved alternatives
6. Employee reporting channel
7. Risk-based enforcement

**Evidence:** tool register, discovery reports, policy acknowledgements and exceptions.  
**Residual risk:** 6 — Moderate.

---

## GAI-009 — Inadequate Human Oversight

### Controls

- Human-in-the-loop requirements
- Decision authority matrix
- Review checklists
- High-impact restrictions
- Reviewer competency requirements
- Escalation procedures
- Quality sampling

**Residual risk:** 6 — Moderate.

---

## GAI-010 — Third-Party AI Dependency

### Controls

- Vendor due diligence
- Security questionnaire
- Contractual security requirements
- Data-processing agreement
- Subprocessor review
- Incident-notification clause
- BCP/DR assessment
- Exit strategy
- Model/service-change monitoring

**Residual risk:** 8 — Moderate.

---

## GAI-011 — System Prompt Leakage

### Controls

- Never store secrets in prompts
- Separate secrets from model context
- Prompt confidentiality testing
- Output filtering
- Access controls
- Monitoring

**Residual risk:** 6 — Moderate.

---

## GAI-012 — Vector and Retrieval Weaknesses

### Controls

- Document-level ACL enforcement
- Identity-aware retrieval
- Vector-store access control
- Tenant isolation
- Metadata filtering
- Retrieval logging
- Embedding security review
- Deletion verification

**Residual risk:** 8 — Moderate.

---

# 8. Enterprise Control Catalogue

| ID | Control | Owner | Frequency | Evidence |
|---|---|---|---|---|
| GOV-01 | AI acceptable-use policy | GRC | Annual | Approved policy |
| GOV-02 | GenAI risk assessment | AI Governance | Annual/trigger | Assessment |
| GOV-03 | Approved AI inventory | GRC | Monthly | Inventory |
| GOV-04 | AI exception process | GRC | As needed | Exception register |
| GOV-05 | Human oversight policy | AI Governance | Annual | Policy |
| GOV-06 | AI incident process | SOC/GRC | Annual test | Exercise report |
| SEC-01 | DLP for AI data flows | Security | Continuous | DLP logs |
| SEC-02 | IAM/RBAC | IAM | Quarterly | Access review |
| SEC-03 | Tool allowlisting | AI Security | Monthly | Tool register |
| SEC-04 | Prompt-injection testing | AppSec | Quarterly | Test report |
| SEC-05 | AI red-team testing | AI Security | Semiannual | Red-team report |
| SEC-06 | Logging and monitoring | SOC | Continuous | SIEM records |
| SEC-07 | Secrets management | Security | Continuous | Scan results |
| SEC-08 | API security | AppSec | Continuous | API tests |
| DAT-01 | Data classification | Data Governance | Annual | Classification register |
| DAT-02 | Data minimisation | Privacy | Annual | Assessment |
| DAT-03 | Data retention | Privacy/Legal | Annual | Retention schedule |
| DAT-04 | Data provenance | AI Engineering | Per change | Lineage record |
| DAT-05 | Retrieval ACL enforcement | AI Engineering | Quarterly | Access test |
| DAT-06 | DLP monitoring | Security | Continuous | DLP reports |

---

# 9. AI Risk-to-Control Matrix

| Risk | DLP | IAM | Human Review | Red Team | Vendor Review | Monitoring |
|---|---:|---:|---:|---:|---:|---:|
| Prompt injection |  | ✓ | ✓ | ✓ |  | ✓ |
| Data leakage | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Hallucination |  |  | ✓ | ✓ |  | ✓ |
| Excessive agency |  | ✓ | ✓ | ✓ | ✓ | ✓ |
| Insecure tools |  | ✓ |  | ✓ | ✓ | ✓ |
| Model manipulation |  | ✓ | ✓ | ✓ | ✓ | ✓ |
| IP leakage | ✓ | ✓ | ✓ |  | ✓ | ✓ |
| Shadow AI | ✓ |  | ✓ |  |  | ✓ |
| Human oversight |  |  | ✓ |  |  | ✓ |
| Third-party dependency |  |  |  |  | ✓ | ✓ |
| Prompt leakage |  | ✓ |  | ✓ |  | ✓ |
| RAG weakness | ✓ | ✓ | ✓ | ✓ |  | ✓ |

---

# 10. Control Testing Programme

## CT-001 Prompt Injection Test

**Objective:** determine whether malicious direct or indirect instructions can bypass policy or cause unauthorised actions.

**Procedure:** establish baseline; submit direct injection; inject instructions through retrieved documents; attempt tool manipulation; attempt data extraction; record outputs; review logs; raise findings.

**Pass criteria:** no restricted-data disclosure; no unauthorised tool execution; security boundary maintained; events logged.

## CT-002 Data Leakage Test

1. Prepare classified test data.
2. Submit PII, financial data, credentials and confidential documents.
3. Verify DLP detection.
4. Verify blocking or policy response.
5. Test generated output for disclosure.
6. Review logs and escalation.

**Pass:** restricted data is blocked or handled according to policy and no unauthorised disclosure occurs.

## CT-003 Excessive Agency Test

1. Enumerate all agent tools.
2. Map each tool to business purpose.
3. Review permissions.
4. Attempt unapproved action.
5. Attempt action after manipulated output.
6. Verify human approval.
7. Test kill switch.

**Pass:** least privilege is enforced; high-impact actions require approval; unapproved actions fail; kill switch works.

## CT-004 Hallucination Evaluation

Use a representative evaluation set; compare outputs against authoritative sources; measure factual errors, unsupported claims, citation accuracy and escalation behaviour.

## CT-005 RAG Access-Control Test

Create users with different entitlements; query restricted information; test cross-department retrieval; test deleted documents; inspect vector-store and application logs.

**Pass:** the AI application never returns information the user is not authorised to access.

## CT-006 Shadow-AI Discovery

Compare approved AI inventory with enterprise AI-service discovery; identify unknown services; identify possible sensitive-data exposure; route discovered services through formal intake.

---

# 11. Human Oversight Model

| Level | Use |
|---|---|
| Human-in-the-loop | Human must approve output/action |
| Human-on-the-loop | Human monitors and can intervene |
| Human-out-of-the-loop | Only for low-risk, bounded automation |

### Mandatory human approval

- Credit decisions
- Employment decisions
- Customer financial recommendations
- Regulatory submissions
- Legal conclusions
- Material security actions
- Account-changing transactions
- Destructive system actions

---

# 12. AI Incident Response

### Trigger examples

- Successful prompt injection
- Confidential information disclosure
- Unauthorised agent action
- Malicious RAG document
- AI vendor incident
- Material model-behaviour change
- Customer harm
- Shadow-AI data exposure

```text
Detection → Triage → Containment → Investigation → Impact Assessment
        → Remediation → Escalation/Notification → Lessons Learned
```

Containment actions can include disabling an agent/tool, revoking access, blocking a data flow or suspending an AI integration.

---

# 13. Third-Party AI Vendor Assessment

Evaluate:

### Security
Encryption, IAM, monitoring, incident response, vulnerability management.

### AI governance
AI risk management, testing, transparency, model governance and change management.

### Data
Retention, training use, location, subprocessors, deletion and tenant isolation.

### Resilience
Availability, BCP/DR, concentration risk and exit strategy.

### Contract
Security obligations, incident notification, audit rights, DPA, confidentiality, subprocessors and service-change notification.

---

# 14. GenAI Security Architecture Principles

1. Do not treat model output as inherently trusted.
2. Do not allow the LLM to define its own security boundary.
3. Enforce permissions outside the model.
4. Apply least privilege to tools and agents.
5. Separate confidential data from general prompts.
6. Validate inputs and outputs.
7. Log material AI interactions.
8. Require human approval for high-impact actions.
9. Test adversarially before production.
10. Reassess after material changes.
11. Maintain vendor exit strategies.
12. Maintain auditable evidence for material controls.

---

# 15. Framework Mapping

## NIST AI RMF

NIST's AI RMF uses **Govern, Map, Measure and Manage**. The NIST Generative AI Profile is a companion resource specifically intended to help organisations identify and manage unique generative-AI risks across the AI lifecycle. citeturn0search4turn0search9

| Project Activity | AI RMF |
|---|---|
| AI policy | Govern |
| Risk identification | Map |
| Risk measurement | Measure |
| Risk treatment | Manage |
| Human oversight | Govern / Manage |
| Red-team testing | Measure |
| Monitoring | Measure / Manage |
| Incident response | Manage |

## NIST CSF 2.0

| GenAI Activity | CSF Function |
|---|---|
| Governance | Govern |
| AI inventory | Identify |
| Vendor assessment | Govern / Identify |
| IAM/DLP | Protect |
| Monitoring | Detect |
| Incident response | Respond |
| Lessons learned | Recover |

## ISO/IEC 27001

ISO/IEC 27001 specifies requirements for an information security management system and supports risk-based management of information-security risks. citeturn0search2

Relevant areas include access control, information classification, supplier security, logging, incident management, secure development and continuity.

## ISO/IEC 42001

ISO/IEC 42001 specifies requirements for establishing, implementing, maintaining and continually improving an AI Management System (AIMS). citeturn0search0

This project demonstrates AIMS-style operationalisation through AI policy, inventory, risk assessment, impact assessment, control ownership, monitoring, incident management, exceptions and continual improvement.

---

# 16. Control Traceability Matrix

| Control | Risk | NIST AI RMF | NIST CSF | ISO 27001 | ISO 42001 |
|---|---|---|---|---|---|
| DLP | Data leakage | Govern/Manage | Protect | Information protection | AI data/risk governance |
| Prompt testing | Prompt injection | Measure/Manage | Protect/Detect | Security testing | AI risk evaluation |
| Tool allowlist | Excessive agency | Govern/Manage | Protect | Access control | AI lifecycle governance |
| Human review | Hallucination | Govern/Manage | Govern/Protect | Operational controls | Human oversight |
| Vendor assessment | Dependency | Govern/Map | Govern/Identify | Supplier security | Third-party AI governance |
| AI inventory | Shadow AI | Govern/Map | Identify | Asset management | AI system governance |
| RAG ACL | Retrieval leakage | Map/Manage | Protect | Access control | AI data governance |
| Monitoring | Multiple risks | Measure/Manage | Detect | Logging/monitoring | AI monitoring |

---

# 17. Findings

## F-001 — GenAI inventory incomplete
**Severity:** High  
**Recommendation:** Establish central AI inventory and mandatory intake.

## F-002 — Agent permissions excessive
**Severity:** Critical  
**Recommendation:** Apply least privilege, function-level access and approval gates.

## F-003 — Prompt-injection testing inconsistent
**Severity:** High  
**Recommendation:** Establish quarterly GenAI security testing.

## F-004 — DLP coverage incomplete
**Severity:** Critical  
**Recommendation:** Extend DLP to approved GenAI applications and high-risk integrations.

## F-005 — Human oversight inconsistent
**Severity:** High  
**Recommendation:** Introduce formal human-oversight requirements and evidence.

---

# 18. Remediation Roadmap

| Finding | Action | Owner | Priority | Target |
|---|---|---|---|---|
| F-001 | Complete AI inventory | GRC | High | 30 days |
| F-002 | Reduce agent permissions | IAM/Security | Critical | 14 days |
| F-003 | Launch prompt testing | AI Security | High | 30 days |
| F-004 | Expand DLP | Security | Critical | 30 days |
| F-005 | Implement oversight matrix | AI Governance | High | 45 days |

### Priority 1
DLP, approved AI inventory, least privilege, prompt testing and incident response.

### Priority 2 — 30–90 days
Vendor assessments, RAG testing, hallucination evaluation, shadow-AI discovery and red-team programme.

### Priority 3 — 90–180 days
GRC integration, automated evidence collection, continuous evaluation and enterprise AI maturity assessment.

---

# 19. Executive Dashboard

| KPI | Target |
|---|---:|
| Approved GenAI systems inventoried | ≥95% |
| GenAI systems with named owner | 100% |
| High-risk GenAI systems assessed | 100% |
| High-risk agents with approval gates | 100% |
| AI vendors assessed | ≥95% |
| Prompt-injection tests completed | ≥95% |
| Critical findings overdue | 0 |
| AI incidents investigated within SLA | ≥95% |
| Employees completing AI training | ≥95% |
| Shadow-AI findings remediated | ≥90% |
| Controls with current evidence | ≥95% |

---

# 20. Maturity Model

| Level | State |
|---|---|
| 1 — Initial | Uncontrolled employee GenAI use |
| 2 — Developing | Policies and approved tools exist |
| 3 — Defined | Inventory, risk assessments and controls established |
| 4 — Managed | Continuous monitoring and testing operate |
| 5 — Optimised | Automated assurance and continuous evaluation |

**NFS current state:** Level 2 — Developing  
**Target:** Level 4 — Managed

---

# 21. Evidence Index

| Evidence ID | Evidence | Control |
|---|---|---|
| EV-001 | AI system inventory | GOV-03 |
| EV-002 | AI acceptable-use policy | GOV-01 |
| EV-003 | DLP configuration/logs | SEC-01 |
| EV-004 | IAM review | SEC-02 |
| EV-005 | Tool/plugin register | SEC-03 |
| EV-006 | Prompt-injection test | SEC-04 |
| EV-007 | Red-team report | SEC-05 |
| EV-008 | AI monitoring records | SEC-06 |
| EV-009 | Vendor risk assessment | GOV-03 |
| EV-010 | Human-review records | GOV-05 |
| EV-011 | RAG access test | DAT-05 |
| EV-012 | AI incident exercise | GOV-06 |
| EV-013 | Training records | GOV-01 |
| EV-014 | Exception register | GOV-04 |
| EV-015 | Data-provenance record | DAT-04 |

---

# 22. GitHub Deliverables

```text
project-3-generative-ai-security-risk-governance/
├── README.md
├── SOURCES.md
├── docs/
│   ├── Project_3_Generative_AI_Security_Risk_Governance_Assessment.md
│   ├── framework-mapping.md
│   └── architecture.md
├── registers/
│   ├── genai-risk-register.csv
│   ├── genai-control-register.csv
│   └── ai-tool-inventory.csv
├── templates/
│   ├── GenAI_Risk_Assessment.md
│   ├── Vendor_Assessment.md
│   ├── Prompt_Test_Report.md
│   ├── Human_Oversight_Assessment.md
│   └── AI_Incident_Report.md
├── tests/
│   ├── Prompt_Injection_Test_Plan.md
│   ├── Data_Leakage_Test_Plan.md
│   ├── Excessive_Agency_Test_Plan.md
│   └── RAG_Access_Control_Test_Plan.md
└── evidence/
    └── Evidence_Requirements.md
```

---

# 23. Definition of Done

- [x] Fictional enterprise defined
- [x] GenAI architecture defined
- [x] Twelve material risks identified
- [x] Inherent and residual risk scored
- [x] Controls mapped
- [x] Control owners assigned
- [x] Evidence requirements defined
- [x] Control testing defined
- [x] AI incident response defined
- [x] Human oversight model defined
- [x] Third-party AI risk addressed
- [x] Shadow AI addressed
- [x] NIST AI RMF mapped
- [x] NIST CSF mapped
- [x] ISO/IEC 27001 aligned
- [x] ISO/IEC 42001 aligned
- [x] Findings and remediation roadmap produced
- [x] GitHub structure defined

---

# 24. Professional Capability Demonstrated

This project demonstrates the ability to:

- Assess real-world GenAI risks.
- Translate technical vulnerabilities into enterprise risk.
- Design AI security controls and technical guardrails.
- Define control ownership and auditable evidence.
- Perform residual-risk assessment.
- Design human-oversight mechanisms.
- Assess AI agents and tool permissions.
- Evaluate third-party AI providers.
- Design GenAI security testing and red-team activities.
- Map technical controls to cybersecurity and AI governance frameworks.
- Communicate GenAI risk to executives.

> **Professional positioning:** I do not treat AI governance as policy writing alone. I translate AI risks into implementable controls, technical guardrails, evidence requirements and assurance activities.

---

# 25. Conclusion

NFS can obtain substantial value from generative AI, but uncontrolled adoption creates material security, privacy, operational, regulatory and reputational risks. The appropriate target state is a cross-functional capability connecting **AI Governance + Cybersecurity + Privacy + Data Governance + IAM + Application Security + Vendor Risk + Internal Audit + Business Ownership**.

The central control principle is:

> **Do not rely on the LLM itself to enforce the security boundary. Enforce security through independent controls around identity, data, tools, applications, monitoring and human decision-making.**

This is particularly important for agentic systems because a model error or prompt injection can become an actual enterprise action when the model has tool access.

---

# 26. Sources and Standards

- NIST AI Risk Management Framework and AI RMF Generative AI Profile. NIST states that the Generative AI Profile is a companion resource to AI RMF 1.0 for identifying and managing risks unique to generative AI. citeturn0search4turn0search9
- OWASP Top 10 for LLM Applications 2025 and individual risk guidance for Prompt Injection, Sensitive Information Disclosure and Excessive Agency. citeturn0search1turn0search10turn0search8turn0search7
- ISO/IEC 42001:2023 — AI management systems. citeturn0search0
- ISO/IEC 27001:2022 — Information security management systems. citeturn0search2

**Note:** Northstar Financial Services, its AI systems, risk scores, findings, controls and evidence are fictional portfolio artifacts created for demonstration and are not claims about a real organisation.
