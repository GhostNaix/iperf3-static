# iperf3-static
Compiling Iperf3 statically on gentoo and cross-platform compile as well

## Use Flags
**Global**
`USE="multilib static-libs"`

## Dependencies
`emerge openssl sys-devel/gcc sys-libs/glibc`

## Dependencies x86
`emerge openssl [abi_x86_32]`

## Compile commands
`./bootstrap.sh`

**Compile x64**
```
./configure --enable-static-bin
make -j$(nproc)
```
**Compile x86**
```
./configure CC="gcc -m32" CXX="g++ -m32" --enable-static-bin
make -j$(nproc)
```
## Compile commands for ARM system (untested and is native, haven't figure out cross-compiling yet)

**Compile ARM**
```
./configure CC="gcc -m32" CXX="g++ -m32" --enable-static-bin
make -j$(nproc)
```

**Compile ARM64**
```
./configure --enable-static-bin
make -j$(nproc)
```

# Compile iperf3 on Windows x64
1. Install Cygwin
2. `wget https://raw.githubusercontent.com/transcode-open/apt-cyg/master/apt-cyg`
3. `install apt-cyg /bin`
4. `apt-cyg install git nano make cmake extra-cmake-modules automake autoconf gcc-core gcc-g++ gdb binutils cygwin-devel w32api-headers w32api-runtime libguile3.0_1 libisl23 libmpc3 libgc1 libffi8`
5. `git clone https://github.com/esnet/iperf.git`
6. `cd iperf`
7. `./configure --enable-static-bin`
8. `make -j$(nproc)`
9. `make install`

# Compile iperf3 on Windows x86
1. Install Cygwin via `setup-x86.exe --allow-unsupported-windows`
2. set mirror to: `http://ctm.crouchingtigerhiddenfruitbat.org/pub/cygwin/circa/2022/11/23/063457`
3. `wget https://raw.githubusercontent.com/transcode-open/apt-cyg/master/apt-cyg`
4. `install apt-cyg /bin`
5. `apt-cyg install git nano make cmake extra-cmake-modules automake autoconf gcc-core gcc-g++ gdb binutils cygwin-devel w32api-headers w32api-runtime libguile3.0_1 libisl23 libmpc3 libgc1 libguile2.2_1 libltdl7`
6. `git clone https://github.com/esnet/iperf.git`
7. `cd iperf`
8. `./configure --enable-static-bin`
9. `make -j$(nproc)`
10. `make install`
