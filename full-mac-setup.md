# Full Mac Setup Process (for Jeff Geerling)

There are some things in life that just can't be automated... or aren't 100% worth the time :(

This document covers that, at least in terms of setting up a brand new Mac out of the box.

## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, and optionally signing into my iCloud account). Once on the macOS desktop, I do the following (in order):

  - Install Ansible (following the guide in [README.md](README.md))
  - **Sign in in App Store** (since `mas` can't sign in automatically)
  - Clone mac-dev-playbook to the Mac: `git clone git@github.com:camdenwebster/mac-dev-playbook.git`
  - Drop `config.yml` from `~/Dropbox/Apps/Config` to the playbook (copy over the network or using a USB flash drive).
  - Run the playbook with `--skip-tags post`.
  - Start Synchronization tasks:
    - Open Photos and make sure iCloud sync options are correct
    - Open Music, make sure computer is authorized, and set Library sync options
    - Open Nextcloud, sign in, and set up sync
  - Install old-fashioned apps:
    - Install UAD drivers (https://www.uaudio.com/downloads/uad)
  - Manually sign into Mail/Calendar accounts
  - Manually copy `~/dev` folder from another Mac (to save time).
  - Manual settings to automate someday:
    - Bitwarden:
        - Provide Vault location, sign in
    - Figma:
      - Sign in
    - Home Assistant
      - Sign in
    - Raycast:
      - Double click backup file, input password
    - Safari:
      - View > Show Status Bar
      - Preferences > Advanced > "Show full website address"
      - Preferences > Advanced > "Show Develop menu in menu bar"
    - Slack:
      - Sign into workspaces
  - _After Dropbox Sync completes_: Run the playbook with `--tags post` to complete setup.
  - Symlink the `provision` script into the ~/bin dir `ln -s ~/dev/mac-dev-playbook/files/provision ~/bin/provision`

## To Wrap in Post-provision automation

The following tasks have to wait for the initial Dropbox sync to complete before they'll succeed. So ideally I'll stick this all in a post-provision script but somehow flag it not to run on first provision.

```
# Symlink SSH folder from Nextcloud to home directory.
sudo ln -s ~/NextCloud/2\ -\ Areas/Mac\ Config/ssh ~/.ssh