# netatalk-truenas

Netatalk AFP file server packaged as a TrueNAS custom app, set up so a
**Mac OS 9** machine can mount a TrueNAS dataset from the Chooser.

Built on the official [`netatalk/netatalk`](https://hub.docker.com/r/netatalk/netatalk) image.

Raw file for copy and paste:
https://raw.githubusercontent.com/chrisleiker/netatalk-truenas/main/compose.yaml

## What's different from the stock image

- Turns on the **DHCAST128 ("DHX")** login method, which Mac OS 9's AppleShare
  client (3.8.x) supports. The stock image only offers DHX2/SRP, which OS 9
  can't use. DHX2 stays on for modern Macs.
- Runs as TrueNAS's built-in `apps` user (UID/GID 568) so files are readable by
  SMB shares and other apps.
- Uses MacRoman for filenames and turns Time Machine off.

## Install on TrueNAS

1. **Create a dataset** for the share: Datasets → Add Dataset → `macshare`.
2. **Copy `compose.yaml`** and edit three things:
   - `POOL` in the volume path → your pool name
   - `AFP_USER` → the login name you want
   - `AFP_PASS` → a real password (8 characters or fewer is safest for OS 9)
3. Apps → Discover Apps → **Install via YAML**, name it `netatalk`, paste the
   file, and save.
4. Check that the app shows **Running** and is listening on port 548.

## Connect from Mac OS 9

1. Apple menu → **Chooser** → **AppleShare**.
2. Click **Server IP Address…** and enter your TrueNAS IP.
3. Log in and choose **iMac Share**. Tick it to mount at startup if you like.

A second, unused "Time Machine" volume also appears; you can ignore it.

## Notes

- Only port 548 (AFP over TCP) is needed. AppleTalk/Chooser browsing is off.
- Keep the port on your LAN; don't forward it to the internet.
