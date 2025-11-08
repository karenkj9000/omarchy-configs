# My Omarchy Configs
## Manual System Setup
* **Bootloader:** Add `acpi_backlight=native` to the kernel command line in `limine.conf`. This is often needed for brightness control on hybrid AMD/Nvidia GPU laptops (especially if `brightnessctl info` shows an NVIDIA device but the controls still don't work).

* **Boot Wallpaper:** To add a boot screen wallpaper, copy the image file to the `/boot` directory. Add the line `wallpaper: boot():/WALLPAPER_PATH` to the `limine.conf` file.
