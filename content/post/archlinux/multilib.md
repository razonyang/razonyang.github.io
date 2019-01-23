---
draft: true
---

```
# vim /etc/pacman.conf
```

Uncomment multilib

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

```
# pacman -Syu
```
