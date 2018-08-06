
Don't boot, but jump right into bash:

The `-serial stdio` argument redirects IO to the guest, but control characters are captured by qemu.

```bash
qemu-system-arm -kernel kernel-qemu-4.9.59-stretch \
				-cpu arm1176 \
				-m 256 \
				-M versatilepb \
				-dtb versatile-pb.dtb \
				-no-reboot \
				-append "root=/dev/sda2 panic=1 rootfstype=ext4 rw init=/bin/bash" \
				-serial stdio \
				-hda {img file}
```

Boot the system:

This uses `console=ttyAMA0,115200` to set up the console. It has the result of passing control characters to the guest.

```bash
qemu-system-arm -kernel kernel-qemu-4.9.59-stretch \
                  -cpu arm1176 \
                  -m 256 \
                  -M versatilepb \
                  -dtb versatile-pb.dtb \
                  -net nic,model=ne2k_pci -net user,hostfwd=tcp::5022-:22 \
                  -no-reboot \
                  -nographic \
                  -append "root=/dev/sda2 console=ttyAMA0,115200 panic=1 rootfstype=ext4 rw" \
                  -drive "file={img file},index=0,media=disk,format=raw"
```
