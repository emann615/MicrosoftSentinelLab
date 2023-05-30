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
15. Under **Destination port ranges**, type ** * ** to select all ports.
16. Under **Priority**, type **100**.
17. Under **Name**, type **DANGER_ANY_IN**.
18. Click **Add**, and click **OK**. 
19. Click **Review + create**.
20. Click **Create**.

