# IMX8-


# 🛠️ i.MX8QXP MEK Board: Linux Kernel Build & Flash Guide

A step-by-step guide to build and flash a custom Linux kernel and device tree on the **NXP i.MX 8QuadXPlus (i.MX8QXP) MEK** board.

---

## 📦 Image & Boot Setup

**Step-1:** Download the image file for the i.MX 8QuadXPlus (i.MX8QXP) MEK board from the [NXP website](https://www.nxp.com).

**Step-2:** Insert a formatted SD card into the i.MX8 board. Connect a USB cable between your laptop and the i.MX8 board. Open a terminal, go to the directory where the image file was downloaded, and flash it using this command:

> `sudo uuu -b sd_all imx-boot-imx8qxpc0mek-sd.bin-flash imx-image-full-imx8qxpc0mek.wic`

```bash
sudo uuu -b sd_all imx-boot-imx8qxpc0mek-sd.bin-flash imx-image-full-imx8qxpc0mek.wic
```

---

## 🔧 Linux Kernel Source & Build

**Step-3:** Clone the Linux kernel source code:

> `git clone https://github.com/nxp-imx/linux-imx -b lf-6.1.22-2.0.0`

```bash
git clone https://github.com/nxp-imx/linux-imx -b lf-6.1.22-2.0.0
```

**Step-4:** Navigate into the cloned source folder:

> `cd linux-imx`

```bash
cd linux-imx
```

**Step-5:** Create a new file named `imx8_build.sh`, paste your build script into it, and update any paths if needed.

**Step-6:** Make the script executable and run it:

> `chmod +x imx8_build.sh`  
> `./imx8_build.sh`

```bash
chmod +x imx8_build.sh
./imx8_build.sh
```

---

## 📁 Generated Output Files

**Step-7:** After building, you’ll find these files:

- Device Tree: `arch/arm64/boot/dts/freescale/imx8qxp-mek.dtb`
- Kernel Image: `arch/arm64/boot/Image`

---

## 📤 Transfer to Board

**Step-8:** Use `ifconfig` on the board to find its IP address:

> `ifconfig`

```bash
ifconfig
```

Then, from your local system, use `scp` to transfer the files:

> `scp imx8qxp-mek.dtb root@<ip-address>:~`  
> `scp Image root@<ip-address>:~`

```bash
scp imx8qxp-mek.dtb root@<ip-address>:~
scp Image root@<ip-address>:~
```

> 💡 If you get an SSH key error, reset it using:

> `ssh-keygen -f "/home/your-username/.ssh/known_hosts" -R "<ip-address>"`

```bash
ssh-keygen -f "/home/your-username/.ssh/known_hosts" -R "<ip-address>"
```

---

## 🧩 Replace in Boot Partition

**Step-9:** On the i.MX8 board, replace the default files with your new files:

> `cp imx8qxp-mek.dtb /run/media/boot-mmcblk1p1/imx8qxp-mek.dtb`  
> `cp Image /run/media/boot-mmcblk1p1/Image`

```bash
cp imx8qxp-mek.dtb /run/media/boot-mmcblk1p1/imx8qxp-mek.dtb
cp Image /run/media/boot-mmcblk1p1/Image
```

> 🧭 Don’t know the location of the files? Search with:

> `find / -name Image`

```bash
find / -name Image
```

---

## 🔁 Final Steps

**Step-10:** Reboot the board to apply changes:

> `reboot`

```bash
reboot
```

---

## ✏️ Optional: Modify & Rebuild Kernel

**Step-11:** To modify the device tree source, edit:

> `arch/arm64/boot/dts/freescale/imx8qxp-mek.dts`

After editing, go back to **Step-6**, re-run the build script, and repeat the transfer and replace steps (Step-8 to Step-10).

---

## ✅ Done!

You're now ready to build, customize, and deploy your own Linux kernel on the i.MX8QXP MEK board.
