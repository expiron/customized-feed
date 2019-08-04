# Customized Feed
[![License](https://img.shields.io/badge/License-GPL--3.0-brightgreen.svg?style=flat-square)](https://opensource.org/licenses/GPL-3.0)
## Description
A customized OpenWRT [feed](https://openwrt.org/docs/guide-developer/feeds) repository.

## Usage
To use this feed, append feed line to `feeds.conf` or `feeds.conf.default`:
```
src-git custom https://github.com/expiron/customized-feed.git
```
To install all its package definitions, run:
```
./scripts/feeds update custom
./scripts/feeds install -a -p custom
```
