# **Zero Trust Asset Inventory Project Plan**

To help the client with their **Zero Trust** efforts, we need to **inventory several thousand assets** (including corporate machines, BYOD, mobile, IoT, and network devices) while leveraging **Microsoft Sentinel** for existing data. Below is a structured **plan of action** to assess, inventory, and initiate the work using an offshore blend model.

---

## **📌 Project Approach**
### **🔹 Phase 1: Discovery & Assessment (1-2 Weeks)**
#### **1️⃣ Stakeholder Discovery Call (Next Week)**
- **Goal:** Understand the **client's environment, existing Sentinel data**, security gaps, and Zero Trust objectives.
- **Participants:** IT Security, Networking, Endpoint, Compliance teams.

#### **2️⃣ Asset Scope & Data Sources**
- Identify existing asset **visibility gaps** in Microsoft Sentinel.
- **Validate asset categories:**
    - **Corporate Machines** (500 misconfigured)
    - **BYOD (500)** – Need to determine **onboarding method** (MDM, NAC, etc.).
    - **Mobile Devices (600)** – **MDM (Intune? Other solutions?)**
    - **IoT & Network (1,600)** – Identify **plant machines, OT, SCADA systems**.

#### **3️⃣ Data Collection Methods**
- Extract logs & asset data from **Microsoft Sentinel**.
- Cross-check with **Intune, Defender for Endpoint, SCCM, Azure AD, network scans**.
- For **IoT & network** devices, leverage **network discovery tools (NAC, CMDB, SNMP, NMAP, OT monitoring tools)**.

---

### **🔹 Phase 2: Initial Inventory & Zero Trust Mapping (3-4 Weeks)**
#### **1️⃣ Data Normalization & Enrichment**
- Aggregate **Sentinel logs, CMDB data, and network scans** into a **single asset inventory**.
- **Identify missing attributes** (e.g., device ownership, OS version, compliance status).

#### **2️⃣ Risk-Based Prioritization**
- Categorize assets based on **Zero Trust principles:**
    - **Trusted, At-Risk, Unknown**
    - **Misconfigured Devices (500 Corporate Machines)**
    - **BYOD Risk Levels**
    - **High-Risk IoT Devices (Plant Machines, Legacy Systems)**

#### **3️⃣ Initial Findings & Reporting**
- Provide a **preliminary asset inventory report** with:
    - **Data coverage gaps**
    - **High-risk misconfigurations**
    - **Zero Trust policy recommendations**

---

### **🔹 Phase 3: Automation & Ongoing Monitoring (6+ Weeks)**
#### **1️⃣ Automated Asset Discovery & Classification**
- Implement **Microsoft Sentinel automation playbooks** to track new and unmanaged devices.
- Integrate **Intune, Defender for Endpoint, Azure Security Center**, and **SIEM correlation rules**.

#### **2️⃣ Zero Trust Policy Implementation**
- **Enforce access controls** via:
    - Conditional Access (Azure AD)
    - Device Compliance Policies (Intune, Defender)
    - Network Segmentation for IoT

#### **3️⃣ Offshore Team Integration**
- Utilize an **offshore blend model** for:
    - **Data analysis & normalization**
    - **Asset enrichment & reporting automation**
    - **Policy scripting (Infrastructure-as-Code, Sentinel Playbooks)**

---

## **🚀 Next Steps:**
✅ **Schedule a Discovery Call** (Next Week) to confirm asset inventory scope.  
✅ **Assess Sentinel & other data sources** for coverage gaps.  
✅ **Define automation strategy** for continuous asset monitoring.

Would you like me to draft an **agenda for the discovery call**? 🚀