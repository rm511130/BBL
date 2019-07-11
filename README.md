# bbl (bosh-bootloader) 

Also known as "bubble", bbl or bosh-bootloader is a command line utility for standing up BOSH on an IaaS without the need for an Ops Manager. It supports AWS, GCP, Microsoft Azure, Openstack and vSphere.

Note: it's has been my experience that using Ops Manager to (a) create Bosh, (b) deal with IaaS idiosyncrasies, and (c) maintain stemcells, is a simpler path to take. However, playing with bbl will give you insight into how Bosh works.

## 1. Start by getting the bbl CLI

I'm using a Mac, so I downloaded the `bbl-v8.1.0_osx` file from [here](https://github.com/cloudfoundry/bosh-bootloader/releases). I then proceeded to place the executable in the right place using the following commands:

```
cd ~/Downloads/
ls -last | head
chmod +x bbl-v8.1.0_osx 
mv ~/Downloads/bbl-v8.1.0_osx /usr/local/bin/bbl
```
The `bbl --version` command will show you, if it's working properly, something like this: `bbl 8.1.0 (darwin/amd64)`

Another, simpler way to achieve the same results would have been to use `brew` per the example below:

```
brew tap cloudfoundry/tap
brew install bbl
```

## 2. The Bosh CLI is also required

Additional instructions can be found [here](https://github.com/cloudfoundry/bosh-bootloader) but please continue to follow the installation steps on this page, the link is just for your information.

```
brew tap cloudfoundry/tap             # just in case you didn't do this earlier
brew install bosh-cli
```

In my case, I got an error messsage which you can read about [here](./xcode-problem.md)

To test use `bosh --version` and you should see something like this: `version 5.5.1-7850ac98-2019-05-21T22:28:39Z Succeeded`

## 3. Let's direnv to unclutter the .profile

[direnv](https://direnv.net/) is an extension for your machine's shell. It augments existing shells with a feature that can load and unload environment variables depending on the current directory. There are many use cases such as (a) loading of  12factor apps environment variables (b) creating per-project isolated development environments (c) loading secrets for deployment.

On a Mac, the installation of direnv is simple:

```
brew install direnv
direnv status    # to test direnv you can issue the direnv status command
```

## 4. Terraform and Ruby

According to the [bbl docs](https://github.com/cloudfoundry/bosh-bootloader) we'll also need:
- [Terraform](https://www.terraform.io/downloads.html) version > 0.11.0
- Ruby

My Mac met both requirements which I was able to test using the commands shown below:

```
terraform --version
ruby -v
```

## 5. Configuring bbl

If your target environment is not `vsphere` take a look [here](https://github.com/cloudfoundry/bosh-bootloader#usage).
I'm going to work under my usual `/work` directory, and my target environment will be `vsphere`. 

```
mkdir -p /work/bbl
cd /work/bbl
```

Let's define some environment variables:

```
export BBL_ENV_NAME=bbl-env
export BBL_IAAS=vsphere
export BBL_VSPHERE_VCENTER_USER=administrator@vsphere.local
export BBL_VSPHERE_VCENTER_PASSWORD=Pivotal2018!
export BBL_VSPHERE_VCENTER_IP=10.0.0.51
export BBL_VSPHERE_VCENTER_DC=LabDatacenter
export BBL_VSPHERE_VCENTER_CLUSTER=LabCluster
export BBL_VSPHERE_VCENTER_RP=RP-Concourse
export BBL_VSPHERE_NETWORK="VM Network"
export BBL_VSPHERE_VCENTER_DS=vsanDatastore
export BBL_VSPHERE_SUBNET_CIDR=10.99.0.0/16
export BBL_VSPHERE_VCENTER_DISKS=bbl/disks
export BBL_VSPHERE_VCENTER_TEMPLATES=bbl/templates
export BBL_VSPHERE_VCENTER_VMS=bbl/vms
mkdir $BBL_ENV_NAME && cd $BBL_ENV_NAME && git init
```

Once I executed `bbl plan` I obtained the following output:

```
step: generating terraform template
step: generating terraform variables
step: terraform init
```

Let's take a look at what files were created under the `/work/bbl/bbl-env` directory:

```
cd /work/bbl/bbl-env
ls -las
```
```
$ ls -las
total 40
0 drwxr-xr-x  14 ralphmeria  wheel   448 Jul 11 16:30 .
0 drwxr-xr-x   3 ralphmeria  wheel    96 Jul 11 16:26 ..
0 drwxr-xr-x   9 ralphmeria  wheel   288 Jul 11 16:26 .git
8 -rw-r--r--   1 ralphmeria  wheel   538 Jul 11 16:30 bbl-state.json
0 drwxr-xr-x  40 ralphmeria  wheel  1280 Jul 11 16:30 bosh-deployment
0 drwxr-xr-x   4 ralphmeria  wheel   128 Jul 11 16:30 cloud-config
8 -rwxr-x---   1 ralphmeria  wheel   644 Jul 11 16:30 create-director.sh
8 -rwxr-x---   1 ralphmeria  wheel   569 Jul 11 16:30 create-jumpbox.sh
8 -rwxr-x---   1 ralphmeria  wheel   644 Jul 11 16:30 delete-director.sh
8 -rwxr-x---   1 ralphmeria  wheel   569 Jul 11 16:30 delete-jumpbox.sh
0 drwxr-xr-x  16 ralphmeria  wheel   512 Jul 11 16:30 jumpbox-deployment
0 drwxr-xr-x   3 ralphmeria  wheel    96 Jul 11 16:30 runtime-config
0 drwxr-xr-x   4 ralphmeria  wheel   128 Jul 11 16:30 terraform
0 drwxr-----   3 ralphmeria  wheel    96 Jul 11 16:30 vars
```

More information about these files and directories is available [here](https://github.com/cloudfoundry/bosh-bootloader#managing-state)

## 6. bbl up

Let's kick-off the installation of Bosh:

```
$ bbl up
step: terraform init
step: terraform apply
step: creating jumpbox
Deployment manifest: '/work/bbl/bbl-env/jumpbox-deployment/jumpbox.yml'
Deployment state: '/work/bbl/bbl-env/vars/jumpbox-state.json'

Started validating
  Downloading release 'os-conf'... Skipped [Found in local cache] (00:00:00)
  Validating release 'os-conf'... Finished (00:00:00)
  Downloading release 'bosh-vsphere-cpi'... Skipped [Found in local cache] (00:00:00)
  Validating release 'bosh-vsphere-cpi'... Finished (00:00:00)
  Validating cpi release... Finished (00:00:00)
  Validating deployment manifest... Finished (00:00:00)
  Downloading stemcell... Skipped [Found in local cache] (00:00:00)
  Validating stemcell... Finished (00:00:00)
Finished validating (00:00:00)

Started installing CPI
  Compiling package 'ruby-2.4-r4/0cdc60ed7fdb326e605479e9275346200af30a25'... Finished (00:00:00)
  Compiling package 'vsphere_cpi/7556c6c966f3efcd1660cf2dc010e6c28a58182d'... Finished (00:00:00)
  Compiling package 'iso9660wrap/2e7db549be4f20243d9e3b835df265e1a23d0ebb'... Finished (00:00:00)
  Installing packages... Finished (00:00:01)
  Rendering job templates... Finished (00:00:00)
  Installing job 'vsphere_cpi'... Finished (00:00:00)
Finished installing CPI (00:00:02)

Starting registry... Finished (00:00:00)
Uploading stemcell 'bosh-vsphere-esxi-ubuntu-xenial-go_agent/250.9'... Finished (00:01:14)

Started deploying
  Creating VM for instance 'jumpbox/0' from stemcell 'sc-f9ad23fc-d34e-4a0c-a9fa-72956fbec3b4'... Finished (00:00:27)
  Waiting for the agent on VM 'vm-b18b248e-783d-41fd-9116-020db2198d23' to be ready... Failed (00:10:09)
Failed deploying (00:10:36)

Stopping registry... Finished (00:00:00)
Cleaning up rendered CPI jobs... Finished (00:00:00)

Deploying:
  Creating instance 'jumpbox/0':
    Waiting until instance is ready:
      Post https://mbus:<redacted>@10.0.0.5:6868/agent: dial tcp 10.0.0.5:6868: i/o timeout

Exit code 1

Create jumpbox: Running /work/bbl/bbl-env/create-jumpbox.sh: exit status 1
```

Yes, unfortunately it failed. On the vSphere VCSA console I was able to see the following message for a VM created by the bbl up command:

```
bosh stemcell: EXT4-fs error (device sdb2): ext4_has_uninit_itable:3135: 
comm mount:Inode table for bg 0 marked as needing zeroing
audit: kauditd hold queue overflow
```

It turns out that we've encountered a [regression](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1817060) described as follows: `EXT4: Mainstream fix for regression caused by fix CVE-2018-10876 is missing from Ubuntu Bionic Kernel`

There's a recommended work-around:

```
For the benefit of anyone else hitting this issue prior to a kernel fix, it is possible to avoid this problem by forcing mkfs to zero the inode tables immediately - e.g.

# mkfs.ext4 -E lazy_itable_init=0

This will cause mkfs to take a little longer creating the filesystem.

With the workaround all block groups on the new filesystem (as shown by dumpe2fs) will look like this until some time after the filesystem was first mounted:
Group 0: (Blocks 0-32767) csum 0x7b1a

Once initialised (either by the ext4lazyinit or by using lazy_itable_init=0 ) they acquire the ITABLE_ZEROED flag and can then be safely remounted without hitting this bug:
Group 0: (Blocks 0-32767) csum 0x7b1a [ITABLE_ZEROED]
```

## 7. Conclusion

Even though we didn't get bbl to work (due to a bug at the Ubuntu level) it looks like the process was not hard to understand and follow. I'll stop here and maybe come back later, once the bug has been corrected.

























