# Hyperledger Fabric network setup to add a new org in existing running network  

Pre Requisite - Hyperledger Binaries and HLF Pre-Requisites software are installed

# Following are the steps to run the setup
1. create a working folder, change directory to working folder
2. git clone https://github.com/ashwanihlf/sample_AddNewPeer.git
3. sudo chmod -R 755 sample_AddNewOrg/
4. cd sample_AddNewOrg  
5. mkdir config  
	<remove config and crypto-config if they are existing before creation of config folder (Optional)>
	5a. sudo rm -rf config
	5b  sudo rm -rf crypto-config

6. export COMPOSE_PROJECT_NAME=net
7. sudo ./generate.sh
8. sudo ./start.sh
9. docker exec -it cli /bin/bash
10. peer chaincode invoke -C mychannel -n samplecc -c '{"function":"initCar","Args":["Ashwani","Blue","BMW"]}'
11. exit
11. docker exec -e "CORE_PEER_ADDRESS=peer0.org2.example.com:7051" -e "CORE_PEER_LOCALMSPID=Org2MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp" cli peer chaincode query -C mychannel -n samplecc -c '{"function":"readCar","Args":["Ashwani"]}'      
12. Open Crypto-config.yaml and change the template count from 1 to 2 for Org3
13.sudo cryptogen extend --config=./crypto-config.yaml
14. docker-compose -f docker-compose-new-peer.yaml up -d
15. docker exec -it cli bash
16. export CHANNEL_NAME=mychannel
17. CORE_PEER_LOCALMSPID="Org3MSP"
18. CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.example.com/users/Admin@org3.example.com/msp
19. CORE_PEER_ADDRESS=peer1.org3.example.com:7051
20. peer chaincode install -n samplecc -v 1.0 -p github.com/ >&log.txt
21. peer chaincode query -C mychannel -n samplecc -c '{"function":"readCar","Args":["Ashwani"]}'      