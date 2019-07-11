# bbl (bosh-bootloader) 

Also known as "bubble", bbl or bosh-bootloader is a command line utility for standing up BOSH on an IaaS without the need for an Ops Manager. It supports AWS, GCP, Microsoft Azure, Openstack and vSphere.

Note: it's has been my experience that using Ops Manager to (a) create Bosh, (b) deal with IaaS idiosyncrasies, and (c) maintain stemcells, is a simpler path to take. However, playing with bbl will give you insight into how Bosh works.

## Start by getting the bbl CLI

I'm using a Mac, so I downloaded the `bbl-v8.1.0_osx` file from [here](https://github.com/cloudfoundry/bosh-bootloader/releases). I then proceeded to place the executable in the right place using the following commands:

```
cd ~/Downloads/
ls -last | head
chmod +x bbl-v8.1.0_osx 
mv ~/Downloads/bbl-v8.1.0_osx /usr/local/bin/bbl
bbl --version
```
The `bbl --version` command will show you, if it's working properly, something like this: `bbl 8.1.0 (darwin/amd64)`

Another, simpler way to achieve the same results would have been to use `brew` per the example below:

```
brew tap cloudfoundry/tap
brew install bbl
```

## The Bosh CLI is also required

Additional instructions can be found [here](https://github.com/cloudfoundry/bosh-bootloader) but please continue to follow the installation steps on this page, the link is just for your information.

```
brew tap cloudfoundry/tap             # just in case you didn't do this earlier
brew install bosh-cli
```

In my case, I got an error messsage which you can read about [here](./xcode-problem.md)

To test use `bosh --version` and you should see something like this: `version 5.5.1-7850ac98-2019-05-21T22:28:39Z Succeeded`











