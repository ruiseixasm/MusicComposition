# Enabling user hardware access
Adding the user to the audio group
```sh
sudo usermod -aG audio rui
sudo usermod -aG pulse,pulse-access,video rui
```
After running this command one has to restart the computer!

How to check if the user was added to the group `audio`
```sh
groups
```

# Verifying the audio configuration file
```sh
sudo nano /etc/security/limits.d/audio.conf
```

```ini
@audio   -   rtprio   95
@audio   -   memlock  unlimited
```

# Running the Waveform in command line
```sh
Waveform14
```
Shows the reporting of errors if any.

# Troubleshooting
Nowadays all Ubuntu/Debian linux distros replaced **PulseAudio** and **JACK** by
**PipeWire**, so, this error may occur
```sh
rui@littleboy:~$ Waveform14 
ALSA lib pcm.c:2722:(snd_pcm_open_noupdate) [error.pcm] Unknown PCM pulse
ALSA lib pcm.c:2722:(snd_pcm_open_noupdate) [error.pcm] Unknown PCM pulse
Cannot connect to server socket err = No such file or directory
Cannot connect to server request channel
jack server is not running or cannot be started
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
Unable to connect to JACK server
Cannot connect to server socket err = No such file or directory
Cannot connect to server request channel
jack server is not running or cannot be started
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
JackShmReadWritePtr::~JackShmReadWritePtr - Init not done for -1, skipping unlock
Unable to connect to JACK server
rui@littleboy:~$ 
```
The `Unable to connect to JACK server` means the Waveform is trying to connect the old
audio servers. Also, the mention of `Unknown PCM pulse` is key, it means that the real
**PulseAudio** isn't natively running in the system. To prove it, run `groups` and
confirm that the group `pulse` is missing.

Type the existence of `pactl` to check the installation of `pulseaudio-utils`
```sh
pactl info | grep "Server Name"
```

Check the installation of **PipeWire**
```sh
dpkg -l | grep -E "pipewire-alsa|pipewire-jack|libasound2-plugins"
```

To install all of them or some of them
```sh
sudo apt update
sudo apt install pipewire-alsa pipewire-jack libasound2-plugins
```

Update the user file system (`systemd`)
```sh
systemctl --user daemon-reload
```

Finally, restart the `pipewire`
```sh
systemctl --user restart pipewire pipewire-pulse wireplumber
```

