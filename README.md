# Virtual Machines Managing

#### *In this tutorial, we will:*
*- Outline the process for setting up virtual machines.*

*- Deploy virtual machines.*

*- Configure availability features for virtual machines, including scaling options.*

#### Tasks:
 1. Deploy Azure virtual machines with zone resilience using the Azure portal.
 2. Manage scaling for compute and storage resources in virtual machines.
 3. Set up and configure Azure Virtual Machine Scale Sets.
 4. Adjust the scaling of Azure Virtual Machine Scale Sets.

## Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal

In this task, you will deploy two Azure virtual machines into different availability zones by using the Azure portal. Availability zones offer the highest level of uptime SLA for virtual machines at 99.99%. To achieve this SLA, you must deploy at least two virtual machines across different availability zones.

1.	Search for and select Virtual machines, on the Virtual machines blade, click "+ Create", and then select in the drop-down Azure virtual machine.

2.	On the Basics tab, in the Availability zone drop down menu, place a checkmark next to Zone 2. This should select both Zone 1 and Zone 2.

3.	On the Basics tab, complete the following settings: Subscription, Resource group, Virtual machine names (Edit names after selecting availability zones), Region, Availability options, Availability zone, Security type, Image, Azure Spot instance, Size, Username, Password, Public inbound ports.

4.  Click Next: Disks >, then configure: OS disk type, Delete with VM, Enable Ultra Disk compatibility.

5.  Click Next: Networking >, take the default values except for: Delete public IP and NIC when VM is deleted, Load balancing options.

6.  Click Next: Management >, then specify: Patch orchestration options.

7.  Click Next: Monitoring >, then configure: Boot diagnostics.

8.  Click Next: Advanced >, accept the defaults, then click Review + Create. After validation completes, click Create. Note: Notice as the virtual machine deploys the NIC, disk, and public IP address (if configured) are independently created and managed resources.

9.  Wait for the deployment to complete, then select Go to resource.
