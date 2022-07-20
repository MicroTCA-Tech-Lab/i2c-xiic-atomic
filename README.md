# i2c-xiic-atomic

This is a fork of the [Xilinx](https://github.com/Xilinx/linux-xlnx) [`i2c-xiic` driver](https://github.com/Xilinx/linux-xlnx/blob/master/drivers/i2c/busses/i2c-xiic.c) with added support for [the `master_xfer_atomic` method](https://github.com/Xilinx/linux-xlnx/blob/cae7610ec31c822f88f92322d17339ad2d1fc995/include/linux/i2c.h#L512).

`master_xfer_atomic` is needed very late before reset/shutdown, in case the system needs to access a PMIC or other I2C peripheral to conduct the actual shutdown operation, at a stage when interrupts are already disabled (and the regular `master_xfer` method would not work due to its usage of interrupts or scheduling).

This driver allows to use the [Xilinx AXI IIC Bus Interface](https://www.xilinx.com/products/intellectual-property/axi_iic.html) on FPGA/SoC systems (like Zynq UltraScale+ MPSoC) to trigger power management actions, such as shutdown or reset, at a very late system stage.

## How to use

Disable the in-kernel version of `i2c-xiic` (`CONFIG_I2C_XILINX=n`) and build/install this driver like a regular kernel module.

## Supported kernels

This driver was forked from the `xilinx-v2022.1` release of [linux-xlnx](https://github.com/Xilinx/linux-xlnx), corresponding to a 5.15.0 kernel version. There are some version checks in place for better backward compatibility. It was tested on a `xilinx-v2020.2` based system running a 5.4.0 kernel.
