# Microsoft Sentinel (SIEM) Lab

## Description

Microsoft Sentinel is a cloud based security information event management (SIEM) tool that helps organizations detect, investigate, and respond to threats. In this lab, I will show step-by-step how to set up Microsoft Sentinel and use it to observe real-time attacks (RDP Brute Force) against a virtual machine. I will also use a PowerShell script to create a custom log file with each attacker’s geolocation. Finally, I will set up a map to visualize this information in Microsoft Sentinel.

## Table of Contents

   * [Languages and Utilities Used](#Languages-and-Utilities-Used)
   * [Environments Used](#Environments-Used)
   * [Part 1: Create a Free Azure Account](#Part-1-Create-a-Free-Azure-Account)
   * [Part 2: Create a Virtual Machine](#Part-2-Create-a-Virtual-Machine)
   * [Part 3: Create a Log Analytics Workspace](#Part-3-Create-a-Log-Analytics-Workspace)
   * [Part 4: Set Up Microsoft Defender](#Part-4-Set-Up-Microsoft-Defender)
   * [Part 5: Connect the Virtual Machine to Your Log Analytics Workspace](#Part-5-Connect-the-Virtual-Machine-to-Your-Log-Analytics-Workspace)
   * [Part 6: Set Up Microsoft Sentinel](#Part-6-Set-Up-Microsoft-Sentinel)
   * [Part 7: Connect to the Virtual Machine](#Part-7-Connect-to-the-Virtual-Machine)
   * [Part 8: Disable Windows Defender Firewall on the Virtual Machine](#Part-8-Disable-Windows-Defender-Firewall-on-the-Virtual-Machine)
   * [Part 9: View Failed Logons in Event Viewer](#Part-9-View-Failed-Logons-in-Event-Viewer)
   * [Part 10: Create a Custom Log File Using PowerShell](#Part-10-Create-a-Custom-Log-File-Using-PowerShell)
   * [Part 11: Create a Custom Table in Your Log Analytics Workspace](#Part-11-Create-a-Custom-Table-in-Your-Log-Analytics-Workspace)
   * [Part 12: Query the Custom Table](#Part-12-Query-the-Custom-Table)
   * [Part 13: Create a World Map Workbook in Microsoft Sentinel](#Part-13-Create-a-World-Map-Workbook-in-Microsoft-Sentinel)

## Languages and Utilities Used

* **Microsoft Sentinel** 
* **PowerShell**
* **ipgeolocation.io**
* **Kusto Query Language (KQL)**

## Environments Used

* **Microsoft Azure**
* **Windows 10**

## Walk-through

### Part 1: Create a Free Azure Account

1. Go to https://azure.microsoft.com/en-us/free/ in your web browser, and click **Start free**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/f1a499e8-ce41-466e-b728-50f7493173f0" height="80%" width="80%"/>
</br>
</br>

2. Create or sign in with a Microsoft account.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/b1105114-239b-4fdf-bd78-828dc89b6bbe" height="80%" width="80%"/>
</br>
</br>

### Part 2: Create a Virtual Machine

1. Once you’re logged into Azure, type **Virtual machines** in the search box at the top of the page. Then select **Virtual machines** listed under **Services**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/4cdd3648-6040-4995-a92c-40cd89f9db9f" height="80%" width="80%"/>
</br>
</br>

2. Click **Create**, and select **Azure virtual machine**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/24a405ad-0c78-49c2-bf37-2a803505a633" height="80%" width="80%"/>
</br>
</br>

3. Next to **Resource group**, click **Create new**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/21fc764d-611a-4d67-8a61-d060a6eb4471" height="80%" width="80%"/>
</br>
</br>

4. Name it **Honeypotlab**, and click **OK**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/01cd5026-1e8b-4790-8365-c78de6906f6d" height="80%" width="80%"/>
</br>
</br>

5. Next to **Virtual machine name**, type in **honeypot-vm**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/6bb604d8-18be-4250-b356-415f46c3e2b0" height="80%" width="80%"/>
</br>
</br>

6. Next to **Image**, select **Windows 10 Pro**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/27607b0d-b76c-4ce0-8559-bb33fd35431e" height="80%" width="80%"/>
</br>
</br>

7. Next to **Size**, select **Standard_DS1 - vcpu, 3.5 GiB memory**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/84b04b88-90bd-4c63-9529-4d219cb5c3fd" height="80%" width="80%"/>
</br>
</br>

8. Under **Administrator account**, type in a username and password you will use to log in to the virtual machine.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/2725c032-d947-4060-af6b-60e9c890d946" height="80%" width="80%"/>
</br>
</br>

9. Under **Licensing**, check the box next to **I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/5f8993ac-729c-4684-8140-f47b6a6672a1" height="80%" width="80%"/>
</br>
</br>

10. Click **Next** until you reach the **Networking** tab.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/4488ea9a-3c9a-4d2d-9feb-54f868b8db3e" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/a5008313-a0ac-4a8b-ba79-7b0db981b566" height="80%" width="80%"/>
</br>
</br>

11. Next to **NIC network security group**, select **Advanced**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/5a8e1976-d54f-481f-bc4d-996de0fe74d2" height="80%" width="80%"/>
</br>
</br>

12. Next to **Configure network security group**, click **Create new**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/b6e065ea-825a-4678-ba3f-56552cd303d1" height="80%" width="80%"/>
</br>
</br>

13. Under **Inbound rules**, click the three dots next to the default rule, and select **Remove**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/ce9ceb9f-177c-4e7c-8f14-5ef1061fd2a9" height="80%" width="80%"/>
</br>
</br>

14. Click **+ Add an inbound rule**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/391aef57-7f5f-4917-b7ef-d0130a62ebd1" height="80%" width="80%"/>
</br>
</br>

15. Under **Destination port ranges**, type "*" to select all ports.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/9e58942f-7df8-48e6-b510-b7cee68b927b" height="80%" width="80%"/>
</br>
</br>

16. Under **Priority**, type **100**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/f8759cea-5d5a-4fe3-bc4d-5adda721865f" height="80%" width="80%"/>
</br>
</br>

17. Under **Name**, type **DANGER_ANY_IN**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/7346abc4-2165-42b1-bf81-69c97f980ecd" height="80%" width="80%"/>
</br>
</br>

18. Click **Add**, and click **OK**. 

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/ab5cb565-98af-458c-aba4-510f0f0d3202" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/fbce371a-44d2-43b9-a456-6d1d6b9c1149" height="80%" width="80%"/>
</br>
</br>

19. Click **Review + create**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/e36ccdab-c5c9-4768-9a84-350cbf9d5280" height="80%" width="80%"/>
</br>
</br>

20. Click **Create**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/94f6f255-6ecb-4090-b65c-4152ae4d64ce" height="80%" width="80%"/>
</br>
</br>

### Part 3: Create a Log Analytics Workspace

1. Type **log analytics** into the search box at the top of the page, and select **Log Analytics workspaces** listed under **Services**.

<img src="" height="80%" width="80%"/>
</br>
</br>

2. Click **Create log analytics workspace**.

<img src="" height="80%" width="80%"/>
</br>
</br>

3. Next to **Resource group**, select **Honeypotlab**.
4. Next to **Name**, type in **law-honeypot**.
   * I had to name it **law-honeypot4** because I did the lab multiple times.
5. Next to **Region**, select **West US 3**.
6. Click **Review + Create**.
7. Click **Create**.

### Part 4: Set Up Microsoft Defender

1. Type **defender** in the search box at the top of the page, and select **Microsoft Defender for Cloud** listed under **Services**.
2. From the left menu options, select **Environment settings**.
3. Click the dropdown arrow next to **Azure subscription 1**, and select **law-honeypot**. 
4. Under **Plan**, turn on **Foundation CSPM** and **Servers**. Then click **Save**.
5. Select the **Data collection** tab.
6. Select **All Events**, and click **Save**.

### Part 5: Connect the Virtual Machine to Your Log Analytics Workspace

1. Type **log analytics** into the search box at the top of the page, and select **Log Analytics workspaces** listed under **Services**.
2. Click **law-honeypot**.
3. From the left menu options, select **Virtual machines**.
4. Click **honeypot-vm**.
5. Click **Connect**.

### Part 6: Set Up Microsoft Sentinel

1. Open a new tab in your web browser.
2. Go to https://portal.azure.com/.
3. Type **sentinel** in the search box at the top of the page, and select **Microsoft Sentinel** listed under **Services**.
4. Click **Create Microsoft Sentinel**.
5. Under **Workspace**, select **law-honeypot**, and click **Add**.

### Part 7: Connect to the Virtual Machine

1. Click in the search box at the top of the page, and select **Virtual machines** listed under **Recent services**.
2. Click **honeypot-vm**.
3. Under **Public IP address**, copy the IP address of the virtual machine.
4. Click the **Start**, and run **Remote Desktop Connection**.
5. Next to **Computer**, paste in the IP address of the virtual machine, and click **Connect**.
6. Click **More choices**, and select **Use a different account**.
7. Type in the username and password you created for the virtual machine, and click **OK**.
8. Check the box next to **Don’t ask me again for connections to this computer**, and click **Yes**.
9. On the **Choose privacy settings for your device** screen, set all options to **No**, and click **Accept**.
10. Click **Yes** when asked “Do you want to allow your PC to be discoverable by other PCs and devices on this network?”

### Part 8: Disable Windows Defender Firewall on the Virtual Machine

1. Click **Start** on your physical computer, and run **Command Prompt**.
2. Enter the the following command:
  ```
  ping <virtual machine IP address> -t
  ```
  * The ping request will time out because Windows Defender Firewall is blocking connections between your physical computer and the virtual machine.
3. Go back to the virtual machine, click **Start**, and open **Windows Defender Firewall**.
   * Type **wf.msc** to go directly to the advanced settings.
4. Click **Windows Defender Firewall Properties**.
5. Go through the **Domain Profile**, **Private Profile**, and **Public Profile** tabs, and set the **Firewall state** to **Off**.
6. Click **Apply** and **OK**. 
7. Go back to **Command Prompt** on your physical computer.
   * The ping request should now be receiving replies back from the virtual machine.
8. Click **Close** to exit **Command Prompt**.

### Part 9: View Failed Logons in Event Viewer

1. Go back to the virtual machine, click **Start**, and open **Event Viewer**.
2. Click the dropdown arrow next to **Windows Logs**, and select **Security**.
3. Click **Start** on your physical computer, and open **Remote Desktop Connection**.
4. Try to log in using a fake username and password.
   * You will see a message that says “Your credentials did not work”.
5. Go back to the virtual machine, right click inside **Event Viewer**, and click **Refresh**.
6. Find the entry with **EventID 4625**, and double click it to view the **Event Properties**.
   * This window will show you different information about the security event, such as the account name that was used, the failure reason, and the source network address.

### Part 10: Create a Custom Log File Using PowerShell

1. Click **Start** on the virtual machine, and open **Windows PowerShell ISE**.
2. Click **New Script**.
3. Open **Microsoft Edge**, and go to the PowerShell script using the following link: https://github.com/emann615/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1
4. Copy the script, and paste it into **PowerShell**.
5. Go to the following link in **Microsoft Edge**: https://ipgeolocation.io/
6. Click **Get Free API Access**.
7. Fill out the name, email, and password information, and click **Sign Up**.
   * You can also sign up using a Google or GitHub account.
8. Once you are logged in, copy the API key.
9. Paste the API key into the Powershell script next to **$API_KEY**.
10. Save the PowerShell script under the name **Log_Exporter**.
11. Click **Run Script**.
    * The script will take failed RDP events from Windows Event Viewer and use the API key to find the geolocation. Then it will output that information into a file named **failed_rdp.log**.

<img src="" height="80%" width="80%"/>
</br>
</br>

12. Perform some more failed logons to see them added to the list.
    * You can find the failed_rdp.log file by opening **File Explorer** and pasting in the following directory path: **C:\ProgramData**
      * **File format:** latitude, longitude, destination, username, source, state, country, label, datetime

<img src="" height="80%" width="80%"/>
</br>
</br>

### Part 11: Create a Custom Table in Your Log Analytics Workspace

1. Open the **failed_rdp.log** file, and copy all the information.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/0742fc50-3102-40be-a6dd-46f1bee426d1" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/2e86f678-0ce6-430d-81cc-99e8742f4bec" height="80%" width="80%"/>
</br>
</br>

2. Go back to your physical computer, and create a new text document using **Notepad**.

3. Paste the information from the **failed_rdp.log** file into the Notepad text document.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/6b4a9868-60d1-4727-af1d-f642d0da6931" height="80%" width="80%"/>
</br>
</br>

4. Save the file to the **Desktop** folder of your physical computer under the name **failed_rdp**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/1892d93d-8540-4df8-bdaf-2fab58a32aea" height="80%" width="80%"/>
</br>
</br>

5. Go back to the log analytics workspace you created in Microsoft Azure named **law-honeypot**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/e548c7bf-93b6-4f55-99df-8f5c8e685a38" height="80%" width="80%"/>
</br>
</br>

6. Select **Tables** from the left menu options.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/32c16676-508f-4a65-bf77-e4bbd726cb4d" height="80%" width="80%"/>
</br>
</br>

7. Click **Create**, and select **New custom log (MMA-based)**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/0c5eb1cb-3ad4-4d46-9159-42b550fce742" height="80%" width="80%"/>
</br>
</br>

8. Next to **Select a sample log**, click **Select a file**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/2c773d5b-df10-4dd6-b6a8-3622a99170fb" height="80%" width="80%"/>
</br>
</br>

9. Select the **failed_rdp** file you saved to the **Desktop** folder, and click **Open**.  

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/dedbd2ec-327a-441a-8195-67eae886e209" height="80%" width="80%"/>
</br>
</br>

10. Click **Next**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/601334c7-d1b1-411a-a4d5-f4ef4a36a383" height="80%" width="80%"/>
</br>
</br>

11. Make sure the information under **Records** looks correct. Then click **Next**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/a7942b38-bb93-4d22-a7c4-5d59e77321f1" height="80%" width="80%"/>
</br>
</br>

12. Under **Type**, select **Windows**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/45de51e9-6d56-4caf-845e-d826412c0340" height="80%" width="80%"/>
</br>
</br>

13. Under **Path**, type in the path to the **failed_rdp.log** file on the virtual machine. Then click **Next**.
    * Path: **C:\ProgramData\failed_rdp.log**

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/ae397902-d2f9-4253-b6b9-1acfbd29e172" height="80%" width="80%"/>
</br>
</br>

14. In the box next to **Custom log name**, type **FAILED_RDP_WITH_GEO**. Then click **Next**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/7b0ea1db-0616-4181-89ce-e378b1429b66" height="80%" width="80%"/>
</br>
</br>

15. Click **Create** to create the custom table.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/bacfc6bd-2c9c-490a-b673-94533f8ee49d" height="80%" width="80%"/>
</br>
</br>

### Part 12: Query the Custom Table

1. Select **Logs** from the left menu options.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/8cde1055-22f0-45e5-9e29-939c9b8a447b" height="80%" width="80%"/>
</br>
</br>

2. Exit the **Queries window**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/366cf456-b8e7-4b45-a245-d61c3dd4ee73" height="80%" width="80%"/>
</br>
</br>

3. Type in **FAILED_RDP_WITH_GEO_CL**, and click **Run**.
   * If no results are found, you may need to wait 15-20 minutes. Then try to run the query again.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/2261c0f7-5fac-41b9-a4b5-8af240c6cccf" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/9e379d02-f5c6-4661-8e0e-45b36e9c3f4b" height="80%" width="80%"/>
</br>
</br>

4. Once the query starts receiving information, view the items listed under **Results**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/ce6468c5-b857-4026-9067-cdf307afe4bf" height="80%" width="80%"/>
</br>
</br>

5. Check the **RawData** column to make sure it has all the information that is being collected in the failed_rdp.log file on your virtual machine.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/47472fab-1389-4a5e-b7de-a1319ad15877" height="80%" width="80%"/>
</br>
</br>

6. You can perform some more failed logons, and run the query again to see that the new logs are added to the results.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/1bdee649-e660-4136-8166-915aee0ec644" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/16bfe519-f5be-4d99-8dc4-4ec6ed3392a2" height="80%" width="80%"/>
</br>
</br>

### Part 13: Create a World Map Workbook in Microsoft Sentinel

1. Click the search box at the top of the page, and select **Microsoft Sentinel** listed under **Recent services**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/91487318-75ef-40b6-a3a4-3ef2d8f081f4" height="80%" width="80%"/>
</br>
</br>

2. A pop up will appear that says “Your unsaved edits will be discarded”. Click **OK**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/f28bfc9c-fd6e-4ddb-9f23-0aea7bd982fd" height="80%" width="80%"/>
</br>
</br>

3. Click **law-honeypot**.
   * If you click the toggle next to **New overview**, you can switch between the old overview layout and the new overview layout.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/4d0ef6b3-6b6e-4b63-a7ce-3d70adbb669f" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/c5e0648b-7b58-4543-bbb5-9b13a7f238b0" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/954619b5-7733-4b81-83f2-470ae8b472ae" height="80%" width="80%"/>
</br>
</br>

4. Select **Workbooks** from the left menu options.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/dc059283-5a00-453d-8d64-f1d38d472ea5" height="80%" width="80%"/>
</br>
</br>

5. Click **Add workbook**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/b977a591-6c09-4385-8e0b-708859d4ece5" height="80%" width="80%"/>
</br>
</br>

6. Click **Edit**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/3c9e214d-d960-4ace-a422-dfa9b08778d0" height="80%" width="80%"/>
</br>
</br>

7. Delete the default queries by clicking the three dots next to **Edit** on the right side, and selecting **Remove**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/05c62180-9e17-41a9-bc5d-d530b75fbf87" height="80%" width="80%"/>
</br>
</br>

8. In the pop up that asks “Remove query?” click **Yes**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/33de3740-f5a9-482f-9ce4-e5dcce0d629b" height="80%" width="80%"/>
</br>
</br>

9. Repeat **steps 7-8** to remove the second default query.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/deecbcea-4cd2-472d-915d-a7c59adafa11" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/bb913600-90f9-4a92-94ea-440a82a1ce3a" height="80%" width="80%"/>
</br>
</br>

10. Click **Add**, and select **Add query**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/4cceda36-ccc8-4ae6-80a8-ee8eee810b90" height="80%" width="80%"/>
</br>
</br>

11. Copy and paste the following query:
```
FAILED_RDP_WITH_GEO_CL
| extend CSVFields  = split(RawData, ',')
| extend Latitude = tostring(CSVFields[0])
| extend Longitude = tostring(CSVFields[1]) 
| extend Destination = tostring(CSVFields[2]) 
| extend Username = tostring(CSVFields[3])
| extend Source = tostring(CSVFields[4])
| extend State = tostring(CSVFields[5])
| extend Country = tostring(CSVFields[6])
| extend Label = tostring(CSVFields[7])
| extend DateTime = todatetime(CSVFields[8])
| summarize event_count=count() by tostring(CSVFields[4]), tostring(CSVFields[0]), tostring(CSVFields[1]), tostring(CSVFields[6]), tostring(CSVFields[7]), tostring(CSVFields[2])
| where CSVFields_2 != "samplehost"
| where CSVFields_4 != ""
```

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/56ac910d-f59e-411f-aafe-7bbe4bf830c1" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/b5428458-31e0-4010-b60f-ec6de9c2dfea" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/f755ce23-416a-49f8-9cc1-4e078fc9151d" height="80%" width="80%"/>
</br>
</br>

12. Click **Run Query**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/79a6b2bb-8312-4c34-be9e-513d870e6d42" height="80%" width="80%"/>
</br>
</br>

13. Under **Visualization**, click the dropdown arrow, and select **Map**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/22a2518e-db7f-465b-8cfa-b2de1b539de3" height="80%" width="80%"/>
</br>
</br>

14. Add the following settings:
    * **Location info using:** Latitude/Longitude
    * **Latitude:** CSVFields_0
    * **Longitude:** CSVFields_1
    * **Size by:** event_count
    * **Metric Label:** CSVFields_7
    * **Metric Value:** event_count

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/3f18fa26-adef-47dc-9abf-6d32d77ad8d1" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/af2247ad-f55c-45b7-9474-1418609ff854" height="80%" width="80%"/>
</br>
</br>

15. Click **Apply**. Then click **Save and Close** to save the map settings.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/b2646138-d1b7-46da-a7a5-baacfe84fa3f" height="80%" width="80%"/>
</br>
</br>

16. Click **Save** to save the query.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/ea8af3e8-118b-4ad6-ada6-782e78580e51" height="80%" width="80%"/>
</br>
</br>

17. Under **Title**, type **Failed RDP World Map**.

18. Under **Resource group**, select **Honeypotlab**.

19. Under **Location**, select **(US) West US 3**.

20. Click **Apply**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/6c8198bc-1cfe-4e7d-9743-44f73cfbdab6" height="80%" width="80%"/>
</br>
</br>

21. Click **Auto refresh**, and select **5 minutes**. Then click **Apply**.

<img src="https://github.com/emann615/ActiveDirectoryLab/assets/117882385/254ba068-c9d4-45fa-a4d0-e058db587c31" height="80%" width="80%"/>
</br>
</br>

22. Check this map throughout the day to see the number of failed RDP attempts and where they are coming from.
