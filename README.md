# Linux 操作系统还原操作说明 v0.1

注：只针对使用再生龙工具备份的系统文件

注意事项：

- 初学模式按源盘分区表原样还原（本例是 1G EFI + 1.8T 系统分区），目标盘容量需 ≥ 源盘。不是看镜像文件有多大，也不是看已用空间。
- 目标盘比源盘小、或换机后硬盘容量/型号不同时，再生龙整盘还原可能失败或影响引导。
- 建议另外用 Systemback 再制作一份系统 ISO 保存。容量对不上、再生龙无法整盘写入时，可用该 ISO 安装到目标盘（目标盘只要放得下系统占用即可）。

## 1 制作再生龙镜像U盘

使用 rufus 写入工具，将“再生龙v3.0.1-8-快速还原.iso” 镜像文件写入U盘。分区类型选择 GPT，文件系统选择 NTFS（后续要把镜像保存在这张盘上，不要用 FAT32），其它可保持默认。烧录过程中可能会失败，多重复尝试几次，如图 1 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/rufus.png" alt="图1" style="max-width:100%">
<p>图1</p>
</div>

## 2.为另一个磁盘制作再生龙镜像文件

BIOS 中将再生龙 U 盘第一启动项（Boot Option #1），保存退出后从 U 盘启动，如图 2 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_002.jpeg" alt="图2" style="max-width:100%">
<p>图2</p>
</div>

进入该镜像后，会弹出 GRUB 菜单。默认项 `1.Auto(Restore)` 为自动还原，3s内若不操作会自动选择第一项。制作镜像请不要直接回车，方向键选择另一项，如图 3 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_003.png" alt="图3" style="max-width:100%">
<p>图3</p>
</div>

用方向键选 `2.live(Restore and Backup)`，回车`Enter`进入备份/还原交互界面，如图 4 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_004.jpeg" alt="图4" style="max-width:100%">
<p>图4</p>
</div>

方向键选 `Start_Clonezilla 使用再生龙`，回车`Enter`确定，如图 5 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_005.jpeg" alt="图5" style="max-width:100%">
<p>图5</p>
</div>

方向键选 `device-image`（硬盘/分区存成镜像，或从镜像还原），回车`Enter`确定，如图 6 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_006.jpeg" alt="图6" style="max-width:100%">
<p>图6</p>
</div>

方向键切换镜像存放位置选 `local_dev`（本机硬盘或 U 盘），回车`Enter`确定，屏幕下方会弹出文字说明，如图 7、图 8 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_007.jpeg" alt="图7" style="max-width:100%">
<p>图7</p>
</div>

屏幕下方的文字说明如下，如图 8 所示。

如果要将镜像文件保存到外部U盘，则需要按提示插入用于存放镜像的 U 盘，等约 5 秒后按`Enter`；

如果不需要，将镜像文件保存到当前再生龙U盘，可以直接回车`Enter`确定；

本文示例是将镜像文件保存到当前再生龙U盘，这里直接回车`Enter`确定。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_008.jpeg" alt="图8" style="max-width:100%">
<p>图8</p>
</div>

系统扫描磁盘。确认能看到系统盘（如 `nvme0n1`）和 U 盘（如 `sda`）后，

按`Ctrl+c`切回主窗口继续，如图 9 所示。

本例扫描到的 U 盘是 62.9GB 的 Fanxiang（`sda`），与图 1 制作启动盘时截到的 16GB 盘不是同一块；后文保存镜像用的是这块容量更大的盘，尽量用大容量的U盘。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_009.jpeg" alt="图9" style="max-width:100%">
<p>图9</p>
</div>

选择要挂载的镜像存储盘。

这里对应图 8中，是否插入了另一个盘作为镜像存储/读取路径，如果插入了并在图 9中识别到了，这里要选择那个盘；

注意选择的盘，存储大小需要足够大。

本文默认都是再生龙U盘路径，方向键选 U 盘数据分区（本例为 `sda1`，约 58.6G NTFS），回车`Enter`确定，也就是名字为再生龙的分区。不要选系统盘分区（`nvme0n1p*`），如图 10 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_010.jpeg" alt="图10" style="max-width:100%">
<p>图10</p>
</div>

文件系统检查，方向键选 `no-fsck`（跳过检查），回车`Enter`确定，如图 11 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_011.jpeg" alt="图11" style="max-width:100%">
<p>图11</p>
</div>

进入目录浏览器后，当前目录已是 `/`（U 盘根目录）。选 `ABORT` 表示退出浏览器并沿用当前目录。不要选标有 `CZ_IMG` 的已有镜像目录，如图 12 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_012.jpeg" alt="图12" style="max-width:100%">
<p>图12</p>
</div>

目录确认无误后，按`Tab`键，然后方向键 选 `<Done>`，然后`Enter`回车，如图 13 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_013.jpeg" alt="图13" style="max-width:100%">
<p>图13</p>
</div>

屏幕底侧会弹出文字，看到 `/dev/sda1` 已挂到 `/home/partimag` 即表示仓库挂载成功，按 Enter 继续，如图 14 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_014.jpeg" alt="图14" style="max-width:100%">
<p>图14</p>
</div>

方向键选 `Beginner 初学模式`，用默认参数即可，如图 15 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_015.jpeg" alt="图15" style="max-width:100%">
<p>图15</p>
</div>

方向键选 `savedisk`（把整块硬盘存成镜像），回车`Enter`确定，如图 16、图 17 所示。

如果再生龙U盘的`/home/partimag/`路径下没有再生龙格式的镜像，或者再生龙U盘没有挂载外部存储镜像的盘，如图16，选项中是没有还原镜像选项的。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_016.jpeg" alt="图16" style="max-width:100%">
<p>图16</p>
</div>

如果再生龙U盘的`/home/partimag/`路径下有再生龙格式的镜像，或者再生龙U盘挂载了外部存储镜像的盘，如图17，选项中是有还原镜像选项的，这个选项对应后续还原镜像时所走的步骤(在一开始选择的不是`1.Auto`而是与本节相同的`2.live`，需要在这里选择`restoredisk`)，由于本节是制作镜像，所以需要选择`savedisk`。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_017.jpeg" alt="图17" style="max-width:100%">
<p>图17</p>
</div>

输入镜像名称（默认为日期，可改），回车`Enter`确定，如图 18 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_018.jpeg" alt="图18" style="max-width:100%">
<p>图18</p>
</div>

源盘选要备份的系统盘（本例 `nvme0n1`，空格打 `*`，如果只有一个系统盘可以直接`Enter`，多个需要方向键选择然后标`*`），不要选 U 盘，`Enter`回车选择要备份的硬盘，然后会弹到`<确定>`，然后继续`Enter`，如图 19 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_019.jpeg" alt="图19" style="max-width:100%">
<p>图19</p>
</div>

压缩方式选择 `-zip`（默认 gzip），方向键选择，回车`Enter`确定，如图 20 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_020.jpeg" alt="图20" style="max-width:100%">
<p>图20</p>
</div>

源分区检查选 `-sfsck`（跳过 fsck），方向键选择，回车`Enter`确定，如图 21 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_021.jpeg" alt="图21" style="max-width:100%">
<p>图21</p>
</div>

保存后是否校验镜像：方向键选 `-scs` 然后回车`Enter`跳过，可缩短耗时；要更稳可选“是”，如图 22 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_022.jpeg" alt="图22" style="max-width:100%">
<p>图22</p>
</div>

是否对镜像加密，方向键选 `-senc`（不加密），回车`Enter`确定，如图 23 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_023.jpeg" alt="图23" style="max-width:100%">
<p>图23</p>
</div>

方向键选 `-p choose`（结束后再选重启/关机），回车`Enter`确定，如图 24 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_024.jpeg" alt="图24" style="max-width:100%">
<p>图24</p>
</div>

屏幕下方会列出本次即将执行的完整命令。核对其中的源盘和镜像名无误后按`Enter`继续，如图 25 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_025.jpeg" alt="图25" style="max-width:100%">
<p>图25</p>
</div>

最终确认提示 `您确认要继续执行？(y/n)`，输入 `y` 回车开始备份，如图 26 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_026.jpeg" alt="图26" style="max-width:100%">
<p>图26</p>
</div>

Partclone 开始克隆分区，进度到 100% 即该分区完成，如图 27 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_027.jpeg" alt="图27" style="max-width:100%">
<p>图27</p>
</div>

出现黄色提示 `镜像保存成功` 即备份完成，如图 28 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_028.jpeg" alt="图28" style="max-width:100%">
<p>图28</p>
</div>

按`Enter`继续。关机/重启前务必走正常流程，不要直接拔 U 盘，如图 29 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_029.jpeg" alt="图29" style="max-width:100%">
<p>图29</p>
</div>

方向键选 `reboot 重新开机`，回车`Enter`确定，如图 30 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_030.jpeg" alt="图30" style="max-width:100%">
<p>图30</p>
</div>

确认卸载仓库后会倒计时重启，如图 31 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_031.jpeg" alt="图31" style="max-width:100%">
<p>图31</p>
</div>

重启过程中要记得按进入BIOS的按键(delete或者其它)，把第一启动项改回系统盘，保存退出后，系统会进入到被备份的系统中。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_032.jpeg" alt="图32" style="max-width:100%">
<p>图32</p>
</div>

注意现在镜像文件在当前再生龙U盘根路径下

## 3.将制作出的镜像文件恢复到另一个电脑中

将再生龙U盘拔出（本机的话不需要拔出），插入一台能正常进入桌面的电脑（本机或其它已有系统的机器均可）。在已开机的系统里用文件管理器拷贝，拷完后再把 U 盘插到目标电脑执行恢复操作。

刚才制作的镜像文件，如果名字没有自定义的话，保存的路径以及名称如图所示，示例名称为`2026-09-10-06-img`，路径在U盘根路径下

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_034.png" alt="图33" style="max-width:100%">
<p>图33</p>
</div>

将这一整个文件夹，拷贝到`/home/partimag/`路径下。这一步是给方法一 `1.Auto` 用的；

若走方法二 `2.live`，且和第 2 节一样把仓库挂在 U 盘根目录，根目录里的镜像也能被 `restoredisk` 看到，不必先拷。

这里执行的是拷贝镜像文件到`/home/partimag/`路径下的操作。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_035.png" alt="图34" style="max-width:100%">
<p>图34</p>
</div>

拷贝成功后，插入到想要烧录镜像的电脑。目标电脑同样在 BIOS 中将再生龙 U 盘设为第一启动项（见图 2），启动后出现 GRUB 菜单，进入后回车`Enter`选 `1.Auto(Restore)`，如图 35。

以下是对两个选项在本节的描述，实际上两个选项都可以走到选镜像（图 36）那一步：

- 方法一：`1.Auto(Restore)` 一键还原

    镜像必须已经在 U 盘的 `/home/partimag/` 下（即图 34）。默认项就是 `1.Auto(Restore)`，直接回车`Enter`即可。Auto 会跳过第 2 节的挂载和选模式，找到镜像后进入选镜像界面（图 36）。若该路径下没有镜像，会报错退出，见图 45。

- 方法二：`2.live` 手动还原

    方向键选 `2.live(Restore and Backup)`，回车`Enter`进入。从启动再生龙到选定模式之前，选项与第 2 节备份相同；到选定模式时改选 `restoredisk`（还原镜像到本机硬盘），如图 17。之后与方法一汇合，同样进入选镜像、选目标盘等界面（从图 36 起）。

本节在前面的操作中已经拷贝img文件夹到`/home/partimag/`下，因此选择第一项即可。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_003.png" alt="图35" style="max-width:100%">
<p>图35</p>
</div>

上下键选择刚才拷贝的镜像文件夹（本例 `2026-09-10-06-img`，只有一个镜像文件，直接回车`Enter`即可），回车`Enter`确定，如图 36 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_001.jpeg" alt="图36" style="max-width:100%">
<p>图36</p>
</div>

方向键选择要写入的目标硬盘（本例 `nvme0n1`，仅一个目标盘，直接回车`Enter`即可），回车`Enter`确定。该盘现有资料会被覆盖，确认型号和容量无误后再继续（容量限制见文首注意事项），如图 37 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_002.jpeg" alt="图37" style="max-width:100%">
<p>图37</p>
</div>

屏幕下方会连续两次弹出黄色警告：目标盘资料将被完全盖掉。核对镜像名和目标盘后，两次都输入 `y` 回车，如图 38、图 39 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_003.jpeg" alt="图38" style="max-width:100%">
<p>图38</p>
</div>

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_004.jpeg" alt="图39" style="max-width:100%">
<p>图39</p>
</div>

Partclone 开始按分区还原。先还原 EFI 分区（`nvme0n1p1`，体积小，很快完成），再还原系统分区（`nvme0n1p2`，耗时较长，进度条会停在这一步），如图 40、图 41 所示。等到两个分区都到 100% 即可。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_005.jpeg" alt="图40" style="max-width:100%">
<p>图40</p>
</div>

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_006.jpeg" alt="图41" style="max-width:100%">
<p>图41</p>
</div>

还原完成后方向键选 `1 重新开机`，回车`Enter`确定，不要选 `2 进入命令列`。确认后会卸载挂载的盘并倒计时重启，如图 42 所示。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_restore_008.jpeg" alt="图42" style="max-width:100%">
<p>图42</p>
</div>

重启过程中要记得按进入BIOS的按键(delete或者其它)，把第一启动项改回系统盘，如图 43，保存退出后，系统会进入到被备份的系统中。

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_032.jpeg" alt="图43" style="max-width:100%">
<p>图43</p>
</div>

## 问题解决

再生龙U盘路径`/home/partimag/`下无镜像文件，进入再生龙U盘，选择恢复选项的话，会弹出如下

需要将再生龙导出的镜像文件夹，放到U盘中`/home/partimag/`路径下，才会找到镜像文件

<div align="center">
<img src="./Linux镜像导出以及恢复_img/linux_dragon_033.jpeg" alt="图45" style="max-width:100%">
<p>图45</p>
</div>
