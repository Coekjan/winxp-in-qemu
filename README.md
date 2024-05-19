# 在 QEMU 上运行 Windows XP 及常见应用程序

## 下载 Windows XP SP3 镜像

从 [Web Archive](https://archive.org/details/WinXPProSP3SimplifiedChinese) 可找到适用的中文简体版本 Windows XP SP3 镜像。下载后，将镜像文件重命名为 `winxp-installer.iso`，并放置在本目录下。

## 创建虚拟硬盘

使用命令：

```console
$ qemu-img create -f qcow2 winxp-zh_cn.qcow2 20G
```

创建一个 QEMU 可识别的 QCOW2 格式的虚拟硬盘文件 `winxp-zh_cn.qcow2`，大小为 20GB（由于 QCOW2 采用稀疏的方式组织硬盘内容，因此初始文件大小远小于 20 GB）。

## 在虚拟硬盘上安装 Windows XP SP3

使用命令：

```console
$ qemu-system-x86_64 \
    -smp 4 \
    -m 4G \
    -vnc :0 \
    -drive file=winxp-zh_cn.qcow2,index=0,media=disk,format=qcow2 \
    -drive file=winxp-installer.iso,media=cdrom,index=1 \
    -boot d
```

将虚拟硬盘与 Windows XP SP3 安装镜像挂载到 QEMU 模拟器上，并启动。使用 VNC 客户端连接 5900 端口，等待进入图形界面，按照提示完成安装。

## 运行 Windows XP SP3

QEMU 命令已封装为脚本 [`launch`](./launch)，使用命令：

```console
$ ./launch
```

即可启动 QEMU 模拟器。若 QEMU 没有支持 GTK GUI 界面，用户需手动连接 VNC 客户端进入图形界面。

## 安装常见应用程序

### Bootstrap & Certificates

由于 Windows XP SP3 根证书过期，可下载并安装下述证书：
- https://secure.globalsign.net/cacert/Root-R1.crt
- https://cacerts.digicert.com/DigiCertGlobalRootG2.crt

但由于 Windows XP SP3 根证书过期，在 Windows XP 中可能无法访问上述链接。为此，可先把这些证书下载到本地，并使用 HTTP/FTP 服务器等方式将其提供给 Windows XP SP3 下载。

### Opera 浏览器

Windows XP 自带的 IE 浏览器存在大量兼容性问题，很多网页没法正常显示，建议用 Opera 浏览器替代 IE 浏览器。

从 [Opera 官网](https://get.opera.com/pub/opera-winxpvista/36.0.2130.80/win/) 可找到 Windows XP 适用的 Opera 浏览器下载链接（Opera\_winxpvista\_36.0.2130.80\_Setup.exe）。

### Adobe Reader

Adobe Reader 可用于查看 PDF 文件，可从 [腾讯软件中心](https://pc.qq.com/detail/1/detail_1201.html) 下载。

### .NET 2.0 Framework

.NET 2.0 框架是运行 Microsoft SQL Server 的必要组件，可从 [微软官网](https://www.microsoft.com/en-us/download/details.aspx?id=16614) 下载。

### SQL Server 2000

SQL Server 2000 是一款老旧的数据库软件，但在 Windows XP 上运行良好，可从 [微软官网](http://download.microsoft.com/download/sqlsvr2000/trial/2000/nt45/cn/sqleval.exe) 下载。并运行 `sqleval.exe` 程序，记住其解压路径（默认为 `C:\SQLEVAL`），解压结束后进入解压路径运行 `autorun.exe` 程序，即可进入图形化的安装向导，按照提示完成安装。
