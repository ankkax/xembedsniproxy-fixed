### Building it manually

#depencies
arch
```
sudo pacman -S base-devel cmake extra-cmake-modules qt5-base qt5-x11extras kwindowsystem xcb-util xcb-util-image libxcb
```
ubuntu
```
sudo apt install build-essential cmake extra-cmake-modules qtbase5-dev libqt5x11extras5-dev libkf5windowsystem-dev libxcb1-dev libxcb-image0-dev libxcb-util-dev libxcb-icccm4-dev libxcb-composite0-dev libxcb-damage0-dev
```
fedora
```
sudo dnf install cmake extra-cmake-modules qt5-qtbase-devel qt5-qtx11extras-devel kf5-kwindowsystem-devel xcb-util-devel xcb-util-image-devel libxcb-devel
```

cmake list
```
cat CMakeLists.txt | grep -E "(find_package|pkg_check_modules)"
```
# buliding
git clone https://github.com/ankkax/xembedsniproxy-fixed.git
```
cd xembedsniproxy-fixed
mkdir build && cd build
cmake ..
make -j$(nproc)
```
# or just use included appimg
`xembed-sni-proxy-1.0.0-x86_64.AppImage`


