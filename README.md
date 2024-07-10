Installation of software from NGIpkgs currently requires Nix [flakes to be enabled](https://nixos.wiki/wiki/Flakes).

The example config in this repo is installing the default module from NGIpkgs, which contains all packages from the repo, as well as a module for one of the projects from NGIpkgs (Vula) and the minimal config to enable it in the `configuration.nix`. More info on installing software from NGIpkgs is also available in the [readme](https://github.com/ngi-nix/ngipkgs?tab=readme-ov-file#ngipkgs) of that repo.

## How to deploy to a local VM

After cloning this repo and entering its directory, run this command to build a NixOS system for a VM:
```
nix build .#nixosConfigurations.myMachine.config.system.build.vm
```

Before starting the VM, run this command to configure QEMU networking to allow logging into its VMs through SSH:
```
export QEMU_NET_OPTS="hostfwd=tcp::2221-:22"
```

If necessary, additional networking options could also be configured here, such as forwarding port 80 for a web app, for example.

Now run the script that has been built to boot the NixOS VM in QEMU:
```
./result/bin/run-nixos-vm
```

QEMU will open its own terminal window that shows the boot log. It is possible to login via this terminal, but the UX will usually be better with a regular SSH login (the default password is simply `password`):
```
ssh -oUserKnownHostsFile=/dev/null -oStrictHostKeyChecking=no user@localhost -p 2221
```
