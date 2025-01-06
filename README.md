# AI Voice Connector
## Connect VOIP SIP calls to a conversational AI

Author [https://github.com/Vince-0](https://github.com/Vince-0/Projects)

My notes for [OpenSIPS'](https://opensips.org/) [AI Voice Connector - Community Edition](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce)

## Why
Enabling a voice call to communicate with your AI is nice.

<p align="center">
<img src="" />
</p>

## How

## 1.Requirements
Public IP addressed virtual manchine works best.

Linux OS. I used Debian 12.

Log in via SSH and prepare the OS

### Security
Refer to my [security basics](https://github.com/Vince-0/Security-Basics) notes for a new Linux server.

Pay attention to the firewall section because having an open SIP port on the Internet is a very bad idea.



### 2.Download

`git clone https://github.com/OpenSIPS/opensips-ai-voice-connector-ce.git
cd opensips-ai-voice-connector-ce/docker`

### 3. Configure
Edit the .env file and adjust the settings accordingly

Alternatively, create a configuration file

### 3.Run
docker compose up

### 4. Test


## To Do

-
-
-

## Reference Links
[AI Voice Connector - Community Edition - Getting Started]([https://github.com/OpenSIPS/opensips-ai-voice-connector-ce](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce/blob/main/docs/getting-started.md))
