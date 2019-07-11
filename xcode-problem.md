## Problem with BOSH CLI installation due to outdated Xcode on Mac

```
$ brew install bosh-cli
==> Installing bosh-cli from cloudfoundry/tap
Error: Your Xcode (10.1) is too outdated.
Please update to Xcode 10.2.1 (or delete it).
Xcode can be updated from the App Store.
```

I used Mac's SpotLight Search to find Xcode and run it. I was prompted to install additional components. 
I eventually got to a `Welcome to Xcode Version 10.1 (10B61)` screen. 
I then went back to `Application` > `App Store` > `Searched for Xcode` and eventually discovered that I wasn't being offered any upgrade options because I needed to be on MacOS 10.14.3 or later before being able to install Xcode 10.2. 
So after several minutes to upgrade my MacOs and then Xcode, I'm back to being able to install my `bosh-cli`.

[Back](./README.md#3-lets-direnv-to-unclutter-the-profile) to the BBL installation instructions.
