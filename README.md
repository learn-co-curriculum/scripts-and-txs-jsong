# Introduction to Scripts and Transactions

```python
# import everything and define a test runner function
from importlib import reload
from helper import run_test

import ecc
import helper
import script
import tx
```

### Exercise 1

#### 1.1. Determine a ScriptSig that will satisfy this scriptPubKey:
```
767695935687
```
#### Hint: use the Script.parse method


```python
# Exercise 1.1

from script import Script

hex_script = '767695935687'

# bytes.fromhex the script
# parse the script
```

### Exercise 2

#### 2.1. Determine what this scriptPubKey is doing:
```
6e879169a77ca787
```

#### Hint: Use the Script.parse method and look up what various OP codes do here:
#### https://en.bitcoin.it/wiki/Script


```python
# Exercise 2.1

from script import Script

hex_script = '6e879169a77ca787'

# bytes.fromhex the script
# parse the script
```

### Exercise 3

#### 3.1. Make [this test](/edit/session4/tx.py) pass
```
tx.py:TxTest:test_serialize
```


```python
# Exercise 3.1

reload(tx)
run_test(tx.TxTest('test_serialize'))
```


```python
# Example of how to look up a transaction using fetch_tx() method

from tx import TxIn

prev_tx = bytes.fromhex('d1c789a9c60383bf715f3f6ad9d14b91fe55f3deb369fe5d9280cb1a01793f81')
tx_in = TxIn(prev_tx, 0, b'', 0xffffffff)
print(tx_in.fetch_tx())
```

### Exercise 4


#### 4.1. What is the value and scriptPubKey of the 0th output of this transaction?
```
d1c789a9c60383bf715f3f6ad9d14b91fe55f3deb369fe5d9280cb1a01793f81
```

#### 4.2. Make [these tests](/edit/session4/tx.py) pass
```
tx.py:TxTest:test_input_value
tx.py:TxTest:test_input_pubkey
```

#### Bonus:
#### Cache the requests so that you don't hit blockcypher.com multiple times for the same transaction output.


```python
# Exercise 4.1

from tx import TxIn

prev_tx = bytes.fromhex('d1c789a9c60383bf715f3f6ad9d14b91fe55f3deb369fe5d9280cb1a01793f81')
prev_index = 0

# create the transaction input (use blank script_sig and 0xffffffff for sequence)
# fetch the transaction
# grab the output at the index
# show the amount
# show the script_pubkey
```


```python
# Exercise 4.2

reload(tx)
run_test(tx.TxTest('test_input_value'))
run_test(tx.TxTest('test_input_pubkey'))
```

### Exercise 5

#### 5.1. How much is the transaction fee of this transaction?
```
010000000456919960ac691763688d3d3bcea9ad6ecaf875df5339e148a1fc61c6ed7a069e010000006a47304402204585bcdef85e6b1c6af5c2669d4830ff86e42dd205c0e089bc2a821657e951c002201024a10366077f87d6bce1f7100ad8cfa8a064b39d4e8fe4ea13a7b71aa8180f012102f0da57e85eec2934a82a585ea337ce2f4998b50ae699dd79f5880e253dafafb7feffffffeb8f51f4038dc17e6313cf831d4f02281c2a468bde0fafd37f1bf882729e7fd3000000006a47304402207899531a52d59a6de200179928ca900254a36b8dff8bb75f5f5d71b1cdc26125022008b422690b8461cb52c3cc30330b23d574351872b7c361e9aae3649071c1a7160121035d5c93d9ac96881f19ba1f686f15f009ded7c62efe85a872e6a19b43c15a2937feffffff567bf40595119d1bb8a3037c356efd56170b64cbcc160fb028fa10704b45d775000000006a47304402204c7c7818424c7f7911da6cddc59655a70af1cb5eaf17c69dadbfc74ffa0b662f02207599e08bc8023693ad4e9527dc42c34210f7a7d1d1ddfc8492b654a11e7620a0012102158b46fbdff65d0172b7989aec8850aa0dae49abfb84c81ae6e5b251a58ace5cfeffffffd63a5e6c16e620f86f375925b21cabaf736c779f88fd04dcad51d26690f7f345010000006a47304402200633ea0d3314bea0d95b3cd8dadb2ef79ea8331ffe1e61f762c0f6daea0fabde022029f23b3e9c30f080446150b23852028751635dcee2be669c2a1686a4b5edf304012103ffd6f4a67e94aba353a00882e563ff2722eb4cff0ad6006e86ee20dfe7520d55feffffff0251430f00000000001976a914ab0c0b2e98b1ab6dbf67d4750b0a56244948a87988ac005a6202000000001976a9143c82d7df364eb6c75be8c80df2b3eda8db57397088ac46430600
```

Fee is simply the sum of the inputs (use the value() method) minus the outputs (use the amount property)

#### 5.2. Make [this test](/edit/session4/tx.py) pass
```
tx.py:TxTest:test_fee
```


```python
# Exercise 5.1

from io import BytesIO
from tx import Tx

hex_tx = '010000000456919960ac691763688d3d3bcea9ad6ecaf875df5339e148a1fc61c6ed7a069e010000006a47304402204585bcdef85e6b1c6af5c2669d4830ff86e42dd205c0e089bc2a821657e951c002201024a10366077f87d6bce1f7100ad8cfa8a064b39d4e8fe4ea13a7b71aa8180f012102f0da57e85eec2934a82a585ea337ce2f4998b50ae699dd79f5880e253dafafb7feffffffeb8f51f4038dc17e6313cf831d4f02281c2a468bde0fafd37f1bf882729e7fd3000000006a47304402207899531a52d59a6de200179928ca900254a36b8dff8bb75f5f5d71b1cdc26125022008b422690b8461cb52c3cc30330b23d574351872b7c361e9aae3649071c1a7160121035d5c93d9ac96881f19ba1f686f15f009ded7c62efe85a872e6a19b43c15a2937feffffff567bf40595119d1bb8a3037c356efd56170b64cbcc160fb028fa10704b45d775000000006a47304402204c7c7818424c7f7911da6cddc59655a70af1cb5eaf17c69dadbfc74ffa0b662f02207599e08bc8023693ad4e9527dc42c34210f7a7d1d1ddfc8492b654a11e7620a0012102158b46fbdff65d0172b7989aec8850aa0dae49abfb84c81ae6e5b251a58ace5cfeffffffd63a5e6c16e620f86f375925b21cabaf736c779f88fd04dcad51d26690f7f345010000006a47304402200633ea0d3314bea0d95b3cd8dadb2ef79ea8331ffe1e61f762c0f6daea0fabde022029f23b3e9c30f080446150b23852028751635dcee2be669c2a1686a4b5edf304012103ffd6f4a67e94aba353a00882e563ff2722eb4cff0ad6006e86ee20dfe7520d55feffffff0251430f00000000001976a914ab0c0b2e98b1ab6dbf67d4750b0a56244948a87988ac005a6202000000001976a9143c82d7df364eb6c75be8c80df2b3eda8db57397088ac46430600'

# bytes.fromhex the tx, make stream
# parse the tx
# initialize input sum
# iterate over all inputs (t.tx_ins)
    # get the values from the TxIn.value method you wrote in 4.2
    # add to input sum
# initialize output sum
# iterate over all outputs (t.tx_outs)
    # get the amounts from the TxOut.amount property
    # add to output sum
# fee is input sum - output sum
```


```python
# Exercise 5.2

reload(tx)
run_test(tx.TxTest('test_fee'))
```


```python
# double_sha256 example to get z

from helper import double_sha256

modified_tx = bytes.fromhex('0100000001813f79011acb80925dfe69b3def355fe914bd1d96a3f5f71bf8303c6a989c7d1000000001976a914a802fc56c704ce87c42d7c92eb75e7896bdc41ae88acfeffffff02a135ef01000000001976a914bc3b654dca7e56b04dca18f2566cdaf02e8d9ada88ac99c39800000000001976a9141c4bc762dd5423e332166702cb75f40df79fea1288ac1943060001000000')
h = double_sha256(modified_tx)
z = int.from_bytes(h, 'big')
print(h.hex())
```

### Exercise 6

#### 6.1. Make [this test](/edit/session4/tx.py) pass
```
tx.py:TxTest:test_sig_hash
```


```python
# Exercise 6.1

reload(tx)
run_test(tx.TxTest('test_sig_hash'))
```


```python
# Validation example

from io import BytesIO

from ecc import S256Point, Signature
from helper import double_sha256
from tx import Tx

modified_tx = bytes.fromhex('0100000001813f79011acb80925dfe69b3def355fe914bd1d96a3f5f71bf8303c6a989c7d1000000001976a914a802fc56c704ce87c42d7c92eb75e7896bdc41ae88acfeffffff02a135ef01000000001976a914bc3b654dca7e56b04dca18f2566cdaf02e8d9ada88ac99c39800000000001976a9141c4bc762dd5423e332166702cb75f40df79fea1288ac1943060001000000')
h = double_sha256(modified_tx)
z = int.from_bytes(h, 'big')

stream = BytesIO(bytes.fromhex('0100000001813f79011acb80925dfe69b3def355fe914bd1d96a3f5f71bf8303c6a989c7d1000000006b483045022100ed81ff192e75a3fd2304004dcadb746fa5e24c5031ccfcf21320b0277457c98f02207a986d955c6e0cb35d446a89d3f56100f4d7f67801c31967743a9c8e10615bed01210349fc4e631e3624a545de3f89f5d8684c7b8138bd94bdd531d2e213bf016b278afeffffff02a135ef01000000001976a914bc3b654dca7e56b04dca18f2566cdaf02e8d9ada88ac99c39800000000001976a9141c4bc762dd5423e332166702cb75f40df79fea1288ac19430600'))
transaction = Tx.parse(stream)
tx_in = transaction.tx_ins[0]
sig = Signature.parse(tx_in.der_signature())
point = S256Point.parse(tx_in.sec_pubkey())
print(point.verify(z, sig))
```

### Exercise 7

#### 7.1. Validate this signature

```
sec = 0349fc4e631e3624a545de3f89f5d8684c7b8138bd94bdd531d2e213bf016b278a
der = 3045022100ed81ff192e75a3fd2304004dcadb746fa5e24c5031ccfcf21320b0277457c98f02207a986d955c6e0cb35d446a89d3f56100f4d7f67801c31967743a9c8e10615bed
z = 27e0c5994dec7824e56dec6b2fcb342eb7cdb0d0957c2fce9882f715e85d81a6
```

Remember:

* `sec` is the serialization of the Public Key
* `der` is the serialization of the Signature
* `z` is the hash that you're verifying the signature against


```python
# Exercise 7.1

from ecc import S256Point, Signature

sec = bytes.fromhex('0349fc4e631e3624a545de3f89f5d8684c7b8138bd94bdd531d2e213bf016b278a')
der = bytes.fromhex('3045022100ed81ff192e75a3fd2304004dcadb746fa5e24c5031ccfcf21320b0277457c98f02207a986d955c6e0cb35d446a89d3f56100f4d7f67801c31967743a9c8e10615bed')
z = 0x27e0c5994dec7824e56dec6b2fcb342eb7cdb0d0957c2fce9882f715e85d81a6

# use S256Point.parse on the sec to get the public point
# use Signature.parse on the der to get the signature
# use S256Point.verify method
```

### Exercise 8

#### 8.1. Validate the signature for the first input in this transaction.
```
01000000012f5ab4d2666744a44864a63162060c2ae36ab0a2375b1c2b6b43077ed5dcbed6000000006a473044022034177d53fcb8e8cba62432c5f6cc3d11c16df1db0bce20b874cfc61128b529e1022040c2681a2845f5eb0c46adb89585604f7bf8397b82db3517afb63f8e3d609c990121035e8b10b675477614809f3dde7fd0e33fb898af6d86f51a65a54c838fddd417a5feffffff02c5872e00000000001976a91441b835c78fb1406305727d8925ff315d90f9bbc588acae2e1700000000001976a914c300e84d277c6c7bcf17190ebc4e7744609f8b0c88ac31470600
```


```python
# Exercise 8.1

from io import BytesIO
from ecc import S256Point, Signature
from tx import Tx

hex_tx = '01000000012f5ab4d2666744a44864a63162060c2ae36ab0a2375b1c2b6b43077ed5dcbed6000000006a473044022034177d53fcb8e8cba62432c5f6cc3d11c16df1db0bce20b874cfc61128b529e1022040c2681a2845f5eb0c46adb89585604f7bf8397b82db3517afb63f8e3d609c990121035e8b10b675477614809f3dde7fd0e33fb898af6d86f51a65a54c838fddd417a5feffffff02c5872e00000000001976a91441b835c78fb1406305727d8925ff315d90f9bbc588acae2e1700000000001976a914c300e84d277c6c7bcf17190ebc4e7744609f8b0c88ac31470600'
stream = BytesIO(bytes.fromhex(hex_tx))
index = 0

# parse the transaction using Tx.parse
# grab the input at index
# use the input's der_signature, sec_pubkey and hash_type methods to get data
# parse the signature using Signature.parse
# parse the sec pubkey using S256Point.parse
# use the sig_hash method on index and hash_type to get z
# use the pubkey to verify the signature (z, sig)
```
