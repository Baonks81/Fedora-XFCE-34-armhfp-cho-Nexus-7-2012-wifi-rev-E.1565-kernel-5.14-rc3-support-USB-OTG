# Fedora-XFCE-34-armhfp-cho-Nexus-7-2012-wifi-rev-E.1565-kernel-5.14-rc3-support-USB-OTG
Linux on Nexus 7 2012

File này chỉ được làm cho vui trong mùa dịch cúm Tàu 08/2021, ngày mai sẽ lên openSUSE-Leap-armv7l-15.3 cho Nexus 7 2012



Link download trên Google drive:



https://drive.google.com/drive/folders/1LbffvuU2B2r6ydbaTxvK2KNX_MdLxrrn?usp=sharing



Cài android-tools và fastboot trên Linux/Windows



Unlock bootloader cho Nexus 7, tham khảo trên mạng

https://wiki.postmarketos.org/wiki/Google_Nexus_7_2012_(asus-grouper)



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM



Do I have grouper or tilapia?







TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia





Which hardware revision of grouper do I have?



TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Cài TWRP 3.3.1-0 trở lên



Đưa máy về bootloader. Kết nối Nexus 7 2012 wifi vào PC/laptop. Chuẩn usb 1.0, 1.1, 2.0 dùng tốt. Chuẩn usb 3.0 dễ bị over cache push vào Nexus 7 2012, unsupport



$ sudo adb reboot bootloader



Flash boot image vào boot partition (đổi tên boot.img-asus-grouper thành boot.img)



$ fastboot flash boot boot.img



Vào TWRP 3.3.1-0 trở lên, vào Advance → Terminal



$ df



$ umount /dev/block/mmcblk0p__  <- fill partition number (# 2 lần)



Dùng lệnh adb để bung rootfs vào mmcblk0p__ trên PC/laptop



$ sudo adb start-server



Chuyển đến thư mục chứa rootfs image



$ sudo adb push Fedora-Xfce-34-1.2.armhfp+asus-grouper.img /dev/block/mmcblk0p__  <- fill partition number



grouper has likely data on /dev/block/mmcblk0p9 but make sure!
tilapia has likely data on /dev/block/mmcblk0p10 but make sure!



Các utils để trong /opt gồm các scripts:



- sysctl.conf tối ưu VMs và thông số kernel



- cpufreq.start tối ưu Ondemand governor



- temp_throttle để kìm hãm con ngựa Tegra thành Troy không quá nhiệt (hơn 60 độ kernel sẽ tự khởi động lại, để 59 độ là max, ở 53 và 55 độ ổn định)



- clear_RAM để remove thêm RAM nếu cần



***Fix sound ALC5642 cho tegra-rt5640***



$ sudo lsmod | grep "^snd" | cut -d " " -f 1



snd_soc_tegra30_i2s

snd_soc_tegra30_ahub

snd_soc_tegra_pcm

snd_soc_tegra_machine

snd_soc_tegra_utils

snd_soc_core

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ sudo nano /etc/modules



snd_soc_tegra30_i2s

snd_soc_tegra30_ahub

snd_soc_tegra_pcm

snd_soc_tegra_machine

snd_soc_tegra_utils

snd_soc_tegra_wm8903

snd_soc_core

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ reboot



Checking soc soundcard loaded:



$ sudo cat /proc/asound/card*/id



ALC5642



$ sudo alsa force-reload



$ alsamixer



Enable các thông số thiết lập (phím M hoặc phím mũi tên lên/xuống): "Speaker R" "Speaker L" "DAC MIXR INF1" "DAC MIXL INF1" "SPOL MIX DAC R1" "SPOL MIX DAC L1" "Stereo DAC MIXR DAC R1" "Stereo DAC MIXL DAC L1"



Wifi dùng wifi-menu/wpa_supplicant/iwd và network-manager



Bluetooth dùng được bluez5 và blueman



NFC dùng được với neard



Image Source: https://download.fedoraproject.org/pub/fedora/linux/releases/34/Spins/armhfp/images/



user/passwd:



root/fedora

root/fidora
