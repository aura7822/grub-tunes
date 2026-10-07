# GRUB Tune Lab

A single-page tool for writing, hearing and saving GRUB boot tunes. It is static HTML, so it runs on GitHub Pages with no build step.

## Run it on GitHub

1. Create a repository (for example `grub-tune-lab`) and push these files to `main`.
2. In the repository go to **Settings → Pages** and set **Source** to **GitHub Actions**.
3. The included workflow publishes the site. It will be at `https://<your-user>.github.io/grub-tune-lab/`.

To run locally, open `index.html` in a browser.

## Tune format

```
GRUB_INIT_TUNE="480 440 1 880 2"
                 │   │   │  │
                 │   │   │  └ duration of the 2nd note, in units
                 │   │   └ pitch of the 2nd note, Hz
                 │   └ duration of the 1st note, in units
                 └ tempo
```

- **Tempo**: one duration unit lasts `60 / tempo` seconds.
- **Pitch**: frequency in Hz. `0` is a rest. Values outside roughly 20 to 20000 Hz are silent.
- **Duration**: number of units. Seconds = `duration * 60 / tempo`.

`480 440 1` is a single 440 Hz beep of 0.125 s.

## Add a tune to GRUB

1. Edit `/etc/default/grub`:
   ```
   GRUB_INIT_TUNE="480 440 1"
   ```
2. Regenerate the config:
   ```
   sudo update-grub                                  # Debian, Ubuntu, Mint
   sudo grub2-mkconfig -o /boot/grub2/grub.cfg       # Fedora, RHEL, openSUSE
   sudo grub-mkconfig -o /boot/grub/grub.cfg         # Arch and most others
   ```
3. Reboot. The tune plays before the menu appears.

Test without rebooting through a full cycle: at the GRUB menu press `c` and run `play 480 440 1`.

For a tune on one entry only, add `play ...` inside a `menuentry` in `/etc/grub.d/40_custom` and regenerate the config.

## Troubleshooting

GRUB plays tunes through the PC speaker, not the sound card. Many laptops, modern desktops and virtual machines have none, or mute it. The browser preview uses a square wave to imitate the sound, so real hardware will differ.

## Adding tunes to the library

Edit the `LIB` array near the top of the script in `index.html`. Each entry is `[name, code, description]`.
