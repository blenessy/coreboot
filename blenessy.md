# Setup

A bit messy due to submodules. 

Clone maintainers repo:
```shell
git clone 'https://review.coreboot.org/coreboot' --depth 1
```

Initiliaze submodule URL, which are relative to `https://review.coreboot.org/`:
```shell
git submodule init
```

Checkout your fork:
```shell
git remote add fork 'https://github.com/blenessy/coreboot.git'
git fetch fork
git checkout fork/master
```

Make sure the submodule URLs look good:
```shell
git config -l | grep 'submodule\..*\.url'`
```

# Building

Read [Starting from scratch](https://doc.coreboot.org/tutorial/part1.html).

Download [Makefile.docker](https://gist.github.com/blenessy/d0444d69cfb6a72b06de6f6293a1a72e) and adapt the `TODO`s:
```shell
curl -fs https://gist.githubusercontent.com/blenessy/d0444d69cfb6a72b06de6f6293a1a72e/raw/c43c1b50e0b795d96c95776c1422f8cb93d78a4b/Makefile.docker | sed "s/buildroot/$(basename $(pwd))/" | tee Makefile.docker
```

Select defconfig:

```shell
cp configs/config.emulation_qemu_x86_i440fx_buildroot configs/defconfig
make -f Makefile.docker defconfig
```

Build the cross compiler:

```shell
make -f Makefile.docker crossgcc-i386
```

Build and copy buildroot images:

```shell
cp ~/buildroot/output/images/bzImage ~/buildroot/output/images/rootfs.cpio.lzma ~/coreboot/
```

Build coreboot:

```shell
make -f Makefile.docker all
```

# Testing

Based on [QEMU x86 PC](https://doc.coreboot.org/mainboard/emulation/qemu-i440fx.html).

```shell
qemu-system-x86_64 -bios build/coreboot.rom -serial stdio -M pc -net nic,model=virtio -net user
```


