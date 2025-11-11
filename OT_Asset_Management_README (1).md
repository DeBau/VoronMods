# 🏭 OT/ICS Asset Management System
**Enterprise Asset Management für Operational Technology**

[![Version](https://img.shields.io/badge/Version-2.0-blue)]()
[![License](https://img.shields.io/badge/License-Commercial-green)]()
[![Standard](https://img.shields.io/badge/ISA--95-Compliant-orange)]()
[![Security](https://img.shields.io/badge/IEC%2062443-Compliant-red)]()

> **Vollständig integrierte Plattform für die Verwaltung, Dokumentation, Analyse und Absicherung industrieller Assets**

---

## 📑 Inhaltsverzeichnis

- [🎯 System-Übersicht](#-system-übersicht)
- [🏗️ Architektur](#️-architektur)
  - [Frontend Layer](#frontend-layer)
  - [API Layer](#api-layer)
  - [Data Collection Layer](#data-collection-layer)
- [📊 ISA-95 Datenmodell](#-isa-95-datenmodell)
  - [Physical Location Hierarchy](#1-physical-location-hierarchy-wo)
  - [Equipment Hierarchy](#2-equipment-hierarchy-was)
  - [Process Segment Hierarchy](#3-process-segment-hierarchy-wie)
  - [Organizational Hierarchy](#4-organizational-hierarchy-wer)
- [💾 Asset-Datenmodell](#-asset-datenmodell)
- [📋 ENUMs & Standards](#-enums--validierte-wertelisten)
- [🔄 Version Control System](#-version-control-system)
- [🛡️ Security & Risk Assessment](#️-security--risk-assessment)
  - [Security Assessment](#security-assessment-wizard)
  - [Risk Assessment](#risk-assessment)
- [📈 Dashboards](#-dashboards)
- [🤖 Automatische Analysen](#-automatische-analysen)
- [📜 License System](#-license-system)
- [📊 Reporting](#-reporting-system)
- [✅ Standards Compliance](#-standards-compliance)

---

## 🎯 System-Übersicht

### Kernphilosophie

| Prinzip | Beschreibung |
|---------|--------------|
| **🎯 Alles aus einer Hand** | Keine fragmentierten Tools mehr |
| **🏗️ ISA-95 Native** | Von Grund auf standardkonform |
| **🔒 Security First** | Entwickelt für höchste OT-Sicherheitsanforderungen |

### Key Features

<table>
<tr>
<td width="50%">

**Asset Management**
- ✅ 280+ Datenfelder pro Asset
- ✅ Multi-Protocol Scanner
- ✅ Automatische Discovery
- ✅ Lifecycle Management

</td>
<td width="50%">

**Security & Compliance**
- ✅ IEC 62443 Assessment
- ✅ NIS2 Compliance
- ✅ CVE Auto-Matching
- ✅ Risk Management

</td>
</tr>
<tr>
<td>

**Versioning & Control**
- ✅ Check-Out/Check-In
- ✅ 4-Augen-Prinzip
- ✅ SPS/EPLAN/HMI Versionierung
- ✅ Audit Trail

</td>
<td>

**Analytics & Reporting**
- ✅ 8 Dashboards
- ✅ Financial Rollup
- ✅ OEE Calculation
- ✅ Custom Reports

</td>
</tr>
</table>

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/assetOverview.png" alt="System Overview" width="1000">

---

## 🏗️ Architektur

### IEC 62443 Konforme Netzwerk-Architektur

```
       IT ZONE (Level 4-5)                    OT ZONE (Level 0-3)
    ┌──────────────────┐                   ┌──────────────────┐
    │   Frontend UI    │                   │   OT Scanner     │
    │    (React)       │                   │   • PROFINET     │
    │                  │                   │   • Modbus       │
    └────────┬─────────┘                   │   • S7/S7+       │
             │                             │   • SNMP         │
             │ HTTPS                       │   • SSH          │
             ▼                             └────────┬─────────┘
    ┌───────────────────────────────────────────────┼─────────┐
    │                     API SERVER (FastAPI)      │         │
    │  ┌────────────────────────────────────────────▼─────┐   │
    │  │         Multi-NIC IEC 62443 Configuration        │   │
    │  ├──────────────────────────────────────────────────┤   │
    │  │  NIC 1: IT Network (Bidirectional)               │   │
    │  │  NIC 2: OT Network (Unidirectional Inbound)      │   │
    │  └──────────────────────────────────────────────────┘   │
    │                                                         │
    │  ┌──────────────────────────────────────────────────┐   │
    │  │          AUTHENTICATION OPTIONS                  │   │
    │  ├──────────────────────────────────────────────────┤   │
    │  │  IT Zone:           │  OT Zone:                  │   │
    │  │  • None (Dev)       │  • API Key (Default)       │   │
    │  │  • Basic Auth       │  • mTLS (Recommended)      │   │
    │  │  • API Key          │  • Basic Auth              │   │
    │  │  • mTLS/SSL         │  • SSL/TLS                 │   │
    │  │  • LDAP/AD          │                            │   │
    │  └──────────────────────────────────────────────────┘   │
    ├─────────────────────────────────────────────────────────┤
    │                    SERVICE LAYER                        │
    │  • Asset Service          • Financial Rollup Service    │
    │  • Version Control        • Discovery Service           │
    │  • Security Assessment    • Risk Calculation            │
    │  • CVE Matching           • Compliance Service          │
    │  • OEE Calculation        • Analytics Engine            │
    └─────────────────────────────┬───────────────────────────┘
                                  │
                                  ▼
    ┌──────────────────────────────────────────────────────────┐
    │                   DATABASE (PostgreSQL)                  │
    │         Assets | Equipment | Processes | Organizations   │
    └──────────────────────────────────────────────────────────┘
```

### Agent Deployment & Kommunikation

```
┌─────────────────────────────────────────────────────────────┐
│                    OT ZONE AGENTS                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Windows Agents (Level 2-3)          OT Network Scanner     │
│  ┌─────────────────────┐            ┌──────────────────┐    │
│  │ • WMI Collection    │            │ • PROFINET DCP   │    │
│  │ • Hardware Info     │            │ • S7 Protocol    │    │
│  │ • Software Inventory│            │ • Modbus TCP     │    │
│  │ • Network Config    │            │ • SNMP Discovery │    │
│  │ • Performance Data  │            │ • SSH Scanner    │    │
│  └──────────┬──────────┘            └────────┬─────────┘    │
│             │                                │              │
│             └─────────────┬──────────────────┘              │
│                           │                                 │
│                    PUSH ONLY (HTTPS)                        │
│                           │                                 │
│             ┌─────────────▼──────────────┐                  │
│             │   Unidirectional Push      │                  │
│             │   • JSON Payload           │                  │
│             │   • Compressed (gzip)      │                  │
│             │   • Encrypted (TLS 1.3)    │                  │
│             │   • Auth (API Key/mTLS)    │                  │
│             └────────────────────────────┘                  │
└─────────────────────────────────────────────────────────────┘
                           ▲
                           │
                     API Endpoint
                  /api/v1/agents/push
```


### Datenfluss-Prinzipien

```
IT → API Server:      ✅ Bidirektional (User Interface)
Agents → API Server:  ✅ Unidirektional PUSH (keine Rückantwort)
API Server → Agents:  ❌ Keine Verbindungen (Pull verboten)
Scanner → OT Devices: ✅ Read-Only Discovery (keine Writes)
API Server → OT:      ❌ Keine aktiven Verbindungen

Authentication per Zone:
IT Zone:  Flexibel (None/Basic/API Key/mTLS/LDAP)
OT Zone:  Strikt (API Key/mTLS bevorzugt)
```

### Security Features

<table>
<tr>
<td width="50%">

**IT Zone Security**
- Session-based Auth
- JWT Tokens
- LDAP/AD Integration
- Role-Based Access (RBAC)
- Multi-Factor Auth (optional)

</td>
<td width="50%">

**OT Zone Security**
- Stateless Auth (API Key/mTLS)
- No Sessions (Push only)
- Certificate Pinning
- Rate Limiting
- IP Whitelisting

</td>
</tr>
</table>

### Frontend Layer

<details>
<summary><strong>📊 Dashboards & Management Interfaces</strong></summary>
<br>

**Dashboards**
- Executive Overview
- Operations Dashboard
- Security Dashboard
- Compliance Dashboard
- Financial Dashboard
- Maintenance Dashboard

**Management**
- Asset Management
- Location Hierarchy
- Equipment Management
- Process Management
- Organization Management
- User Management

**Security & Risk**
- Security Assessment Wizard
- Risk Assessment Wizard
- Vulnerability Management
- Compliance Reports
- Audit Trail Viewer

**Analytics & Reporting**
- Financial Analysis
- Performance Analysis
- Trend Analysis
- Predictive Maintenance
- Custom Report Builder
- Template Library

</details>

### API Layer

<details>
<summary><strong>🔌 REST API & Services</strong></summary>
<br>

**Authentication & Authorization**
- Zone-aware Auth (IT Zone / Shopfloor Zone)
- Multi-mode: None / Basic / API Key / mTLS
- Role-based Access Control (RBAC)
- LDAP/Active Directory Integration

**Core API Endpoints**
```
/api/v1/assets          - Asset Management
/api/v1/locations       - Location Management
/api/v1/equipment       - Equipment Management
/api/v1/processes       - Process Management
/api/v1/organizations   - Organization Management
/api/v1/networks        - Network Management
/api/v1/versions        - Version Control
/api/v1/security        - Security Assessments
/api/v1/risk           - Risk Assessments
/api/v1/cves           - CVE Management
/api/v1/agents         - Agent Management
/api/v1/reports        - Reports & Analytics
```

**Business Services**
- Asset Service (Lifecycle Management)
- Version Control Service (Check-in/Check-out)
- Financial Rollup Service
- Discovery Service
- Security Service
- Risk Service
- Vulnerability Service
- Compliance Service

</details>

### Data Collection Layer

<details>
<summary><strong>🔍 Agents & Scanner</strong></summary>
<br>

**Windows Agent**
- System Information (WMI)
- Hardware Detection
- Software Inventory
- Network Configuration
- Service Status
- Performance Metrics
- Event Log Analysis

**OT Network Scanner**
- PROFINET Discovery (DCP)
- Siemens S7 Scanner
- Modbus TCP Scanner
- SNMP Discovery
- SSH Scanner
- Protocol Detection
- Manufacturer Detection

**Integration Connectors**
- NVD API (Vulnerabilities)
- Vendor Advisories
- Custom REST/ODBC

</details>

---

## 📊 ISA-95 Datenmodell

> **Vier separate, unabhängige Hierarchien mit N:M Relationen**

### 1. Physical Location Hierarchy (Wo?)

<details>
<summary><strong>📍 10 Location Types mit 45+ Feldern</strong></summary>
<br>

```
Enterprise              → Konzern/Holding
├── Site                → Werk/Standort
    ├── Building        → Gebäude
        ├── Floor       → Etage
            ├── Area    → Bereich
                ├── Room        → Raum
                    ├── Zone    → Zone
                        ├── Cabinet → Schaltschrank
                            ├── Rack    → Rack
                                └── Slot    → Slot/Position
```

**Datenfelder**
- **Basis:** Name, Type, Description, Parent
- **Adresse:** Address, City, State, ZIP, Country
- **GPS:** Latitude, Longitude, Altitude
- **Sicherheit:** Hazardous Area, Classification, Security Level
- **Umgebung:** Environmental Zone, Temperature/Humidity Range
- **Organisation:** Responsible Person, Department, Cost Center
- **Notfall:** Emergency Contact, Assembly Point, Evacuation Route
- **Dokumentation:** Floor Plans, Access Restrictions, Notes

</details>

### 2. Equipment Hierarchy (Was?)

<details>
<summary><strong>⚙️ 13 Equipment Classes mit Financial Rollup</strong></summary>
<br>

```
Enterprise              → Gesamtunternehmen
├── Site                → Werk-Ebene
    ├── Area            → Produktionsbereich
        ├── Production_Line     → Fertigungslinie
            ├── Work_Center     → Arbeitszentrum
                ├── Work_Cell   → Arbeitszelle
                    └── ...     → bis Component
```

**Financial Rollup Feature**
```sql
-- Automatische rekursive Kostenberechnung
SELECT calculate_equipment_asset_value('Fertigungslinie A');
→ 800.000 EUR (Equipment + alle Assets)

SELECT calculate_equipment_operating_cost('Fertigungslinie A');
→ 120.000 EUR/Jahr (rekursiv aggregiert)
```

**Datenfelder (60+)**
- **Finanzen:** Acquisition Cost, Book Value, Operating/Maintenance Cost
- **Performance:** OEE Target/Actual, Availability, Performance, Quality
- **Reliability:** MTBF, MTTR, Failure Count
- **Energie:** Power Rating, Energy Consumption
- **Kapazität:** Throughput, Max Production Rate

</details>

### 3. Process Segment Hierarchy (Wie?)

<details>
<summary><strong>🔄 7 Segment Types mit 5 Execution Types</strong></summary>
<br>

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/processSegments.png" alt="Process Segments" width="800">

**Segment Types**
```
Enterprise → Site → Area → Process_Cell 
→ Unit_Procedure → Operation → Phase
```

**Execution Types**
- `Continuous` - Durchlaufprozess (24/7)
- `Batch` - Chargenprozess
- `Discrete` - Stückgut
- `Semi_Continuous` - Hybrid
- `Campaign` - Kampagnenfertigung

**Process Documentation**
```json
{
  "input_materials": [
    {"material": "Rohmaterial A", "quantity": 100, "unit": "kg"}
  ],
  "output_materials": [
    {"material": "Produkt X", "quantity": 95, "unit": "kg"}
  ],
  "process_parameters": {
    "temperature": {"target": 180, "tolerance": 5, "unit": "°C"},
    "pressure": {"target": 2.5, "tolerance": 0.2, "unit": "bar"}
  }
}
```

</details>

### 4. Organizational Hierarchy (Wer?)

<details>
<summary><strong>👥 9 Organization Types mit Budgetverwaltung</strong></summary>
<br>

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/organization.png" alt="Organization" width="800">

```
Corporation → Company → Business_Unit → Division 
→ Department → Team → Cost/Profit/Investment Center
```

**Datenfelder (35+)**
- **Finanzen:** Cost/Profit Center Number, Budget
- **Personal:** Manager, Deputy, Headcount
- **Verantwortung:** Area of Responsibility, Asset Count

</details>

---

## 💾 Asset-Datenmodell

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/assetSoftware.png" alt="Asset Software" width="800">

### Assets Haupttabelle

<details>
<summary><strong>📋 ~100 Basis-Felder</strong></summary>
<br>

**Kategorien**
- ✅ Basis-Daten & Identifikation
- ✅ Network Configuration
- ✅ Lifecycle Management
- ✅ Finanzen & Kosten
- ✅ Verantwortlichkeiten
- ✅ Sicherheit & Compliance
- ✅ Technische Daten
- ✅ Standort & Location
- ✅ Dokumentation
- ✅ Nutzung & Performance
- ✅ Produktion & OEE
- ✅ Custom Fields (JSONB)

</details>

### Asset Inventory Extended

<details>
<summary><strong>🔧 ~135 OT-spezifische Erweiterungsfelder</strong></summary>
<br>

**Erweiterte Kategorien**
- **Classification:** Criticality, Redundancy, Safety Critical
- **Hardware:** Vendor, Product Family, Revisions
- **Network:** Multi-IP, Purdue Level, 45 Protocols
- **Function:** PLC Programs, HMI Configs, SCADA Tags
- **Lifecycle:** 14 Phasen von Planning bis Disposed
- **Maintenance:** 8 Strategien, 9 Typen, MTBF/MTTR

</details>

### Network Adapters

<details>
<summary><strong>🌐 ~45 Felder für Netzwerkschnittstellen</strong></summary>
<br>

**N:M Relation** - Ein Asset kann mehrere NICs haben
- Adapter Types (9): Ethernet, WiFi, Fiber, Industrial Ethernet...
- Protocols (45): PROFINET, EtherNet/IP, Modbus, S7, OPC UA...
- VLAN & Segmentation
- Performance Metrics
- Security Settings

</details>

---

## 📋 ENUMs - Validierte Wertelisten

<details>
<summary><strong>50+ Standardisierte Wertelisten</strong></summary>
<br>

### Highlights

| Kategorie | ENUMs | Beispiele |
|-----------|-------|-----------|
| **Asset Types** | 32 Werte | PLC, HMI, SCADA, Robot, Drive... |
| **Manufacturers** | 50 Hersteller | Siemens, Rockwell, ABB, Schneider... |
| **Protocols** | 45 Protokolle | PROFINET, EtherNet/IP, Modbus, OPC UA... |
| **Criticality** | 7 Level | Mission_Critical bis Not_Critical |
| **Lifecycle** | 14 Phasen | Planning bis Disposed |
| **Maintenance** | 8 Strategien | Preventive, Predictive, TPM... |

</details>

---

## 🔄 Version Control System

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/versionConceptOverview.png" alt="Version Control" width="1000">

### Check-Out / Check-In Workflow

<details>
<summary><strong>📦 Asset-Daten-Kategorien & Workflow</strong></summary>
<br>

**Versionierte Daten**
1. **SPS-Programme** - TIA Portal, Step 7, CODESYS
2. **EPLAN-Projekte** - Electric P8, Stromlaufpläne
3. **Dokumentation** - Handbücher, P&IDs, Zertifikate
4. **HMI-Projekte** - WinCC, FactoryTalk
5. **Konfiguration** - Network, Firewall, SCADA

**Status Workflow**
```
DRAFT → REVIEW → APPROVED → PRODUCTIVE → WITHDRAWN → ARCHIVED
```

**Features**
- ✅ 4-Augen-Prinzip
- ✅ Semantic Versioning (Major.Minor.Patch)
- ✅ File Hashing (SHA256)
- ✅ Diff Viewer
- ✅ Complete Audit Trail

</details>

---

## 🛡️ Security & Risk Assessment

### Security Assessment Wizard

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/securityWizard.png" alt="Security Wizard" width="1000">

<details>
<summary><strong>13 Kategorien mit 65+ Fragen</strong></summary>
<br>

**Assessment Kategorien**
1. Architecture & Segmentation
2. CIA & Business Criticality
3. Legacy Status & Support
4. Safety Integration
5. Access Control & Authentication
6. Remote Access & VPN
7. Patch Management & Vulnerability
8. Monitoring & Logging
9. Backup & Business Continuity
10. Resilience & Redundancy
11. Physical Security
12. Governance & Policies
13. Incident Readiness

**Features**
- ✅ Progressive Wizard Experience
- ✅ Radar Chart Visualization
- ✅ Color-Coded Ratings
- ✅ Best Practice Recommendations
- ✅ IEC 62443 & NIS2 konform

</details>

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/security.png" alt="Security Results" width="1000">

### Risk Assessment

<details>
<summary><strong>ISO 27005 / IEC 62443 Risk Management</strong></summary>
<br>

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/riskWizard.png" alt="Risk Wizard" width="800">

**Risk Dimensions**
- **Likelihood:** 0-10 Score mit 5 Ratings
- **Impact:** 6 Dimensionen (CIA + Safety + Financial + Reputation)
- **Risk Score:** Likelihood × Impact (0-100)

**Risk Treatment**
- Avoid / Mitigate / Transfer / Accept
- Control Effectiveness (0-10)
- Residual Risk Calculation

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/risk.png" alt="Risk Results" width="800">

</details>

---

## 📈 Dashboards

### 8 Spezialisierte Dashboards

<details>
<summary><strong>🎯 Dashboard-Übersicht</strong></summary>
<br>

| Dashboard | Zielgruppe | Key Features |
|-----------|------------|--------------|
| **Executive** | C-Level | Asset Value, Risk Overview, Compliance Status |
| **Operations** | Plant Manager | OEE, MTBF/MTTR, Downtime Analysis |
| **Security** | CISO | Vulnerabilities, Security Score, Threat Analysis |
| **Compliance** | Auditor | Standards Tracking, Audit Trails, Certifications |
| **Financial** | CFO | Cost Analysis, Budget Tracking, ROI |
| **Maintenance** | Technicians | PM Schedule, Work Orders, Spare Parts |
| **Network** | Engineers | Topology Map, Protocol Distribution, Zones |
| **Vulnerability** | Security Team | CVE Tracking, Patch Status, CVSS Scores |

</details>

---

## 🤖 Automatische Analysen

<details>
<summary><strong>10+ Automatisierte Prozesse</strong></summary>
<br>

### Highlights

**1. Asset Discovery**
```python
# Nightly Scan
- Protocol Detection
- Manufacturer Recognition
- Model & Firmware Detection
- Auto-Population to Database
```

<img src="https://github.com/DeBau/bau-Version---OT-Assetmanagement-Versioning-Tool/blob/main/images/agent.png" alt="Agent" width="600">

**2. CVE Matching**
```python
# Daily Job
- Generate CPE strings
- Query NVD API
- Match vulnerabilities
- Calculate risk scores
- Send critical alerts
```

**3. Financial Rollup**
- Equipment value calculation
- Operating cost aggregation
- Depreciation updates
- Budget utilization

**4. Performance Analysis**
- OEE calculation
- Bottleneck detection
- Predictive maintenance
- Trend analysis

</details>

---

## 📜 License System

<details>
<summary><strong>4 Tiers + Module System</strong></summary>
<br>

### License Tiers

| Tier | Assets | Features | Target |
|------|--------|----------|--------|
| **Demo** | 10 | Basic | Test |
| **Starter** | 50 | Core + Security | Small Plants |
| **Professional** | 250 | Full Features | Medium Sites |
| **Enterprise** | Unlimited | All + API | Multi-Site |

### Module System
- Core (immer dabei)
- Security Assistant
- Risk Assistant
- Compliance
- Advanced Reporting
- Version Control
- CVE Management

### Asset Packs
50 / 100 / 150 / 200 / 250 / 500 / unlimited

</details>

---

## 📊 Reporting System

<details>
<summary><strong>20+ Reports + Custom Builder</strong></summary>
<br>

### Built-In Reports
**Asset:** Inventory, Lifecycle, Financial  
**Security:** Assessments, Vulnerabilities, Compliance  
**Operations:** OEE, MTBF/MTTR, Downtime  
**Financial:** TCO, Budget, Depreciation  

### Custom Report Builder
- Drag & Drop Interface
- 50+ Templates
- Export: Excel, PDF, CSV
- Scheduling & Distribution

</details>

---

## ✅ Standards Compliance

<details>
<summary><strong>Implementierte Standards</strong></summary>
<br>

| Standard | Bereich | Status |
|----------|---------|--------|
| **ISA-95 / IEC 62264** | Production Hierarchies | ✅ Vollständig |
| **IEC 62443** | OT Security | ✅ Implementiert |
| **ISO 13849** | Safety Categories | ✅ Integriert |
| **IEC 61508** | SIL Levels | ✅ Unterstützt |
| **ISO 27005** | Risk Management | ✅ Konform |
| **NIS2** | Network Security | ✅ Compliant |
| **GDPR** | Data Protection | ✅ Konform |

</details>

---

## 🚀 Zusammenfassung

### Das System in Zahlen

<table>
<tr>
<td width="33%">

**📊 Daten**
- 280+ Asset-Felder
- 50+ ENUMs
- 45 Protokolle
- 4 ISA-95 Hierarchien

</td>
<td width="33%">

**🛠️ Funktionen**
- 8 Dashboards
- 20+ Reports
- 10+ Auto-Analysen
- Version Control

</td>
<td width="33%">

**🔒 Security**
- IEC 62443
- NIS2
- CVE Tracking
- Risk Management

</td>
</tr>
</table>

> **Ein System. Alles drin. Keine Kompromisse.**

---

## 📞 Kontakt

**Bauer Automation Solutions**  
Hoher Weg 28  
59073 Hamm, Germany  

**Owner:** Dennis Bauer  
**Year:** 2025  

---

<p align="center">
  <strong>© 2025 Bauer Automation Solutions - Professional OT Asset Management</strong>
</p>
