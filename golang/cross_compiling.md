# Cross-Compiling Golang Code

I needed to cross-compile some Golang code for my RaspberryPi, so I needed to
remember how to do it. The consensus on the internet was to use this command:

```bash
GOOS=linux GOARCH=arm64 go build
```

However, this didn't work for my particular case as this is an old RapberryPi 3
which I haven't updated in a long time (yes, yes, I know...) So I had to change
the command slightly.

Firstly, I needed to remember which architectures that the Golang compiler supported:

```bash
# go tool dist list
aix/ppc64
android/386
android/amd64
android/arm
android/arm64
darwin/amd64
darwin/arm64
dragonfly/amd64
freebsd/386
freebsd/amd64
freebsd/arm
freebsd/arm64
illumos/amd64
ios/amd64
ios/arm64
js/wasm
linux/386
linux/amd64
linux/arm
linux/arm64
linux/mips
linux/mips64
linux/mips64le
linux/mipsle
linux/ppc64
linux/ppc64le
linux/riscv64
linux/s390x
netbsd/386
netbsd/amd64
netbsd/arm
netbsd/arm64
openbsd/386
openbsd/amd64
openbsd/arm
openbsd/arm64
openbsd/mips64
plan9/386
plan9/amd64
plan9/arm
solaris/amd64
windows/386
windows/amd64
windows/arm
windows/arm64
```

The v3 RaspberryPi is a ARMv7 based processor, so the correct command is:

```bash
env GOARCH=arm GOOS=linux go build
```
