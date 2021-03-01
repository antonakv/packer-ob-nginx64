# Create a vagrant virtualbox box with nginx ubuntu lts - name it nginx64 - README.md with instructions (github repo)

## Intro
This manual is dedicated to create vagrant virtualbox base ubuntu lts box with nginx package installed. Tested on Mac OS X.

## Requirements
- Oracle Virtualbox recent version installed
[VirtualBox installation manual](https://www.virtualbox.org/manual/ch01.html#intro-installing)

- Hashicorp packer recent version installed
[Packer installation manual](https://learn.hashicorp.com/tutorials/packer/getting-started-install)

- Hashicorp vagrant recent version installed
[Vagrant installation manual](https://learn.hashicorp.com/tutorials/vagrant/getting-started-install)

- git installed
[Git installation manual](https://git-scm.com/download/mac)

## Preparation 
- Clone git repository. 

```bash
git clone https://github.com/antonakv/packer-ob-nginx64
```

Expected command output looks like this:

```bash
Cloning into 'packer-ob-nginx64'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 12 (delta 1), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (12/12), done.
Resolving deltas: 100% (1/1), done.
```

- Change folder to packer-ob-nginx64

```bash
cd packer-ob-nginx64
```

## Build
- In the same folder you were before run 

```bash
packer build template.json
```

Sample result

```bash
➜  packer-ob-nginx64 git:(main) packer build template.json
nginx64-vbox: output will be in this color.

==> nginx64-vbox: Retrieving Guest additions
==> nginx64-vbox: Trying /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso
==> nginx64-vbox: Trying /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso
==> nginx64-vbox: /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso => /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso
==> nginx64-vbox: Retrieving ISO
==> nginx64-vbox: Trying https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-amd64.iso
==> nginx64-vbox: Trying https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-amd64.iso?checksum=sha256%3A8c5fc24894394035402f66f3824beb7234b757dd2b5531379cb310cedfdf0996
    nginx64-vbox: ubuntu-18.04.5-server-amd64.iso 951.00 MiB / 951.00 MiB [====================================================================] 100.00% 2m19s
==> nginx64-vbox: https://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.5-server-amd64.iso?checksum=sha256%3A8c5fc24894394035402f66f3824beb7234b757dd2b5531379cb310cedfdf0996 => /Users/aakulov/Documents/Development/Hashicorp/packer-ob-nginx64/packer_cache/a37af95ab12e665ba168128cde2f3662740b21a2.iso

[ Skipped some messages ]

    nginx64-vbox (vagrant): Copying from artifact: output-nginx64-vbox/nginx64-vbox-disk001.vmdk
    nginx64-vbox (vagrant): Copying from artifact: output-nginx64-vbox/nginx64-vbox.ovf
    nginx64-vbox (vagrant): Renaming the OVF to box.ovf...
    nginx64-vbox (vagrant): Compressing: Vagrantfile
    nginx64-vbox (vagrant): Compressing: box.ovf
    nginx64-vbox (vagrant): Compressing: metadata.json
    nginx64-vbox (vagrant): Compressing: nginx64-vbox-disk001.vmdk
Build 'nginx64-vbox' finished after 12 minutes 8 seconds.

==> Wait completed after 12 minutes 8 seconds

==> Builds finished. The artifacts of successful builds are:
--> nginx64-vbox: VM files in directory: output-nginx64-vbox
--> nginx64-vbox: 'virtualbox' provider box: nginx64-vbox.box
```

- Add the built image box to Virtualbox

```bash
vagrant box add --force --name nginx64  nginx64-vbox.box
```

Sample result
```bash
➜  packer-ob-nginx64 git:(main) vagrant box add --force --name nginx64  nginx64-vbox.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'nginx64' (v0) for provider: 
    box: Unpacking necessary files from: file:///Users/aakulov/Documents/Development/Hashicorp/packer-ob-nginx64/nginx64-vbox.box
==> box: Successfully added box 'nginx64' (v0) for 'virtualbox'!
```

