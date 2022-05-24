# 通过VNC访问docker容器的图形界面_spylyt的博客-CSDN博客_docker vnc
Save From : [通过VNC访问docker容器的图形界面_spylyt的博客-CSDN博客_docker vnc](https://blog.csdn.net/spylyt/article/details/79944772) 

## Content
From Docker Index

```null
docker pull dorowu/ubuntu-desktop-lxde-vnc

```

Build yourself

```null
git clone https://github.com/fcwu/docker-ubuntu-vnc-desktop.gitdocker build --rm -t dorowu/ubuntu-desktop-lxde-vnc docker-ubuntu-vnc-desktop
```

Run

```null
docker run -it --rm -p 8080:80 dorowu/ubuntu-desktop-lxde-vnc
```
## Note