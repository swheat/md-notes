# File Transfer Topology Diagram

### Test File(s)
1. 2.5 GB incompressible, pseudo-random file
2. 2.5 TB incompressible, pseudo-random file

### Text Topology Diagram
```
[[Azure Storage Account A (West US)]
       |
       |----- (Move file 1 with azcopy: 2.5 GB, 21 seconds) --------------|
       |----- (Move file 2 with azcopy: 2.5 TB, 6 minutes 4 seconds) -----|
       v
[[Azure Storage Account B (West US)]
       |
       |----- (Download File 1 with azcopy: 2.5 GB, ~1 minute) -----------|
       |----- (Download File 2 with azcopy: 2.5 TB, 6 hours 27 minutes) --|
       v
[On-Prem Server in Sacramento, CA]
       |
       |----- (Upload File 1 with azcopy: 2.5 GB, ~1 minutes) ------------|
       |----- (Upload File 2 with azcopy: 2.5 TB, 6 hours 1 minute) --- --|
       v
[Azure Storage Account B (West US)]
```
### Data Flow Details

1. **Transfer between storage accounts in Azure West US**:
   - **Source**: Azure storage account West US region
   - **Destination**: Azure storage account West US region
   - **2.5 GB file transfer time**: 21 seconds
   - **2.5 TB file transfer time**: 6 minutes 4 seconds
   - **Transfer Speeds**: ~150,000 Mbs

2. **Download from Azure West US**:
    - **Source**: Azure storage account West US region
    - **Destination**: On-prem data center in Sacramento, CA
    - **2.5 GB file transfer time**: 1 minute
    - **2.5 TB file transfer time**: 6 hours 27 minutes
    - **Transfer Speeds**: ~9,000 Mbs

3. **Upload to Azure West US (Results Monday)**:
   - **Source**: On-prem data center in Sacramento, CA
   - **Destination**: Azure storage account West US region
   - **2.5 GB file transfer time**: 1 minute
   - **2.5 TB file transfer time**: 6 hours 1 minute
   - **Transfer Speeds**: ~10,000 Mbs

