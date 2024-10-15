# SIEM_Home_Lab
Azure Sentinel SIEM Home Lab

First I'm going to set up an Azure account. This will be the cloud environment used in this case.

Head to this link: [https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account?icid=azurefreeaccount](url)

Then click “Try Azure for free”, enter personal information.

![3](https://github.com/user-attachments/assets/4389bc06-4cce-4046-a145-bced8623eb53)

Then select “Go to Azure Portal”

![3](https://github.com/user-attachments/assets/1a2fea78-6409-4a3e-ad70-42b217763196)

You will be greeted with the home page.

![3](https://github.com/user-attachments/assets/e8936307-5bcd-4ca7-ae1d-3a475da854bd)

First thing I want to do is start up a virtual machine, to do this, in the search type “Virtual Machines” and select “Virtual Machines”.

![3](https://github.com/user-attachments/assets/1cf70bf4-5a0c-41ea-bc4c-6ca7be0e2180)

Select “Create” on the top left of the screen.

![3](https://github.com/user-attachments/assets/8e62bb2c-b354-41da-b2e3-8d50a8d21e8a)

Select “Azure virtual machine” since I don't have any presets.

![3](https://github.com/user-attachments/assets/337ac2d8-1ec8-4f9a-9ad5-6c96c634182f)

Next step in configuring the VM, create a resource group. Select “Create new”, then name this resource group.

VM stands for Virtual Machine.

Resource group is a logical grouping of resources in Azure. This resource group will be used for all resources in this use case.

![3](https://github.com/user-attachments/assets/042d6abe-104e-477d-b4c3-ba5909f50e34)

Next name the VM.

Region is specific for wherever you and your customers are located in the world.

As for Availability options I selected “No infrastructure redundancy required” since this is not a critical machine for anything.

![3](https://github.com/user-attachments/assets/04c75419-0a50-47b5-a3e9-9ab368bdc8da)

Next is the security type. I select standard since I am wanting more traffic to come to this machine, and won't use the extra security options.

Then Image, which is the OS of your choosing. I chose Windows 10 for this configuration.

![3](https://github.com/user-attachments/assets/ab8810cd-6330-434a-9d53-1f6700a3084a)

I wont worry about VM architecture since it selects the option automatically.

I don't select “Run with Azure Spot Discount”, since I don't want this machine to be stopped with a short notice.

![3](https://github.com/user-attachments/assets/86f96404-395d-4c02-bfc5-05115211841e)

For the size of the VM I will go with the default since it will be sufficient and remains in the free tier. 

Also I don't select “Enable Hibernation” . This feature basically deallocates VM RAM to the root volume, in turn saving you money and time. But this doesn't apply to this use case.

![3](https://github.com/user-attachments/assets/3e95c287-ce9d-4a05-b458-9422d38a5706)

Next I create the Username and Password that will be used on this VM. Make sure to remember these credentials.

Select “Allow selected ports” 

Then check the Licensing box at the bottom.

Select “Next: Disks >” at the bottom to move to the next section.

![image](https://github.com/user-attachments/assets/11fc0ce0-de77-442a-a67f-90ac29406363)

Everything in Disks is fine as default. Select “Next: Networking >”

In the Networking section that says “NIC network security group”, this is essentially a firewall. It applies security rules that apply to the given group. I am wanting to allow basically everything from the public to this VM. 

To do this select “advanced” then “Create new”.

![image](https://github.com/user-attachments/assets/ced3ba0b-8a47-444f-9d61-e489e3cf957f)

In the security group I will delete any previous rules. Then Add my own inbound rule.

![image](https://github.com/user-attachments/assets/aefed9f4-97a0-421b-be58-00e1d760f094)

To make essentially all ports open in the new inbound security rule, I am looking for ANY source from ANY port to ANY destination in ANY port, from ANY protocol. 

* represents ANY for port ranges.

For Priority the lower the number, the higher the priority.

Then name your new rule, and select “Add” to add the rule.

![image](https://github.com/user-attachments/assets/ca3a256b-45bd-47b3-b00b-18409f7b5af1)

Now that the machine has been configured accepting any traffic, select “Review + create” to Validate the VM. Then select “Create” to deploy the VM. 

![image](https://github.com/user-attachments/assets/274c00f6-14c3-40b4-b385-cebe2d43e317)

Next I will want a way to view the data from this VM. To do this at the top type “Log analytics workspace”

![image](https://github.com/user-attachments/assets/eaeaeaf8-372e-4ed4-a70c-bc7f7ba3a10c)

Then select “Create log analytics workspace”

![image](https://github.com/user-attachments/assets/1a862877-b89c-4929-89ed-9ee9d2984af5)

Select the resource group made previously.

![image](https://github.com/user-attachments/assets/f38179d7-cd5e-4df8-bfdb-f979c6257be7)

Then name it, and confirm the region is the same, as previously used.

![image](https://github.com/user-attachments/assets/007c96fc-9445-4d2d-82c4-76ebc9f1696c)

Once complete select “Review + Create”.

![image](https://github.com/user-attachments/assets/89560f8a-041e-4019-8232-c8a84e98b38c)














