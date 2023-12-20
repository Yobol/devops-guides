# 虚拟桌面解决方案

## 桌面

### LXQt

轻量级桌面解决方案，基于 [QT](https://wiki.archlinux.org/title/Qt) 构建并使用了 Razor-qt & [LXDE](https://wiki.archlinux.org/title/LXDE) 组件。

- LXDE：性能更好、更节能的 `Lightweight X11 Desktop Environment`，需要 [lxde-common](https://archlinux.org/packages/?name=lxde-common)、[lxsession](https://archlinux.org/packages/?name=lxsession) 和 [Openbox](https://wiki.archlinux.org/title/Openbox)（或者其它 [window manager](https://wiki.archlinux.org/title/Window_manager)）

  - lxsession: session manager
  - openbox: window manager
  - [lxdm](https://wiki.archlinux.org/title/LXDM): display manager

  [lxde](https://archlinux.org/groups/x86_64/lxde/) 组包含了完整依赖。

## 远程桌面

### noVNC

> [VNC client web application](https://github.com/novnc/noVNC)
