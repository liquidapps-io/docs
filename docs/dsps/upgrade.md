Upgrade DSP Node
================

For all new releases, please test on the Kylin testnet for at least one week before deploying to a production environment. 

Link: [sample-config.toml](https://raw.githubusercontent.com/liquidapps-io/zeus-sdk/master/boxes/groups/dapp-network/dapp-services-deploy/sample-config.toml)

```bash
sudo su -
systemctl stop dsp
systemctl stop ipfs
systemctl stop nodeos
# if changes to sample-config.toml syntax:
nano ~/.dsp/config.toml
pm2 del all
pm2 kill
npm uninstall -g @liquidapps/dsp
exit

# as USER
sudo chown ubuntu:ubuntu /home/ubuntu/.pm2/rpc.sock /home/ubuntu/.pm2/pub.sock
npm uninstall -g @liquidapps/dsp

sudo su -
npm install -g @liquidapps/dsp --unsafe-perm=true
# Ensure no new updates to the `sample-config.toml` file are present, if so, update your config.toml accordingly.
sudo find / -name sample-config.toml
# nano <PATH>
setup-dsp
systemctl start nodeos
systemctl start ipfs
systemctl start dsp
exit
```