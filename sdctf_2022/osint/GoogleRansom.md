---
layout: load_md
title: The White Circle | Sdctf 2022 | Google-Ransom Writeup
desc: Check out our writeup for Google-Ransom for Sdctf 2022 capture the flag competition.
image: images/twc_og_banner.jpg
ctf: Sdctf 2022
parent: sdctf_2022
category: osint
challenge: Google-Ransom
tags: "osint, ava, starry"
date: 2022-05-10T00:00:00+00:00
last_modified_at: 2022-05-10T00:00:00+00:00
---



> Solved by: Avantika (iamavu) and Starry-Lord

```
Google Ransom
Oh no! A hacker has stolen a flag from us and is holding it ransom. Can you help us figure out who created this document? Find their email address and demand they return the flag!

Ransom Letter - https://docs.google.com/document/d/1MbY-aT4WY6jcfTugUEpLTjPQyIL9pnZgX_jP8d8G2Uo/edit
```

We can find the owner of any drive file via google API, simply query the fileID which is present in the URL itself
https://developers.google.com/drive/api/v3/reference/files/get

![](https://i.imgur.com/o1FkoAo.png)

the `*` tells to print all possible fields in the metadata, we get the email as `amy.sdctf@gmail.com` send them a email and we get back our flag

FLAG - `sdctf{0p3n_S0uRCE_1S_aMaz1NG}`

