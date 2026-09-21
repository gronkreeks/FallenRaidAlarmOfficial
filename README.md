# FallenRaidAlarmOfficial
A raid alarm dedicated to stopping offline raids in their tracks. Works by detecting audio loudness and bass, able to be calibrated to your exact sound settings. Currently in beta testing phase.


Get woken up when someone starts hitting your base in Fallen Survival.

Leave the game running overnight and leave Raid Alarm listening. When it hears an explosion, it pings your Discord server. Your phone does the rest.

<img width="516" height="232" alt="image" src="https://github.com/user-attachments/assets/74771efa-fbcf-426f-ba03-dcb8e6b12dd9" />

<img width="756" height="662" alt="image" src="https://github.com/user-attachments/assets/ac2670ee-d322-4a01-bcc7-5f0ac4ea19f5" />

Free while I'm testing it. A Windows version is next.

What it actually does

It listens to your game audio and watches for two things at once: a sound loud enough to matter, and one where most of the energy is low-frequency rumble. Rockets, C4 and satchels thump hard under 200 Hz in a way that gunfire, footsteps and voice comms don't, so it can tell a raid from a firefight instead of pinging every time someone shoots near you.

It never records anything, and nothing leaves your machine except the alert message you send to your own Discord webhook.

What you need
A Mac with Apple Silicon (M1 or newer)
BlackHole, a free audio driver — the app can't hear your Mac without it
A Discord server where you can create a webhook
Your Mac left awake with the game running
Install
Download RaidAlarm-macOS.zip from Releases, unzip it, and drag Raid Alarm to your Applications folder.
Open it. macOS will refuse, because the app isn't signed by a registered Apple developer — that costs $99/year and this is free.
Go to System Settings → Privacy & Security, scroll to the bottom, and click Open Anyway next to the message about Raid Alarm.

If you'd rather not run an unsigned app from a stranger, don't — build it yourself from the source instead. Instructions at the bottom.

Setup

The app opens on a Setup tab that walks you through this and checks each step off as you go. The short version:

1. Install BlackHole. In Terminal:

brew install --cask blackhole-2ch

Enter your Mac password when asked, then restart. If Terminal says brew: command not found, install Homebrew first.

2. Split your audio. Open Audio MIDI Setup, press Cmd+1, click + at the bottom left and choose Create Multi-Output Device. Tick both your headphones (or monitor) and BlackHole 2ch. Set your headphones as the Primary Device, tick Drift Correction on BlackHole only. Then right-click the Multi-Output Device and choose Use This Device For Sound Output.

This is what lets you hear the game while the app listens to it. Your volume keys stop working afterwards, so set volume in-game instead.

3. Allow the microphone. Press Test capture in the app with something playing. macOS asks once; choose Allow. It only ever listens to BlackHole.

4. Add your Discord webhook. In Discord: Server Settings → Integrations → Webhooks → New Webhook. Pick the channel you want alerts in, copy the URL, paste it into the app and press Save and send test.

Treat that URL like a password — anyone who has it can post in that channel.

5. Calibrate. Easiest on a combat server: spawn a rocket launcher, stand a short way from a wall, and fire at it during the calibration window. The app listens for 45 seconds, works out your settings from what it heard, and shows you what it suggests. Press Use these settings.

Using it

Before you log off: leave the game running in your base, open Raid Alarm, and press Start listening.

Keep the lid open and the charger plugged in. The screen can sleep.
Turn off in-game music, and gunfire if you like, so explosions stand out.
Check Discord on your phone will actually notify you for that channel and isn't silenced by Do Not Disturb or a Focus mode.

By default you get one ping per 90 seconds, so a long raid doesn't send forty alerts. Every alert is saved to a log file you can check in the morning.

Settings
Setting	What it does
Loudness trigger	How loud a sound has to be. Lower catches distant explosions but risks false alarms.
Bass trigger	How much of the sound must be low rumble. This is what separates explosions from gunfire.
Blocks in a row	How long the sound must last. Higher ignores clicks and pops.
Wait between alerts	Minimum gap between pings.

Calibrating sets the first three for you.

If something's wrong

Nothing is being detected. Press Test capture on the Setup tab. If it says it heard silence, your sound output isn't set to the Multi-Output Device, or microphone access is off in System Settings → Privacy & Security → Microphone. Quit the app fully and reopen it after changing that.

It pings at things that aren't raids. Open the alert log (Show file on the Alarm tab) and look at the bass column on the false alarms. If they're low, raise the bass trigger. If they're high, raise the loudness trigger instead.

It missed a raid. Lower the loudness trigger a few dB and recalibrate with a rocket fired from further away.

One ping, then nothing during the rest of the raid. That's the cooldown working as intended. Shorten it in Settings if you want a heartbeat.

I can't hear anything at all. Your output is probably set to BlackHole instead of the Multi-Output Device — BlackHole alone goes nowhere.

Build it yourself

Needs Python from python.org (the Homebrew one works too if you also brew install python-tk).

cd RaidAlarm
./build_app.sh

It sets up its own environment, builds the app, and offers to install it. Or skip the app entirely and run it as a script:

pip3 install numpy sounddevice certifi
python3 raid_alarm_app.py
Feedback

It's new and I'm still testing it, so tell me what breaks — open an issue here or find me in the Fallen Survival Discord. Useful things to include: what you expected, what happened, and the loudness and bass numbers from the log.

Settings and logs live in ~/Library/Application Support/Raid Alarm/.

Built with Python, sounddevice and NumPy. Source is here so you can see exactly what it does before you run something that asks for microphone access.
