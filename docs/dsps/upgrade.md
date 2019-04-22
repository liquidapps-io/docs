Upgrade DSP Node
================

## Upgrade NPM Package
```bash
sudo su -
nvm use 10
npm update -g @liquidapps/dsp --unsafe-perm=true
setup-dsp
systemctl stop dsp
systemctl start dsp
exit
```

## Check logs
```bash
sudo pm2 logs
```

output should look like:
```
1|demux    | demux listening on port 3195!
1|demux    | ws connected
1|demux    | got abi
0|dapp-services-node  | services listening on port 3115!
0|dapp-services-node  | service node webhook listening on port 8812!
2|ipfs-dapp-service-node  | ipfs listening on port 13115!
2|ipfs-dapp-service-node  | commited to: ipfs://zb2rhmy65F3REf8SZp7De11gxtECBGgUKaLdiDj7MCGCHxbDW
2|ipfs-dapp-service-node  | ipfs connection established
```
