# Prerequisite software
- Linux
- Git
- Curl
- Docker
- JQ

# Install Fabric samples
```
mkdir -p $HOME/go/src/github.com/<your_github_userid>
cd $HOME/go/src/github.com/<your_github_userid>
curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
```

# Pull the Docker containers
```
# use -h option to see the options
./install-fabric.sh -h
Usage: ./install-fabric.sh [-f|--fabric-version <arg>] [-c|--ca-version <arg>] <comp-1> [<comp-2>] ... [<comp-n>] ...
        <comp>: Component to install one or more of  d[ocker]|b[inary]|s[amples]. If none specified, all will be installed
        -f, --fabric-version: FabricVersion (default: '2.5.4')
        -c, --ca-version: Fabric CA Version (default: '1.5.7')

./install-fabric.sh docker samples binary
```

# Running the test network
```
cd /fabric-samples/test-network

# Remove any containers or artifacts
./network.sh down

#Up the network
./network.sh up

# Verify the containers
docker ps -a
```

# Create Channel
```
./network.sh createChannel -c <your-channel-name>
```

# Deploy chaincode on peers and channel
```
# You can replace [go, javascript, typescript, java] to ... in chaincode-... and -ccl ... to deploy your chaincode with your favorite programming language
# If you don't use -c argument, chaincode will deploy on myChannel (default)

./network.sh deployCC -c channel1 -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
```

# Set the path for peer binary and config for core.yaml
```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
```

# Set the environment variables to operate the Peer as Org1
```
# Environment variables for Org1

export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
```

# Initialize the ledger with assets
```
# Replace -C channel1 with -C <your-channel-name>

peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C channel1 -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"InitLedger","Args":[]}'
```

# Query the ledger
```
# Replace -C channel1 with -C <your-channel-name>

# Get all assets
peer chaincode query -C channel1 -n basic -c '{"Args":["GetAllAssets"]}'

# Transfer or change assets
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C channel1 -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'

# You can get all assets to verify the change of this asset
peer chaincode query -C channel1 -n basic -c '{"Args":["GetAllAssets"]}'
```

# Set the environment variables to operate the Peer as Org2
```
# Environment variables for Org2

export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051
```

# Query the ledger
```
# Replace -C channel1 with -C <your-channel-name>

# Get specific assets
peer chaincode query -C channel1 -n basic -c '{"Args":["ReadAsset","asset6"]}'
```
