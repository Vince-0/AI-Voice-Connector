# AI Voice Connector
## Connect VOIP SIP calls to a conversational AI

Author [https://github.com/Vince-0](https://github.com/Vince-0/Projects)

My notes for OpenSIPS [AI Voice Connector - Community Edition] (https://github.com/OpenSIPS/opensips-ai-voice-connector-ce)

## Requires
Public IP addressed virtual manchine works best
Linux OS. I used Debian 12

Log in via SSH and prepare the OS:

### Security
#### User:
Add a new user. You shouldnt be using root to log in directly

`adduser mynewuser`

Add your user to the sudoers file for the appropriate permissions

`nano /etc/sudoers`

Add the line under the "User privilege specification" comment 

`mynewuser ALL=(ALL:ALL) ALL`

#### SSH:
Edit sshd config

`nano /etc/ssh/sshd_config`

Or rather create a custom file

`nano /etc/ssh/sshd_config.d/10_custom.conf`

Add the line to disallow root login via SSH

`PermitRootLogin prohibit-password`

For preferred key security remove password log in altogether and use only key files

`PasswordAuthentication no`

On your client computer generate a public SSH key file for your current user

`ssh-keygen`

Copy the SSH key file to your server

`ssh-copy-id mynewuser@[myserver]`

Test that you can log in without passwords before restarting sshd service

`systemctl restart sshd`

Check that sshd restarted without any errors and its Active status is "active (running)"

`systemctl status sshd`

#### Firewall:


### Download

`git clone https://github.com/OpenSIPS/opensips-ai-voice-connector-ce.git
cd opensips-ai-voice-connector-ce/docker`

### Edit the .env file and adjust the settings accordingly


### Alternatively, create a configuration file

### Run
docker compose up

## Why
Organisations with MS Teams may want to enable their users to make phone calls from the MS Teams application. This is done with MS Teams Direct Routing.
<p align="center">
<img src="https://github.com/Vince-0/MSTeams-FreePBX/blob/9660cbc6282b76b1156d93897cc81612802bca68/MSTEAMS-Asterisk.png" />
</p>

MS Teams users get a phone calls dialpad inside their Teams client
<p align="center">
<img src="https://github.com/Vince-0/MSTeams-FreePBX/blob/bfe585223027dddd8220907ff325088090d5cb41/MSTeams-dialpad2.png" />
</p>


MS Teams does not oficially support [Asterisk](https://en.wikipedia.org/wiki/Asterisk_(PBX)) as an SBC to connect VOIP services to MS Teams Direct Routing but [SIP](https://en.wikipedia.org/wiki/Session_Initiation_Protocol) is SIP and each implementation is **almost** close enough to work out of the box.

MS Teams uses an implementation of Session Initiation Protocol and [Asterisk](https://www.asterisk.org/) is a SIP back-to-back user agent. 

This allows Asterisk to bridge SIP channels together for example a telecoms provider on one side and an MS Teams Direct Routing channel on the other.

Asterisk implements a SIP channel driver called [PJSIP](https://github.com/pjsip/pjproject). PJSIP is a [GNU GPL](https://www.gnu.org/) [licensed](https://docs.pjsip.org/en/latest/overview/license_pjsip.html), multimedia communication library written in C.

By default the PJSIP NAT module does not present a FQDN in the CONTACT and VIA SIP headers so one can change this behavior in the module's source code.

Asterisk under FreePBX is an easy way to connect a SIP server with a GUI to MS Teams but any SIP switch/proxy like FreeSwitch or Kamailio could do it.

MS Teams can route media (audio) directly between MS Teams users and the SBC to shorten the path media takes, greatly decreasing latency and network hops and so increasing call quality and reliability. This requires an ICE (Interactive Connectivity Establishment) server configured in Asterisk to offer its public IP as a candidate for peer to peer connections for VOIP.

MS Teams offers a number of media codecs for VOIP calls but the best for Internet connections is SILK because it offers forward error correction, is quite tolerant of packet loss and has various bandwidth options.

## How

1. Prepare and install a custom PJSIP NAT module for Asterisk under FreePBX.

2. Configure TLS certificates from [LetsEncrypt](https://letsencrypt.org/) using (acme.sh)[https://github.com/acmesh-official/acme.sh] for Asterisk to provide SRTP encryption on calls. This requires a publicly accessible DNS FQDN on your server.
  
3. Use FreePBX to control Asterisk dialplan to route calls in and out of MS Teams and any SIP connection like a telecoms carrier.

4. Configure MS Teams, with the appropriate "Phone System" licenses, to use MS Teams Direct Routing for your tenant's users via this Asterisk as an SBC.

## To Do

- Fix email option for SSL provisioning - currently does not configure SSL properly
- Asterisk mulitple version options
- Precompile PJSIP NAT module for mulitple Asterisk versions
- Asterisk basic standalone option

## Compiled PJSIP NAT module for Asterisk 21

[Vince-0/MSTeams-PJSIPNAT](https://github.com/Vince-0/MSTeams-PJSIPNAT)

## Reference Links

[Asterisk Developer Mail List](https://asterisk-dev.digium.narkive.com/ucZYhaLE/asterisk-16-pjsip-invite-contact-field-and-fqdn#post12)

[Nick Bouwhuis](https://nick.bouwhuis.net/posts/2022-01-02-asterisk-as-a-teams-sbc)

[Ayonik](https://www.ayonik.de/blog/item/90-microsoft-teams-direct-routing-with-asterisk-pbx)

[godril at Otakudang.org](https://www.otakudang.org/?p=969)


## MS Documentation

[Plan Direct Routing](https://learn.microsoft.com/en-us/microsoftteams/direct-routing-border-controllers)

[Configure Voice Routing](https://learn.microsoft.com/en-us/microsoftteams/direct-routing-configure#configure-voice-routing)

[Media Bypass](https://learn.microsoft.com/en-us/microsoftteams/direct-routing-plan-media-bypass)




