# Gprinter GP-1324D CUPS Driver

PPD and filter for the Gprinter GP-1324D thermal label printer on Linux and macOS.

It may work for other generic 4x6" thermal printers that use tspl.

## Dependencies

The filter binary requires `libcrypt.so.1` which may not be present on newer distros.

### Ubuntu/Debian

    sudo apt install libxcrypt-compat

### Arch

    sudo pacman -S libxcrypt-compat

### Fedora/RHEL

    sudo dnf install libxcrypt-compat

### macOS

May require `libxcrypt`:

    brew install libxcrypt

Unconfirmed whether this is required — please open an issue if you can verify.

## Installation

The binaries are platform-specific and will not run on the wrong OS.

### Linux

1. Copy `rastertotspl` to `/usr/lib/cups/filter/`
2. `sudo chmod 755 /usr/lib/cups/filter/rastertotspl`
3. Copy the Linux PPD to `/etc/cups/ppd/`

### macOS

1. Copy `rastertotspl` to `/usr/libexec/cups/filter/`
2. Copy the macOS PPD to `/Users/Shared/` so it appears in the printer dialog
3. Add printer via System Settings → Printers & Scanners → MyPrinterName
4. Select the PPD inside `/Users/Shared/` via the Use dropdown → Other...

## Network Printing (macOS → Linux)

If using macOS as a client with Linux as the print server, macOS will intercept
the job and mangle it before sending. Edit [`macos/gprinter_1324D.ppd#L29`](macos/gprinter_1324D.ppd#L29) to

    `*cupsFilter2: "application/pdf application/pdf 0 -"`

This passes the PDF unmodified to Linux which handles all filtering itself.

## Troubleshooting

- **Linux logs:** `/var/log/cups/error_log`
- **macOS logs:** `/private/var/log/cups/error_log`
- Enable debug logging: `cupsctl --debug-logging`

## Credit

[PPD driver](https://tifan.net/blog/2018/03/27/gprinter-thermal-printer-unix-driver/)

[Linux installation instructions](https://github.com/TheGU/ubuntu_thermal_printer_setup/tree/main)

[rastertospl macos binary, requires extracting .pkg](https://fs.tscprinters.com/en/dl/3/3445)

[Alternatively search for drivers here. Mobile Printers -> Alpha Series 4-Inch Performance Mobile RFID Printers -> Driver](https://emea.tscprinters.com/en/downloads)
