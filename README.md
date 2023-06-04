# iperf3-static
Compiling Iperf3 statically on gentoo and cross-platform compile as well

## Use Flags
**Global**
`USE="multilib static-libs"`

## Dependencies
`emerge openssl sys-devel/gcc sys-libs/glibc`

## Dependencies x86
`emerge openssl []`

## Compile commands
`./bootstrap.sh`
** Compile x64 **
```
./configure --enable-static-bin
make -j$(nproc)
```
** Compile x86 **
```
./configure CC="gcc -m32" CXX="g++ -m32" --enable-static-bin
make -j$(nproc)
```
** Compile ARM **
```

make -j$(nproc)
```

** Compile ARM64 **
```

make -j$(nproc)
```
