meta::
#class_behavior
#status_completed

[[Class_Share]]

Class Name:
Share

Behavioral Purpose:
A **Share** acts as a **financial and governance unit of ownership within a business entity**, granting stakeholders rights to profits, influence in decision-making, and exposure to business risks or returns. It is fundamental to **corporate finance**, **capital structuring**, and **investment governance**.

Core Behaviors:
- **Represent Ownership Stake** – Captures fractional ownership in a business entity.
- **Enable Capital Formation** – Serves as a vehicle to raise equity financing.
- **Distribute Financial Rights** – Grants dividend eligibility and residual claim on earnings.
- **Grant or Deny Governance Rights** – Allocates voting rights and influence in corporate governance.
- **Structure Control & Incentives** – Differentiates classes of ownership and incentives via share types (e.g., restricted, preferred).
- **Interface with Markets** – Connects ownership structures with private or public equity markets.

Interaction Targets:
- **Business Entity** – Shares are issued by and tied to corporate structures.
- **Stakeholder** – Individuals or institutions that hold ownership and associated rights.
- **Agreement** – Defines vesting, transfer rights, restrictions, and special clauses.
- **Financial Market** – Marketplace for trade and valuation (public or private).
- **Capital Structure** – Share types directly contribute to how equity capital is modeled.

Sensing Triggers:
- **Equity Financing Events** – Issuance of new shares in fundraising rounds or IPOs.
- **Ownership Transfers** – Sale, gifting, or vesting of shares triggers updates in ownership.
- **Governance Actions** – Voting or board decisions based on share class influence.
- **Dividend or Earnings Announcements** – Triggers payout rights and distribution policies.
- **Corporate Events** – Mergers, acquisitions, buybacks, or recapitalization schemes.

Decision Rules:
- `shareType = Preferred` → requires fixed dividend definition.
- `shareVotingPower = true` → implies governance influence (unless overridden by agreement).
- `shareQuantity > 10% of total` → may trigger major influence or regulatory reporting.
- `shareClass = Restricted` → must link to a vesting schedule or lockup period.

Adaptation Behavior:
- **Convertible Share Activation** – Changes share type and rights upon trigger conditions.
- **Split or Consolidation** – Adjusts share quantity and value to match strategic financial goals.
- **Voting Right Adjustment** – Based on corporate restructuring or agreement amendments.
- **Equity Dilution Monitoring** – Impacts valuation and stakeholder equity positions.

Lifecycle Phases:
- **Authorization** – Board or shareholder approval for new issuance.
- **Issuance** – Shares created and allocated.
- **Trading/Holding** – Shares actively traded or retained.
- **Governance Participation** – Shareholder voting and engagement.
- **Exit / Redemption / Conversion** – Shares retired, sold, or converted into another class.

Special Traits:
- **Rights-Bearing Unit** – Grants financial and governance privileges.
- **Contractual Anchoring** – Heavily influenced by legal and shareholder agreements.
- **Valuation-Driven** – Subject to external and internal valuation mechanisms.
- **Sensitive to Control Structure** – Used in controlling interests and corporate takeovers.

Fine-Tuning Parameters:
- **Governance Modeling Utility:** 9.7/10 – Critical in board structure and voting rights modeling.
- **Financial Structuring Relevance:** 9.6/10 – Core to capital architecture and ownership reporting.
- **Stakeholder Modeling Fit:** 9.3/10 – Links stakeholder identity to financial power.
- **Regulatory Alignment Importance:** 9.5/10 – Required for transparency and compliance in most jurisdictions.

