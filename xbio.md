# Xbio

## Request Account
xbio is a jump server to allow users to signin to juno from home without VPN. This is optional and not required if you have VPN access. VPN is also recommended as it will also allow access to all other MSK services and is more secure.

- Go to https://thespot.mskcc.org to request xbio account.
- Enter your User ID (UID). This can be found by typing the command: id once you have logged into Juno.
- Select Dr. Sohrab Shah as your PI
- Provide your public key.
- Set the expiration date as end of year, for the following year.
- After you submit, you should get an email notifying you that the ticket is awaiting approval.


## Quick Start


xbio is the gateway machine to the HPC cluster:

You can log into xbio with
```
ssh -A -p 2222 xbio.mskcc.org
```
then from xbio, ssh into the specific cluster

```
ssh -A <juno>
```

Note that for -A (SSH agent forwarding to work, you need have the ssh-agent daemon running and setup properly)

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa.pub
```
Reset your Terminal.

But ensure your ssh-agent is still alive...
```
$ ps auxw | grep ssh
<user>            1098   0.0  0.0  4289368   5212   ??  S    11:43AM   0:00.01 /usr/bin/ssh-agent -l
<user>             1725   0.0  0.0  4286488    844 s000  S+    3:06PM   0:00.00 grep ssh
```
Once setup properly you can also jump straight to juno with the following configs:

```
Host xbio
Hostname xbio.mskcc.org
Port 2222
ForwardAgent yes
ForwardX11 yes
ForwardX11Trusted yes
User YOUR_USERNAME
 
Host juno
Hostname juno
ProxyCommand ssh -q -x xbio -W %h:22
ForwardX11 yes
ForwardX11Trusted yes
User YOUR_USERNAME
```

This is not default behavior in MacOS 10.12 “Sierra”.

To use the keychain in Sierra add “AddKeysToAgent yes” and “UseKeychain yes” to your config file (~/.ssh/config). Here is an example config file:

```
Host *

   ForwardAgent yes
   Protocol 2
   AddKeysToAgent yes
   UseKeychain yes
```
Useful links: http://mskcchpc.org/dashboard.action

