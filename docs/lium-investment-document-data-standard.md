# LIUM Investment Document Data Standard

Generated: 2026-05-11

This artifact defines a shared data standard for investment documents moving across a Finternet + Beckn application. It expands the actor payload map with the detailed BESS investment data found in the downloaded diligence files.

The goal is to make every project, contract, financial model, operating metric, risk signal, and investment document shareable across the platform with a stable index. The same data point should have the same identifier whether it appears in a Project Developer upload, a Financier underwriting screen, a Securitization Agent pool, a Portfolio Manager diligence memo, or an Investor holding view.

## Platform Split

| Platform | Responsibility |
| --- | --- |
| Finternet | Financial asset definition, trust, risk, valuation, ownership, composability, transaction rules, waterfall logic, settlement state, data provenance, and asset-level financial logic. |
| Beckn | Network discovery, participant registry, catalog exposure, protocol interoperability, transaction flow, document exchange, API/webhook routing, and application-level actor interaction. |

## Shared Data Index

Every material data point should carry an index record. This lets actors share the same data across the platform without losing source, ownership, verification state, or routing context.

```json
{
  "data_index": {
    "index_id": "idx_asset_bess_banaskantha_capacity_mw_v1",
    "canonical_field": "asset.technical.contracted_capacity_mw",
    "display_name": "Contracted Capacity MW",
    "value": 65,
    "unit": "MW",
    "asset_id": "asset_bess_banaskantha_65mw",
    "project_id": "project_banaskantha_bess",
    "source_document_id": "doc_dpr_revised",
    "source_document_type": "detailed_project_report",
    "source_location": {
      "section": "Project Identity Data",
      "table": "Project Identity Data"
    },
    "owner_actor": "project_developer",
    "verified_by": ["risk_provider"],
    "consumed_by": ["financier", "securitization_agent", "portfolio_manager", "investor"],
    "platform_owner": "finternet",
    "beckn_route": {
      "domain": "lium.energy.asset",
      "catalog_item_id": "catalog_bess_banaskantha",
      "endpoint": "/asset/details"
    },
    "confidence": "high",
    "status": "active",
    "version": "1.0.0",
    "last_updated_at": "2026-05-11T00:00:00Z"
  }
}
```

### Index Rules

| Rule | Description |
| --- | --- |
| Stable canonical field | A data point should have one canonical path, such as `asset.technical.round_trip_efficiency_min_pct`. |
| Source traceability | Every material number should point to a document, model, contract, telemetry feed, or attestation. |
| Actor ownership | Each field should have an owning actor responsible for updates. |
| Platform ownership | Finternet or Beckn should be identified as the system of record for the field. |
| Versioning | Updates should create a new version instead of silently overwriting diligence data. |
| Verification state | Fields should be marked `raw`, `reviewed`, `verified`, `disputed`, `superseded`, or `archived`. |
| Consumer visibility | The index should list which actor profiles consume the field. |

## Canonical Data Domains

The data inventory shows that LIUM needs more than high-level project metrics. These domains should become reusable schemas across actor profiles.

| Domain | Example Fields | Primary Platform |
| --- | --- | --- |
| Project identity | project type, capacity, developer, implementation model, location, coordinates, grid connection, COD target | Beckn for discovery, Finternet for asset identity |
| Policy and scheme | VGF scheme, implementing agency, tranche, classification, regulatory references | Finternet |
| Technical capacity | contracted capacity, installed capacity, containers, oversizing, cycles/day, throughput, DoD, RtE, availability | Finternet |
| Degradation schedule | yearly minimum dispatchable capacity, augmentation trigger, warranty reference | Finternet |
| Electrical architecture | battery chemistry, PCS, transformers, switchgear, SCADA, EMS, metering, communication | Beckn for documents, Finternet for risk/asset model |
| BOQ / BOM | battery containers, PCS, transformers, GIS bays, relays, fire systems, civil works, cables | Beckn for document exchange, Finternet for valuation/collateral |
| Licenses and approvals | CTU approval, SLDC approval, CEIG/CEA, fire, land, environmental, commissioning, metering | Finternet |
| Commissioning and COD | COD target, commissioning mode, test records, authorities, acceptance evidence | Finternet |
| O&M | LTSA, 24x7 monitoring, preventive/predictive/corrective maintenance, spares, HSE | Finternet |
| BESPA performance | availability, RtE, LD formulas, incentives, grid non-availability, force majeure relief | Finternet |
| Financial model | capex, OPEX, debt/equity, interest, tax, depreciation, DSRA, revenue, IRR, DSCR | Finternet |
| VGF | amount per MWh, milestone schedule, repayment split, disbursement state | Finternet |
| Billing and payment | monthly bill, calculation support, LC, late surcharge, rebate, disputes, reconciliation | Finternet |
| Cash flow | revenue, GST, OPEX, debt service, tax, FCFF, equity cash flow, DSCR | Finternet |
| Security package | financial closure, loan/equity proof, PBG, PBG release, LD exposure | Finternet |
| Insurance | insurance rate, policy status, proceeds, total loss treatment | Finternet |
| Force majeure | definition, exclusions, notice, mitigation, schedule relief, long-FM termination | Finternet |
| Change in law | pre/post COD relief, notice period, certificate, supplementary bill | Finternet |
| Default and termination | default events, cure period, takeover, compensation, adjusted equity | Finternet |
| Open diligence flags | mismatched container count, capacity mismatch, battery chemistry unknown, signed agreement missing | Finternet |

## Actor Payload Matrix

### Project Developer

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, incorporation details, beneficial ownership, tax IDs, bank account, authorized signers, DID/wallet, licenses, credit profile, prior defaults, trust credentials | Beckn subscriber ID, participant registry entry, domain role `project_originator`, subscriber URL, callback URLs, public key, protocol permissions | Financial asset ID, asset definition, ownership model, collateral definition, composability rules, transaction rules, financing eligibility, post-COD securitization eligibility | Catalog provider ID, catalog item ID, search tags, discovery filters, visibility rules, transaction endpoint, status callback |
| App | Project experience, technology track record, EPC/O&M partners, financial closure history, insurance history, compliance attestations | Email, organization name, service geographies, supported asset categories, document upload API, project details webhook/API | Asset type, asset location, project construct, estimated cost, monthly cost, BOM/BOQ, IRR, valuation, average cost of interest, debt/equity, risk profile, yearly cash flow, timeline, revenue assumptions | Project title, description, location display, capacity MW/MWh, current stage, target COD, document manifest, map/images, offer intake endpoint |

### Financier

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, lending license, capital source, bank rails, DID/wallet, authorized signers, risk limits, compliance profile | Beckn subscriber ID, domain role `financier`, offer API, callback URLs, service geographies, protocol permissions | Loan asset ID, security interest, repayment waterfall, disbursement rules, exit/securitization preference, transfer restrictions | Loan offer catalog item, offer submission endpoint, loan status callback, negotiation state, quote expiry |
| App | AUM/lending book, credit policy, sector exposure limits, underwriting methodology, collections capability | Organization name, contact email, supported instruments, document requirements, SLA for offer response | Loan amount, coupon, term, moratorium, DSCR covenant, collateral coverage, LTV, average cost of funds, target yield, project exposure, repayment cash flow | Indicative terms, eligible project types, processing timeline, required documents, offer status, acceptance endpoint |

### Securitization Agent

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, arranger/trustee licenses, DID/wallet, signer authority, trust/bank accounts, rating agency relationships, issuance history | Beckn subscriber ID, domain role `securitization_agent`, structuring API, issuance callbacks, protocol permissions | Pool ID, tranche IDs, ownership registry, transfer rules, credit enhancement, waterfall logic, rating target, token metadata | Securitization service item, pool discovery endpoint, structuring request endpoint, issuance status callback |
| App | Structuring methodology, waterfall templates, credit enhancement policy, tokenization capability, servicer relationships | Accepted asset categories, service geographies, required documents, structuring timeline, contact endpoint | Asset pool composition, tranche sizing, expected rating, target yield, expected loss, DSCR history, cash-flow waterfall, eligibility tests, stress scenarios | Issuance summary, accepted loans/assets, document checklist, investor presentation URL, book-building status |

### Portfolio Manager

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, fund identity, investor classification, custody account, DID/wallet, signer authority, risk limits | Beckn subscriber ID, domain role `portfolio_manager`, buyer permissions, subscription APIs, callback URLs | Allocation account, ownership record, transfer eligibility, valuation model, risk attribution, settlement account | Investment order endpoint, bid/subscribe flow, allocation status callback, portfolio reporting callback |
| App | Investment mandate, AUM, target sectors, concentration limits, rating constraints, duration limits, liquidity preference | Discovery preferences, watchlist API, reporting endpoint, document access preferences, notification preferences | Target yield, expected IRR by tranche, rating, tranche seniority, duration, expected loss, DSCR, offtaker risk, telemetry reliability, portfolio concentration, mark-to-model valuation | Investment card, diligence memo, document room, open diligence flags, subscribe amount, holding dashboard, monitoring webhook |

### Investor

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | KYC/KYB, investor classification, tax profile, bank account, DID/wallet, suitability profile, jurisdiction, investment limits | Beckn subscriber ID, domain role `investor`, consent permissions, callback URL, app identity | Ownership record, token holding, transfer restrictions, payout ledger, tax treatment, redemption rules | Subscription endpoint, holding update webhook, redemption request endpoint, notification callback |
| App | Risk tolerance, income needs, ticket size range, accreditation documents, payout instructions | Notification preferences, document access consent, order API access, dashboard preferences | Subscribed amount, risk band, expected return, payment frequency, lockup, tranche type, accrued payout, realized return | Investment listing, minimum ticket, risk label, disclosure document, holding dashboard, payout schedule |

### Risk Provider

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, certification/license, DID/wallet, data attestation rights, model governance profile, signer authority | Beckn subscriber ID, domain role `risk_provider`, risk API endpoint, callback URLs, supported domains | Risk signal ID, attestation signature, score provenance, confidence interval, risk indicator registry | Risk signal catalog, score API, alert callback, report publication endpoint |
| App | Risk methodology, validation history, model version, conflict policy, audit logs | Coverage areas, update frequency, report formats, alert webhook | Risk score, telemetry uptime, data latency, performance variance, anomaly count, default probability, loss-given-default, contract risk, counterparty risk | Project coverage, latest score, score trend, alert list, report URL, monitoring subscription |

### Utility / Procurer

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, regulatory license, payment credibility, procurement authority, DID/wallet, signer authority | Beckn subscriber ID, domain role `utility_procurer`, dispatch APIs, demand APIs, callback URLs | Offtake contract ID, payment obligation, settlement logic, default clauses, tariff rules | Flexibility need catalog, dispatch request endpoint, metering callback, settlement endpoint |
| App | Grid jurisdiction, payment history, procurement rules, tariff approval references | Service territory, grid nodes, metering requirements, procurement contact | Contracted capacity MW/MWh, tariff, availability calculation, RtE calculation, LD exposure, billing cycle, payment status, dispatch obligation | Capacity request, location/grid node, dispatch schedule, delivery confirmation, revenue pool, settlement status |

### EdgeGrid / Grid Services Platform

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, platform certification, DID/wallet, telemetry permissions, settlement account | Beckn subscriber ID, domain role `edgegrid_platform`, routing APIs, dispatch APIs, telemetry callbacks | Aggregation pool ID, capacity verification, delivery attestation, revenue allocation rules, penalty allocation | Available capacity catalog, dispatch response endpoint, telemetry stream endpoint, revenue allocation endpoint |
| App | Grid integration credentials, aggregation methodology, settlement allocation model, cybersecurity profile | Supported grid nodes, asset onboarding API, event notification API, capacity discovery API | Available capacity, committed capacity, dispatch success rate, response time, revenue earned, asset contribution share | Capacity listing, active dispatches, telemetry feed, delivery confirmation, revenue allocation report |

### Project / Asset Operator

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, O&M authority, insurance, safety certifications, DID/wallet, signer authority | Beckn subscriber ID, domain role `asset_operator`, telemetry APIs, maintenance APIs, callback URLs | Performance attestation, telemetry trust score, availability proof, maintenance proof | Operational status endpoint, telemetry callback, maintenance update callback, incident alert webhook |
| App | O&M contract, LTSA terms, operator experience, maintenance capability, incident history | Asset IDs operated, SCADA endpoint, maintenance notification endpoint, operator contact | Actual availability, actual RtE, cycles/day, throughput MWh, outage events, maintenance schedule, warranty status, failure history | Live operational status, availability dashboard, outage schedule, telemetry endpoint, maintenance updates |

### Data Center / Offtaker

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, credit profile, offtake authority, DID/wallet, signer authority, payment account | Beckn subscriber ID, domain role `offtaker`, demand APIs, callback URLs, consent permissions | Offtake agreement ID, payment obligation, SLA rules, curtailment rules, settlement record | Power request catalog, contract status callback, delivery confirmation endpoint, settlement callback |
| App | Demand profile, SLA requirements, payment history, sustainability reporting needs | Facility locations, load profile API, delivery confirmation endpoint, contract API | Contracted load, demand forecast, price/tariff, delivery SLA, payment status, carbon/clean energy attribution | Power request, active contracts, demand schedule, delivery confirmation, invoice status |

### Telemetry Provider

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, device/data certification, data signing authority, DID/wallet, cybersecurity profile | Beckn subscriber ID, domain role `telemetry_provider`, stream APIs, alert callbacks, protocol permissions | Telemetry proof ID, data provenance, measurement signature, anomaly attestation, audit log | Telemetry feed catalog, stream endpoint, alert webhook, historical data endpoint |
| App | Device registry, calibration records, data schema, audit history, measurement confidence methodology | Supported asset types, frequency, API documentation, data availability SLA | State of charge, charge/discharge MW, throughput MWh, RtE, availability, temperature, fault codes, data latency, missing data count | Live feed URL, frequency, supported metrics, alert types, feed status |

### Rating Agency / Valuation Agent

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Legal entity ID, rating/valuation license, DID/wallet, signer authority, conflict policy | Beckn subscriber ID, domain role `rating_valuation_agent`, rating API, callback URLs | Rating ID, valuation ID, attestation signature, surveillance state, methodology reference | Rating service catalog, rating request endpoint, report publication webhook, surveillance callback |
| App | Rating methodology, valuation model, stress scenario library, surveillance policy, historical accuracy | Eligible instrument types, report formats, request intake API, publication endpoint | Tranche rating, pool rating, probability of default, loss-given-default, stress case DSCR, fair value, discount rate, rating watch flag | Published rating, valuation report, surveillance updates, required inputs, turnaround time |

### Custodian / Token Registry

| Layer | Onboarding Payload - Finternet | Onboarding Payload - Beckn | Catalog Payload - Finternet | Catalog Payload - Beckn |
| --- | --- | --- | --- | --- |
| Network | Custody license, legal entity ID, wallet infrastructure, DID/wallet, compliance controls, signer authority | Beckn subscriber ID, domain role `custody_registry`, transfer APIs, ownership proof APIs, callback URLs | Ownership ledger, token metadata, transfer restriction rules, lien/pledge status, settlement state | Ownership proof endpoint, transfer request endpoint, redemption request endpoint, event webhook |
| App | Beneficial owner policy, transfer screening rules, pledge/lien handling, settlement rails | Supported token standards, holding view API, redemption API, compliance API | Beneficial owner, token quantity, instrument ID, holding value, accrued payouts, transfer history, redemption eligibility | Holding dashboard, transfer request, redemption request, ownership certificate, event notifications |

## Investment Document Schemas

### Document Manifest

```json
{
  "document_manifest": {
    "document_id": "doc_nvvn_financial_model",
    "document_type": "financial_model",
    "document_name": "Financial model NVVN.pdf",
    "asset_id": "asset_bess_banaskantha_65mw",
    "project_id": "project_banaskantha_bess",
    "uploaded_by_actor": "project_developer",
    "document_status": "reviewed",
    "source_system": "lium_document_room",
    "sha256": null,
    "version": "1.0.0",
    "created_at": "2026-05-11T00:00:00Z"
  }
}
```

### Asset Technical Profile

```json
{
  "asset_technical_profile": {
    "asset_id": "asset_bess_banaskantha_65mw",
    "project_type": "standalone_bess",
    "contracted_capacity_mw": 65,
    "contracted_energy_mwh": 130,
    "duration_hours": 2,
    "installed_battery_capacity_mwh": 153,
    "oversizing_pct": 15,
    "cycles_per_day": 2,
    "daily_throughput_mwh": 260,
    "depth_of_discharge_pct": 98,
    "minimum_round_trip_efficiency_pct": 85,
    "minimum_annual_availability_pct": 95,
    "planned_maintenance_outage_hours_per_year": 204,
    "response_time_ms": 500,
    "end_of_life_dispatchable_capacity_pct": 70
  }
}
```

### Financial Model Snapshot

```json
{
  "financial_model_snapshot": {
    "asset_id": "asset_bess_banaskantha_65mw",
    "currency": "INR",
    "project_capacity_mw": 65,
    "energy_capacity_mwh": 130,
    "total_project_cost": 1814900000,
    "annual_revenue_excluding_gst": 185240000,
    "debt_equity": "75:25",
    "term_loan_years": 9,
    "moratorium_years": 1,
    "term_loan_interest_pct": 8.5,
    "corporate_tax_pct": 25.17,
    "tariff_inr_per_mw_month": 237490,
    "project_irr_post_tax_pct": 10.05,
    "equity_irr_post_tax_pct": 16.78,
    "project_irr_pre_tax_pct": 10.58,
    "equity_irr_pre_tax_pct": 17.94,
    "dscr_range": "0.88_to_1.88"
  }
}
```

### Cash Flow Timeseries

```json
{
  "cash_flow_timeseries": {
    "asset_id": "asset_bess_banaskantha_65mw",
    "currency": "INR",
    "frequency": "yearly",
    "periods": [
      {
        "year": 2027,
        "revenue_excluding_gst": 185240000,
        "opex": 19550000,
        "ebdit": 165690000,
        "debt_service": null,
        "tax": null,
        "fcff": 515030000,
        "pat": -40580000,
        "ending_cash_balance": 330840000,
        "dscr": null
      }
    ]
  }
}
```

### Degradation Schedule

```json
{
  "battery_degradation_schedule": [
    { "year": 1, "minimum_dispatchable_capacity_pct": 97.5 },
    { "year": 2, "minimum_dispatchable_capacity_pct": 95.0 },
    { "year": 3, "minimum_dispatchable_capacity_pct": 92.5 },
    { "year": 4, "minimum_dispatchable_capacity_pct": 90.0 },
    { "year": 5, "minimum_dispatchable_capacity_pct": 87.5 },
    { "year": 6, "minimum_dispatchable_capacity_pct": 85.0 },
    { "year": 7, "minimum_dispatchable_capacity_pct": 82.5 },
    { "year": 8, "minimum_dispatchable_capacity_pct": 80.0 },
    { "year": 9, "minimum_dispatchable_capacity_pct": 78.5 },
    { "year": 10, "minimum_dispatchable_capacity_pct": 75.0 },
    { "year": 11, "minimum_dispatchable_capacity_pct": 72.5 },
    { "year": 12, "minimum_dispatchable_capacity_pct": 70.0 }
  ]
}
```

### Performance Rules

```json
{
  "bespa_performance_rules": {
    "max_daily_cycles": 2,
    "minimum_annual_availability_pct": 95,
    "monthly_availability_cap_pct": 95,
    "availability_ld_formula": "(A - B) * C * D * n * 2",
    "minimum_monthly_rte_pct": 85,
    "rte_underperformance": {
      "rte_70_to_85_pct": "LD at APPC charge for excess conversion losses",
      "rte_below_70_pct": "LD at APPC plus no tariff payment for month",
      "rte_above_85_pct": "INR 0.50/unit incentive for excess discharge energy"
    }
  }
}
```

### Billing And Payment

```json
{
  "billing_payment_profile": {
    "tariff_basis": "INR/MW/month",
    "tariff_inr_per_mw_month": 237490,
    "monthly_bill_required": true,
    "bill_supporting_data": ["system_availability", "round_trip_efficiency", "time_block_data"],
    "payment_security": "revolving_irrevocable_letter_of_credit",
    "lc_amount": "100_percent_estimated_or_average_monthly_billing",
    "late_payment_surcharge": "SBI_1Y_MCLR_plus_5_percent_base",
    "early_payment_rebate": "SBI_1Y_MCLR_plus_7_percent_per_annum",
    "disputed_bill_payment_pct": 50,
    "quarterly_reconciliation": true,
    "annual_reconciliation": true
  }
}
```

### Diligence Flags

```json
{
  "diligence_flags": [
    {
      "flag_id": "flag_container_count_mismatch",
      "severity": "high",
      "issue": "DPR BOQ has 30 containers, NVVN financial model has 31 containers",
      "why_it_matters": "Affects installed MWh, capex, layout, warranties, and performance margins",
      "owner_actor": "project_developer",
      "status": "open"
    },
    {
      "flag_id": "flag_signed_agreement_missing",
      "severity": "high",
      "issue": "Exact signed agreement not included",
      "why_it_matters": "Final tariff, VGF, COD, security, LD, and termination terms depend on signed agreement",
      "owner_actor": "project_developer",
      "status": "open"
    },
    {
      "flag_id": "flag_dscr_below_one",
      "severity": "medium",
      "issue": "DSCR below 1.0 in some years",
      "why_it_matters": "Financing model should be reviewed with lender assumptions",
      "owner_actor": "financier",
      "status": "open"
    }
  ]
}
```

## End-To-End Publish Request

```json
{
  "request_id": "req_publish_investment_document_001",
  "network": "lium-finternet-beckn",
  "actor": "project_developer",
  "action": "publish_investment_document_package",
  "payload_type": "catalog",
  "timestamp": "2026-05-11T00:00:00Z",
  "finternet_payload": {
    "asset_id": "asset_bess_banaskantha_65mw",
    "financial_asset_type": "project_finance_asset",
    "technical_profile_ref": "idx_asset_bess_banaskantha_technical_profile_v1",
    "financial_model_ref": "idx_asset_bess_banaskantha_nvvn_model_v1",
    "cash_flow_ref": "idx_asset_bess_banaskantha_cash_flow_v1",
    "risk_profile_ref": "idx_asset_bess_banaskantha_risk_profile_v1",
    "diligence_flags_ref": "idx_asset_bess_banaskantha_flags_v1"
  },
  "beckn_payload": {
    "catalog_item": {
      "catalog_item_id": "catalog_bess_banaskantha",
      "title": "65 MW / 130 MWh Standalone BESS - Banaskantha",
      "provider": "Kintech Synergy Private Limited",
      "location": "Banaskantha, Gujarat",
      "target_cod": "2026-12-31",
      "document_room_url": "https://lium.example/document-room/project_banaskantha_bess",
      "project_details_url": "https://api.lium.example/projects/project_banaskantha_bess",
      "webhooks": {
        "document_update": "https://developer.example/lium/document-update",
        "diligence_flag_update": "https://developer.example/lium/diligence-flag-update"
      }
    }
  }
}
```

## Collaboration Notes

Requested collaborators/reviewers:

- `rt-finternet`
- `praveenjha527`
- `Tanmoydada`

If these users are collaborators on `arhamshah08/lium-platform`, request them as reviewers on the pull request. If they are not collaborators, add them to the repository or organization first.
