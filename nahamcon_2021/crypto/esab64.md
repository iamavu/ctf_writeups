---
layout: load_md
title: The White Circle | Nahamcon 2021 | esab64 Writeup
desc: Check out our writeup for esab64 for Nahamcon 2021 capture the flag competition.
image: images/twc_og_banner.jpg
ctf: Nahamcon 2021
parent: nahamcon_2021
category: crypto
challenge: esab64
tags: "crypto"
date: 2021-03-15T00:00:00+00:00
last_modified_at: 2021-03-15T00:00:00+00:00
---



Its base64 backwards

initial string in file: `mxWYntnZiVjMxEjY0kDOhZWZ4cjYxIGZwQmY2ATMxEzNlFjNl13X`

The name is backwards so i reversed the string to:

`X31lNjFlNzExMTA2YmQwZGIxYjc4ZWZhODk0YjExMjViZntnYWxm`
 
base64 decode to: `_}e61e711106bd0db1b78efa894b1125bf{galf`
 
reverse the string once again for flag: `flag{fb5211b498afe87b1bd0db601117e16e}_`

