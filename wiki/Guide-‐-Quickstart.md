### NOTE: Dedicated Server is currently available for Windows Only
### If you are using Linux the community have a [guide](https://github.com/DFJacob/AbioticFactorDedicatedServer/issues/3#issuecomment-2094369127) on how to get it working.
# Installation

## Option 1: Steam Client
If you own Abiotic Factor on Steam, you can find Abiotic Factor Dedicated Server in your library under tools.

## Option 2: SteamCMD (Recommended)
The Dedicated Server tool for Abiotic Factor is downloaded and setup using Valve's Steam server tool, SteamCMD.

1) Download and Install SteamCMD, which can be obtained [here](https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip)

2) Run SteamCMD.exe

3) Log in to an anonymous Steam account by typing `login anonymous` and pressing Enter

Optional Step) Set an install directory using the command `force_install_dir <DesiredPath>`. If you dont do this, the install dir will default to the location your steamCMD is.

4) Download and Install the Abiotic Factor Dedicated Server Tool by typing `app_update 2857200 validate` and pressing Enter

# Configuration

Once you have the server files, navigate to where it's installed.

1) Create a new txt file called RunServer and place it in` \(Server Install)\AbioticFactor\Binaries\Win64\`
An example RunServer file is provided in this github.

2) Open RunServer add the following: `AbioticFactorServer-Win64-Shipping.exe -log -newconsole -useperfthreads -NoAsyncLoadingThread -MaxServerPlayers=6 -PORT=7777 -QueryPort=27015 -ServerPassword=YourServerPassword -AdminPassword=YourAdminPassword -SteamServerName="Your Server Name"`
Ensure you replace `YourServerPasword` and `"Your Server Name"` with the correct info.
If you don't want a password to access the admin menu in-game, remove the `-AdminPassword=` launch parameter

3) Rename `RunServer.txt` to `RunServer.bat`, this allows you to double click it to run the server. Run the server and check if a console window appears.

4) If you didn't use the `-AdminPassword` launch parameter, you will need to manually add your SteamID (Doesn't work for Xbox or Playstation) to the server config. Close the server and navigate to `\(Server Install)\AbioticFactor\Saved\SaveGames\Server\Admin.ini`. Edit this file and add your SteamID under `[Moderators]`, eg: `Moderator=76561198053306820`

5) Use `RunServer` again, if you can't see the server in the server browser you may have to port forward 7777 and 27015

# Connecting to your server

If you want your server available on the public Internet, debugging the local dedicated server deployment (for any game, usually) is generally a process of checking things in sequence, moving in steps from the server machine out to the Internet.

1) verify that the server executable is running and listening on the specified port(s) (netstat on Windows, and either that or ss on Linux)
check that you have only one instance of the server running (or however many you expect should be running)
check that the port(s) haven't changed, and aren't being used by another program (this also applies to accidentally-launched multiple instances of the same config - just because there's no window doesn't mean it's not running; check the Task Manager / Details tab for the executable)
check the server log (Saved/Logs/AbioticFactor.log) for any errors (SSL (install certificate), STEAM networking (change ports), other?)
always make any and all changes to configuration (changing .ini files, moving files around etc.) when the server is shut down
do not use -lanonly or -uselocalips in your launch command - they are context-specific, don't apply in 99.99999% of cases, and break Internet connectivity when used
ensure time/timezone is set correctly on the server PC
If this step fails, continue working until it starts up without errors and listens on the correct ports.

2) verify that it's possible to connect to the server within its LAN, using the server's LAN IP (do not use 127/8 or localhost - may bypass the networking stack)
use Direct Connect for this purpose, using the server machine's LAN IP
verify that the server has permission (firewall?) to accept connections on the configured port(s)
restart the server PC if possible - this has, on occasion, fixed stubborn issues before
ensure time/timezone is set correctly on the client(s)
If this step fails, continue working on the configuration until you can connect to the server using Direct Connect / IP.

3) verify that your server PC has a static DHCP lease (address reservation) configured for it in your router
many routers have port-forward records pointing at a specific IP; if this IP changes, those records become invalid
consult your router's manual for this, as there are as many routers as there are printers out there, with new ones coming out each year, and every company has their own idea of what "makes sense"

4) verify that you have a public IP assigned to your router - it's required for hosting services on the Internet
go to https://whatismyip.com/ and note down the IPv4 address shown there
go into your router config and find this IPv4 IP (may be listed under "Broadband", "Internet", "WAN" etc.)
If you can't find this address in your router (esp. if your WAN IP is in the 100.64.*.* to 100.127.*.* range), you don't have a public IP. You should probably look into other options (Hamachi, ZeroTier etc.) to make your server available to your friends.

5) verify that your configuration of the server's port forward(s) is correct
determine what port(s) the game actually uses and which need to be port-forwarded (for Abiotic Factor, that's port number 7777 protocol UDP by default)
Test this by asking someone on the Internet to connect to your server. They should first try Direct Connect/IP to your public WAN IP address.

At this point, if you've gone through all steps successfully, the server should be contactable from the Internet. If it works with Direct Connect/IP but not from the browser/lobby code, you should also:

6) verify that you don't have multiple WANs attached to your Internet hookup (yes, using a public VPN counts as a second WAN)
if you do, make sure that the server's outgoing HTTPS self-registration goes out on the correct WAN, because the browser will serve the IP address it sees on the incoming connection

# Server Shutdown

Do not close the window running the server to shut it down. This will not properly shut down the server. 
To properly shut down the server either do this in game or through the admin console that is launched when running `RunServer.bat`

To accomplish this in game
1) Pause game
2) Click `player management`
3) Click `admin` (you will need to enter your password)
4) Type `exit` into server console and hit enter

In admin console
1) Type `exit` into server console and hit enter