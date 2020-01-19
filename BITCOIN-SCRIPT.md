`DUP HASH160 6fad...ab90 EQUALVERIFY CHECKSIG`


# Bitcoin Script

_A mini blockchain contract programming language for locking (and unlocking) bitcoins (that is, unspent transaction outputs)_.


> The script is actually a predicate. It's just an equation that evaluates to true or false. 
> Predicate is a long and unfamiliar word so I called it script. 
>
> – [Satoshi Nakamoto](https://bitcointalk.org/index.php?topic=195.msg1611#msg1611), June 2010




## Guides / Booklets

[**Programming Bitcoin Script Transaction (Crypto) Contracts Step-by-Step**](https://github.com/openblockchains/programming-bitcoin-script) - 
Let's start with building your own bitcoin stack machine from zero / scratch and let's run your own bitcoin ops (operations)...


## More Resources

- [**Script - A mini programming language @ Learn Me A Bitcoin**](https://learnmeabitcoin.com/guide/script) by Greg Walker
- [**Script @ Bitcoin Wiki**](https://en.bitcoin.it/wiki/Script)
- [**The Bitcoin Script Language**](http://davidederosa.com/basic-blockchain-programming/bitcoin-script-language-part-one/) by Davide De Rosa
 




## Standard (Lock) Scripts / Contracts

- **P2PK** (Pay To Pubkey)
  - Example: `<pubKey> OP_CHECKSIG` - unlock with `<sig>`
- **P2PKH** (Pay To Pubkey Hash) 
  - Example: `OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG` - unlock with `<sig> <pubKey>`

**More**

- **P2MS** (Pay To Multisig) - lets you to lock bitcoins to multiple public keys, and require signatures for some (or all) of those public keys to unlock it
  - Example 1 out of 2 keys required: `OP_1 <pubKey> <pubKey> OP_2 OP_CHECKMULTISIG` - unlock with `<sig>` 
- **P2SH** (Pay To Script Hash) - lets you create your own custom "redeem scripts", but still be able to share them easily with other people
  - Example: `OP_HASH160 <scriptHash> OP_EQUAL` - unlock with inputs required by the script (e.g. sig etc.) and the "redeem script" itself
- **NULL DATA** - lets you store data in bitcoin transactions
  - Example: `OP_RETURN <data>`



## Higher-Level Script Languages

- Ivy
- Miniscript by Blockstream


## Alternative Languages

- Simplicity by Blockstream

 
 
 ## Script Grammar
 
Script in the BNF (Backus-Naur form) grammar:

```
<script> ::= <unlocking-script> <locking-script>
<unlocking-script> ::= <constants> | <empty>
<locking-script> ::= <script-block> ["OP_RETURN" <non-script-data>]
<script-block> ::= <function> [<script-block>] | 
        <push-data> [<script-block>] |
        <branch> [<script-block>] |
        "OP_RETURN" [<script-block>] |
        <empty>
<branch> ::= <op-if> <script-block> ["OP_ELSE" <script-block>]
 			"OP_ENDIF"
<op-if> ::= "OP_IF" | "OP_NOTIF"
<constants> ::= <constant> [<constants>]
<constant> ::= "OP_PUSHDATA1" <count-1> "bytes" |
				"OP_PUSHDATA2" <count-2> "bytes" |
            "OP_PUSHDATA4" <count-4> "bytes" |
            <op-push-xx> "bytes" | <op-false> | 
            <op-true> | "OP_2" | "OP_3" | "OP_4" |
            "OP_5" | "OP_6" | "OP_7" | "OP_8" | "OP_9" | 
            "OP_10" | "OP_11" | "OP_12" | "OP_13" | 
            "OP_14" | "OP_15" | "OP_16"
<count-1> ::= a one byte unsigned integer (0-255)
<count-2> ::= a two byte unsigned integer (0-65535)
<count-4> ::= a four byte unsigned integer (0-4294967295)
<op-push-xx> ::= hex codes 0x01 to 0x4b
<op-false> ::= "OP_0" | "OP_FALSE"
<op-true> ::= "OP_1" | "OP_TRUE"
<function> ::= "OP_NOP" | "OP_VERIFY" | 
    "OP_TOALTSTACK" | "OP_FROMALTSTACK" | "OF_IFDUP" | 
    "OP_DEPTH" | "OP_DROP" | "OP_DUP" | "OP_NIP" | "OP_OVER" |
    "OP_PICK" | "OP_ROLL" | "OP_ROT" | "OP_SWAP" | "OP_TUCK" |
    "OP_2DROP" | "OP_2DUP" | "OP_3DUP" | "OP_2OVER" | 
    "OP_2ROT" | "OP_2SWAP" | "OP_SIZE" | "OP_EQUAL" | 
    "OP_EQUALVERIFY" | "OP_1ADD" | "OP_1SUB" | "OP_NEGATE" |
    "OP_ABS" | "OP_NOT" | "OP_0NOTEQUAL" | "OP_ADD" | 
    "OP_SUB" | "OP_BOOLAND" | "OP_BOOLOR" | "OP_NUMEQUAL" |
    "OP_NUMEQUALVERIFY" | "OP_NUMNOTEQUAL" | "OP_LESSTHAN" |
    "OP_GREATERTHAN" | "OP_LESSTHANOREQUAL" |
    "OP_GREATERTHANOREQUAL" | "OP_MIN" | "OP_MAX" | 
    "OP_WITHIN" | "OP_RIPEMD160" | "OP_SHA1" | "OP_SHA256" |
    "OP_HASH160" | "OP_HASH256" | "OP_CODESEPARATOR" | 
    "OP_CHECKSIG" | "OP_CHECKSIGVERIFY" | "OP_CHECKMULTISIG" |
    "OP_CHECKMULTISIGVERIFY" | "OP_CAT" | "OP_SPLIT" | 
    "OP_AND" | "OP_OR" | "OP_XOR" | "OP_DIV" | "OP_MOD" |
    "OP_NUM2BIN" | "OP_BIN2NUM" | "OP_NOP1" | "OP_NOP2" | 
    "OP_NOP3" | "OP_NOP4" | "OP_NOP5" | "OP_NOP6" | 
    "OP_NOP7" | "OP_NOP8" | "OP_NOP9" | "OP_NOP10" |
    "OP_MUL" | "OP_LSHIFT" | "OP_RSHIFT" | "OP_INVERT" | 
    "OP_2MUL" | "OP_2DIV" | "OP_VERIF" | "OP_VERNOTIF"
<non-script-data> ::= any sequence of bytes
```

It's worth highlighting the following features of this formal grammar:

-   The complete script consists of two sections, the unlocking script and the locking script. The locking script is 
    included in the transaction output that is being spent, the unlocking script is included in the transaction input 
    that is spending the output.
-   The unlocking script can only contain constants. This requirement is part of Validity of Script Consensus Rule, 
    defined later.
-   A branching operator (OP_IF or OP_NOTIF) must have a matching OP_ENDIF.
-   An OP_ELSE can only be included between a branching operator and OP_ENDIF pair. There can only be at most one 
    OP_ELSE between a branching operator and an OP_ENDIF.
-   OP_RETURN may appear at any location in a valid script.

(Source: [Bitcoin SV Specs](https://github.com/bitcoin-sv-specs/protocol/blob/master/updates/genesis-spec.md#formal-grammar-for-bitcoin-script))
