# ANVM Mainnet Node Deployment

## ANVM Mainnet Node Deployment（docker compose）

    mkdir -p /mnt/ali2-mainnet/node-data
    cd /mnt/ali2-mainnet
    chmod 777 node-data
    cat > docker-compose.yaml <<EOF
    version: "3"
    services:
      ali2-node:
        image: public.ecr.aws/v7k6d7f5/ainn:v2.2.0
        restart: always
        environment:
          RUST_LOG: info
        volumes:
          - "./node-data:/data"
        command: |
          --chain=mainnet
          --bootnodes /ip4/47.253.115.236/tcp/30333/p2p/12D3KooWMwhQ4TS9qv2e24VUYwjXthhU8E5z3e3R2HWBkXRTpqZi
          --port 30333
          --rpc-port 9944
          --unsafe-rpc-external
          --rpc-methods Unsafe
          --rpc-cors all
          --enable-offchain-indexing true
          --blocks-pruning=archive
          --state-pruning=archive
          --ethapi=debug,trace,txpool
          --validator
        ports:
          - 30333:30333
          - 9944:9944
    EOF

Run docker-compose

1.  Run node
    

    #Run node
    docker compose up -d
    docker compose ps

2.  Check status
    

    docker compose logs --tail 200 -f

Observing that both "best" and "finalized" are consistently increasing is considered a normal synchronization.
