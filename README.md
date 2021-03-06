# Easy-Cryptocurrency

## Installation
```bash
go get github.com/minhtuan221/Easy-Cryptocurrency
```

## Getting Started

#### Import package

```go
package main

import (
	"github.com/minhtuan221/Easy-Cryptocurrency/blockchain"
	"crypto/elliptic"
	"encoding/json"
	"fmt"
)

func main() {
    // ... put your code here
}
```

#### Open a wallet in main()

```go
Curve := elliptic.P256()
var user01, user02 blockchain.Wallet
user01.GenerateKey(Curve)
user02.GenerateKey(Curve)
```

#### Create a transaction and verify it

```go
trans01 := blockchain.Trans{Sender: user01.GetPublicKey(), Balance: 1000, Receiver: user02.GetPublicKey(), Amount: 10.5, Timestamp: "today", PreviousTX: []string{"c", "b", "a"}}
json01, _ := trans01.ForCheckSign()
trans01.Signature = user01.Signature(json01)

checksign := user02.Verify(json01, trans01.Sender, trans01.Signature)
fmt.Println("check signature of transaction: ", checksign)
beforeHash, _ := trans01.ForHash()
fmt.Println("Transaction before hash: \n", beforeHash)
fmt.Println("Transaction hash is: \n", trans01.Hash())
blockchain.PrettyPrint(trans01)
```

#### Blockchain receive a transaction and check it

```go
var recTrans blockchain.Trans
var jsonBlob = []byte(beforeHash)
err := json.Unmarshal(jsonBlob, &recTrans)
if err != nil {
    fmt.Println("error:", err)
}
fmt.Println(recTrans.ForCheckSign())
```
