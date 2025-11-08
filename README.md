# My Omarchy Configs
## Manual System Setup
* **Bootloader:** Add `acpi_backlight=native` to the kernel command line in `limine.conf`. This is often needed for brightness control on hybrid AMD/Nvidia GPU laptops (especially if `brightnessctl info` shows an NVIDIA device but the controls still don't work).
