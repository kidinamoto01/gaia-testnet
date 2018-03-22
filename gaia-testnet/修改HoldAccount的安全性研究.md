修改account的地址，通过go install获得不同的binary

执行如下命令

MYNAME=test1
gaia client keys new $MYNAME
gaia client keys list
MYADDR=328883038E4781129EFC0B249D70C7204E409250


$GOPATH/bin/gaia-1 node init $MYADDR --home=$HOME/.gaia1 --chain-id=test
$GOPATH/bin/gaia-2 node init $MYADDR --home=$HOME/.gaia2 --chain-id=test
cp $HOME/.gaia1/config/genesis.json $HOME/.gaia2/config/genesis.json

vi $HOME/.gaia2/config/config.toml
tendermint show_node_id --home=$HOME/.gaia1
b3982990cfd06ebf62f7225b764611f78e64ffc3

$GOPATH/bin/gaia-1 node start --home=$HOME/.gaia1
$GOPATH/bin/gaia-2 node start --home=$HOME/.gaia2


$GOPATH/bin/gaia-2 client init --chain-id=test --node=tcp://localhost:46657
$GOPATH/bin/gaia-2 client query account $MYADDR

$GOPATH/bin/gaia-2 client query candidates

cat $HOME/.gaia2/config/priv_validator.json | jq .pub_key.data
BA92C46AE7DF7DBC66196F525FC0CE16858102FE0EC40A648D21F588C3235E0C
$GOPATH/bin/gaia-2 client tx delegate --amount=10fermion --name=$MYNAME --pubkey=BA92C46AE7DF7DBC66196F525FC0CE16858102FE0EC40A648D21F588C3235E0C

I[03-22|04:01:08.527] Committed state                              module=state height=1 txs=0 appHash=",\ufffd\u0002\ufffd\ufffd\ufffd\ufffd\u0002H\ufffd'F\ufffdf\ufffdq\u000f\ufffdo;"
panic: Panicked questionably: Failed to process committed block (2:44343338363342353534364236443144454436374633463035343346363730314137444143453233): Wrong Block.Header.AppHash.  Expected 2C9902FEC0A7EE0248C427469B66D8710FE96F3B, got 894397388CAAF828C6882E1874927100FCA6858A

goroutine 42 [running]:
github.com/cosmos/gaia/vendor/github.com/tendermint/tmlibs/common.PanicQ(0x18197c0, 0xc422a34f90)
	/Users/b/Documents/go/src/github.com/cosmos/gaia/vendor/github.com/tendermint/tmlibs/common/errors.go:48 +0xf3
github.com/cosmos/gaia/vendor/github.com/tendermint/tendermint/blockchain.(*BlockchainReactor).poolRoutine(0xc42013c000)
	/Users/b/Documents/go/src/github.com/cosmos/gaia/vendor/github.com/tendermint/tendermint/blockchain/reactor.go:308 +0xaa6
created by github.com/cosmos/gaia/vendor/github.com/tendermint/tendermint/blockchain.(*BlockchainReactor).OnStart
	/Users/b/Documents/go/src/github.com/cosmos/gaia/vendor/github.com/tendermint/tendermint/blockchain/reactor.go:108 +0x90



env GOOS=linux GOARCH=amd64 go build github.com/irisnet/iris-hub/cmd/iris


env GOOS=linux GOARCH=amd64 go build github.com/tendermint/tendermint/cmd/tendermint

