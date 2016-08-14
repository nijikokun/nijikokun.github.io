---
layout: post
date: 2014-12-17 04:39:20
episode: 4
title: Remote File Editing with OSXFuse & SSHFS
---


## Install Dependencies

- Install [OSXFuse][1] with Mac Fuse Compatibility enabled.
- Install [SSHFS][2]

## Verify Installation

```bash
$ sshfs --version
```

You should read something similar to the following:

```
SSHFS version 2.4 (OSXFUSE SSHFS 2.4.1)
OSXFUSE library version: FUSE 2.7.3 / OSXFUSE 2.7.3
no mount point
```

### Mount directory

```bash
$ mkdir ~/<remote-site-domain>
$ sshfs -o IdentityFile=<private-key> user@host:/<directory> ~/<remote-site-domain>
```

Since we are using SSH this step requires you to have [setup SSH keys][3] for the remote location.

Replace `<remote-site-domain>` with directory name, generally I use the remote site domain, 
such as `nijikokun.github.io`.

## Start Editing

```bash
$ cd ~/<remote-site-domain>
$ atom .
```

Happy Editing, no complex sync systems or editor plugins required.

## Unmounting

When you're done editing to unmount the remote server simply use the `umount` command:

```bash
$ sudo umount -f ~/<remote-site-domain>
```

[1]: http://sourceforge.net/projects/osxfuse/
[2]: https://github.com/osxfuse/sshfs/downloads
[3]: http://www.howtogeek.com/168147/add-public-ssh-key-to-remote-server-in-a-single-command/
