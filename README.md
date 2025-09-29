## UoE Compsec CW1 on a personal laptop

Tested using a fresh Ubuntu 24.04.1 LTS.

<img width="1920" height="1198" alt="image" src="https://github.com/user-attachments/assets/a240d82c-93a5-4f74-b500-bddb03ac4868" />

### Steps

* Update your system.
```
sudo apt update
```
* Install 7zip.
```
sudo apt install -y 7zip
```
* Download `cw1.7z` from Learn.
* Unzip the file.
```
7z x cw1.7z
```
* Install Qemu and Remote Viewer
```
sudo apt install qemu-system virt-viewer
```
* Patch the `startvm` script with the following changes.
```diff
#!/usr/bin/env bash
set -e
DIR=$(realpath $(dirname "${BASH_SOURCE[0]}"))
source "$DIR/qemu.env"
# Test for commands
command -v "$QEMU" >/dev/null 2>&1 || { echo >&2 'Qemu was not found.'; exit 1; }
- command -v vinagre >/dev/null 2>&1 || { echo >&2 'vinagre was not found.'; exit 1; }
if [ ! -e /dev/kvm ]; then
    echo >&2 '/dev/kvm missing. Is hardware virtualisation enabled?'; exit 1;
fi
TMPIMG1=$(mktemp)
# ... rest of the code ...
trap clean_up EXIT
echo ":: Starting SPICE client"
- spice "$((5900 + RND))" 2>/dev/null
+ remote-viewer spice://127.0.0.1:$((5900 + RND))
```
* Make the script executable `chmod +x startvm`.
* Run the script `./startvm`.

* You're good to go!
