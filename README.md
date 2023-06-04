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
## Compile commands for ARM system (untested and is native)

**Compile ARM**
```
./configure CC="gcc -m32" CXX="g++ -m32" --enable-static-bin
make -j$(nproc)
```

** Compile ARM64 **
```
./configure --enable-static-bin
make -j$(nproc)
```
