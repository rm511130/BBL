# BBL
Playing with BBL (Bosh Boot Loader)
The instructions

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

In my case, I got an error messsage which you can read about here.



```
$ brew install bosh-cli
==> Installing bosh-cli from cloudfoundry/tap
Error: Your Xcode (10.1) is too outdated.
Please update to Xcode 10.2.1 (or delete it).
Xcode can be updated from the App Store.
```

I used Mac's SpotLight Search to find Xcode and run it. I was prompted to install additional components. I eventually got to a `Welcome to Xcode Version 10.1 (10B61)` screen. I then went back to `Application` > `App Store` > `Searched for Xcode` and eventually discovered that I wasn't being offered any upgrade options because I needed to be on MacOS 10.14.3 or later before being able to install Xcode 10.2. 
So after several minutes to upgrade my MacOs and then Xcode, I'm back to being able to install my `bosh-cli`.







