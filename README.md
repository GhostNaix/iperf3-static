# iperf3-static
Compiling Iperf3 statically on gentoo and cross-platform compile as well as on other platforms

**Some Warnings**
- Configure doesn't like spaces in directories

# Prep
I presumed you are cd into `~` or `$HOME`
```
mkdir Compile_Drop
cd Compile_Drop
```

# Gentoo/Linux
## Set USE FLAGS
`sudo nano /etc/portage/package.use/openssl`
and insert the line
`dev-libs/openssl static-libs`

## Dependencies 

**Depedencies x64**
`sudo emerge openssl sys-devel/gcc sys-libs/glibc`

**Dependencies x86**
```
sudo emerge openssl[abi_x86_32] --autounmask-write
sudo dispatch-conf
sudo emerge openssl[abi_x86_32]
```
**Dependencies ARM**
(This step is only necessary if cross Compiling)
```
sudo emerge crossdev
```
Then follow this guide https://wiki.gentoo.org/wiki/Crossdev
```
crossdev --stable -t aarch64-unknown-linux-gnu
crossdev --stable -t arm-linux-gnueabi
```


## Compile commands
`./bootstrap.sh`

**Compile x64**
```
./configure --enable-static-bin
make -j$(nproc)
strip -s src/iperf3
```
**Compile x86**
```
export CC="gcc -m32"
export CXX="g++ -m32"
./configure --enable-static-bin
make -j$(nproc)
strip -s src/iperf3
```
## Compile commands for ARM system

**Get openssl**
```
mkdir opensslARM opensslAARCH64
wget https://www.openssl.org/source/openssl-1.1.1u.tar.gz
tar -zxvf openssl-1.1.1u.tar.gz
cd openssl-1.1.1u
```
**Cross compile openssl for ARM**
```
export INSTALL_DIR=~/Compile_Drop/opensslARM # You might want to change this line to a valid directory where you wanna store openssl
./Configure linux-armv4 shared enable-egd threads enable-md2 enable-rc5 --prefix="$INSTALL_DIR" --openssldir="$INSTALL_DIR/openssl" -static --release --cross-compile-prefix=arm-linux-gnueabi-
make -j$(nproc)
make -j$(nproc) depend
make -j$(nproc) install
```

**Cross compile openssl for AARCH64**
```
export INSTALL_DIR=~/Compile_Drop/opensslAARCH64 # You might want to change this line to a valid directory where you wanna store openssl
./Configure linux-aarch64 shared enable-egd threads enable-md2 enable-rc5 --prefix="$INSTALL_DIR" --openssldir="$INSTALL_DIR/openssl" -static --release --cross-compile-prefix=aarch64-unknown-linux-gnu- 
make -j$(nproc)
make -j$(nproc) depend
make -j$(nproc) install
```

**Compile ARM**

change directory to iperf 3
```
# You might want to change this line to a valid directory where you compiled stored openssl
./configure --host=arm-linux-gnueabi --enable-static-bin --with-openssl="${HOME}/Compile_Drop/opensslARM" 
make -j$(nproc)
arm-linux-gnueabi-strip -s src/iperf3
```

**Compile ARM64**
```
# You might want to change this line to a valid directory where you compiled stored openssl
./configure --host=aarch64-unknown-linux-gnu --enable-static-bin --with-openssl="${HOME}/Compile_Drop/opensslAARCH64" 
make -j$(nproc)
aarch64-unknown-linux-gnu-strip -s src/iperf3
```

# Windows

## Compile iperf3 on Windows x64
- Install Cygwin
- Install Wget
- Run these commands
```
wget https://raw.githubusercontent.com/transcode-open/apt-cyg/master/apt-cyg
install apt-cyg /bin
apt-cyg install git nano make cmake extra-cmake-modules automake autoconf gcc-core gcc-g++ gdb binutils cygwin-devel w32api-headers w32api-runtime libguile3.0_1 libisl23 libmpc3 libgc1 libffi8
git clone https://github.com/esnet/iperf.git
cd iperf
./configure --enable-static-bin
make -j$(nproc)
make install
```

## Compile iperf3 on Windows x86

- Install Cygwin via `setup-x86.exe --allow-unsupported-windows`
- set mirror to: `http://ctm.crouchingtigerhiddenfruitbat.org/pub/cygwin/circa/2022/11/23/063457`
- Install wget
- Run these commands
```
wget https://raw.githubusercontent.com/transcode-open/apt-cyg/master/apt-cyg
install apt-cyg /bin
apt-cyg install git nano make cmake extra-cmake-modules automake autoconf gcc-core gcc-g++ gdb binutils cygwin-devel w32api-headers w32api-runtime libguile3.0_1 libisl23 libmpc3 libgc1 libguile2.2_1 libltdl7
git clone https://github.com/esnet/iperf.git
cd iperf
./configure --enable-static-bin
make -j$(nproc)
make install
```
