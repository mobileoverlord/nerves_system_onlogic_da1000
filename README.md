# OnLogic da1000_atom System

[![CircleCI](https://circleci.com/gh/verypossible/nerves_system_onlogic_da1000_atom/tree/master.svg?style=svg)](https://circleci.com/gh/verypossible/nerves_system_onlogic_da1000_atom/tree/master)
[![Hex version](https://img.shields.io/hexpm/v/nerves_system_onlogic_da1000_atom.svg "Hex version")](https://hex.pm/packages/nerves_system_onlogic_da1000_atom)

This is the base Nerves System configuration for an [OnLogic DA-1000 with Intel Atom](https://www.onlogic.com/da-1000/) system.

![OnLogic da1000_atom Image](assets/images/da1000.jpg)
<br>

| Feature              | Description                     |
| -------------------- | ------------------------------- |
| CPU                  | Intel® AtomTM Processor E3845   |
| Memory               | Up to 8 GB 1066/1333 MHz DDR3L  |
| Storage              | 32 GB eMMC Flash and MicroSD    |
| Linux kernel         | 5.4.6                           |
| IEx terminal         | tty1                            |
| I/ O                 | RS-232/422/485 with Auto Flow Control, DB9|
| Display              | DVI-I                           |
| Ethernet             | Dual                            |
| Audio                | Stereo out w/ mic in            |


## Using

The most common way of using this Nerves System is create a project with `mix
nerves.new` and to export `MIX_TARGET=onlogic_da1000_atom`.

Then, change the `x86_64` system dependency to
`{:nerves_system_onlogic_da1000_atom, "~> 0.1", runtime: false, target: :onlogicv_da1000_atom}`

See the [Getting started
guide](https://hexdocs.pm/nerves/getting-started.html#creating-a-new-nerves-app)
for more information.

If you need custom modifications to this system for your device, clone this
repository and update as described in [Making custom
systems](https://hexdocs.pm/nerves/systems.html#customizing-your-own-nerves-system)

## Provisioning devices

This system supports storing provisioning information in a small key-value store
outside of any filesystem. Provisioning is an optional step and reasonable
defaults are provided if this is missing.

Provisioning information can be queried using the Nerves.Runtime KV store's
[`Nerves.Runtime.KV.get/1`](https://hexdocs.pm/nerves_runtime/Nerves.Runtime.KV.html#get/1)
function.

Keys used by this system are:

Key                    | Example Value     | Description
:--------------------- | :---------------- | :----------
`nerves_serial_number` | `"12345678"`      | By default, this string is used to create unique hostnames and Erlang node names. If unset, it defaults to part of the Ethernet adapter's MAC address.

The normal procedure would be to set these keys once in manufacturing or before
deployment and then leave them alone.

For example, to provision a serial number on a running device, run the following
and reboot:

```elixir
iex> cmd("fw_setenv nerves_serial_number 12345678")
```

This system supports setting the serial number offline. To do this, set the
`NERVES_SERIAL_NUMBER` environment variable when burning the firmware. If you're
programming MicroSD cards using `fwup`, the commandline is:

```sh
sudo NERVES_SERIAL_NUMBER=12345678 fwup path_to_firmware.fw
```

Serial numbers are stored on the MicroSD card so if the MicroSD card is
replaced, the serial number will need to be reprogrammed. The numbers are stored
in a U-boot environment block. This is a special region that is separate from
the application partition so reformatting the application partition will not
lose the serial number or any other data stored in this block.

Additional key value pairs can be provisioned by overriding the default provisioning.conf
file location by setting the environment variable
`NERVES_PROVISIONING=/path/to/provisioning.conf`. The default provisioning.conf
will set the `nerves_serial_number`, if you override the location to this file,
you will be responsible for setting this yourself.
