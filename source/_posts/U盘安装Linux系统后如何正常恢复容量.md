# U盘安装Linux系统后如何正常恢复容量

## 使用Windows 系统

1. 使用cmd
2. 键入 diskpart
3. list disk  And select the U disk (etc: select disk 1 )
4. clean
5. create partition primary
6. format fs fat32(比较耗时)