## Go Ethereum
___
## genesis.json 파일 생성

```json
{
  "config": {
    "chainId": 1337,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "londonBlock": 0
  },
  "alloc": {},
  "coinbase": "0x0000000000000000000000000000000000000000",
  "difficulty": "0x20000",
  "extraData": "",
  "gasLimit": "0x2fefd8",
  "nonce": "0x0000000000000042",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp": "0x00"
}
```
___
## Geth 명령어

### geth 초기화

```geth -datadir C:\Geth init "c:\Program Files"\Geth\genesis.json```

### geth 구동 및 javascript console 실행

```geth -networkid 18282  -nodiscover -maxpeers 0 -datadir c:\Geth console```

### console에 표시되는 log를 파일에 저장

```geth --networkid 18282 --nodiscover --maxpeers 0 --datadir c:\Geth console 2>> c:\Geth\geth.log```

### 채굴 수행, HTTP-RPC 활성화 옵션 추가

```geth --networkid 18283 --nodiscover --maxpeers 0 --datadir c:\Geth --mine --miner.threads 1 --http 2>> c:\Geth\geth2.log```

### HTTP-RPC 옵션이 활성화된 실행 중인 네트워크에 RPC로 접속

```geth attach rpc:<http://localhost:8545>```

### HTTP-RPC의 다양한 옵션 추가

```geth --networkid 18283 --nodiscover --maxpeers 0 --datadir c:\Geth --mine --miner.threads 1 --allow-insecure-unlock --http --http.addr "0.0.0.0" --http.port 8545 --http.corsdomain "*" --http.api "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" 2>> c:\Geth\geth2.log```

### 계정 잠금 해제 옵션 추가

```geth --networkid 18283 --nodiscover --maxpeers 0 --datadir c:\Geth --mine --miner.threads 1 --allow-insecure-unlock --unlock 1 --http --http.addr "0.0.0.0" --http.port 8545 --http.corsdomain "*" --http.api "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" 2>> c:\Geth\geth3.log```

### 파일을 이용해서 계정 잠금 해제

```echo password#1> c:\Geth\password.dat```

```geth --networkid 18283 --nodiscover --maxpeers 0 --datadir c:\Geth --mine --miner.threads 1 --allow-insecure-unlock --unlock 0,1 --password c:\Geth\password.dat --http --http.addr "0.0.0.0" --http.port 8545 --http.corsdomain "*" --http.api "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --verbosity 6 2>> c:\Geth\geth3.log```

### Log를 Detail 수준까지 표시하는 옵션 추가

```geth --networkid 18283 --nodiscover --maxpeers 0 --datadir c:\Geth --mine --miner.threads 1 --allow-insecure-unlock --unlock 1 --password c:\Geth\password.dat --http --http.addr "0.0.0.0" --http.port 8545 --http.corsdomain "*" --http.api "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --verbosity 6 2>> c:\Geth\geth4.log```
___
## Geth console 명령어

```javascript
// geth console 명령

// 종료 
exit

// 계정 생성
personal.newAccount("비밀번호")

// 계정 목록 확인
eth.accounts;

// Etherbase 조회
eth.coinbase;

// Etherbase 변경
miner.setEtherbase(eth.accounts[1]);

// 잔고 확인
eth.getBalance(eth.accounts[0]);

// Block number 확인
eth.blockNumber;

// 채굴 (사용할 스레드 수)
miner.start('스레드 수');

// 채굴 종료
miner.stop();

// 계정 잠금 해제 및 Ether 송금
personal.unlockAccount(eth.accounts[1]);
eth.sendTransaction({ from: eth.accounts[1], to: eth.accounts[2], value: web3.toWei(10, 'ether') });

// 트랜잭션 조회
eth.getTransaction('YOUR_TRANSACTION_ADDRESS');

// 미처리된 트랜잭션 조회
eth.pendingTransactions;

// 이더 단위로 잔고 확인
web3.fromWei(eth.getBalance(eth.accounts[1]), 'ether');

// 컨트랙트 배포 시나리오
// 필요한 변수 선언
let abi, bytecode, address, helloWorld, helloWorldContract, helloWorldInstance, newHelloWorld;

// 컨트랙트 컴파일 후 얻은 정보를 미리 변수에 할당
helloWorld = {
  abi: [
    {
      inputs: [{ internalType: 'string', name: 'initMessage', type: 'string' }],
      stateMutability: 'nonpayable',
      type: 'constructor'
    },
    {
      inputs: [],
      name: 'message',
      outputs: [{ internalType: 'string', name: '', type: 'string' }],
      stateMutability: 'view',
      type: 'function'
    },
    {
      inputs: [{ internalType: 'string', name: 'newMessage', type: 'string' }],
      name: 'update',
      outputs: [],
      stateMutability: 'nonpayable',
      type: 'function'
    }
  ],
  bytecode:
    '0x608060405234801561001057600080fd5b5060405161054a38038061054a83398101604081905261002f916100f8565b8051610042906000906020840190610049565b5050610202565b828054610055906101c7565b90600052602060002090601f01602090048101928261007757600085556100bd565b82601f1061009057805160ff19168380011785556100bd565b828001600101855582156100bd579182015b828111156100bd5782518255916020019190600101906100a2565b506100c99291506100cd565b5090565b5b808211156100c957600081556001016100ce565b634e487b7160e01b600052604160045260246000fd5b6000602080838503121561010b57600080fd5b82516001600160401b038082111561012257600080fd5b818501915085601f83011261013657600080fd5b815181811115610148576101486100e2565b604051601f8201601f19908116603f01168101908382118183101715610170576101706100e2565b81604052828152888684870101111561018857600080fd5b600093505b828410156101aa578484018601518185018701529285019261018d565b828411156101bb5760008684830101525b98975050505050505050565b600181811c908216806101db57607f821691505b602082108114156101fc57634e487b7160e01b600052602260045260246000fd5b50919050565b610339806102116000396000f3fe608060405234801561001057600080fd5b50600436106100365760003560e01c80633d7403a31461003b578063e21f37ce14610050575b600080fd5b61004e6100493660046101c2565b61006e565b005b610058610085565b6040516100659190610273565b60405180910390f35b8051610081906000906020840190610113565b5050565b60008054610092906102c8565b80601f01602080910402602001604051908101604052809291908181526020018280546100be906102c8565b801561010b5780601f106100e05761010080835404028352916020019161010b565b820191906000526020600020905b8154815290600101906020018083116100ee57829003601f168201915b505050505081565b82805461011f906102c8565b90600052602060002090601f0160209004810192826101415760008555610187565b82601f1061015a57805160ff1916838001178555610187565b82800160010185558215610187579182015b8281111561018757825182559160200191906001019061016c565b50610193929150610197565b5090565b5b808211156101935760008155600101610198565b634e487b7160e01b600052604160045260246000fd5b6000602082840312156101d457600080fd5b813567ffffffffffffffff808211156101ec57600080fd5b818401915084601f83011261020057600080fd5b813581811115610212576102126101ac565b604051601f8201601f19908116603f0116810190838211818310171561023a5761023a6101ac565b8160405282815287602084870101111561025357600080fd5b826020860160208301376000928101602001929092525095945050505050565b600060208083528351808285015260005b818110156102a057858101830151858201604001528201610284565b818111156102b2576000604083870101525b50601f01601f1916929092016040019392505050565b600181811c908216806102dc57607f821691505b602082108114156102fd57634e487b7160e01b600052602260045260246000fd5b5091905056fea264697066735822122038a1c30bd04d2c4f9a0958e8e818fffde6dfae31fd65b83a8ebeba0660c185dd64736f6c634300080c0033'
};

// 컨트랙트 객체 정의
helloWorldContract = eth.contract(helloWorld.abi);

// 컨트랙트 배포
helloWorldInstance = helloWorldContract.new(
  'Hello World!',
  {
    from: eth.accounts[1],
    data: helloWorld.bytecode,
    gas: 1000000
  },
  function (e, contract) {
    if (e) {
      console.log('err creating contract', e);
    } else {
      if (!contract.address) {
        console.log(
          'Contract transaction send: TransactionHash: ' + contract.transactionHash + ' waiting to be mined...'
        );
      } else {
        console.log('Contract mined! Address: ' + contract.address);
        address = contract.address;
        console.log(JSON.parse(contract));
      }
    }
  }
);

// 배포 완료된 컨트랙트 인스턴스 확인
helloWorldInstance;

// 데이터 조회 함수 호출
// 인스턴스.함수명.call();
helloWorldInstance.greet.call();

// 데이터 변경 함수 호출
// 인스턴스.함수명.sendTransaction(매개변수, 옵션);
helloWorldInstance.setGreeting.sendTransaction('Hello World!!!', { from: eth.accounts[1] });

// 네트워크에 배포된 컨트랙트의 ABI와 주소를 이용해서 접근하기
newHelloWorld = eth.contract(helloWorld.abi);

newHelloWorld.at(address).greet.call();
newHelloWorld.at(address).setGreeting.sendTransaction('Hello Ethereum!', { from: eth.accounts[1] });

```

## Mining
채굴을 통해 ether를 보상을 획득
```
miner.start(1) or miner.start()
```

- DAG(Directed Acyclic Graph) 생성
  - 다소 시간이 걸림(약 15초/1블록)
  - ASIC 내성을 위해 만들어지는 파일
    - ASIC : 주문형 반도체
  - 30,000 블록마다 갱신(약 100시간 마다 갱신되어 약 8MB씩 추가됨)

1. 채굴자 coinbase 확인
   - eth.coinbase;
2. 채굴자 변경 
   - miner.setEtherbase(eth.accounts[1]);
3. 채굴 진행(븡록생성)
   - miner.start() 
4. 채굴 스탑
   - miner.stop()
5. 채굴된 이더 확인
   - eth.getBalance(eth.accounts[0]);
6. 채굴된 이더를 전송하기 위해 계정 unlock
   - personal.unlockAccount(eth.accounts[1]);
7. 이더 전송(트랜잭션 발생 -> 수수료 차감)
   - eth.sendTransaction({ from: eth.accounts[1], to: eth.accounts[2], value: web3.toWei(10, 'ether') });
8. 트랜잭션 조회(트랜잭션만 생성되어 있는 상태. 블록을 생성하며 해당 블록에 트랜잭션을 넣어야함. 블록넘버와 체인id 등이 비어있음)
   - eth.getTransaction('YOUR_TRANSACTION_ADDRESS');
9. 채굴 진행(블록생성, 트랜잭션을 생성한 계좌의 수수료도 함께 받는다.)
   - miner.start()
10. 채굴 스탑
    - miner.stop()
11. 트랜잭션 조회(block Number, chainId 값등이 적혀있는 것을 확인)
    - eth.getTransaction('YOUR_TRANSACTION_ADDRESS');
12. 잔고 조회
    - eth.getBalance(eth.accounts[1])
