### 软设备恢复说明（中文）（过期文档）

由于原RMK固件移除了SoftDevice，在刷入ZMK固件之前，必须先恢复SoftDevice：

> 注意， zmk 支持 no-sd 模式， 也就是和 rmk 一样， 把 sd 分区去掉（sd 区块从原来的150kb+ 减少到 4kb大小）
> 目前 cornix zmk 固件已经改成 no-sd flash 布局， 不再需要恢复 sd 

1. **先用SoftDevice恢复包刷入**，地址：[bootloader/s140_restore.uf2](./s140_6.1.1_restore_no_mbr.uf2)
2. **Recovery模式进入方法**：双击RESET键进入bootloader
3. **刷入zmk固件**：使用UF2拖拽方式刷入

**注意顺序**：先SoftDevice → 后ZMK固件

    
### Extracting SoftDevice (s140) for nRF52840

Download the bootloader for nRF52840 from:

- Official repository: https://github.com/joric/nrfmicro/wiki/Bootloader
- This bootloader is compatible with nRF52840-based keyboards using ZMK firmware

These commands extract the SoftDevice (s140) from the bootloader HEX file and convert it to UF2 format for flashing:

```bash
# Convert HEX to binary
arm-none-eabi-objcopy -I ihex -O binary pca10056_bootloader-0.2.11_s140_6.1.1.hex full_flash.bin

# Extract SoftDevice section
# Skip bootloader (4096 bytes)
# Count matches SoftDevice size (155648 bytes)
dd if=full_flash.bin of=s140_sd_only.bin bs=1 skip=4096 count=155648

# Convert to UF2 format for flashing
python3 uf2conv.py s140_sd_only.bin -f 0xada52840 -b 0x1000 -c -o s140_restore.uf2
```

### Extracting SoftDevice (s130) for nRF51

Command to extract SoftDevice (s130) section:

```bash
# Skip bootloader (4096 bytes)
dd if=full_flash.bin of=s132_sd_only.bin bs=1 skip=4096 count=155648
```
