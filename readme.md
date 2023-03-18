# OpenWRT dist
[![](https://github.com/simonsmh/openwrt-dist/workflows/Openwrt%20Build%20Bot/badge.svg)](https://github.com/zlwu/passwall-dist/actions)

Build with GitHub Action Workflow daily.

This project is only for OpenWRT routers. Currently it's based on 2203.

[You may want original project here.](http://openwrt-dist.sourceforge.net)

## Openwrt Package Builder

### Usage
#### Step 1
First, Add the public key [pub-dist.pub](./pub-dist.pub) which is paired with private key [key-build](./key-build) for building.

```
wget http://cdn.jsdelivr.net/gh/zlwu/passwall-dist@master/pub-dist.pub
opkg-key add pub-dist.pub
```

#### Step 2
You can get target branch from distfeeds on your router.

```
# cat /etc/opkg/distfeeds.conf
src/gz openwrt_core https://downloads.openwrt.org/releases/21.02.0/targets/x86/64/packages
...
```

Here means _x86/64_ is your's target, you got **packages/_x86/64_** as **branch name** now.

#### Step 3
Search your branch name in the branches list and add the following line to `/etc/opkg/customfeeds.conf`.

```
src/gz pwpkgs http://cdn.jsdelivr.net/gh/zlwu/passwall-dist@{{$BRANCH_NAME}}/pwpkgs
src/gz pwluci http://cdn.jsdelivr.net/gh/zlwu/passwall-dist@{{$BRANCH_NAME}}/pwluci
```

For example, if you want to use `x86_64` packages and you got the branch name as `packages/x86/64`, You could use this line after the previous step.

```
src/gz pwpkgs http://cdn.jsdelivr.net/gh/zlwu/passwall-dist@packages/x86/64/pwpkgs
src/gz pwluci http://cdn.jsdelivr.net/gh/zlwu/passwall-dist@packages/x86/64/pwluci
```

Then install whatever you want.

```
opkg update
opkg install luci-app-passwall luci-i18n-passwall-zh-cn
```

For more detail please check the manifest.

You can also search and install them in LuCI or upload these downloaded files to your router with SCP/SFTP, then login to your router and use opkg to install these ipk files.

## Openwrt Image Builder

Build configurable images with ImageBuilder after the SDK finished building packages. The images are stored in the device named branches, like *image/generic.x86_64*

[Reference for installation](https://openwrt.org/docs/guide-user/installation/generic.sysupgrade)

## Build it yourself
[Check here](https://github.com/zlwu/passwall-dist/blob/master/.github/workflows/main.yml)

You need to make a fork and chage items in the matrix yourself to match your needs. If you need to keep your packages safe, please use `usign` to regenerate private key and make the repo private.

## License
GPLv3

