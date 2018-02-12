# ethOS GPU Monitoring NRPE

Prerequisites: Mining RIG using ethOS+ a Nagios server already setup and Nagios-NRPE-server installed on your ethOS mining rig. ( this way for help regarding this:
[Installing Nagios-Nrpe-Server on ethOS] http://www.davidbayle.com/knowledge-base/install-nagios-nrpe-server-on-ethos/

This procedure will help you fetching the Hash rate data from your ethOS RIG and alert you if you are loosing cards in your mining RIG or if your cards are mining bellow your defined threshold.

This script has to be set in your Mining RIG, then called by your Nagios Server remotely using NRPE.



REQUIREMENTS:

Let's set it up in your nrpe.cfg file ( usually in /etc/nagios/nrpe.cfg )

```bash
# RIG
command[check_rig-hash]=python /usr/lib/nagios/plugins/check_rig-hash[/cc]
```


As described in requirements section of the check/script, if you want it to be able to restart your rig you will need to :

Create a gpu_crashReboot.log file with 0 as value inside and set it with the right permissions :
(as ethos user:)

```bash
sudo echo 0 > /usr/lib/nagios/plugins/gpu_crashReboot.log
sudo chown nagios:nagios /usr/lib/nagios/plugins/gpu_crashReboot.log
```

Edit Sudoers file using :
(as ethos user:)

```bash
sudo visudo
```

Add the following at the end:

```bash
# Nagios NRPE/Check_rig-hash
nagios ALL=(ALL) NOPASSWD: ALL
```

Save and quit.

Do not forget to adjust your panel URL [gpuJsonSite] and the minimum hashrate value by gpu [gpuMinHashRate] to your needs :

```python
  gpuJsonSite = “http://XXXXX.ethosdistro.com/?json=yes”
```

by replacing the XXXXX with your ID [http://XXXXX.ethosdistro.com/?json=yes]

(you can find your ID by typing helpme on your rig directly in command line.)
 
and
```python
  gpuMinHashRate = XX.X
```
 

And the check itself has to be placed in /usr/lib/nagios/plugins/check_rig-hash


Source: http://www.davidbayle.com/knowledge-base/nagios-check-nrpe-check-mining-rig-hash/
