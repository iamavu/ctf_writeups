---
layout: load_md
title: The White Circle | Hacktivitycon 2021 | Reactor Writeup
desc: Check out our writeup for Reactor for Hacktivitycon 2021 capture the flag competition.
image: images/twc_og_banner.jpg
ctf: Hacktivitycon 2021
parent: hacktivitycon_2021
category: mobile
challenge: Reactor
tags: "mobile, starry"
date: 2021-09-20T00:00:00+00:00
last_modified_at: 2021-09-20T00:00:00+00:00
---


> Solved by: Starry-Lord


Flag gets more and more unscrambled with correct digits. 
Basically 4 digit Pin probabilities plus dynamic deobfuscating made the total of possibilities go down to less than 40, like an Eval situation, where you would have result if your first characters are correct. 

A. Input 1 digit, 0 to 5, (5)
B. Input second digit 0-9(9)
C. Input third digit 0-2(2)
D. Input fourth digit 0-7(flag!)

27 tries on /40

I agree it's most likely not the intended way but 4 digits pin plus Eval like function is vulnerable enough 😉

![](https://i.imgur.com/zkFlMup.jpg)


5 was the only one starting with letter f, four characters and a promising { like in other flags. 

![](https://i.imgur.com/k1LEdFA.jpg)

![](https://i.imgur.com/i0ZIN3Z.jpg)

![](https://i.imgur.com/dVezqS5.jpg)

![](https://i.imgur.com/odN6hY0.jpg)


