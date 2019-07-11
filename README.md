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
export BBL_VSPHERE_VCENTER_CLUSTER=LanCluster
export BBL_VSPHERE_VCENTER_RP=RP-Concourse
export BBL_VSPHERE_NETWORK="VM Network"
export BBL_VSPHERE_VCENTER_DS=vsanDatastore
export BBL_VSPHERE_SUBNET_CIDR=10.0.0.0/16
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


























