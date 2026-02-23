# Instruction Document for AI App Builder

## 1) Project Title
**Public Finance Management Platform Using Blockchain**  
A permissioned blockchain-based platform for:
1. **Tax Management** (GST/VAT, income tax workflows, invoice validation, refunds), and
2. **Government Expenditure Management** (budget allocation, milestone-based release, DBT, procurement transparency, auditability).

---

## 2) Product Vision
Build a secure, transparent, and automation-first fiscal governance platform where:
- Tax collection is tamper-proof, near real-time, and less prone to evasion/fraud.
- Government spending is traceable end-to-end with programmable controls.
- Auditors and regulators get immutable logs and analytics.
- Citizens get privacy-preserving transparency dashboards.

---

## 3) Core Objectives
1. Reduce tax evasion, fake invoices, and manual reconciliation.
2. Automate tax calculation, settlement, and refund workflows using smart contracts.
3. Track government funds from allocation to final beneficiary/project milestone.
4. Prevent fund diversion and increase public trust through transparent reporting.
5. Enable role-based access and strong cryptographic security.

---

## 4) Scope

### 4.1 In Scope (MVP)
#### A. Tax Management Module
- Invoice registration with hash anchoring on-chain.
- GST/VAT tax computation per transaction.
- Buyer-side invoice validation flow.
- Input tax credit adjustment logic.
- Refund request + rules-based auto-approval (where conditions are met).
- Compliance status dashboard for tax authority.

#### B. Government Expenditure Module
- Annual/periodic budget allocation on-chain.
- Department-wise allocations.
- Milestone-based fund release for projects.
- Contractor payment workflow linked to verified milestones.
- Direct Benefit Transfer (DBT) with beneficiary eligibility checks.
- Public procurement workflow (tender publish, bid commit, reveal/open, compliant winner selection).

#### C. Shared Platform Features
- Permissioned participant onboarding.
- Role-based dashboards.
- Immutable audit trail explorer.
- Alerts/anomaly flags (rule-based in MVP).
- Basic analytics and downloadable reports.

### 4.2 Out of Scope (Phase 2+)
- Full national-scale production deployment.
- Deep AI fraud models (MVP: simple heuristic flags only).
- Complete CBDC rail integration (MVP: mock/simulated interface).
- Cross-country treaty automation.

---

## 5) Actors and Roles
1. **Taxpayer**: submits invoices/returns, views liabilities, refunds, compliance status.
2. **Tax Authority Officer**: monitors compliance, approves exceptions, audits records.
3. **Bank/Treasury Node Operator**: validates payment/fund-transfer transactions.
4. **Department Officer**: requests/reports budget and milestone completion.
5. **Auditor**: validates milestones, reviews immutable logs, creates audit reports.
6. **Regulator**: supervisory analytics and policy-level oversight.
7. **Citizen (Public View)**: sees non-sensitive expenditure transparency dashboards.
8. **System Admin**: onboarding, role assignment, network and policy management.

---

## 6) Functional Requirements

### 6.1 Tax Management
- Create taxpayer profile (Tax ID, organization metadata).
- Upload invoice metadata and compute content hash.
- Store hash + key fields on-chain, sensitive details off-chain.
- Smart contract tax calculation based on configurable rates/rules.
- Buyer confirmation/validation workflow for invoice authenticity.
- Automatic tax credit adjustment and payable amount calculation.
- Tax payment recording and government wallet allocation.
- Refund workflow:
  - taxpayer submits refund claim,
  - contract checks eligibility and risk flags,
  - auto-approve or send to manual review.
- Compliance certificate generation for eligible taxpayers.

### 6.2 Expenditure Management
- Budget creation and department allocation recording.
- Project creation with milestone and percentage release rules.
- Milestone evidence upload (off-chain) + hash anchoring on-chain.
- Auditor milestone verification and decision logging.
- Smart contract-based fund release after milestone approval.
- DBT disbursement:
  - beneficiary registration (hashed identity reference),
  - eligibility validation,
  - direct wallet transfer,
  - duplicate/fraud checks.
- Procurement:
  - tender publish on-chain,
  - cryptographic bid commitment,
  - reveal/open post deadline,
  - compliant bid evaluation,
  - milestone-based contractor payment.

### 6.3 Audit & Transparency
- End-to-end transaction timeline view.
- Search/filter by Tax ID, project ID, department, date range, status.
- Immutable event log with block references and signatures.
- Public dashboard (aggregated, privacy-safe):
  - tax collected by category,
  - expenditure by department/project,
  - DBT distribution totals.

---

## 7) Non-Functional Requirements
1. **Security**: PKI-based identity, digital signatures, encryption at rest/in transit.
2. **Access Control**: strict RBAC + scoped permissions.
3. **Immutability**: on-chain records append-only.
4. **Privacy**: PII and sensitive financial data stored off-chain; only hashes and minimal metadata on-chain.
5. **Performance**: near real-time transaction confirmation for typical loads.
6. **Scalability**: modular services and queue-backed async processing.
7. **Observability**: logs, metrics, traces, audit reports.
8. **Reliability**: fault-tolerant services and retry-safe workflows.
9. **Compliance**: data retention and privacy-law alignment.

---

## 8) Suggested Technical Architecture

### 8.1 Blockchain Layer
- **Type**: Permissioned blockchain (prefer Hyperledger Fabric or equivalent).
- **Nodes**: Tax authority, treasury/banks, select departments, auditors.
- **Smart Contracts/Chaincode**:
  - TaxCalculationContract
  - InvoiceValidationContract
  - RefundContract
  - BudgetAllocationContract
  - MilestoneReleaseContract
  - ProcurementContract
  - DBTEligibilityContract

### 8.2 Application Layer
- Web frontend for each role with role-aware UX.
- Backend API server for orchestration, business logic, and integration.
- Event listener/indexer to consume blockchain events and update query-optimized datastore.
- Notification service (email/SMS/in-app) for approvals, failures, compliance alerts.

### 8.3 Data Layer
- **On-chain**: hashes, IDs, tax/expenditure amounts, timestamps, status, signatures.
- **Off-chain**: full invoices, documents, beneficiary records, milestone evidence.
- **Storage**: relational DB + object/document storage.
- **Hash linking**: every off-chain record anchored by on-chain hash.

### 8.4 Integration Layer
- Payment gateway/bank simulation or API.
- Identity verification service (simulated for MVP).
- Optional GIS or registry integration for property-tax use cases.

---

## 9) Data Model (High-Level Entities)
- User
- Organization
- Role
- Taxpayer
- Invoice
- TaxRule
- TaxAssessment
- TaxPayment
- RefundClaim
- Budget
- DepartmentAllocation
- Project
- Milestone
- FundRelease
- Beneficiary
- DBTDisbursement
- Tender
- BidCommitment
- ProcurementAward
- AuditLog
- BlockchainTransactionReference

Include standard fields: `id`, `createdAt`, `updatedAt`, `createdBy`, `status`, and where relevant `onChainTxId`, `blockNumber`, `documentHash`.

---

## 10) Key Workflows to Implement

### Workflow A: GST/VAT Invoice-to-Settlement
1. Seller submits invoice.
2. System hashes invoice and stores hash on-chain.
3. Smart contract computes tax.
4. Buyer validates invoice.
5. Tax credit and liability auto-adjust.
6. Payment record written; authority dashboard updates.

### Workflow B: Tax Refund
1. Taxpayer files refund claim.
2. Contract evaluates eligibility (paid tax, validation status, risk flags).
3. If low-risk and valid → auto-approve and release.
4. Else route to officer review.

### Workflow C: Milestone-Based Public Project Funding
1. Department creates project with milestone percentages.
2. Budget locked/allocated on-chain.
3. Department submits milestone proof.
4. Auditor verifies.
5. Contract releases funds automatically based on approved milestone.

### Workflow D: DBT
1. Beneficiary enrolled with hashed ID.
2. Eligibility checked by contract.
3. Funds transferred directly.
4. Public aggregate dashboard updated.

### Workflow E: Procurement
1. Tender published.
2. Bids committed before deadline.
3. Bids revealed/opened post deadline.
4. Lowest compliant bidder selected.
5. Contract-managed payment linked to milestones.

---

## 11) UI Pages / Screens
1. Login + MFA + role-aware landing.
2. Admin console (users, orgs, node/role policies).
3. Taxpayer dashboard (invoices, liabilities, payments, refunds).
4. Tax authority dashboard (risk flags, compliance map, pending approvals).
5. Budget dashboard (allocation, utilization, pending milestones).
6. Project & milestone management page.
7. DBT management page.
8. Tender/procurement page.
9. Auditor workbench (verification queue + chain evidence).
10. Public transparency dashboard (aggregated data only).
11. Audit explorer with transaction drill-down.

---

## 12) Security Requirements
- PKI-issued identities for all institutional actors.
- Digital signature verification on critical transactions.
- Multi-signature approvals for high-value disbursements/refunds.
- End-to-end encryption for sensitive payloads.
- Secrets management via secure vault.
- Tamper-evident logs and regular integrity checks.

---

## 13) Reporting & Analytics
Provide charts/tables for:
- Tax collected over time by category/region.
- Invoice mismatch and fraud indicators.
- Refund turnaround time.
- Department budget vs utilization.
- Project milestone progress and payment status.
- DBT distribution totals and exception rates.

Export options: CSV and PDF.

---

## 14) Acceptance Criteria (MVP)
1. Invoice hash anchoring and retrieval works reliably.
2. Tax calculation and credit adjustment execute through smart contracts.
3. Refund workflow supports auto and manual branches.
4. Budget allocation and milestone-based auto-release work end-to-end.
5. DBT disbursement prevents duplicates in test scenarios.
6. Procurement commit-reveal flow executes correctly.
7. Audit logs are immutable and queryable by authorized roles.
8. Public dashboard exposes aggregated, non-PII fiscal data.

---

## 15) Recommended Build Plan for AI Bot

### Phase 1: Foundation
- Set up permissioned blockchain network (dev mode).
- Implement identity, RBAC, and base schemas.
- Build blockchain event indexer.

### Phase 2: Tax Module
- Invoice hashing + on-chain registration.
- Tax calculation + buyer validation.
- Payment, credit adjustment, and refund logic.

### Phase 3: Expenditure Module
- Budget and allocation contracts.
- Milestone verification + fund release.
- DBT + procurement flows.

### Phase 4: Dashboards & Audit
- Role dashboards + public dashboard.
- Audit explorer + report exports.

### Phase 5: Hardening
- Security controls, performance tuning, test coverage, documentation.

---

## 16) Testing Requirements
- Unit tests for contract logic and backend services.
- Integration tests for end-to-end workflows A-E.
- Security tests for authZ/authN and signature validation.
- Data integrity tests for hash mismatch detection.
- Performance smoke tests for concurrent transactions.

---

## 17) Deliverables Expected from AI Bot
1. Source code for frontend, backend, smart contracts/chaincode.
2. Dockerized local development setup.
3. Seed data and demo scripts.
4. API documentation (OpenAPI/Postman).
5. Architecture diagram and workflow diagrams.
6. Test suites and execution report.
7. Deployment and operations guide.

---

## 18) Constraints and Design Principles
- Prefer modular architecture and clean separation of concerns.
- Keep sensitive data off-chain; use hash anchoring.
- Design for auditability first.
- Favor deterministic smart contract logic.
- Ensure every critical action is role-checked and logged.

---

## 19) Optional Advanced Features (Post-MVP)
- AI-based fraud/anomaly detection.
- CBDC integration for programmable tax/expenditure rails.
- Property tax registry integration.
- Cross-border customs-tax interoperability.
- Real-time national fiscal command dashboard.

---

## 20) Final Instruction to AI Builder
Build a working MVP that demonstrates **tamper-proof tax workflows** and **transparent government expenditure workflows** on a **permissioned blockchain**, with a strong focus on **security, auditability, automation, and privacy-preserving transparency**.
