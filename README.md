# TIXON-ROOT
roots
services:
  btcpayserver:
    restart: unless-stopped
    image: btcpayserver/btcpayserver:${BTCPAY_VERSION}
    network_mode: host
#    expose:
#    - "49392"
    environment:
      BTCPAY_POSTGRES: User ID=postgres;Host=localhost;Port=5432;Database=btcpayserver${NBITCOIN_NETWORK:-regtest}
      BTCPAY_NETWORK: ${NBITCOIN_NETWORK:-regtest}
      BTCPAY_BIND: 0.0.0.0:49392
      BTCPAY_ROOTPATH: ${BTCPAY_ROOTPATH:-/}
      BTCPAY_SSHCONNECTION: "root@localhost"
      BTCPAY_SSHTRUSTEDFINGERPRINTS: ${BTCPAY_SSHTRUSTEDFINGERPRINTS}
      BTCPAY_SSHKEYFILE: ${BTCPAY_SSHKEYFILE}
      BTCPAY_SSHAUTHORIZEDKEYS: ${BTCPAY_SSHAUTHORIZEDKEYS}
      BTCPAY_DEBUGLOG: btcpay.log
      BTCPAY_CHAINS: "btc"
      BTCPAY_BTCEXPLORERURL: http://localhost:32838/
      BTCPAY_BTCLIGHTNING: "type=lnd-rest;server=https://localhost:10080/;macaroonfilepath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/admin.macaroon;allowinsecure=true"
      BTCPAY_BTCEXTERNALRTL: "server=/rtl/api/authenticate/cookie;cookiefile=/etc/lnd_bitcoin_rtl/.cookie"
      BTCPAY_BTCEXTERNALLNDGRPC: "server=/;macaroonfilepath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/admin.macaroon;macaroondirectorypath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/"
      BTCPAY_BTCEXTERNALLNDREST: "server=/lnd-rest/btc/;macaroonfilepath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/admin.macaroon;macaroondirectorypath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet"
      BTCPAY_BTCEXTERNALLNDSEEDBACKUP: "/etc/lnd_bitcoin/data/chain/bitcoin/${NBITCOIN_NETWORK:-regtest}/walletunlock.json"
      VIRTUAL_NETWORK: nginx-proxy
      VIRTUAL_PORT: 49392
      VIRTUAL_HOST: ${BTCPAY_HOST},${BTCPAY_ADDITIONAL_HOSTS}
      VIRTUAL_HOST_NAME: "btcpay"
      SSL_POLICY: Mozilla-Modern
      LETSENCRYPT_HOST: ${BTCPAY_HOST},${BTCPAY_ADDITIONAL_HOSTS}
      LETSENCRYPT_EMAIL: ${LETSENCRYPT_EMAIL:-<no value>}
    volumes:
    - "btcpay_datadir:/datadir"
    - "nbxplorer_datadir:/root/.nbxplorer"
    - "/mnt/hdd/mynode/lnd:/etc/lnd_bitcoin"
    - "/root/.ssh/authorized_keys:${BTCPAY_SSHAUTHORIZEDKEYS}"
    - "/root/.ssh/id_rsa_btcpay:${BTCPAY_SSHKEYFILE}"
  nbxplorer:
    restart: unless-stopped
    image: nicolasdorier/nbxplorer:${NBXPLORER_VERSION}
    network_mode: host
#    expose:
#    - "32838"
    environment:
      NBXPLORER_NETWORK: ${NBITCOIN_NETWORK:-regtest}
      NBXPLORER_BIND: 0.0.0.0:32838
      NBXPLORER_SIGNALFILESDIR: /datadir
      NBXPLORER_CHAINS: "btc"
      NBXPLORER_BTCRPCURL: http://localhost:8332/
      NBXPLORER_BTCNODEENDPOINT: localhost:8333
    volumes:
    - "nbxplorer_datadir:/datadir"
    - "/mnt/hdd/mynode/bitcoin:/root/.bitcoin"
  postgres:
    restart: unless-stopped
    image: postgres:${POSTGRES_VERSION}
    network_mode: host
    volumes:
    - "postgres_datadir:/var/lib/postgresql/data"
volumes:
  btcpay_datadir: 
  postgres_datadir: 
  nbxplorer_datadir: 
networks: {}
end

services:  btcpayserver:    restart: unless-stopped    image: btcpayserver/btcpayserver:${BTCPAY_VERSION}    network_mode: host#    expose:#    - "49392"    environment:      BTCPAY_POSTGRES: User ID=postgres;Host=localhost;Port=5432;Database=btcpayserver${NBITCOIN_NETWORK:-regtest}      BTCPAY_NETWORK: ${NBITCOIN_NETWORK:-regtest}      BTCPAY_BIND: 0.0.0.0:49392      BTCPAY_ROOTPATH: ${BTCPAY_ROOTPATH:-/}      BTCPAY_SSHCONNECTION: "root@localhost"      BTCPAY_SSHTRUSTEDFINGERPRINTS: ${BTCPAY_SSHTRUSTEDFINGERPRINTS}      BTCPAY_SSHKEYFILE: ${BTCPAY_SSHKEYFILE}      BTCPAY_SSHAUTHORIZEDKEYS: ${BTCPAY_SSHAUTHORIZEDKEYS}      BTCPAY_DEBUGLOG: btcpay.log      BTCPAY_CHAINS: "btc"      BTCPAY_BTCEXPLORERURL: http://localhost:32838/      BTCPAY_BTCLIGHTNING: "type=lnd-rest;server=https://localhost:10080/;macaroonfilepath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/admin.macaroon;allowinsecure=true"      BTCPAY_BTCEXTERNALRTL: "server=/rtl/api/authenticate/cookie;cookiefile=/etc/lnd_bitcoin_rtl/.cookie"      BTCPAY_BTCEXTERNALLNDGRPC: "server=/;macaroonfilepath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/admin.macaroon;macaroondirectorypath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/"      BTCPAY_BTCEXTERNALLNDREST: "server=/lnd-rest/btc/;macaroonfilepath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet/admin.macaroon;macaroondirectorypath=/etc/lnd_bitcoin/data/chain/bitcoin/mainnet"      BTCPAY_BTCEXTERNALLNDSEEDBACKUP: "/etc/lnd_bitcoin/data/chain/bitcoin/${NBITCOIN_NETWORK:-regtest}/walletunlock.json"      VIRTUAL_NETWORK: nginx-proxy      VIRTUAL_PORT: 49392      VIRTUAL_HOST: ${BTCPAY_HOST},${BTCPAY_ADDITIONAL_HOSTS}      VIRTUAL_HOST_NAME: "btcpay"      SSL_POLICY: Mozilla-Modern      LETSENCRYPT_HOST: ${BTCPAY_HOST},${BTCPAY_ADDITIONAL_HOSTS}      LETSENCRYPT_EMAIL: ${LETSENCRYPT_EMAIL:-<no value>}    volumes:    - "btcpay_datadir:/datadir"    - "nbxplorer_datadir:/root/.nbxplorer"    - "/mnt/hdd/mynode/lnd:/etc/lnd_bitcoin"    - "/root/.ssh/authorized_keys:${BTCPAY_SSHAUTHORIZEDKEYS}"    - "/root/.ssh/id_rsa_btcpay:${BTCPAY_SSHKEYFILE}"  nbxplorer:    restart: unless-stopped    image: nicolasdorier/nbxplorer:${NBXPLORER_VERSION}    network_mode: host#    expose:#    - "32838"    environment:      NBXPLORER_NETWORK: ${NBITCOIN_NETWORK:-regtest}      NBXPLORER_BIND: 0.0.0.0:32838      NBXPLORER_SIGNALFILESDIR: /datadir      NBXPLORER_CHAINS: "btc"      NBXPLORER_BTCRPCURL: http://localhost:8332/      NBXPLORER_BTCNODEENDPOINT: localhost:8333    volumes:    - "nbxplorer_datadir:/datadir"    - "/mnt/hdd/mynode/bitcoin:/root/.bitcoin"  postgres:    restart: unless-stopped    image: postgres:${POSTGRES_VERSION}    network_mode: host    volumes:    - "postgres_datadir:/var/lib/postgresql/data"volumes:  btcpay_datadir:   postgres_datadir:   nbxplorer_datadir: networks: {}
