# BBL (Bosh Boot Loader) 

Also known as "bubble", bbl or bosh-bootloader is a command line utility for standing up BOSH on an IaaS without the need for an Ops Manager. bbl supports AWS, GCP, Microsoft Azure, Openstack and vSphere.

Note: it's has been my experience that using Ops Manager to create Bosh, deal with IaaS idiosincracies, and maintain stemcells is a simpler path to take. However, playing with bbl will give you insight into how Bosh works.

## Start by getting the BBL CLI

I'm using a Mac, so I downloaded the bbl-v8.1.0_osx file using this [link](https://github.com/cloudfoundry/bosh-bootloader/releases). I then proceeded to place the executable in the right place.

```
cd ~/Downloads/
ls -last | head
chmod +x bbl-v8.1.0_osx 
mv ~/Downloads/bbl-v8.1.0_osx /usr/local/bin/bbl
bbl --version
```
The `bbl --version` command will show you something like this: `bbl 8.1.0 (darwin/amd64)`

Another, simpler way of achieving the same results would have been to use `brew` per the example below:

```
brew tap cloudfoundry/tap
brew install bbl
```

## We also need the Bosh CLI

The instructions for all these steps can be found [here](https://github.com/cloudfoundry/bosh-bootloader) but please continue to follow the installation steps on this page. The link is just for your information.

So let's continue:

```
brew tap cloudfoundry/tap             # just in case you didn't do this earlier
brew install bosh-cli
```

In my case, I got an error messsage which you can read about [here](./xcode-problem.md)









