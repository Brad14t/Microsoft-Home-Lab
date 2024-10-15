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













