---
layout: post
title: Install 13-gaps pada opensuse
---
## Dependencies
```
zypper in automake autoconf gcc libev-devel startup-notification-devel libxcb-devel xcb-util-devel xcb-util-cursor-devel xcb-util-keysyms-devel xcb-util-wm-devel util-macros-devel libtool libxkbcommon-devel libxkbcommon-x11-devel cmake gcc-c++ pango-devel
```

Install [xcb-util-xrm v1.2](https://github.com/Airblader/xcb-util-xrm.git) 
```
git clone https://github.com/Airblader/xcb-util-xrm.git
cd xcb-util-xrm
git checkout v1.2
git submodule update --init
./autogen.sh
./configure --prefix=/usr
./make
./make install
```

Install yajl
```
git clone git://github.com/lloyd/yajl
cd yajl
git checkout 2.1.0
./configure --prefix=/usr
./make
./make install
```

Install libpcre
```
git clone https://github.com/vmg/libpcre.git
cd libpcre
git checkout pcre-8.36
./autogen.sh
./configure --prefix=/usr
./make
./make install
```

Install cairo
```
git clone git://anongit.freedesktop.org/git/cairo
cd cairo
git checkout 1.15.2
./autogen.sh
./configure --prefix=/usr
./make
./make install
```

## Compilation
```
# clone the repository
git clone https://www.github.com/Airblader/i3 i3-gaps
cd i3-gaps

# compile & install
autoreconf --force --install
rm -rf build/
mkdir -p build && cd build/

../configure --prefix=/usr --sysconfdir=/etc
make
sudo make install
```
