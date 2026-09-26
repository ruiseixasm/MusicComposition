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

