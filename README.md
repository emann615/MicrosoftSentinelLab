# Microsoft Sentinel (SIEM) Lab

## Description
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed vitae mi nisi. Pellentesque et dui a enim cursus lobortis. Aliquam eleifend imperdiet libero, id imperdiet tellus aliquam in. Integer sed fringilla arcu, ac dignissim arcu. Suspendisse nec pellentesque turpis. Donec eget malesuada nunc. Morbi fermentum aliquet magna venenatis tincidunt. Ut imperdiet fermentum quam, malesuada pharetra orci. Quisque auctor ex et cursus aliquet.

<div style="page-break-after: always; visibility: hidden"></div>

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed vitae mi nisi. Pellentesque et dui a enim cursus lobortis. Aliquam eleifend imperdiet libero, id imperdiet tellus aliquam in. Integer sed fringilla arcu, ac dignissim arcu. Suspendisse nec pellentesque turpis. Donec eget malesuada nunc. Morbi fermentum aliquet magna venenatis tincidunt. Ut imperdiet fermentum quam, malesuada pharetra orci. Quisque auctor ex et cursus aliquet.
<br />

## Table of Contents

   * [Languages and Utilities Used](#Languages-and-Utilities-Used)
   * [Environments Used](#Environments-Used)
   * [Prerequisite](#Prerequisite)

## Languages and Utilities Used

* **Microsoft Sentinel** 
* **PowerShell**

## Environments Used

* **Microsoft Azure**
* **Windows 10**

## Walk-through

### Part 1: Create a Free Azure Account

1. Go to https://azure.microsoft.com/en-us/free/ in your web browser, and click **Start free**.
2. Create or sign in with a Microsoft account.

### Part 2: Create a Virtual Machine

1. Once you’re logged into Azure, type **Virtual machines** in the search box at the top of the page. Then select **Virtual machines** listed under **Services**.
2. Click **Create**, and select **Azure virtual machine**.
3. Next to **Resource group**, click **Create new**.
4. Name it **Honeypotlab**, and click **OK**.
5. Next to **Virtual machine name**, type in **honeypot-vm**.
6. Next to **Image**, select **Windows 10 Pro**.
7. Next to **Size**, select **Standard_DS1 - vcpu, 3.5 GiB memory**.
8. Under **Administrator account**, type in a username and password you will use to log in to the virtual machine.
9. Under **Licensing**, check the box next to **I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights**.
10. Click **Next** until you reach the **Networking** tab.
11. Next to **NIC network security group**, select **Advanced**.
12. Next to **Configure network security group**, click **Create new**.
13. Under **Inbound rules**, click the three dots next to the default rule, and select **Remove**.
14. Click **+ Add an inbound rule**.
15. Under **Destination port ranges**, type "*" to select all ports.
16. Under **Priority**, type **100**.
17. Under **Name**, type **DANGER_ANY_IN**.
18. Click **Add**, and click **OK**. 
19. Click **Review + create**.
20. Click **Create**.

### Part 3: Create a Log Analytics Workspace

1. Type **log analytics** into the search box at the top of the page, and select **Log Analytics workspaces** listed under **Services**.
2. Click **Create log** analytics workspace.
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
6. Click **More choices**.
7.Select **Use a different account**.
8. Type in the username and password you created for the virtual machine, and click **OK**.
9. Check the box next to **Don’t ask me again for connections to this computer**, and click **Yes**.
10. On the **Choose privacy settings for your device** screen, set all options to **No**, and click **Accept**.
11. Click **Yes** when asked “Do you want to allow your PC to be discoverable by other PCs and devices on this network?”

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
