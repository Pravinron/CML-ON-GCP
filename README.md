# CML-ON-GCP
This project describes the process of migrating a VMware-based virtual machine (VM) to Google Cloud Platform (GCP) using an exported OVA file. The migration process involves setting up the environment, exporting the VM, and importing it into GCP. Detailed steps are provided below.

## Overview
This project details the migration process of a VMware-based virtual machine (VM) to Google Cloud Platform (GCP) using an exported OVA file.

## Steps
1. **Prepare the VMware VM**
   Ensure the VM in VMware is properly configured and ready for export. This includes ensuring all required applications and services are working and the system is    updated. Verify that the VM has the required disk space, CPU, and memory for migration. Clean up any unnecessary files, ensuring only required data is   exported. You may use guidelines provide by jeremyIT lab to set up the CML on the VMware Workstation. https://youtu.be/Ww7_qK5JNz4?si=QW-53Uc_LEZoM8qt
2. **Export the VM to OVA**
  In VMware, navigate to File > Export > Export OVF Template.
  Select the VM you want to export and choose OVA as the file format.
  Save the .ova file to your local machine.
3. **Upload the OVA to GCP Storage**
   Once you have the .ova file, the next step is to upload it to a Google Cloud Storage bucket.
   1. Create a Cloud Storage Bucket:
      In the GCP Console, navigate to Storage > Browser.
      Click Create Bucket.
      Follow the prompts to create a bucket. Choose a globally unique name and select a location for your bucket.
      Upload the OVA File:
   2.  Once the bucket is created, click on the bucket name.
      Click Upload Files and select your .ova file to upload.
4. **Convert OVA to GCP Image**
   GCP does not support OVA files directly, so you'll need to convert the OVA to a compatible format (Google Cloud Image).

6. **Import the OVA into GCP:**
    Use the gcloud command to import the OVA file into Google Cloud. Ensure the Cloud Storage bucket is set up.
    
        gcloud compute images import your-image-name \
            --source-uri gs://your-bucket-name/your-file.ova \
            --os-type linux \
            --project your-project-id
            
Verify the Image:
After the image is imported, verify its availability in your project by listing images:

        gcloud compute images list --filter="name=your-image-name"

6. **Create GCP VM Instance from Image**
    Now that the image is ready, you can create a VM instance based on this image.

    Create the VM:
    In the GCP Console or using the gcloud CLI, create a VM instance from the image:

        gcloud compute instances create instance-name \
            --zone=us-central1-c \
            --image=your-image-name \
            --image-project=your-project-id \
            --machine-type=n2-standard-16 \
            --network-interface=nic-type=GVNIC,subnet=default \
            --boot-disk-size=240GB \
            --boot-disk-type=pd-balanced \
            --tags=http-server,https-server
            --enable-nested-virtualization
    Configure the VM:
    Customize the VM settings based on your requirements (e.g., machine type, disk size, network settings).

7. **Configure and Test the VM**
Once the VM is created, you will need to configure it and test it to ensure everything works as expected:
Access the VM: Use SSH to connect to your VM and perform necessary configuration:

        gcloud compute ssh instance-name --zone=us-central1-c

Check System Logs: Ensure that the system is booting correctly by checking system logs and confirming the network and storage configurations.
Test Services: If your VM hosts specific services (e.g., web server, database), verify that they are running as expected.


## Notes
- Make sure to choose the correct machine type and network settings during VM creation.
- Consider running a firewall on GCP and ensuring the correct SSH keys are set up.
- As per testing few CPU is seems only n2-standard-16 is supported with virtualization process. You may consider other CPUs if supported

## Conclusion
This project demonstrates a successful migration from VMware to Google Cloud Platform using an OVA file. By following the steps outlined in this guide, you can migrate any VMware-based VM to GCP, benefiting from GCP's scalability, security, and global infrastructure.

Feel free to fork this repository and adapt the process for your own use case.
