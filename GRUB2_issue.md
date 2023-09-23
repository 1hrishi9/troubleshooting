# Recovering GRUB2 File on EC2 Instances

If the GRUB2 file of an EC2 instance gets deleted or becomes corrupted, it can prevent the instance from booting properly. This guide outlines the steps to recover the GRUB2 file using three EC2 instances: one for recovery, one for creating a snapshot, and one for copying the recovered file back.

## Prerequisites

1. Access to AWS Management Console.
2. Basic understanding of EC2 instances and AWS services.

## Steps

### Step 1: Launch a Recovery Instance

1. Launch a new EC2 instance (Recovery Instance) in the same region as the affected instance.
2. Choose an instance type that matches the operating system of the affected instance.
3. Ensure that the instance is in the same Virtual Private Cloud (VPC) and subnet as the affected instance.
4. Attach the root volume of the affected instance to the Recovery Instance as an additional volume.

### Step 2: Recover GRUB2 File

1. Connect to the Recovery Instance using SSH.
2. Identify the attached volume from the affected instance using the following command:
   ```
   lsblk
   ```
3. Mount the attached volume to a directory:
   ```
   sudo mkdir /mnt/affected-instance
   sudo mount /dev/xvdX /mnt/affected-instance  # Replace xvdX with the correct device name
   ```
4. Copy the GRUB2 file from the Recovery Instance to the affected instance volume:
   ```
   sudo cp /boot/grub2/grub.cfg /mnt/affected-instance/boot/grub2/
   ```
5. Unmount the volume:
   ```
   sudo umount /mnt/affected-instance
   ```

### Step 3: Create a Snapshot

1. Go to the AWS Management Console.
2. Navigate to the "Volumes" section.
3. Select the volume that was attached to the affected instance.
4. Choose "Actions" and then "Create Snapshot".
5. Give the snapshot a meaningful name and description.
6. Click "Create Snapshot".

### Step 4: Attach Recovered Volume to Original Instance

1. Stop the affected EC2 instance.
2. Detach the root volume of the affected instance.
3. Go to the "Snapshots" section in the AWS Management Console.
4. Select the snapshot created in Step 3.
5. Choose "Actions" and then "Create Volume".
6. Select the availability zone and size for the new volume.
7. Once the volume is created, attach it to the original affected instance as the root volume.

### Step 5: Test and Verify

1. Start the affected EC2 instance.
2. Monitor the instance's status in the AWS Management Console.
3. If the instance starts successfully and operates as expected, the GRUB2 recovery was successful.

## Conclusion

Recovering a deleted or corrupted GRUB2 file on an EC2 instance can be achieved by launching a recovery instance, copying the GRUB2 file, creating a snapshot, and attaching the recovered volume back to the original instance. Following these steps should help resolve boot-related issues caused by GRUB2 file problems. Always exercise caution and ensure you have backups before performing such recovery operations.
                           ***THE END***
                           