# Omarchy Config Jounral

## <code style="color : Aqua">Dotfiles Management (Bare Git Repo)</code>

This system tracks dotfiles using a **bare Git repository** method. This allows tracking files directly in the `$HOME` directory without a `.git` folder cluttering it up.

### 1. The Bare Repository

The first step in this setup is to create the "bare" repository itself. This is done with:

```bash
git init --bare $HOME/.dotfiles
```

* **`--bare`**: This flag creates a repository that is only the Git database (the commit history, etc.). It has no "working tree" (the actual files), which is why it's "bare."

* **`$HOME/.dotfiles`**: This is simply the folder created to store this database. It's kept separate from the working files in $HOME.


### 1. The `.bashrc` Alias

To make this work, the following line was added to `~/.bashrc` and restart the shell:

```bash
alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
```

### 2. What This Alias Means

This creates a new terminal command called `config`. Running this command executes `git` with two special instructions:

* **`--git-dir=$HOME/.dotfiles/`**: Tells Git to use the `~/.dotfiles` folder as its database (instead of a `.git` folder).
* **`--work-tree=$HOME`**: Tells Git that the files it should track are in the `$HOME` directory.

This is why you must use `config add .bashrc` instead of `git add .bashrc`.

### 3. Initial Repository Setup

After creating the alias, the following one-time setup command must be run to make the repository usable:

```bash
config config --local status.showUntrackedFiles no
```

This command tells Git to hide all untracked files from the config status command. Without this, config status would show every file in your home directory (Downloads, Music, etc.) as "untracked", which is very messy.

### 4. Note on Adding Files

```bash
config add .
```
does not work, because the "work-tree" is the entire home directory (~/), running config add . would try to add every file and folder in the home directory to Git. Files must always be added explicitly, one by one (e.g., config add .config/hypr).


## <code style="color : Lime">Manual System Setup</code>

### 1. Bootloader 
To the kernel command line in `limine.conf`, add us

```bash
acpi_backlight=native
```
This is needed for brightness control on hybrid AMD/Nvidia GPU laptops (especially if `brightnessctl info` shows an NVIDIA device but the controls still don't work).

### 2. Boot Wallpaper
To add a boot screen wallpaper, copy the image file to the `/boot` directory and add the below line to the `limine.conf` file.
```bash
wallpaper: boot():/WALLPAPER_PATH
```