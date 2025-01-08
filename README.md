# AI Voice Connector
## Connect VOIP SIP calls using OpenSIPS to a conversational AI provider like Deepgram and OpenAI

Author [https://github.com/Vince-0](https://github.com/Vince-0/Projects)

My notes for [OpenSIPS'](https://opensips.org/) [AI Voice Connector - Community Edition](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce)

## Why
Enabling a voice call to communicate with your AI is nice.

<p align="center">
<img src="https://github.com/Vince-0/AI-Voice-Connector/blob/740a2b46a9db2c0c00f61886f9bb6d4db5b76c60/pics/AIConnector3.png" />
</p>

## How

### 1.Requirements

[Deepgram](https://console.deepgram.com/) - comes with $200 credit.

[OpenAI](https://platform.openai.com/) - purchase $5 credit.

A softphone [MicroSIP](https://www.microsip.org/downloads/?file) to call the OpenSIPS service.

A public IP addressed virtual manchine works best with a Linux OS. I used Debian 12. Log in via SSH and use root to prepare the OS.

Git

`apt get install git`

Docker

`curl -sSL https://get.docker.com/ | sudo sh`

### Security
Refer to my [security basics](https://github.com/Vince-0/Security-Basics) notes for a new Linux server.

Pay attention to the firewall section because having an open SIP port on the Internet is a very bad idea.

Allow your client IP and block all others to port 5060

`iptables -A INPUT -s [IP-GOES-HERE] -j ACCEPT`

`iptables -A INPUT -p udp --destination-port 5060 -j DROP`

### 2.Download
Get the OpenSIPS AI Voice Connector code from the repository using git

`git clone https://github.com/OpenSIPS/opensips-ai-voice-connector-ce.git`

### 3. Configure
Edit the .env file and adjust the settings accordingly. I couldnt get the config file to work

`cd opensips-ai-voice-connector-ce/docker`

```
DEEPGRAM_API_KEY=xxxxxxxxxxxxxxxx
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxx
MI_IP=127.0.0.1
MI_PORT=8080
CONFIG_FILE=config.ini
RTP_IP=[PUBLICIP]
```

I had audio issues until I specified the RTP_IP.

### 4. Pull docker image

`docker compose pull`

### 3.Run

`docker compose up`

Should keep the service running and show:

`ai-voice-connector-engine    | 2025-01-07 11:02:44,356 - tid: 140233888802624 - INFO - Starting server at 127.0.0.1:50060`

Check for any errors like invalid API credentials.

Ctrl-c will stop the services.

### 4. Test

I configured a softphone account to use my servers' DNS name as the SIP Server and Proxy with no password, a random user and kept G.711 A-law and u-law codecs enabled.

---
OpenAI [implementation](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce/blob/addad5c94dd1a96bbdadbb565a4630657cb679ae/docs/ai/openai.md): uses OpenAI Real-Time Speech-to-Speech engine
---
Called openai from the dialpad:

Engine: Loud and clear! How can I help you today?

Speaker: Testing 1 2 3

Speaker: Who am I speaking to?

Engine: You're chatting with me, your friendly AI assistant! What's on your mind?

Speaker: Oh, I'm just messing.

Speaker: voice over IP service that connects.

Speaker: to AI systems like yours.

Engine: Well, I'm here to make things easier, whether it's answering questions, helping with tasks, or just having a chat. What can I help you with today?

Speaker: Oh nothing, that'll be all. Please hang up.

Engine: Alright! Take care, and feel free to reach out anytime. Goodbye!

The call did hang up correctly.

<details>
<summary>OpenAI log</summary>

```
ai-voice-connector-opensips  | Jan  7 11:15:52 [26] NOTICE:Started new call for B2B.486.112.1736248552.9724861
ai-voice-connector-engine    | 2025-01-07 11:15:52,767 - tid: 140019193558848 - INFO - Bound to 0.0.0.0:61218
ai-voice-connector-engine    | 2025-01-07 11:15:52,768 - tid: 140019193558848 - INFO - handling B2B.486.112.1736248552.9724861 using openai AI
ai-voice-connector-engine    | 2025-01-07 11:15:53,982 - tid: 140019193558848 - INFO - session.updated
ai-voice-connector-engine    | 2025-01-07 11:15:57,063 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:15:59,164 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:15:59,165 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:15:59,172 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:15:59,764 - tid: 140019193558848 - INFO - rate_limits.updated
ai-voice-connector-engine    | 2025-01-07 11:15:59,768 - tid: 140019193558848 - INFO - response.output_item.added
ai-voice-connector-engine    | 2025-01-07 11:15:59,783 - tid: 140019193558848 - INFO - response.content_part.added
ai-voice-connector-engine    | 2025-01-07 11:15:59,785 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:15:59,788 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:15:59,796 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:15:59,803 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:15:59,819 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:15:59,937 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,065 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,254 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,254 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,254 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,254 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,257 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:00,419 - tid: 140019193558848 - INFO - response.audio.done
ai-voice-connector-engine    | 2025-01-07 11:16:00,419 - tid: 140019193558848 - INFO - Engine: Loud and clear! How can I help you today?
ai-voice-connector-engine    | 2025-01-07 11:16:00,420 - tid: 140019193558848 - INFO - response.content_part.done
ai-voice-connector-engine    | 2025-01-07 11:16:00,420 - tid: 140019193558848 - INFO - response.output_item.done
ai-voice-connector-engine    | 2025-01-07 11:16:00,420 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:01,522 - tid: 140019193558848 - INFO - Speaker: Testing 1 2 3
ai-voice-connector-engine    | 2025-01-07 11:16:05,707 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:16:06,984 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:16:06,985 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:16:06,999 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:16:07,218 - tid: 140019193558848 - INFO - rate_limits.updated
ai-voice-connector-engine    | 2025-01-07 11:16:07,223 - tid: 140019193558848 - INFO - response.output_item.added
ai-voice-connector-engine    | 2025-01-07 11:16:07,237 - tid: 140019193558848 - INFO - response.content_part.added
ai-voice-connector-engine    | 2025-01-07 11:16:07,237 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,242 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,248 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,259 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,264 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,389 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,395 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,402 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,527 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,534 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,653 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,664 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,671 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,678 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,685 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:07,877 - tid: 140019193558848 - INFO - Speaker: Who am I speaking to?
ai-voice-connector-engine    | 2025-01-07 11:16:08,045 - tid: 140019193558848 - INFO - response.audio.done
ai-voice-connector-engine    | 2025-01-07 11:16:08,047 - tid: 140019193558848 - INFO - Engine: You're chatting with me, your friendly AI assistant! What's on your mind?
ai-voice-connector-engine    | 2025-01-07 11:16:08,047 - tid: 140019193558848 - INFO - response.content_part.done
ai-voice-connector-engine    | 2025-01-07 11:16:08,048 - tid: 140019193558848 - INFO - response.output_item.done
ai-voice-connector-engine    | 2025-01-07 11:16:08,049 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:14,582 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:16:16,502 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:16:16,503 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:16:16,509 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:16:16,546 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:16:16,584 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:16,985 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:16:16,985 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:16:16,989 - tid: 140019193558848 - INFO - Speaker:
ai-voice-connector-engine    | 2025-01-07 11:16:16,990 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:16:17,031 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:16:17,037 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:17,973 - tid: 140019193558848 - INFO - Speaker: Oh, I'm just messing.
ai-voice-connector-engine    | 2025-01-07 11:16:19,708 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:16:19,710 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:16:19,720 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:16:19,943 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:16:19,948 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:20,822 - tid: 140019193558848 - INFO - Speaker: voice over IP service that connects.
ai-voice-connector-engine    | 2025-01-07 11:16:22,465 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:16:22,466 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:16:22,473 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:16:22,774 - tid: 140019193558848 - INFO - rate_limits.updated
ai-voice-connector-engine    | 2025-01-07 11:16:22,778 - tid: 140019193558848 - INFO - response.output_item.added
ai-voice-connector-engine    | 2025-01-07 11:16:22,793 - tid: 140019193558848 - INFO - response.content_part.added
ai-voice-connector-engine    | 2025-01-07 11:16:22,795 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,802 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,803 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,809 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,817 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,823 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,953 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,956 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:22,966 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,100 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,218 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,221 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,232 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,237 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,243 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,253 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,483 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,486 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,493 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,499 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,504 - tid: 140019193558848 - INFO - Speaker: to AI systems like yours.
ai-voice-connector-engine    | 2025-01-07 11:16:23,506 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,513 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,521 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,527 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,535 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,889 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,895 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,921 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,923 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,923 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:23,924 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:24,704 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:24,707 - tid: 140019193558848 - INFO - response.audio.done
ai-voice-connector-engine    | 2025-01-07 11:16:24,708 - tid: 140019193558848 - INFO - Engine: Well, I'm here to make things easier, whether it's answering questions, helping with tasks, or just having a chat. What can I help you with today?
ai-voice-connector-engine    | 2025-01-07 11:16:24,708 - tid: 140019193558848 - INFO - response.content_part.done
ai-voice-connector-engine    | 2025-01-07 11:16:24,712 - tid: 140019193558848 - INFO - response.output_item.done
ai-voice-connector-engine    | 2025-01-07 11:16:24,712 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:34,782 - tid: 140019193558848 - INFO - input_audio_buffer.speech_started
ai-voice-connector-engine    | 2025-01-07 11:16:36,745 - tid: 140019193558848 - INFO - input_audio_buffer.speech_stopped
ai-voice-connector-engine    | 2025-01-07 11:16:36,746 - tid: 140019193558848 - INFO - input_audio_buffer.committed
ai-voice-connector-engine    | 2025-01-07 11:16:36,756 - tid: 140019193558848 - INFO - response.created
ai-voice-connector-engine    | 2025-01-07 11:16:37,064 - tid: 140019193558848 - INFO - rate_limits.updated
ai-voice-connector-engine    | 2025-01-07 11:16:37,067 - tid: 140019193558848 - INFO - response.output_item.added
ai-voice-connector-engine    | 2025-01-07 11:16:37,085 - tid: 140019193558848 - INFO - response.content_part.added
ai-voice-connector-engine    | 2025-01-07 11:16:37,085 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,089 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,094 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,100 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,108 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,240 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,253 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,370 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,376 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,385 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,394 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,517 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,524 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,538 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,540 - tid: 140019193558848 - INFO - response.audio_transcript.delta
ai-voice-connector-engine    | 2025-01-07 11:16:37,665 - tid: 140019193558848 - INFO - Speaker: Oh nothing, that'll be all. Please hang up.
ai-voice-connector-engine    | 2025-01-07 11:16:37,872 - tid: 140019193558848 - INFO - response.audio.done
ai-voice-connector-engine    | 2025-01-07 11:16:37,874 - tid: 140019193558848 - INFO - Engine: Alright! Take care, and feel free to reach out anytime. Goodbye!
ai-voice-connector-engine    | 2025-01-07 11:16:37,874 - tid: 140019193558848 - INFO - response.content_part.done
ai-voice-connector-engine    | 2025-01-07 11:16:37,880 - tid: 140019193558848 - INFO - response.output_item.done
ai-voice-connector-engine    | 2025-01-07 11:16:38,250 - tid: 140019193558848 - INFO - response.output_item.added
ai-voice-connector-engine    | 2025-01-07 11:16:38,250 - tid: 140019193558848 - INFO - response.function_call_arguments.delta
ai-voice-connector-engine    | 2025-01-07 11:16:38,271 - tid: 140019193558848 - INFO - response.function_call_arguments.done
ai-voice-connector-engine    | 2025-01-07 11:16:38,275 - tid: 140019193558848 - INFO - response.output_item.done
ai-voice-connector-engine    | 2025-01-07 11:16:38,290 - tid: 140019193558848 - INFO - response.done
ai-voice-connector-engine    | 2025-01-07 11:16:41,331 - tid: 140019193558848 - INFO - Terminating call B2B.486.112.1736248552.9724861
ai-voice-connector-engine    | 2025-01-07 11:16:41,334 - tid: 140019193558848 - INFO - Call B2B.486.112.1736248552.9724861 closing
```
</details>

OpenAI usage graph
<p align="center">
<img src="https://github.com/Vince-0/AI-Voice-Connector/blob/844a9f72e7e76ecec938903c650232cee588c43d/pics/openai1.png" />
</p>

This short conversation cost $0.13 cents and used the OpenAI Speech-to-Speech API for real-time interpretation and response generation, streamlining interactions without intermediate steps. This should make for a more fluid conversation user experience.

---
Deepgram [implementation](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce/blob/addad5c94dd1a96bbdadbb565a4630657cb679ae/docs/ai/deepgram.md): convert to text using Deepgram Speech-to-Text, push transcribe to OpenAI and then push the response back to Deepgram Text-to-Speech engine
---
Called deepgram from the dialpad:

Speaker: Testing. One, two, three.

Assistant: Loud and clear! How can I assist you today?

Speaker: Oh, I'm just testing my voice over IP service which connects to AI assistance like yours.

Assistant: Great! It seems to be working well. If you have any other questions or need further testing, feel free to let me know!

Speaker: It seems to be working well.

Assistant: That's good to hear! If there's anything else you'd like to test or discuss, just let me know.

Speaker: That's This No. That'll be all. Please hang up.

Assistant: Alright, no problem. If you need anything else in the future, feel free to reach out. Have a great day!

Speaker: Alright.

Assistant: Take care!

The call did not hang up.


<details>
<summary>OpenAI log</summary>
  
```
ai-voice-connector-opensips  | Jan  7 11:17:23 [20] NOTICE:Started new call for B2B.373.208.1736248643.1669313877
ai-voice-connector-engine    | 2025-01-07 11:17:23,148 - tid: 140019193558848 - INFO - Bound to 0.0.0.0:63415
ai-voice-connector-engine    | 2025-01-07 11:17:23,148 - tid: 140019193558848 - INFO - handling B2B.373.208.1736248643.1669313877 using deepgram AI
ai-voice-connector-engine    | 2025-01-07 11:17:31,452 - tid: 140019193558848 - INFO - Speaker: Testing. One, two, three.
ai-voice-connector-engine    | 2025-01-07 11:17:32,413 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:17:32,425 - tid: 140019193558848 - INFO - Assistant: Loud and clear! How can I assist you today?
ai-voice-connector-engine    | 2025-01-07 11:17:33,527 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.deepgram.com/v1/speak?model=aura-asteria-en&encoding=alaw&container=none&sample_rate=8000 "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:17:46,191 - tid: 140019193558848 - INFO - Speaker: Oh, I'm just testing my voice over IP service which connects to AI assistance like yours.
ai-voice-connector-engine    | 2025-01-07 11:17:47,383 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:17:47,391 - tid: 140019193558848 - INFO - Assistant: Great! It seems to be working well. If you have any other questions or need further testing, feel free to let me know!
ai-voice-connector-engine    | 2025-01-07 11:17:48,478 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.deepgram.com/v1/speak?model=aura-asteria-en&encoding=alaw&container=none&sample_rate=8000 "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:17:51,192 - tid: 140019193558848 - INFO - Speaker: It seems to be working well.
ai-voice-connector-engine    | 2025-01-07 11:17:52,499 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:17:52,505 - tid: 140019193558848 - INFO - Assistant: That's good to hear! If there's anything else you'd like to test or discuss, just let me know.
ai-voice-connector-engine    | 2025-01-07 11:17:53,281 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.deepgram.com/v1/speak?model=aura-asteria-en&encoding=alaw&container=none&sample_rate=8000 "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:18:03,676 - tid: 140019193558848 - INFO - Speaker: That's This No. That'll be all. Please hang up.
ai-voice-connector-engine    | 2025-01-07 11:18:04,899 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:18:04,902 - tid: 140019193558848 - INFO - Assistant: Alright, no problem. If you need anything else in the future, feel free to reach out. Have a great day!
ai-voice-connector-engine    | 2025-01-07 11:18:05,706 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.deepgram.com/v1/speak?model=aura-asteria-en&encoding=alaw&container=none&sample_rate=8000 "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:18:06,673 - tid: 140019193558848 - INFO - Speaker: Alright.
ai-voice-connector-engine    | 2025-01-07 11:18:07,363 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:18:07,504 - tid: 140019193558848 - INFO - Assistant: Take care!
ai-voice-connector-engine    | 2025-01-07 11:18:08,471 - tid: 140019193558848 - INFO - HTTP Request: POST https://api.deepgram.com/v1/speak?model=aura-asteria-en&encoding=alaw&container=none&sample_rate=8000 "HTTP/1.1 200 OK"
ai-voice-connector-engine    | 2025-01-07 11:18:21,351 - tid: 140019193558848 - INFO - Call B2B.373.208.1736248643.1669313877 closing
ai-voice-connector-engine    | tasks cancelled error:
ai-voice-connector-engine    | 2025-01-07 11:18:21,857 - tid: 140019193558848 - ERROR - tasks cancelled error:
```
</details>

<p align="center">
<img src="https://github.com/Vince-0/AI-Voice-Connector/blob/844a9f72e7e76ecec938903c650232cee588c43d/pics/deepgram2.png" />
</p>

Although that was more than this test which was 1 /listen and 5 /speak endpoint calls.

This short conversation cost about $0.04. and used Deepgram's Speech-to-Text API to transcribe the SIP user's spoken input into text. This transcription is then sent to the ChatGPT API, which interprets the message and generates a response. The response text is subsequently passed back to Deepgram's Text-to-Speech API, which converts it into voice format, allowing the response to be played back to the user.

---

## To Do
- Security concerns
- Compare Deepgram to OpenAI flavours for conversation quality, cost, options tweaking.
- Connect a custom AI provider
- Outline a production use case

## Reference Links
[AI Voice Connector - Community Edition - Getting Started](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce)

[AI Voice Connector Implementation](https://github.com/OpenSIPS/opensips-ai-voice-connector-ce/blob/main/docs/implementation.md)
