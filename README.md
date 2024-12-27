# CML-ON-GCP
This project describes the process of migrating a VMware-based virtual machine (VM) to Google Cloud Platform (GCP) using an exported OVA file. The migration process involves setting up the environment, exporting the VM, and importing it into GCP. Detailed steps are provided below.

## Overview
This project details the migration process of a VMware-based virtual machine (VM) to Google Cloud Platform (GCP) using an exported OVA file.

## Steps
1. **Prepare the VMware VM**
2. **Export the VM to OVA**
3. **Upload the OVA to GCP Storage**
4. **Convert OVA to GCP Image**
5. **Create GCP VM Instance from Image**
6. **Configure and Test the VM**

## Notes
- Make sure to choose the correct machine type and network settings during VM creation.
- Consider running a firewall on GCP and ensuring the correct SSH keys are set up.
