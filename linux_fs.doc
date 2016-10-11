#Linux文件系统
Linux中存在多种类型的文件系统

可以通过 ``cat /proc/filesystems`` 或者 ``mount`` 来查看

例如：rootfs、tmpfs、proc

#rootfs文件系统
——文件管理

##存储
* sector (扇区) ：最小存储单位、512B
* block (块) ：文件存取的最小单位、4KB，8个sector组成1个block

##分区
* 文件数据区 (data block)
* 元信息区 (inode table)
	* 每个文件都有对应的inode，包含了文件描述信息
	* 每个文件占用1个或多个块
	* 1个block只能被1个文件使用

##文件类型
* file (-)
	* 普通文件
* dictionary (d)
	* 目录，用于管理文件节点
* char device，block device (c) (b)
	* 字符设备，支持非定长数据流有序传递
	* 块设备，支持定长数据流随机传递
* link (l)
	* hard link都指向文件的inode节点，默认文件生成方式
		* 可直接修改文件
		* inode节点内部有引用计数，删除不影响其他hard link
	* soft link保存指向文件的路径
		* 不可直接修改文件
		* 删除不影响原文件
* socket (s)
	* 套接字文件，用于网络间或本地IPC通信

Notice：

* ``ls -l`` 查看文件类型
* ``touch`` 创建普通文件
* ``mkdir`` 创建目录
* ``mknode c/b`` 创建字符/块设备
* ``ln -d/-s``  创建链接
* ``socket()``  创建套接字


##inode节点
Linux系统采用Posix标准，强制规范了文件系统对象结构

* 文件唯一标识，inode no
* 文件字节大小，占用block大小
* 普通文件的block块链表
	* 像七龙珠一样
* 目录文件的目录项链表
	* 目录项保存文件名、inode节点的映射关系
	* "."表示当前目录的硬链接，“..”表示父目录的硬链接
* 文件所有者的User ID、所在Group ID
* 文件模式，确定了类型，以及它的所有者、group成员、其它用户的访问权限
* 时间戳，inode自身被修改ctime、文件内容被修改mtime、最后一次访问（atime）
* 链接数，有多少个硬链接指向此inode
* 文件操作函数表 

Notice：

* 使用``stat``系统调用可以查询一个文件的inode号码及一些元信息。

##文件索引
文件系统查询文件过程如下：

文件系统是树状结构，顶端为根目录，节点是目录，叶子是文件。当给出文件的完整路径时，我们从根目录出发，经过沿途各个目录，最终到达文件

* 从根目录按照文件路径找到目录项inode
* inode table中找到inode信息
* inode信息中查找下个子目录项inode
* 循环以上2步
* 找到目标文件inode信息，查询文件数据所在的block
* 通过block驱动，访问文件

Notice：

* init进程启动时，从根分区中获取根目录"/"的inode信息

#tmpfs文件系统
——提升文件存储效率

* tmpfs是基于内存的文件系统，由内存和swap组成，swap是通过硬盘虚拟出来的内存空间，读写速度比内存要慢
* 内核将频繁操作的数据放到tmpfs的内存中，增加读写速度
* 当内核发现tmpfs中没有足够的内存时，会把内存中不常用的数据交换到swap中，需要的时候再交换回来
* 共享内存基于tmpfs实现


#proc文件系统
——系统运行时信息

* /proc/cpuinfo - 处理器信息
* /proc/meminfo - 内存信息
* /proc/mounts - 已挂载的文件系统
* /proc/devices - 设备列表
* /proc/filesystems - 被支持的文件系统
* /proc/modules - 已加载的驱动模块
* /proc/version - 内核版本
* /proc/cmdline - 系统启动时的命令行参数
