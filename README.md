# Omarchy Config Journal

## Dotfiles Management (Bare Git Repo)

This system tracks dotfiles using a **bare Git repository** method. This allows tracking files directly in the `$HOME` directory without a `.git` folder cluttering it up.

### 1. The Bare Repository

The first step in this setup is to create the "bare" repository itself. This is done with:

```bash
git init --bare $HOME/.dotfiles
```

* **`--bare`**: This flag creates a repository that is only the Git database (the commit history, etc.). It has no "working tree" (the actual files), which is why it's "bare."

* **`$HOME/.dotfiles`**: This is simply the folder created to store this database. It's kept separate from the working files in $HOME.


### 2. The `.bashrc` Alias

To make this work, the following line was added to `~/.bashrc` and restart the shell:

```bash
alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
```

### 3. What This Alias Means

This creates a new terminal command called `config`. Running this command executes `git` with two special instructions:

* **`--git-dir=$HOME/.dotfiles/`**: Tells Git to use the `~/.dotfiles` folder as its database (instead of a `.git` folder).
* **`--work-tree=$HOME`**: Tells Git that the files it should track are in the `$HOME` directory.

This is why you must use `config add .bashrc` instead of `git add .bashrc`.

### 4. Initial Repository Setup

After creating the alias, the following one-time setup command must be run to make the repository usable:

```bash
config config --local status.showUntrackedFiles no
```

This command tells Git to hide all untracked files from the config status command. Without this, config status would show every file in your home directory (Downloads, Music, etc.) as "untracked", which is very messy.

### 5. Note on Adding Files

```bash
config add .
```
does not work, because the "work-tree" is the entire home directory (~/), running config add . would try to add every file and folder in the home directory to Git. Files must always be added explicitly, one by one (e.g., config add .config/hypr).

### 6. Initial Setup on a New System

* Add the alias mentioned in [step 2](#2-the-bashrc-alias) and restart the shell.
* Clone the bare repository to the desired location. The desired location in my case is `$HOME/.dotfiles`.
```bash
git clone --bare "repository link" "desired location"
```
* Copy the files from the repository into the `$HOME` directory. The `--force` flag is used to overwrite any existing configuration files. The usage of `--force` flag is necessary only for cloning the exact previous setup.

```bash
config checkout --force
```
* Hide untracked files by following [step 4](#4-initial-repository-setup).

## Manual System Setup

### 1. Fix Brightness Controls Not Working
This is needed for brightness control on hybrid AMD/Nvidia GPU laptops (especially if `brightnessctl info` shows an NVIDIA device but the controls still don't work).

This system uses a Unified Kernel Image (UKI), which means the kernel command line parameters are embedded directly into the kernel's .efi file at build time. Simply editing `limine.conf` in `/boot` will not work. The default Limine configuration file must be edited and then the UKI should be rebuilt.

* Go to `/etc/default/limine`. Find the `KERNEL_CMDLINE="..."` line and add the below boot parameters inside the quotes.
```bash
acpi_backlight=native

```
* Run the limine-update script

```bash
sudo limine-update
```

This reads the modified config file and builds a new UKI with the changes embedded. Reboot the system and it will use the new kernel parameters.

### 2. Boot Wallpaper
To add a boot screen wallpaper, copy the image file to the `/boot` directory and add the below line to the `limine.conf` file.
```bash
wallpaper: boot():/WALLPAPER_PATH
```

## Add Windows Boot Manager to Limine and Secure Boot

[Omarchy Dual-Boot Secure Boot Setup (Custom Keys Guide)](https://github.com/basecamp/omarchy/discussions/2296)

## Remove Default Apps and Webapps
[Omarchy Cleaner](https://github.com/maxart/omarchy-cleaner)

## Acknowledgement
A huge thanks to DHH and the contributors to the official [Omarchy GitHub](https://github.com/basecamp/omarchy) repository for creating such a great setup.
