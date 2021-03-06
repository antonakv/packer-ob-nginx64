# Kitchen-vagrant test that checks the vagrant virtualbox box have nginx - README.me with instructions

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

- rbenv installed
[Rbenv installation manual](https://github.com/rbenv/rbenv#installation)

- have account created on [Vagrant cloud](https://app.vagrantup.com/)

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

## Test OS image to ensure if nginx is installed
Do tests to ensure if nginx package is installed

- Install ruby 2.6.6 using rbenv

```bash
rbenv install -l
rbenv install 2.6.6
rbenv local 2.6.6 
```

Sample output

```bash
➜  packer-ob-nginx64 git:(master) rbenv install -l
2.5.8
2.6.6
2.7.2
3.0.0
jruby-9.2.14.0
mruby-2.1.2
rbx-5.0
truffleruby-21.0.0
truffleruby+graalvm-21.0.0

Only latest stable releases for each Ruby implementation are shown.
Use 'rbenv install --list-all / -L' to show all local versions.

➜  packer-ob-nginx64 git:(master) rbenv install 2.6.6
rbenv: /Users/aakulov/.rbenv/versions/2.6.6 already exists
continue with installation? (y/N) y
Downloading ruby-2.6.6.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.6.tar.bz2
Installing ruby-2.6.6...
ruby-build: using readline from homebrew
Installed ruby-2.6.6 to /Users/aakulov/.rbenv/versions/2.6.6

➜  packer-ob-nginx64 git:(master) rbenv local 2.6.6 
➜  packer-ob-nginx64 git:(master) 
```

- Install kitchen-test locally
```bash
bundle install --path vendor/bundle
```

Expected result

```
➜  packer-ob-nginx64 git:(master) bundle install --path vendor/bundle
Fetching gem metadata from https://rubygems.org/........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies....
Fetching concurrent-ruby 1.1.8
Installing concurrent-ruby 1.1.8
Fetching i18n 1.8.9
Installing i18n 1.8.9
Fetching minitest 5.14.4

[Skipped some messages]

Installing kitchen-inspec 2.4.1
Fetching kitchen-vagrant 1.8.0
Installing kitchen-vagrant 1.8.0
Bundle complete! 4 Gemfile dependencies, 180 gems now installed.
Bundled gems are installed into `./vendor/bundle`
```
- Perform tests
```bash
bundle exec kitchen test
```

Expected command output
```bash
-----> Starting Test Kitchen (v2.10.0)
-----> Cleaning up any prior instances of <default-vbox-nginx64>
-----> Destroying <default-vbox-nginx64>...
       Finished destroying <default-vbox-nginx64> (0m0.00s).
-----> Testing <default-vbox-nginx64>
-----> Creating <default-vbox-nginx64>...
       Bringing machine 'default' up with 'virtualbox' provider...
       ==> default: Importing base box 'nginx64'...

[Skipped some messages]

  System Package nginx
     ✔  is expected to be installed

Test Summary: 1 successful, 0 failures, 0 skipped
       Finished verifying <default-vbox-nginx64> (0m2.20s).
-----> Destroying <default-vbox-nginx64>...
       ==> default: Forcing shutdown of VM...
       ==> default: Destroying VM and associated drives...
       Vagrant instance <default-vbox-nginx64> destroyed.
       Finished destroying <default-vbox-nginx64> (0m5.30s).
       Finished testing <default-vbox-nginx64> (0m49.54s).
-----> Test Kitchen is finished. (0m51.31s)

```

## Publish vagrant box to Vagrant cloud 

- Create vagrant box record on the Vagrant cloud
```bash
vagrant cloud box create Replace_With_your_vagrant_cloud_account_name/nginx64 --no-private
```
Expected result:
```
Created box Replace_With_your_vagrant_cloud_account_name/nginx64
Box:              Replace_With_your_vagrant_cloud_account_name/nginx64
Description:      
Private:          no
Created:          2021-03-02T09:22:41.029Z
Updated:          2021-03-02T09:22:41.029Z
Current Version:  N/A
Versions:         
Downloads:        0
```
- Publish the local vagrant box image to the Vagrant cloud
Run from the same folder you were before 
```bash
vagrant cloud publish --box-version `date +%y.%m.%d` \
  --force --no-private --release Replace_With_your_vagrant_cloud_account_name/nginx64 \
  `date +%y.%m.%d` virtualbox nginx64-vbox.box 
```
Expected result:
```
You are about to publish a box on Vagrant Cloud with the following options:
aakulov/nginx64:   (v21.03.02) for provider 'virtualbox'
Automatic Release:     true
Saving box information...
Uploading provider with file /Users/aakulov/Documents/Development/Hashicorp/packer-ob-nginx64/nginx64-vbox.box
Releasing box...
Complete! Published Replace_With_your_vagrant_cloud_account_name/nginx64
Box:              Replace_With_your_vagrant_cloud_account_name/nginx64
Description:      
Private:          no
Created:          2021-03-02T09:22:41.029Z
Updated:          2021-03-02T09:23:35.476Z
Current Version:  N/A
Versions:         21.03.02
Downloads:        0

```

## How to use Vagrant box uploaded to Vagrant cloud

- Prepare folder where you store downloaded vagrant boxes
```bash
cd
mkdir my_vagrantboxes_folder
```
Expected result
```bash
$ cd
$ mkdir my_vagrantboxes_folder
$
```

- Change folder to my_vagrantboxes_folder
```
cd my_vagrantboxes_folder
```
Expected result
```bash
$ cd my_vagrantboxes_folder
$
```

- Download vagrant box from Vagrant cloud
```bash
vagrant box add Replace_With_your_vagrant_cloud_account_name/nginx64
```

Example
```bash
$ vagrant box add Replace_With_your_vagrant_cloud_account_name/nginx64
==> box: Loading metadata for box 'Replace_With_your_vagrant_cloud_account_name/nginx64'
    box: URL: https://vagrantcloud.com/Replace_With_your_vagrant_cloud_account_name/nginx64
==> box: Adding box 'Replace_With_your_vagrant_cloud_account_name/nginx64' (v21.03.02) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/Replace_With_your_vagrant_cloud_account_name/boxes/nginx64/versions/21.03.02/providers/virtualbox.box
Download redirected to host: vagrantcloud-files-production.s3.amazonaws.com
==> box: Successfully added box 'Replace_With_your_vagrant_cloud_account_name/nginx64' (v21.03.02) for 'virtualbox'!
```
