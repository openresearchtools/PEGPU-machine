# PEGPU Machine

> [!IMPORTANT]
> **PEGPU Machine Documentation and Installation**
>
> For installation instructions and documentation on using PEGPU Machine, refer
> directly to the official PEGPU resource:
>
> **https://pegpu.com**

The PEGPU Machine repository provides the infrastructure to operate the Linux
virtual machine specifically required by PEGPU, enabling NVIDIA Thunderbolt
external GPUs (eGPUs) to function on Apple Silicon Macs. It complements the
main PEGPU application and maintains clear architectural and licensing
distinctions.

For deeper insights into development, architecture, or contribution guidelines,
refer to the main PEGPU repository:

https://github.com/openresearchtools/PEGPU

It includes QEMU, Apple VFIO, and bundled guest-driver components distributed under open-source licenses. QEMU is distributed as a GPL version 2 work; bundled dependencies carry their own notices and license texts.

## Source Layers

This repository is organized as a patch stack, not as a checked-in full QEMU source tree. The build workflow applies the patches to the recorded QEMU base and then packages the app.

1. Vanilla QEMU base: `ac0cc20ad2fe0b8df2e5d9458e90a095ac711ab1`
2. `scottjg/qemu-vfio-apple` `wip` layer: `41f61cecb5a371fb97dced5cd6970845e0c069cf`
3. QEMU-side visual runtime layer adapted from `utmapp/qemu` and `utmapp/virglrenderer`
4. OpenResearchTools PEGPU Machine integration layer

The source-layer records are in `patch-stack/00-base/`, and the three patch layers are in `patch-stack/01-*`, `patch-stack/02-*`, and `patch-stack/03-*`. The UTM QEMU visual-runtime record pins the adapted UTM QEMU branch/revision and keeps it on the GPL QEMU side.

## Acknowledgements

DriverKit, VFIO, and QEMU integration work in this distribution is based on Scott J. Goldman's `scottjg/qemu-vfio-apple` repository:

https://github.com/scottjg/qemu-vfio-apple

We are grateful for his work enabling eGPU use on macOS, and users who are interested in the original project should follow that repository for its upstream progress.

The QEMU-side macOS SPICE/virgl rendering runtime also adapts work from the `utmapp/qemu` QEMU fork and the `utmapp/virglrenderer` fork:

https://github.com/utmapp/qemu

https://github.com/utmapp/virglrenderer

PEGPU Machine is not endorsed by, sponsored by, or affiliated with Scott J. Goldman, `scottjg/qemu-vfio-apple`, `utmapp/qemu`, `utmapp/virglrenderer`, or their maintainers.

## Licensing

The source tree produced by this patch stack is QEMU-derived. QEMU as a whole is released under the GNU General Public License, version 2. Individual files and bundled components may carry more specific GPL, LGPL, BSD-style, MIT-style, UBDL, or other compatible notices; those notices remain authoritative for the files or components that contain them.

QEMU is a trademark of Fabrice Bellard. This repository uses the QEMU name only to identify the upstream QEMU project, the recorded QEMU base revision, and QEMU-derived source/runtime components. PEGPU Machine is not endorsed by, sponsored by, or affiliated with Fabrice Bellard or the QEMU project.

See `LICENSE` for the repository-level license notice.

## Releases

Release artifacts are produced from the recorded QEMU base plus the patch stack in this repository.

Each release is expected to include:

- `PEGPU-Machine-<version>.zip`: the macOS app bundle.
- `PEGPU-Machine-<version>-source.tar.gz`: the corresponding source archive, including the patch stack, source records, and source materials used for bundled runtime components.

Release notices are included with the app and identify the bundled runtime components and collected license texts for the release payload.

## Headless Driver Requests

The built `PEGPU Machine.app` can submit macOS DriverKit requests without
opening the normal setup window:

```sh
"/Applications/PEGPU Machine.app/Contents/MacOS/PEGPU Machine" --driver-activate
"/Applications/PEGPU Machine.app/Contents/MacOS/PEGPU Machine" --driver-deactivate
```

macOS may still require user approval in System Settings or a reboot. These
commands only avoid showing the app's own setup UI; they do not bypass Apple's
system-extension approval flow.
