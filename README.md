# SIEM_Home_Lab

# Description:

This walkthrough details how I utilized Microsoft Azure to create a Windows 10 virtual machine in the cloud. I exposed the VM to the internet and employed Azure Log Analytics Workspace, Microsoft Defender for Cloud, and Azure Sentinel to collect and aggregate attack data, which was then visualized on a map in Microsoft Sentinel. The project demonstrates the integration of several tools and resources. I used PowerShell to scan the Event Viewer on the exposed VM, focusing on EventID 4625 (failed logon attempts), and logged that data. Additionally, the PowerShell script sends the IP addresses of failed logon attempts to IPgeolocation.io via an API, allowing Microsoft Sentinel to map the origin of these attempts.

This project was a valuable opportunity to gain hands-on experience with SIEMs, cloud technologies, APIs, and Microsoft Azure. Through this, I learned how to provision and configure cloud resources, analyze SIEM logs, and more. It was an enjoyable and educational experience, and I hope readers will appreciate the effort involved in completing this project.
Azure Sentinel SIEM Home Lab

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Utilities:
- Microsoft Azure
- Windows 10
- Microsoft Sentinel (SIEM)
- Log Analytic Workbooks
- Microsoft Defender for Cloud
- Virtual Machines
- Remote Desktop
- PowerShell
- API's
- Event Viewer
- Firewalls

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Walkthrough:

First I'm going to set up an Azure account. This will be the cloud environment used in this case.

Head to this link: [https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account?icid=azurefreeaccount](url)

Then click “Try Azure for free”, enter personal information.

This will give me a free $200 credit with Azure Cloud enviorment. Which wont be close to used by the end of this lab.

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

Now I have a log analytics workspace. Next I want the ability for logs to be gathered from my VM.

To do this, in the search bar at the top type “Microsoft Defender for Cloud”.

![image](https://github.com/user-attachments/assets/3618aa17-6ef3-445c-b20b-9127ea2a4037)

![image](https://github.com/user-attachments/assets/2d5d35b3-777f-4b0a-b4c7-d8bcbd7e13f2)

Next scroll down to and select “Environment settings”.

![image](https://github.com/user-attachments/assets/05be5de3-f17f-46d6-a584-a14b75722712)

Then expand the arrows to get to the log analytics workspace. Then Select it.

![image](https://github.com/user-attachments/assets/969136cd-84bf-4340-9015-6bbc74cc0cab)

Once inside settings, select Servers On and leave SQL servers off, since nothing will be used with SQL for this use case. 

Then select “Save”

![image](https://github.com/user-attachments/assets/5a692e4a-af24-4048-a725-c3523ec4f2a3)

Once saved select “Data collection” to the left.

![image](https://github.com/user-attachments/assets/419e3fb4-8f5f-4997-be08-9cd1e4d9326f)

Select “All Events” since I want all data to be collected. Then click “Save”.

![image](https://github.com/user-attachments/assets/665c6482-de94-4c27-8d08-20e64a064586)

Now heading back to log analytics in the top search bar. I will select my workspace. Select “Virtual machines”. Lastly, select my VM.

![image](https://github.com/user-attachments/assets/48ded32d-8a74-44d0-972d-3d1c4cf8a669)

Then I will select “Connect”, this is connecting the VM to the workspace.

![image](https://github.com/user-attachments/assets/a8ef45ae-d348-4d45-8ee4-139dc3d03d7d)

Next I want to visualize the inbound data. For that I want a SIEM. Microsoft Sentinel will be used in this use case.

To do this, in the search bar at the top type “Sentinel”.

![image](https://github.com/user-attachments/assets/47b03c0a-1856-403e-865b-a11b492454b6)

Select “Create Microsoft Sentinel”

![image](https://github.com/user-attachments/assets/33f79a30-b3d1-44e2-b2e2-4184db42a90a)

Select the log analytics workspace you want to connect to Sentinel. Then select “Add”

![image](https://github.com/user-attachments/assets/6324d179-10ac-4a39-8415-ba927b2d72be)

After Sentinel is set up and connected. I head back to the new Virtual machine.

To do this, type “Virtual machines” in the top search bar. And select new VM.

Inside of the new VM locate the public IP address. Copy to clipboard, this will be used to RDP to this VM from home device.

![image](https://github.com/user-attachments/assets/c2ca48fa-257b-46f2-8ec3-9f9f5de230ce)

Next select the start button on home device.

Type and select “Remote Desktop Connection”

![image](https://github.com/user-attachments/assets/1636b4da-1674-479f-af8d-f6789ba93586)

Paste the public IP address. Then select “Connect”.

![image](https://github.com/user-attachments/assets/dc86e025-e319-41d3-adf4-c44136e10a4d)

Select “More choices” to use a different account and enter the credentials used previously to create the account.

![image](https://github.com/user-attachments/assets/5dc60a2e-0174-493e-ab59-f57b7072bf83)

Then accept the certificate warning.

After this step you will connect to the VM, this might take a few minutes.

![image](https://github.com/user-attachments/assets/d41a8e7a-6ade-4440-aaf8-d2a21ceb1eb8)

Once inside the VM, confirm by looking at the top of the screen. It will show the IP address.

Then select no for all privacy settings, they are not important to this use case. Select “Accept” once done.

![image](https://github.com/user-attachments/assets/d46831c3-9dbb-496f-be92-10df927bc9f8)

Next to view logs type “Event Viewer” in the search bar in the bottom left of the screen.

![image](https://github.com/user-attachments/assets/38d9a094-f760-489d-9147-0be14ac8c3d0)

Next I want to see if my new VM is able to receive ICMP echo requests (Ping command). I am wanting this to be true so the VM can be easily discoverable by attackers.

First minimize the VM. Then in the home computer open the command line by typing “cmd” in the search bar in the bottom left of the screen.

![image](https://github.com/user-attachments/assets/175b4f9a-69e7-4058-b461-02f81cbe3345)

I can test to see if the VM is able to receive ICMP echo requests by using the Ping command. 

In the command line of your home computer type “ping -t [VM’s IP address]” hit “enter” on the keyboard.

The -t used with the ping command makes it perpetual, meaning it will continuously ping the VM.

![image](https://github.com/user-attachments/assets/4702180e-2f58-4be7-982a-5d9af401c8fe)

I received “request timed out” meaning the new VM cant receive ICMP echo request from the public.

![image](https://github.com/user-attachments/assets/37e362c3-e3c5-472d-8a15-6f933ce67b0f)

To fix this, go back to the VM. Type and select “wf.msc” in the search bar in the bottom left of the VM.

![image](https://github.com/user-attachments/assets/bbcc9fe2-f286-4d31-a074-ecb978e2c08d)

For this use case I am going to turn off the firewall, this is not recommended. I am doing this to be able to receive traffic from anyone in the public.

To do this, inside of Windows Defender Firewall. Select “WIndows Defender Firewall Properties”.

![image](https://github.com/user-attachments/assets/15633b78-b5c6-4f37-8295-0326e7dea4f3)

Next select the arrow connected to “Firewall state” and turn it off for Domain, Private, and Public Profile. Then select “Apply”, then “Ok”

![image](https://github.com/user-attachments/assets/8475a0ef-0888-4905-a043-1c5b52140e3b)

Now that the firewall is off, I will test the Ping command again from my home computer.

If responses are returning, turning off the firewall was successful. 

![image](https://github.com/user-attachments/assets/fad36289-78ac-490b-910b-9e2673d3e8eb)

Back in the new VM, the next step will be to get a powershell script to parse out Windows Event Logs for Failed RDP attacks. This powershell script will also use an API to collect the geographic information.

You can find the API used here: https://ipgeolocation.io/

In the VM type and select “PowerShell ISE”. This is more of a GUI than normal Powershell, making it easier for code.

![image](https://github.com/user-attachments/assets/8822ab08-2f3b-4460-91f9-76052f4ce4f8)

Copy the Script provided.

Select the “New Script” in the top left, paste the copied script.

The only thing needed to be changed for this script to work is, on line 2 you would have to get an API key from the provided link, and input it on line 2 in the "".
(Will show how to get API key in next few steps)

![image](https://github.com/user-attachments/assets/e85db21c-58c9-43c6-abe7-a99d89e535ce)

Select “File”, then “Save as”. Save to anywhere it can be accessed.

![image](https://github.com/user-attachments/assets/aa631427-696a-4fae-8958-41c8efd8ff87)

Next the only thing needed to make this script work, is an API key from https://ipgeolocation.io/

This step will need you to create an account if you don't already have one. Create an account if needed.

![image](https://github.com/user-attachments/assets/bd374b11-5724-44e0-b3a3-19d0a126e811)

Once an account is created, go to the dashboard and view your API key.

![image](https://github.com/user-attachments/assets/895971db-5ab0-4c80-84fb-7bafbcd83f63)

Then back in the script in the VM, on line 2 replace the string given for your new API key.

![image](https://github.com/user-attachments/assets/2dc83c5b-d669-4116-81a4-af3b0be81f1f)

Next I will test the script. To do this, select “Run Script”. 

The purple text is the API working and outputting the data to a new hidden folder.

![image](https://github.com/user-attachments/assets/5640f394-fcae-4f29-8659-4c6c79cf46de)

To view this data in the hidden folder, copy the file location.

![image](https://github.com/user-attachments/assets/be28a3e2-7e43-4293-85a0-77bbaa90aaea)

Click the windows key + the “r” button, or:

Type and select “Run” in the bottom left search bar.

![image](https://github.com/user-attachments/assets/13f6e22d-9d40-4387-a119-8f923f99dc27)

Paste file location.

![image](https://github.com/user-attachments/assets/2f003405-fc23-4646-baa9-21dcef84de82)

Open “failed_rdp” 

![image](https://github.com/user-attachments/assets/f0246a80-eb46-497e-bddb-013d0a8104fd)

The first 10ish are sample hosts for the script and API.

After those I can view all outputs from the script.

![image](https://github.com/user-attachments/assets/350d8a41-fcf9-451f-bbb0-71f90b3c14b7)

Next I want a way to input this data with the geo location info to the log analytics workspace.

To do that, I need to create a custom log rule. 
Go back to Microsoft Azure on the home computer. Type “Log Analytics workspaces” in the search bar at the top. 

Select the workspace.

Select the arrow connected to “Settings”

![image](https://github.com/user-attachments/assets/ee57a8a9-3f92-4487-abea-950ff58aff7d)

Select “Tables”.

Select the arrow connected to “Create”.

Select “New custom log (MMA-based)”

![image](https://github.com/user-attachments/assets/7bc4b220-7629-4b32-b9e6-2e175b630283)

To get the sample log from the VM to the home computer.

Click the windows key + the “r” button in the VM. And paste file location: C:\ProgramData

Open “failed_rdp”.

![image](https://github.com/user-attachments/assets/89705f6d-924c-47ad-aff3-61579a03f6b6)

Click: Ctrl + a to select all, then Ctrl + c to copy.

Back on the home computer, open notepad and click Ctrl + v to paste.

![image](https://github.com/user-attachments/assets/1d368c9c-56d1-4e1a-bf93-78813384e579)

Select “File”,  “Save as” to an accessible location.

Back in the Custom log page, select the new file just created.

![image](https://github.com/user-attachments/assets/c5e630c0-f5d1-4a65-82a0-2ae6d9df4532)

Select “Next”

![image](https://github.com/user-attachments/assets/3b81512d-dcab-49a6-b37b-eed4fd1772cd)

Select “Next”

Collection path: is the machine where this data is being collected. Type: select “Windows”. Path: will be the path to the data output by the script. C:\ProgramData\failed_rdp.log\

Then select “Next”

![image](https://github.com/user-attachments/assets/8fb96df6-ba5e-41a1-9a66-a31a560ffe65)

Name new custom log name.

![image](https://github.com/user-attachments/assets/67110760-1347-42f1-be66-9e8c93d6f10f)

Confirm info is correct and select “Create”

![image](https://github.com/user-attachments/assets/ed360b67-ca12-4509-9bc1-948070b27cce)

Back in the workspace, select “Logs” to see if the custom rule is working.

Type name of custom rule just made.

![image](https://github.com/user-attachments/assets/04d695d0-42cd-4155-8ba2-4ce1b03c0279)

Then select “Run”

If no data is loading give it ~10 minutes. Eventually it will show.

![image](https://github.com/user-attachments/assets/872539f5-ca10-4a5a-b0ca-b7aee14b3eb2)

Now I have the raw data, but I want the columns of this data to show whats in the raw data, in a more understandable way.

To do this, use the Custom_KQL_Query code in KQL Query mode. I used an old not working query in this picture but solve this issue later.

![image](https://github.com/user-attachments/assets/efd7627d-75f1-4c34-8365-5c5f8061c6e0)

This gives me clean columns with everything labeled correctly.

Now that I have all the data needed to know where everyone is, and when trying to connect to my VM. I want a map to better visualize this data.

To do this, type “ Microsoft Sentinel” in the top search bar in Microsoft Azure.

Then select “Workbooks” in the “Threat Management drop down”.

![image](https://github.com/user-attachments/assets/ef34f8d1-f22b-42be-a48e-afff99e0e95d)

Select “Add Workbook”

![image](https://github.com/user-attachments/assets/c9e43e91-545e-459d-8d19-c6e91aac0f5e)

Select “Edit”

![image](https://github.com/user-attachments/assets/1adcc65a-b15f-4ef1-8f54-386088d54e56)

Select the … to the right of the 2 widgets and select “Remove”.

![image](https://github.com/user-attachments/assets/9dbe8773-de39-4ff3-b89c-d60a5972c9bd)

Then select the arrow attached to “Add”, select “Add query”

![image](https://github.com/user-attachments/assets/3ad36757-dd12-4728-892f-56c48696a6cc)

In the new workbook use the Custom_KQL_Query to get the correct data, for the map.

I discussed with Chat GPT on how to make this and tweak it till this Query worked perfectly. 

Make sure to change the “Visualization” tab to “Map”. Then select “Map Settings”

![3](https://github.com/user-attachments/assets/a8df76b8-8abe-46c8-bbda-b6ec3168bd87)

For all Map Settings its best to tweak for each use case, these settings worked great for mine.

For Layout Settings:

![image](https://github.com/user-attachments/assets/9570b96b-f3b1-4502-b929-0ffb4c969d9b)

For color I want a heatmap to show where the most attempts are coming from.

For Color Settings:

![image](https://github.com/user-attachments/assets/c626a9b2-1db4-427c-a585-49fed1a729c8)

For Metric Settings:

![image](https://github.com/user-attachments/assets/f7af2638-0f06-47ed-ad24-51cee41afbe4)


Now with all the correct settings I have a map that shows the location of all RDP logon attempts that have failed. With correct colors showing “hot” areas with many attempts. Also A counter at the bottom to show the number of attempts.

![image](https://github.com/user-attachments/assets/dd7ff691-386b-42fd-8e0f-0bb73c48c7d7)

Once happy with how the map looks select “Save and close”.

Then make sure to select the “Save” at the top to save to the workbook.

Enter a name, then fill the rest with your resources.

![image](https://github.com/user-attachments/assets/06994fa9-5b75-4ac7-8341-682db909405f)

Conclusion:

During this project, I set up a virtual machine (VM) to monitor brute-force login attempts from around the world. Over the course of several hours, I observed increasing login attempts from various countries. The data illustrated how vulnerable open services like Remote Desktop Protocol (RDP) can be, as even a non-prominent VM attracted thousands of login attempts. I learned the importance of implementing strong passwords and avoiding common usernames, as these were frequently targeted by attackers. The value of multi-factor authentication (MFA) and restricted access to services was also highlighted as essential security practices. Analyzing the login attempts reinforced the need for continuous monitoring and quick response to threats. Overall, this hands-on experience deepened my understanding of cybersecurity risks and defenses in a practical context.

FYI: Only $3 was used setting all this up, and using all 1000 free API credits for the day.

This allows for anyone to go back and practice creating this and familiarizing yourself with the envirment, applications and administration of all steps for future use in a company.






























