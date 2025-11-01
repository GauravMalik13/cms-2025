# CMSSW TIPS AND TRICKS

  
## Table of Content
- [Use VS Code instead of vim in terminal.](#use-vs-code-instead-of-vim-in-terminal)
- [Kerberos Ticket System](#kerberos-ticket-system)
- [SSH Configuration](#ssh-configuration)
- [Kerberos Commands](#kerberos-commands)
- [SSH and scp Commands](#ssh-and-scp-commands)


## Use VS Code instead of vim in terminal.

* ### Install Remote-Development Extensions pack from Microsoft.
  
    * Configure the Remote-SSH Extension to use a tmp storage not afs
    * Follow the instructions given in this webpage : [Link](https://atlas-software.docs.cern.ch/athena/ide/ssh/)
    
    * Or Use This Setting config (Edit Accordingly):  

        ```json
        "remote.SSH.lockfilesInTmp": true,
        "remote.SSH.serverInstallPath": {
            "Hostname/IP Address Eg. lxplus.cern.ch": "/tmp/<USERNAME>/"
        },
        "remote.SSH.useLocalServer": false,
        "remote.SSH.showLoginTerminal": true,
        "remote.SSH.remotePlatform": {
            "Hostname/IP Address": "linux"
        },
            ```
* ### Configure C/C++ Extension or clangd extension with the `compile_commands.json`:
  
    * The path for compile commands could be found by the command: 
        
        ```bash 
        echo $CMSSW_RELEASE_BASE
        ```
    * You can verify if the compile commands file is present in the dir by using grep like this: 

        ```bash
        ls $CMSSW_RELEASE_BASE | grep compile
        ```

  * Add that path to the extensions (for clangd add the path in clangd.arguments like `–compile-commands-dir=”path/to/the/compile-commands.json’ ` or add this line in `settings.json` : 
    
    ```json
    "clangd.arguments": [
        "--compile-commands-dir=/path/to/compile_commands.json"
    ],
    ```

* ### Configure Python Extension like this: 

  * Find the python interpreter in the `cmssw`: 
    
    ```bash
    which python3
    ``` 

  * Open command palette with `Ctrl + Shift + P `
  * Type : `Python: Select Interpreter` 
  * Select: `Enter Interpreter Path`
  * Add the path you found in box. 

<b> Make Sure You have extensions installed on lxplus otherwise all the above steps won't apply. </b>
  
## Kerberos Ticket System
Kerberos can be used to generate a ticket so that you don't have to type ssh password multiple times a day.
Follow this webpage if the below steps don't work : [Link](https://linux.web.cern.ch/docs/kerberos-access/)

First step is to install kerberos in your system

```bash
sudo apt update && sudo apt install krb5-user
```

Next step is to configure krb5 :

```bash
sudo nano /etc/krb5.conf
```

Add this in the file: 

```
[libdefaults]
    default_realm = CERN.CH
    dns_lookup_kdc = true
    dns_lookup_realm = false
    ticket_lifetime = 25h
    forwardable = true
    proxiable = true
    renew_lifetime = 120h
    default_tkt_enctypes = aes256-cts-hmac-sha1-96 aes256-cts-hmac-sha384-192 c>
    chpw_prompt = true
    rdns = false

[realms]
    CERN.CH = {
        kdc = cerndc.cern.ch
        admin_server = cerndc.cern.ch
        default_domain = cern.ch
    }

[domain_realm]
    .cern.ch = CERN.CH
    cern.ch = CERN.CH
```

## SSH Configuration

Configure SSH to use kerberos ticket and Control Master(for not having to put 2FA authentication again and again along with password):

### The Control Master part for linux is taken from this [Link](https://cern.service-now.com/service-portal?id=kb_article&n=KB0009800)

Open ssh config file with 

```bash
nano ~/.ssh/config
```

Then paste this in the config file.

```bash
Host lxplus
        HostName lxplus*.cern.ch
        User magaurav
        ForwardX11 yes
        GSSAPIAuthentication yes
        GSSAPIDelegateCredentials yes
        RequestTTY yes
        StrictHostKeyChecking no
        UserKnownHostsFile /dev/null
        # Don't Use Proxy Jump with lxplus
        ProxyJump none
        # Public Key Authentication is impossible with lxplus
        PubkeyAuthentication no
        # Forwarding public keys may make sense however to login to other hosts.
        ForwardAgent yes
        # IP addresses move around too much at CERN so ignore them.
        CheckHostIP no
        # Normally a good idea always to keep things alive.
        ServerAliveInterval 100
        # Finally configure the ControlMaster
        # On  a Mac use ControlPath ~/.ssh/%r@%h:%p
        ControlPath /run/user/%i/%r@%h:%p
        ControlMaster auto
        # Persist the socket after the first session is destroyed.
        ControlPersist 1m
```

### Configuration usage:

After Configuring SSH config file and Kerberos config file, 

### Kerberos Commands

Run this command to get your Kerberos ticket:

```bash
kinit username
```

It will ask for your lxplus password and if the ticket is generated successfully then you won't get any output. To verify run: 

```bash 
klist
```

This will print the kerberos ticket with the expiry time. 

To renew the kerberos ticket, run: 

```bash
kinit
```

### SSH and scp Commands:

To login to any lxplus node, use

```bash
ssh lxplus
```

To login to lxplus8, use

```bash
ssh username@lxplus8.cern.ch
```

To copy from `source` to `destination`

```bash
scp path/to/source/file path/to/destination/dir
```

Example to copy file to lxplus(works only if the ssh is configured as provided above):

```bash
scp path/to/file lxplus:/path/to/destinations/
```

If not configured as said, use this instead:

```bash
scp path/to/file username@lxplus.cern.ch:/path/to/destination/dir
```
