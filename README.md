# kuberestore
Restore all PVCs in namespace from snapshots. It is using `VolumeSnapshots`[1]
which must be first deployed in your cluster, e.g. using `Gemini`[2].

## prerequisites
`kuberestore` requires Bash >= 4.0. If you are on Mac it is also safer to use
GNU `coreutils`[3] while running it.

## install
Copy executable to your `$PATH`.

## usage
In order to use `kuberestore` first you need to have `kubectl` properly
configured with the right context.

Next, check out what `VolumeSnapshots` are available:
```bash
$ kubectl get volumesnapshot -n test
NAME                           AGE
test-volume-1585945609         30m
test-volume-1585945610         15m
```
Run `kuberestore` and specify which snapshots you want to use, e.g. 1 for the
last available, 2 for the 2nd to last, etc.

```bash
$ kuberestore -n test -s 1
```

It will automatically scale down your pods, delete old PVCs, and restore them
from snapshots. Then it will scale your pods back up. In the end you should
have fully restored namespace.


[1] https://kubernetes.io/docs/concepts/storage/volume-snapshots/
[2] https://github.com/FairwindsOps/gemini
[3] https://ports.macports.org/port/coreutils/
