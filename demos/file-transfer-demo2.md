# File Transfer Topology Diagram

### Test File(s)
2.5 GB incompressible, pseudo-random file
2.5 TB incompressible, pseudo-random file

### Text Topology Diagram
```
[[Azure Storage Account A (West US)]
       |
       |----- (Download File 1: 2.5 GB, ? minutes) -------------|
       |----- (Download File 2: 2.5 TB, ? hours ? minutes) -----|
       v
[On-Prem Server in Sacramento, CA]
       |
       |----- (Upload File 1: 2.5 GB, ? minutes) ---------------|
       |----- (Upload File 2: 2.5 TB, ? hours ? minutes) -------|
       v
[Azure Storage Account A (West US)]
```
### Data Flow Details

1. **Download from Azure West US**:
    - **Source**: Azure storage account West US region
    - **Destination**: On-prem data center in Sacramento, CA
    - **2.5 GB file transfer time**: ? minutes
    - **2.5 TB file transfer time**: ? hours ? minutes
    - **Transfer Speeds**: ? Mbs

2. **Upload to Azure West US**:
   - **Source**: On-prem data center in Sacramento, CA
   - **Destination**: Azure storage account West US region
   - **2.5 GB file transfer time**: ? minutes
   - **2.5 TB file transfer time**: ? hours ? minutes
   - **Transfer Speeds**: ? Mbs

