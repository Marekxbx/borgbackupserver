
![Borg_Backup_Server](https://raw.githubusercontent.com/Marekxbx/borgbackupserver/master/info/borg-backup-server.png)


We own a small webhosting shop and currently use R1Soft to backup our servers. It's great software, but much too expensive. The goal is to build something comparable based on borg and save us the enormous license fees we currently pay which is over $400 a month (just for software). We will not have bare-metal recovery.

Borg Backup Server is intended to be a server with a large amount of storage that backs up 1 to dozens of client machines. It is a Server/Client type of setup. Borg Server initiates the backups over SSH to the client.

Some of the features planned (quite a bit of this is built already):
- Front End: Responsive GUI, ability to monitor, restore and setup backups even from a Smartphone
- Backend: Built in PHP / MariaDB  Sorry, neither of us know Python
- Security: The repo keys are not kept on the client. There is no way for the client to access the backups without using the software. The backup keys are stored encrypted & obfuscated on the server. The decryption algorythm is encrypted with zend encoder for php. Accessing the DB would not expose any keys. The interface will generate Let's Encrypt SSL and keep it updated as long as there is a hostname.
- Communication: Communicates over SSH. The server uses SSH-Public/Private Keys to communicate. A system is being developed so ssh access is only available during the backup, then is closed back off.
- File Cache: After a successful backup, a cache of the file system is stored and indexed to allow for advanced searching without locking the borg repository.
- Intelligent Queue: You can set the number of simultaneous backups at one time. It will not allow build up of more than one similar task at a time. It will also only allow one task to complete per client so nothing overlaps.
-  Recovery: Select files to recover, advanced searching and compare features. For example, show me which backup where this file changed. Restore back to client, download, or send link to download files. 
- Auto Installer: The client installer connects to the server over ssh, detects operating system, installs correct borg client, and installs SSH keys
- Backup Scheduling: You can set hourly, daily, weekly, monthly schedules at specific times with most borg options supported with custom pruning. Multiple schedules are allowed. For example, a weekly backup every Thursday and Sunday, and then an Hourly Backup, every hour for last 24 hours.
- Repos: Create a repo with the encryption options you want. Repo key is kept encrypted in database. Multiple Repos per client are allowed for custom segmentation of backups. For example, you could have a repo of your MySQL data and then another of your Web files. For security purposes: You can't share repos between clients to gain de-duplication benefits.
- MySQL/MariaDB Feature will dump databases before backup, to ensure consistency. More work is being explored on this feature and it's very rudimentary right now.
- Multi-User: Give access to users to specific clients for self-service recovery. 
- Other Security: Each client is backed up on the server in it's own user directory. Looking at adding quotas, but not implemented anytime soon right now. Each client uses SSH keys, each repo has it's own encryption key. Allowed to use several of the borg encryption options.
- Notifications/Reports: Email notifications about issues. Daily backup summary report/email.
- Interactive Dashboard: Shows at a glace the state of backups, server health, how many are in queue, etc. 
- We have a need to be able to backup remote machines, behind firewalls. So we are looking at building a listener for the client that checks in with the server to initiate commands. However, there's a lot of security issues with this, so it may not make it into the initial release.

--
Is it "open source"? I really don't know. I am open for suggestions.  We've got hundreds of hours of programming in this project and honestly, it was never going to be more than an internal utility for our company, but I know the need is out there for something like this. So extra work is being done to make it a usable product for others.



