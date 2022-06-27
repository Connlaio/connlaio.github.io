# 创建 WISA隐藏分区步骤

1.  使用管理员权限打开命令行终端
2. 输入`diskpart` 指令，进入diskpart管理部分；
3. 输入`list disk` ；
4. 选中要目标分区 `select  disk 1`
5. 输入 `rescan`
6. 输入 `list partition`，显示所有的分区；
7. 选中目标分区 ，`select partition 1`(假设目标分区的序号为1)；
8. 输入 `set id = 12`，此时，即可隐藏分区；
9. 如果需要取消隐藏，在上一步时，输入  `set id = 7`，即可。