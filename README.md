<h1 align="center">Virtual Machine Management on Azure</h1>

<p align="center">
  <img src="https://github.com/user-attachments/assets/3e7c5a02-1ae5-4b1c-8106-55172f3e581c" alt="azure-vms-svgrepo-com" width="150" height="auto">
</p>

#### *In this tutorial, we will:*
*- Outline the process for setting up virtual machines.*

*- Deploy virtual machines.*

*- Configure availability features for virtual machines, including scaling options.*

#### Tasks:
 1. Deploy Azure virtual machines with zone resilience using the Azure portal.
 2. Manage scaling for compute and storage resources in virtual machines.
 3. Set up and configure Azure Virtual Machine Scale Sets.
 4. Adjust the scaling of Azure Virtual Machine Scale Sets.

## Task 1: Deploy Azure virtual machines with zone resilience using the Azure portal.

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

## Task 2: Manage scaling for compute and storage resources in virtual machines.

In this task, you will scale a virtual machine by adjusting its size to a different SKU. Azure provides flexibility in VM size selection so that you can adjust a VM for periods of time if it needs more (or less) compute and memory allocated. This concept is extended to disks, where you can modify the performance of the disk, or increase the allocated capacity.

1. On the VM1 virtual machine, in the Availability + scale blade, select Size.
2. Set the virtual machine size to DS1_v2 and click Resize. When prompted, confirm the change. Note: Choose another size if Standard DS1_v2 is not available. Resizing is also known as vertical scaling, up or down.
3. In the Settings area, select Disks.
4.	Under Data disks select + Create and attach a new disk. Configure the settings and click Apply (leave other settings at their default values): Disk name, Storage type, Size.
5.	After the disk has been created, click Detach (if necessary, scroll to the right to view the detach icon), and then click Apply. Note: Detaching removes the disk from the VM but keeps it in storage for later use.
6.	Search for and select Disks. From the list of disks, select the VM1 disk object. Note: The Overview blade also provides performance and usage information for the disk.
7.	In the Settings blade, select Size + performance. Set the storage type to another one, and then click Save.
8.	Navigate back to the VM1 virtual machine and select Disks.
9.	In the Data disk section, select Attach existing disks.
10.	In the Disk name drop-down, select the VM1 disk.
11.	Verify the disk is now diferent. Select Apply to save your changes. Note: You have now created a virtual machine, scaled the SKU and the data disk size. In the next task we use Virtual Machine Scale Sets to automate the scaling process.

## Task 3: Set up and configure Azure Virtual Machine Scale Sets.

In this task, you will deploy an Azure virtual machine scale set across availability zones. VM Scale Sets reduce the administrative overhead of automation by enabling you to configure metrics or conditions that allow the scale set to horizontally scale, scale in or scale out.

 1. In the Azure portal, search for and select Virtual machine scale sets and, on the Virtual machine scale sets blade, click + Create.
 2. On the Basics tab of the Create a virtual machine scale set blade, configure the following settings: Subscription, Resource group, Virtual machine scale set name, Region, Availability zone, Orchestration mode, Security type, Scaling options (review defaults), Image, Run with Azure Spot discount, Size, Username, Password, Already have a Windows Server license. Then, click Next: Spot >.
 3. On the Networking page, click the Edit virtual network link below the Virtual network textbox and create a new virtual network with the following settings (leave others with their default values). When finished, select OK: Network Name, Address range	10.82.0.0/20, Subnetwork name, Subnet range	10.82.0.0/24.
 4. In the Networking tab, click the Edit network interface icon to the right of the network interface entry.
 5. For NIC network security group section, select Advanced and then click Create new under the Configure network security group drop-down list.
 6. On the Create network security group blade, specify the name (leave others with their default values).
 7. Click Add an inbound rule and configure: Source (Any), Source port ranges (*), Destination (Any), Service (HTTP), Action (Allow), Priority (1010), Name (allow-http), then save the rule.
 8. Click Add and, back on the Create network security group blade, click OK.
 9. In the Edit network interface blade, in the Public IP address section, click Enabled and click OK.
 10. In the Networking tab, under the Load balancing section, configure: Load balancing options (Azure load balancer), Select a load balancer (Create a load balancer).
 11. On the Create a load balancer page, specify the load balancer name and take the defaults. Click Create when you are done then Next : Management >.
 12. In the Management tab, set Boot diagnostics to Disable, then click Next: Health >. Review the default settings in the Health tab and proceed with Next: Advanced >. In the Advanced tab, click Review + Create, ensure validation passes, and click Create. Note: Wait for the virtual machine scale set deployment to complete. This should take approximately 5 minutes.

## Task 4: Adjust the scaling of Azure Virtual Machine Scale Sets.

In this task, you scale the virtual machine scale set using a custom scale rule.

1.	Select Go to resource or search for and select the scale set created before.
2.	Choose Availability + Scale from the left side menu, then choose Scaling. In this case a Scale out rule.
3.	Select Custom autoscale. Then change the Scale mode to Scale based on metric. And then select Add a rule.
4.	Create a rule to automatically scale out VM instances when the average CPU load exceeds 70% over 10 minutes, increasing instances by 20%. Configure: Metric source, Metric namespace (Virtual Machine Host), Metric name (Percentage CPU), Operator (Greater than), Metric threshold (70), Duration (10 minutes), Time grain statistic (Average), Operation (Increase percent by), Cool down (5 minutes), Percentage (50).
5.	Be sure to Add then Save your changes.
6.	Create a scale-in rule to decrease the number of VM instances when the average CPU load drops below 30% over a 10-minute period, reducing instances by 20%. Select Add a rule, configure: Operator (Less than), Threshold (30), Operation (Decrease percentage by), Percentage (20), then click Add. Be sure to Save your changes.
7.	Set the instance limits to ensure autoscale rules don't exceed the maximum or minimum instances. Configure: Minimum (2), Maximum (10), Default (2). These limits are shown on the Scaling page after the rules. Be sure to Save your changes.
8.	On the scale set page, select Instances. This is where you would monitor the number of virtual machine instances.













