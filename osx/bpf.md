# BPF

On OSX you will need to give yourself permissions to `/dev/bpf0` otherwise you won't be able to access the `en0` interface from code.

You'll see an error like this:

```
<my_app>: en0: You don't have permission to capture on that device
((cannot open BPF device) /dev/bpf0: Permission denied)
```

To allow this you need to install the [ChmodBPF](https://github.com/boundary/wireshark/tree/master/packaging/macosx/ChmodBPF) tool which is part of [Wireshark](https://www.wireshark.org/).

## References

* https://stackoverflow.com/questions/57614760/how-to-fix-bpf-device-permissions-on-mac-os-to-use-tcpdump
