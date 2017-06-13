ansible-grub
==============

Deploy grub default configuration and make grub.cfg

Requirements
---------------

* Ansible 2.0;

Example configuration
-------------------------

```yaml
---
# Set to true to execute grub-mkconfig and override real boot config.
grub_deploy_grub_conf: 'true'

grub:
# The file /etc/default/grub controls the operation of grub-mkconfig.
# Valid keys in /etc/default/grub are as follows:
#
# The default menu entry. This may be a number, in which case it identifies the
# Nth entry in the generated menu counted from zero, or the title of a menu
# entry, or the special string 'saved'. Using the id may be useful if you want
# to set a menu entry as the default even though there may be a variable number
# of entries before it. Previously it was documented the way to use entry title.
# While this still works it's not recommended since titles often contain
# unstable device names and may be translated. If you set this to 'saved', then
# the default menu entry will be that saved by 'GRUB_SAVEDEFAULT' or
# grub-set-default. This relies on the environment block, which may not be
# available in all situations. The default is '0'.
- default: '0'
# If this option is set to 'true', then, when an entry is selected, save it as
# a new default entry for use by future runs of GRUB. This is only useful if
# 'GRUB_DEFAULT=saved'; it is a separate option because 'GRUB_DEFAULT=saved'
# is useful without this option, in conjunction with grub-set-default. Unset by
# default. This option relies on the environment block, which may not be
# available in all situations.
  savedefault: 'false'
# Boot the default entry this many seconds after the menu is displayed, unless
# a key is pressed. The default is '5'. Set to '0' to boot immediately without
# displaying the menu, or to '-1' to wait indefinitely.
  timeout: '5'
# If this option is unset or set to 'menu', then GRUB will display the menu and
# then wait for the timeout set by 'GRUB_TIMEOUT' to expire before booting the
# default entry. Pressing a key interrupts the timeout. If this option is set to
# 'countdown' or 'hidden', then, before displaying the menu, GRUB will wait for
# the timeout set by 'GRUB_TIMEOUT' to expire. If ESC is pressed during that
# time, it will display the menu and wait for input. If a hotkey associated with
# a menu entry is pressed, it will boot the associated menu entry immediately.
# If the timeout expires before either of these happens, it will boot the
# default entry. In the 'countdown' case, it will show a one-line indication of
# the remaining time.
  timeout_style: 'menu'
# Set by distributors of GRUB to their identifying name. This is used to
# generate more informative menu entry titles.
  distributor: 'Arch'
# Select the terminal input device. You may select multiple devices here. Valid
# terminal input names depend on the platform, but may include 'console' (native
# platform console), 'serial' (serial terminal), 'serial_<port>' (serial
# terminal with explicit port selection), 'at_keyboard' (PC AT keyboard), or
# 'usb_keyboard' (USB keyboard using the HID Boot Protocol, for cases where the
# firmware does not handle this). The default is to use the platform’s native
# terminal input.
  terminal_input:
  - 'console'
# Select the terminal output device. You may select multiple devices here,
# separated by spaces. Valid terminal output names depend on the platform, but
# may include 'console' (native platform console), 'serial' (serial terminal),
# 'serial_<port>' (serial terminal with explicit port selection), 'gfxterm'
# (graphics-mode output), 'vga_text' (VGA text output), 'mda_text' (MDA text
# output), 'morse' (Morse-coding using system beeper) or 'spkmodem' (simple data
# protocol using system speaker). The default is to use the platform’s native
# terminal output.
  terminal_output:
  - 'console'
# If this option is set, it overrides both 'GRUB_TERMINAL_INPUT' and
# 'GRUB_TERMINAL_OUTPUT' to the same value.
  terminal:
  - 'console'
# A command to configure the serial port when using the serial console.
# Defaults to 'serial'.
  serial_command: 'serial'
# Normally, grub-mkconfig will generate menu entries that use universally-unique
# identifiers (UUIDs) to identify the root filesystem to the Linux kernel, using
# a 'root=UUID=...' kernel parameter. This is usually more reliable, but in some
# cases it may not be appropriate. To disable the use of UUIDs, set this option
# to 'true'.
  disable_linux_uuid: 'false'
# If this option is set to 'true', disable the generation of recovery mode menu
# entries.
  disable_recovery: 'true'
# If graphical video support is required, either because the 'gfxterm' graphical
# terminal is in use or because 'GRUB_GFXPAYLOAD_LINUX' is set, then
# grub-mkconfig will normally load all available GRUB video drivers and use the
# one most appropriate for your hardware. If you need to override this for some
# reason, then you can set this option. After grub-install has been run, the
# available video drivers are listed in /boot/grub/video.lst.
  video_backend: ''
# Set the resolution used on the 'gfxterm' graphical terminal. Note that you can
# only use modes which your graphics card supports via VESA BIOS Extensions
# (VBE), so for example native LCD panel resolutions may not be available. The
# default is 'auto', which tries to select a preferred resolution.
  gfxmode: 'auto'
# Set a background image for use with the 'gfxterm' graphical terminal. The
# value of this option must be a file readable by GRUB at boot time, and it must
# end with '.png', '.tga', '.jpg', or '.jpeg'. The image will be scaled if
# necessary to fit the screen.
  background: ''
# Set a theme for use with the ‘gfxterm’ graphical terminal.
  theme: ''
# You can set the menu colors in GRUB.
  color_normal: 'light-red/black'
  color_highlight: 'light-red/red'
# Set to 'text' to force the Linux kernel to boot in normal text mode, 'keep' to
# preserve the graphics mode set using 'GRUB_GFXMODE', 'widthxheight'['xdepth']
# to set a particular graphics mode, or a sequence of these separated by commas
# or semicolons to try several modes in sequence. Depending on your kernel, your
# distribution, your graphics card, and the phase of the moon, note that using
# this option may cause GNU/Linux to suffer from various display problems,
# particularly during the early part of the boot sequence. If you have problems,
# set this option to 'text' and GRUB will tell Linux to boot in normal text mode.
  gfxpayload_linux: 'keep'
# Normally, grub-mkconfig will try to use the external os-prober program, if
# installed, to discover other operating systems installed on the same system
# and generate appropriate menu entries for them. Set this option to 'true' to
# disable this.
  disable_os_prober: 'false'
# List of space-separated FS UUIDs of filesystems to be ignored from os-prober
# output. For efi chainloaders it’s <UUID>@<EFI FILE>.
  os_prober_skip_list:
  - '65168eab-b6ef-4bd0-8da0-c00acaa8e37d'
  - '5df7e569-230b-4579-bd26-358c2bc60cad'
# Normally, grub-mkconfig will generate top level menu entry for the kernel
# with highest version number and put all other found kernels or alternative
# menu entries for recovery mode in submenu. For entries returned by os-prober
# first entry will be put on top level and all others in submenu. If this option
# is set to 'y', flat menu with all entries on top level will be generated
# instead. Changing this option will require changing existing values of
# 'GRUB_DEFAULT', 'fallback' and 'default' environment variables as well as
# saved default entry using grub-set-default and value used with grub-reboot.
  disable_submenu: ''
# If set to 'y', grub-mkconfig and grub-install will check for encrypted disks
# and generate additional commands needed to access them during boot. Note that
# in this case unattended boot is not possible because GRUB will wait for
# passphrase to unlock encrypted container.
  enable_cryptodisk: ''
# Play a tune on the speaker when GRUB starts. This is particularly useful for
# users unable to see the screen. The value of this option is passed directly
# to 'play'.
  init_tune: ''
# If this option is set, GRUB will issue a badram command to filter out
# specified regions of RAM.
  badram: ''
# This option may be set to a list of GRUB module names. Each module will be
# loaded as early as possible, at the start of grub.cfg.
  preload_modules:
  - 'part_gpt'
  - 'part_msdos'
# Command-line arguments to add to menu entries for the Linux kernel.
  cmdline_linux:
  - 'vconsole.keymap=us'
  - 'crashkernel=auto'
# Unless 'GRUB_DISABLE_RECOVERY' is set to 'true', two menu entries will be
# generated for each Linux kernel: one default entry and one entry for recovery
# mode. This option lists command-line arguments to add only to the default menu
# entry, after those listed in 'GRUB_CMDLINE_LINUX'.
  cmdline_linux_default:
  - 'elevator=noop'
  - 'acpi_backlight=video'
# As 'GRUB_CMDLINE_LINUX' and 'GRUB_CMDLINE_LINUX_DEFAULT', but for NetBSD.
  cmdline_netbsd: ''
  cmdline_netbsd_default: ''
# As 'GRUB_CMDLINE_LINUX', but for GNU Mach.
  cmdline_gnumach: ''
# The values of these options are passed to Xen hypervisor Xen menu entries,
# for all respectively normal entries.
  cmdline_xen: ''
  cmdline_xen_default: ''
# The values of these options replace the values of 'GRUB_CMDLINE_LINUX' and
# 'GRUB_CMDLINE_LINUX_DEFAULT' for Linux and Xen menu entries.
  cmdline_linux_xen_replace: ''
  cmdline_linux_xen_replace_default: ''
```
