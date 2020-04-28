# Commands

## Kali

+ ### NMap

    > Top Scan without Firewall Protection (Fast)

    ```zsh
    nmap -Pn -n -v -sV -sC 0.0.0.0
    ```

    > Full Scan with Firewall Protection (Slow)

    ```zsh
    nmap -sT -sU -sV -O -T4 -A -Pn --open --max-retries 3 --max-scan-delay 1s --defeat-rst-ratelimit --script-args unsafe=1 -oN "<Outpout Dir>" 0.0.0.0
    ```

---

## Linux

---

## Windows
