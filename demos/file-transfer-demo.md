# File Transfer Topology Diagram

### Test File
2.25 TB incompressible (already at max compression) pgp-encrypted file of AVI video data

### Text Topology Diagram
```
[On-Prem Server] (2.25 TB file, ~8 hours)  
|  
v  
[Azure Storage Account A (US East region)]  
|  
|----- (6 minutes 2 seconds) ----> [Azure Storage Account B (US East region)]  
|  
|----- (7 minutes 10 seconds) ---> [Azure Storage Account C (US West region)]
```
### Data Flow Details

1. **On-Prem Upload (from home with a 1 Gbs fiber connection)**:
    - **Source**: On-Prem Server (2.25 TB file)
    - **Destination**: Azure Storage Account A (US East region)
    - **Time Taken**: ~8 hours
    - **Max Transfer Speeds**: 842 Mbs

2. **Same Region Azure Transfer**:
    - **Source**: Azure Storage Account A (US East region)
    - **Destination**: Azure Storage Account B (US East region)
    - **Time Taken**: 6 minutes 2 seconds
    - **Max Transfer Speeds**: 101,000 Mbs

3. **Cross-Region Azure Transfer**:
    - **Source**: Azure Storage Account A (US East region)
    - **Destination**: Azure Storage Account C (US West region)
    - **Time Taken**: 7 minutes 10 seconds
    - **Max Transfer Speeds**: 83,000 Mbs
    - **Data Egress Charges Cross Region**: ~$50 (estimated still to be verified, not showing up in billing yet)