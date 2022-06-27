# Ubuntu 解决耳机驱动异常
```shell
echo "options snd_hda_intel probe_mask=1" | sudo tee /etc/modprobe.d/tensorbook.conf

sudo update-initramfs -u -k all

```
重新启动即可。