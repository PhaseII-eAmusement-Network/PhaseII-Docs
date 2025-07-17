---
title: Getting Connected
layout: home
parent: Getting Started
---

# Getting Connected
This guide will walk you through the steps for requesting an arcade, configuring it, and connecting to the VPN.

## Requesting an Arcade
> *Please ensure that you have direct messages from BadManiac enabled before continuing.*

Wether you're running a private controller setup or a public arcade, you'll need to submit a PCBID request to BadManiac via `/pcbid-request` in the `#pcbid-request` channel. This will require the secret guide code.

BadManiac will provide an interactive prompt which will walk you through every step. Here's some general information on each step.

1. Arcade Type
- Choice between Arcade/Setup and Controller
  - > **Arcade/Setup:** An arcade with cabinets or a dedicated setup for one specific game
  - > **Controller:** One PCBID for a generic controller setup, where the game may change

2. Arcade Name
- Provide the name you'd like to call your arcade

3. (Cabinet/Dedicated Setup Only) Game selection
- Using the selector, pick which games you'll need PCBIDs for

4. Verification Image
- You're required to upload a photo of your cabinet, controller, or setup for verification

Once you've submitted the request, all moderators will recieve a notification. Once your arcade is approved, BadManiac will provide all details for your arcade and its machines.

## Connecting a Setup/Controller
This guide applies to setup and controller arcades that will connect to the VPN using software on the computer.

### VPN
The VPN client supported by PhaseII is [OpenVPN Community](https://openvpn.net/community/){:target="_blank"}. While OpenVPN Connect or another client may work, PhaseII provides no support for clients other than OpenVPN Community.

1. Install OpenVPN Community
   - OpenVPN will need Admin privileges to install
   - If asked, you need to approve the TAP Adapter installation
2. Import your OpenVPN Config
   - Your configuration will be provided by BadManiac
   - Usually, you can just open the OVPN file after downloading. You may need to right-click the tray icon and select `Import`
3. Connect
   - Once your configuration is imported, you may right-click the tray icon and select `Connect`
   - The connection window will open while it initializes the connection but should close once connected
> *Please note that you'll need to connect to the VPN every time you intend to use PhaseII. Please do not leave the VPN connected when not in use.*

You can test your connection by opening [http://xrpc.phaseii.network](http://xrpc.phaseii.network){:target="_blank"}. You should see "the price is wrong :p" (that's our OK message)

### Game Configuration
Once connected to the VPN, you can prepare your game configuration.

- SpiceTools:
  - You can set variables using `spicecfg` or via command line arguments
  - `-url <url> -p <pcbid>`
- Editing Configs:
  - You'll modify `prop/eamuse-config.xml` or `prop/ea3-config.xml`, depending on what you have. If you have both, edit both!
  - An example of how you'd configure this way:

```xml
<ea3>
  <id>
    <pcbid __type="str">YOUR PCBID</pcbid>
    <softid __type="str">YOUR PCBID</softid>
  </id>
  <!-- Ignore until <network> -->
  <network>
    <!-- There are usually commented out tags here! Be sure to edit the one that isn't commented. -->
    <services>http://xrpc.phaseii.network</services>
    <!-- Ignore this one! -->
    <services_localstrap></services_localstrap>
  </network>
...
```

After updating your configuration, start your game. If all goes well, you'll get `Network OK` during startup and eAmusement services will be available.

Please enter the test menu and proceed to `Network Settings` (`Shop Settings` for Jubeat and GFDM) and set your `Shop Name` to your arcade's registered name. Play your first game and enjoy!

Once you've logged out, proceed to [Getting Started - As a Player](/getting-started#as-a-player){:target="_blank"} to create your account. Once you've made your account, you may proceed to [Getting Connected - Claiming your Arcade](/getting-started/connecting#claiming-your-arcade)

## Connecting a Cabinet
This guide applies to official arcade cabinets that will connect via a GL.iNet router.

### Required Hardware
PhaseII only supports GL.iNet router hardware running stock firmware. While another routing solution may connect and work, we are unable to provide end-user support for other hardware/software. We may look into writing custom firmware for GL.iNet hardware in the future but it is not currently planned.

Nearly any GL.iNet router will work but here's a basic buying guide.
- [Mango (GL-MT300N-V2)](https://www.amazon.com/GL-iNET-GL-MT300N-V2-Repeater-300Mbps-Performance/dp/B073TSK26W){:target="_blank"}
  - Tiny, cheap, slow
  - This little router does the job, and that's about it. I wouldn't recommend running more than one or two cabinets at a time from this router as it gets overwhelmed quite easily.
  - Does not include a power supply, needs at least a 15W USB brick! Underpowered power supplies cause stability issues and can corrupt the router.
- [Opal (GL-SFT1200)](https://www.amazon.com/GL-iNet-GL-SFT1200-Secure-Travel-Router/dp/B09N72FMH5){:target="_blank"}
  - Small, often on sale, more than fast enough
  - The Opal is our personal favorite router for is great speed and reliability. This router often goes on sale for less than $40 and is well worth the money.
  - Includes a power supply, USB-C powered.

### Hooking it Up
- Your router will have at least two ethernet jacks, sometimes 3. The jacks will be labeled separately.
  - **WAN:** Connect this cable to your existing local network. This is the "Uplink" to the GL.iNet router.
  - **LAN:** Connect this either directly to a cabinet or to a network switch. You'll need to connect it to your computer for initial setup.
  > *If your router has 3 ports, two of them are LAN. They'll be labeled as LAN1 and LAN2, either works.*

> **Important Note:** *Please ensure that your router is connected to power at least 45 seconds **before** the cabinet, as they boot fairly slow. Routers may be left online, even when not in use.*

### Configuring the Router
GL.iNet routers ship with a basic default configuration to get them up and running. We'll need to log into the router and adjust some settings.
> ***NEVER** under **ANY** circumstance configure your router over WIFI. We'll be disabling the WIFI broadcasting in this guide. Configuring over WIFI can lead to configuration corruption and needing to de-brick your router using UBOOT*

1. Logging into the Router
   - Connect the router's LAN port to your computer
   - Open a web browser and navigate to [http://192.168.8.1](http://192.168.8.1){:target="_blank"}
     - This is the default IP for *most* GL.iNet routers. Yours may be different. The label on the bottom will tell you the defaults.
   - The router may ask for a default password, which should be `goodlife`
   - Create a new admin password following the prompts
2. Update Firmware
   > *Updating the firmware on your router is important to protect against security risks and connection issues*
   - Check for Updates:
     - Go to **System** > **Firmware Upgrade**
     - Click **Check for Updates** to find the latest GL.iNet firmware
   - Install the Firmware:
      - If an update is available, click **Install**
   - Log In Again:
      - After reboot, reconnect to the router at its IP (likely still `192.168.8.1`)
3. Configure LAN IP
   > *Certain titles will not connect if the IP address is in the `192.168` range, so we'll need to change that*
   - Go to **Network** > **LAN**
   - Set the LAN IP to `10.0.10.1` in the text box
   - Click **Apply**. The router will reconfigure its IP scheme (takes ~1-2 minutes)
   - Unplug and reconnect the LAN cable going to your computer
4. Reconnecting
   - After applying the new LAN IP and reseating the cable, you'll need to navigate to [http://10.0.10.1](http://10.0.10.1){:target="_blank"}
   - Log back in with your new password
5. Disable WIFI
   > *We do not want the router broadcasting a network for security and speed reasons*
   - Go to **Wireless** > **WI-FI Settings**
   - Locate the 2.4GHz Wi-Fi and/or 5GHz Wi-Fi sections (depending on your router model, e.g., Mango or Opal)
   - Toggle the Enable switch to OFF for each Wi-Fi band
   - Click **Apply** to save changes
6. Connecting to the VPN
   - Go to **VPN** > **OpenVPN Client**
   - Click **Add Manually** or **Add Configuration**
        > *This layout tends to change, but the general idea is the same*
   - Drag the VPN file into the upload box or browse to select it
   - After seeing the `SUCCESS!` message, enter a description (e.g., `PhaseII`)
   - Click **Apply**
   - Click **Connect**
7. DNS Configuration
   - Go to **Network** > **DNS**
   - Toggle **DNS Rebinding Attack Protection** to **OFF**
   - Toggle **Override DNS Settings for All Clients** to **ON**
   - In **DNS Server Settings** set **Mode** to **Manual DNS**
   - Set **DNS Server 1** to `10.5.7.1`
   - Click **Apply**

After you finish that last step, ensure that everything is working by navigating to [http://xrpc.phaseii.network](http://xrpc.phaseii.network){:target="_blank"}.

### Configuring the Cabinet
For networking, it's very straightforward. Plug the LAN port from your router into your cabinet either directly or through a switch.

Software configuration is nearly identical to [Getting Connected - Game Configuration](/getting-started/connecting#game-configuration) but your cabinet's image may use a `gamestart.bat` file for launching. Please note that this will not work directly with encrypted drives, unless it's an older game (round dongles)

After finishing your first round, proceed to [Getting Connected - Claiming your Arcade](/getting-started/connecting#claiming-your-arcade).

## Claiming your Arcade
Once you've set up your games and have created your WebUI account, you'll need to claim your arcade using the PCBID. This can only be done on WebUI V3.

Proceed to [Claim Arcade](https://web3.phaseii.network/#/arcade/claim){:target="_blank"} and input your PCBID. This will move the arcade into your name and allow you to change settings and active events.