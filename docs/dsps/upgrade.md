Upgrade DSP Node
================

## Upgrade
```bash
sudo su -
systemctl stop dsp
systemctl stop ipfs
systemctl stop nodeos
pm2 del all
pm2 kill
npm uninstall -g @liquidapps/dsp
exit

# as USER
sudo chown ubuntu:ubuntu /home/ubuntu/.pm2/rpc.sock /home/ubuntu/.pm2/pub.sock
npm uninstall -g @liquidapps/dsp

sudo su -
npm install -g @liquidapps/dsp --unsafe-perm=true
setup-dsp
systemctl start nodeos
systemctl start ipfs
systemctl start dsp
exit
```
