## Hardware
* Raspberry Pi 5 8GB
* Geekworm X1001 PCIe to M.2 Key-M NVMe SSD PIP TOP
* WD_BLACK SN770 SSD 1 TB M.2 2280 NVMe SSD
* [Sabrent USB Nvme enclosure](https://sabrent.com/collections/nvme-enclosures/products/ec-snve) - used only for imaging and setup
* SD card or USB drive

## Setup
This guide lists steps required to boot Home Assistant from an NVMe drive on a Raspberry Pi 5 and confirmed to work.

#### 1. Configure raspberry pi firmware.<br>
As far as I understand, some variants of the X1001 adapter are missing some auto detection feature and the firmware requires one additional parameter.
* Write the raspbian os image to either an SD card or USB drive using your preferred method, i.e. dd, Raspberry Pi Imager.
* Boot and [update pi firmware](https://www.raspberrypi.com/documentation/computers/os.html#rpi-update) to the latest version.
* Run
```
sudo rpi-eeprom-config --edit
```
and add `PCIE_PROBE=1` to the config.<br>
* Power off your pi and pull out the USB or SD card.

#### 2. Insert the NVMe drive into the USB enclosure and write the Home Assistant image onto the drive.<br>
Don't pull the drive out of the enclosure just yet.

#### 3. Install the SSD top onto the raspberry pi following manufacturer's manual.

#### 4. Configure Hassio boot
If you were to connect the NVMe drive onto the Geekworm adapter before you're doing the following steps, you'd see various IO errors and HASS would be unable to start.
* Connect the USB enclosure with the NVMe drive still mounted in it to the raspberry pi and boot up Home Assistant.<br>
* Mount the boot partition:
```
sudo mkdir /mnt/boot
sudo mount -t vfat /dev/sda1 /mnt/boot
```
* Run
```
sudo nano /mnt/boot/config.txt
```
and add the following 2 lines to the config
```
dtparam=pciex1
dtparam=pciex1_gen=3 
```
* Shutdown home assistant<br>
If you haven't gone through the HASS setup and don't have access to the Settings UI, you can run the following command to trigger a shutdown:
```
sudo shutdown -h now
```
#### 5. Boot from NVMe
* Disconnect the USB enclosure from your pi and pull the NVMe drive out.
* Mount the drive onto the PCIe adapter.
* Power up your pi.

At this point you should expect your pi to boot into Hassio within seconds without any sort of errors printed on your screen.
