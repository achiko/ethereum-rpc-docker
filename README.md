Tested on Ubuntu 20.04.3 LTS

#### Prerequisites:
docker
docker-compose
DNS A-Record pointing to your server

#### Usage
```
git clone https://github.com/cventastic/POKT_DOKT.git
git submodule update --init --recursive
git pull --recurse-submodules
cd POKT_DOKT
git reset --hard origin/main && git pull && chown 1005:1001 -R chains/ bootstrap_skript/ && chmod u+x bootstrap_skript/pokt_mainnet.shdocker-compose up -d
```

This compose file needs the following env-vars for example in a .env file inside the repo root-folder:
```
# Private Key for Pocket-Account mainnet can be generated here (https://wallet.pokt.network/)
POCKET_CORE_KEY=
POCKET_CORE_PASSPHRASE=
# Private Key for Pocket-Account testnet can be generated here (https://wallet.testnet.pokt.network/)
POCKET_CORE_KEY_TEST=
POCKET_CORE_PASSPHRASE_TEST=
# POCKET SEEDS (https://docs.pokt.network/home/resources/references/seeds)
POCKET_MAIN_SEEDS=
POCKET_TEST_SEEDS=
# SNAPSHOT (https://github.com/pokt-network/pocket-snapshots)
POCKET_SNAPSHOT=
# LETSENCRYPT
DOMAIN=
MAIL=
```

!!! I added a simple test-script (util/test_relay.sh) to see if the (geth)chains are synced. Dont try to relay before they are.
To test if relaying chains works, pokt-testnet-service and pokt-mainnet service have to be provided with the following command parameters:

```
command: pocket start --simulateRelay
``` 

# EXAMPLES

POKT QUERY:
```
curl -X POST --data '{"relay_network_id":"0002","payload":{"data":"{}","method":"POST","path":"v1/query/height","headers":{}}}' http://pocket-testnet:8081/v1/client/sim
```
GETH QUERY:
```
curl -X POST --data '{"relay_network_id":"0020","payload":{"data":"{\"jsonrpc\":\"2.0\",\"method\":\"eth_getBalance\",\"params\":[\"0x1a8c807a6E4F624fCab01FEBf76a541d31B8345A\", \"latest\"],\"id\":1}","method":"POST","path":"","headers":{}}}' http://pocket-testnet:8081/v1/client/sim
```

# SSL
I you want to test SSL comment in:
```
# - "--certificatesresolvers.myresolver.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory" 
```
Check if there is a file here /traefic/letsencrypt/acme.json if yes, you have to delete it. Otherwise traefik will not issue the certificate for an existing domain.

#### TODO !!!! 
mal alle geth clients mit erigon vergleichen
poly_submodule? -> relativer pfad vom submodule, macht probleme wenn man docker-compose vom root des repos aufruft.

General Monitoring
Bootstrapping from Snapshots

AVALANCHE:
- Archive?
- Monitoring https://docs.avax.network/build/tools/dashboards/README