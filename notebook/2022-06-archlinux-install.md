# ArchLinux 安装

粗略记一下 ArchLinux 安装过程，使用 btrfs 和 wayfire。

1. 禁用 reflector

```shell
systemctl stop reflector.service
```

2. 更新系统时钟

```shell
timedatectl set-ntp true
```

3. 更换国内镜像

```shell
vim /etc/pacman.d/mirrorlist

# China
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.bfsu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

4. 分区格式化

```shell
mkfs.vfat /dev/sdxn
mkswap /dev/sdxn
mkfs.btrfs -L myArch /dev/sdxn
mount -t btrfs -o compress=zstd /dev/sdxn /mnt
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
umount /mnt
```

5. 挂载

```shell
mount -t btrfs -o subvol=/@,compress=zstd /dev/sdxn /mnt
mkdir /mnt/home
mount -t btrfs -o subvol=/@home,compress=zstd /dev/sdxn /mnt/home
mkdir -p /mnt/boot/efi
mount /dev/sdxn /mnt/boot/efi
swapon /dev/sdxn
```

6. 安装系统

```shell
pacstrap /mnt base base-devel linux linux-firmware
pacstrap /mnt dhcpcd iwd vim sudo zsh zsh-completions
```

7. 生成 fstab

```shell
genfstab -U /mnt > /mnt/etc/fstab
```

8. chroot

```shell
arch-chroot /mnt
```

9. 设置主机名

```shell
vim /etc/hostname

vim /etc/hosts
127.0.0.1	localhost
::1		    localhost
127.0.1.1	xiaohui
```

10. 设置时区

```shell
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

11. 硬件时间

```shell
hwclock --systohc
```

12. 设置locale

```shell
vim /etc/locale.gen
locale-gen
echo 'LANG=en_US.UTF-8'  > /etc/locale.conf
```

13. 设置密码

```shell
passwd root
```

14. 安装微码

```shell
pacman -S intel-ucode
pacman -S amd-ucode
```

15. 安装引导程序

```shell
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=ARCH
grub-mkconfig -o /boot/grub/grub.cfg
```

16. 完成安装

```shell
exit
umount -R /mnt
reboot
```

17. 添加用户

```shell
pacman -Syyu
useradd -m -G wheel -s /bin/bash myusername
passwd myusername
EDITOR=vim visudo
```

18. 添加 archlinuxcn 镜像

```shell
vim /etc/pacman.conf

[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

19. 偷懒，下载 KDE 桌面环境

```shell
pacman -S plasma-meta konsole dolphin
systemctl enable sddm # 可选，我喜欢 startx
sudo systemctl disable --now iwd
sudo systemctl enable --now NetworkManager
sudo pacman -S ntfs-3g
sudo pacman -S adobe-source-han-serif-cn-fonts wqy-zenhei
sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
sudo pacman -S firefox
sudo pacman -S ark
sudo pacman -S packagekit-qt5 packagekit appstream-qt appstream
sudo pacman -S gwenview
sudo pacman -S archlinuxcn-keyring
sudo pacman -S paru
```

20. 安装输入法

```shell
sudo pacman -S fcitx5-im
sudo pacman -S fcitx5-chinese-addons

vim ~/.pam_environment

INPUT_METHOD DEFAULT=fcitx5
GTK_IM_MODULE DEFAULT=fcitx5
QT_IM_MODULE DEFAULT=fcitx5
XMODIFIERS DEFAULT=\@im=fcitx5
SDL_IM_MODULE DEFAULT=fcitx
```

21. 显卡驱动（只用核显）

```shell
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

22. 其他配置

可以看 wiki,想装啥装啥，就不写了。

23. 安装 wayfire

```shell
sudo pacman -S wayfire-lily-git alacritty waybar wofi xorg-xwayland xorg-xlsclients qt5-wayland glfw-wayland
```

24. dotconfig

[huihu-moe/dotconfig](https://github.com/huihu-moe/dotconfig)