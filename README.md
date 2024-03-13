[![PyPI](https://badge.fury.io/py/dtsh.svg)](https://badge.fury.io/py/dtsh)

[**Getting Started Guide**](https://dottspina.github.io/dtsh/getting-started.html)

[**Handbook**](https://dottspina.github.io/dtsh/handbook.html)


# DTSh - A Devicetre Shell

**DTSh** is a Devicetree Source ([DTS](https://devicetree-specification.readthedocs.io/en/latest/chapter6-source-language.html)) files viewer with a shell-like command line interface:

- *navigate* and *visualize* the devicetree
- *search* for devices, bindings, buses or interrupts with flexible criteria
- redirect command output to files (text, HTML, SVG) to *document* hardware configurations
  or illustrate notes
- *rich* Textual User Interface, command line auto-completion, command history, user themes

You can use it with:

- all DTS files generated by **Zephyr** at build-time (aka `build/zephyr/zephyr.dts`)
- arbitrary DTS files with bindings compatible with Zephyr's [Devicetre bindings syntax](https://docs.zephyrproject.org/latest/build/dts/bindings-syntax.html)

![dtsh](doc/img/buses.png)


## Status

This project started as a Proof of Concept of a simple tool that could *open and understand* the famous `zephyr.dts` file [generated](https://docs.zephyrproject.org/latest/build/cmake/index.html#configuration-phase) at built-time.
Source code and documentation for this prototype (DTSh 0.1.x) are still available from the [main](https://github.com/dottspina/dtsh/tree/main) branch of this repository.

This branch (`dtsh-next`) mirrors and packages the new code base which serves as a proposal to upstream DTSh as a [Zephyr extension to West](https://docs.zephyrproject.org/latest/develop/west/index.html) (would be `west dtsh`): [RFC - DTSh, shell-like interface with Devicetree](https://github.com/zephyrproject-rtos/zephyr/pull/59863)

This is the branch that shall be installed, used and commented on: it is now the **default** branch of this repository.

**Latest release**: DTSh [0.2.1](https://github.com/dottspina/dtsh/releases/tag/v0.2.1) ([PyPI](https://pypi.org/project/dtsh/0.2.1/)).

> [!NOTE]
> Although this branch *reflects* the state of the PR's [content](https://github.com/dottspina/zephyr/tree/rfc-dtsh), it's not an actual mirror:
> - while the implementation in the RFC can *find* the [python-devicetree](https://github.com/zephyrproject-rtos/zephyr/tree/main/scripts/dts/python-devicetree) library the same way other [Zepyhr's devicetree tool](https://github.com/zephyrproject-rtos/zephyr/tree/main/scripts/dts) do, DTSh has to re-distribute snapshots of this library when published on PyPI (see [Possible multiple different versions of python-devicetree](https://github.com/dottspina/dtsh/issues/2))
> - this code base does not include the West integration which we couldn't install from PyPI
> - changes may happen here first, and be *backported* to the RFC, or vice versa


## DTSh in a Nutshell

Here are some one-liner steps for the very impatient:

- install DTSh in *your* Zephyr development environment
- generate the DTS for the board you're interested in
- open this devicetree in DTSh

**Please** refer to the [project's documentation](https://dottspina.github.io/dtsh/), where you'll find:

- a [Getting Started Guide](https://dottspina.github.io/dtsh/getting-started.html): install, configure and run DTSh
- the [DTSh Handbook](https://dottspina.github.io/dtsh/handbook.html): the shell and its *rich* Textual User Interface, reference manual of built-in commands, numerous usage examples

There's also a beginners friendly blog post at golioth: [DTSh – A Devicetree viewer for Zephyr](https://blog.golioth.io/dtsh-a-devicetree-viewer-for-zephyr/) 


### Install DTSh

This method installs the latest DTSh version in the Python virtual environment that belongs to *your* West workspace.

From the same prompt where you usually enter `west` commands:

```
$ pip install -U dtsh
$ dtsh -h
usage: dtsh [-h] [-b DIR] [-u] [--preferences FILE] [--theme FILE] [DTS]

shell-like interface with Devicetree

optional arguments:
  -h, --help            show this help message and exit

open a DTS file:
  -b DIR, --bindings DIR
                        directory to search for binding files
  DTS                   path to the DTS file

user files:
  -u, --user-files      initialize per-user configuration files and exit
  --preferences FILE    load additional preferences file
  --theme FILE          load additional styles file
```


### Generate the DTS

```
$ cd zephyr/samples/sensor/bme680
$ west build -b nrf52840dk_nrf52840
```

> [!TIP]
> - Actually building the sample is not necessary, the configuration phase is sufficient: `cmake -B build -DBOARD=nrf52840dk_nrf52840`
> - Replace `nrf52840dk_nrf52840` with the *name* of the [board](https://docs.zephyrproject.org/latest/boards/index.html) you're interested in.
> - Replace `sensor/bme680` with any sample supported by your board.

### Open the devicetree

```
$ cd zephyr/samples/sensor/bme680
$ dtsh
dtsh (0.2.1): A Devicetree Shell
How to exit: q, or quit, or exit, or press Ctrl-D

/
> ls -l
 Name              Labels          Binding
 ───────────────────────────────────────────────────────
 chosen
 aliases
 soc
 pin-controller    pinctrl         nordic,nrf-pinctrl
 entropy_bt_hci    rng_hci         zephyr,bt-hci-entropy
 sw-pwm            sw_pwm          nordic,nrf-sw-pwm
 cpus
 leds                              gpio-leds
 pwmleds                           pwm-leds
 buttons                           gpio-keys
 connector         arduino_header  arduino-header-r3
 analog-connector  arduino_adc     arduino,uno-adc
```
