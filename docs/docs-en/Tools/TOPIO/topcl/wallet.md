# Wallet

## Overview

Wallet lets you create local accounts and public-private key pairs, set default account,list all existing accounts and key pairs,reset keystore file password.

## Wallet Commands

Execute `wallet -h` or ` wallet --help` view wallet commands.

```
COMMANDS:
    createAccount                   Create and manage accounts and public-private key pairs.
    createKey                       Create the public-private key pair.
    list                            List existing accounts and keys.
    resetPassword                   Reset keystore file password.
    setDefault                      Import the account keystore file to default account for sending transactions.

OPTIONS:
    -h --help                       Show a list of commands or help for one command.
```

Execute `wallet createKey -h` or `wallet createKey --help` view help for subcommand `createKey`.

```
$> wallet createkey -h
Create the public-private key pair.

USAGE:
    createKey [options]

OPTIONS:
    -h --help                       Show help information for one command.
    -d --dir                        [the keystore file storage path].

EXAMPLE:
    wallet createKey -d /home/ttt
```

## Command Instructions

### Creating an Account

Create accounts locally. 

Multiple accounts can be created simultaneously.

Please remember your keystore file password. If the passwrod is lost, you will not be able to use the account. And the system has no "forget password" option, you can only retrieve the password through the password hint.

Caution:

> The account created here is a local account, which does not exist on the blockchain. To interact with the blockchain, you need to create an account on the blockchain. 
>
> You can create an account on the blockchain via transferring TOP token to the local account through an account with balance on the blockchain.

**Request**

`wallet createAccount`

**Request Parameter**

无。

**Options**

| Option Name | Default Value             | Type   | Description                                |
| ----------- | ------------------------- | ------ | ------------------------------------------ |
| -d，--dir   | System default directory. | String | The account keystore file store directory. |
| -h,--help   | -                         | -      | Help for the command。                     |

**Response Parameters**

| 参数名称                   | 类型   | 说明                                                         |
| -------------------------- | ------ | ------------------------------------------------------------ |
| Publice Key                | String | Publice key，used for encryption verifying the signature.    |
| Account Address            | String | You can share your public key and account address with anyone.Others need them to interact with you.<br/>The address created here is a normal user account address, beginning with a "T-0" identifier. |
| Account Keystore File Path | String | The account keystore file stores information such as public and private keys and account addresses which is used to set accounts. The private key is used for decryption and transaction signature.<br/>You must nerver share the private key and account keystore file with anyone!They control access to your funds! |

**Request Sample**

`wallet createAccount`

**Response**

* Successful

```
Please set a password for the account keystore file. The password must consist of Numbers and Letters, 8 to 16 characters.
Or Ctrl+D skips this step.
Successfully create an account locally!

You can share your public key and account address with anyone.Others need them to interact with you!
You must nerver share the private key and account keystore file with anyone!They control access to your funds!
You must backup your account keystore file!Without the file,you’ll be impossible to access account funds!
You must remember your password!Without the password,it’s impossible to use the keystore file!
Public Key: BBtwShz7qgisA4RsjpvgmijBAAPlh9m/bRh2OsRLK7erroPUD0vFQcwWh4cwVlaIRugxq9b+L67JMztdLipeygc=
Account Address: T-0-LKQULGZTa6uGPDmEtLMaCLgy922NLQntNs
Account Keystore File Path: /root/Topnetwork/keystore/T-0-LKQULGZTa6uGPDmEtLMaCLgy922NLQntNs
```

* Failed

Enter a password that does not meet the format.

```
Password error!
```

### Create Public-private Key Pairs

You can use the public-private key pair of the node account as the node key registered with the node.<br/>It is recommended that you use `wallet createKey` to create asset-free public and private key pairs to protect your account assets better, which are used to sign the nodes when they are working after they have joined the network.

**Request**

`wallet createKey`

**Request Parameters**

无。

**Options**

| Option Name | Default Value             | Type   | Description                        |
| ----------- | ------------------------- | ------ | ---------------------------------- |
| -d，--dir   | System default directory. | String | The keystore file store directory. |
| -h,--help   | -                         | -      | Help for the command。             |

**Response Parameter**

| Parameter Name     | Parameter Type | Description                                                  |
| ------------------ | -------------- | ------------------------------------------------------------ |
| Publice Key        | String         | Publice key，used for encryption verifying the signature.    |
| Keystore File Path | String         | The keystore file stores information such as public and private keys which is used for decryption and transaction signature.<br/>You must nerver share the private key and account keystore file with anyone!They control access to your funds! |

**Request Sample**

`wallet createKey`

**Response**

* Successful

```
Please set a password for the keystore file. The password must consist of Numbers and Letters, 8 to 16 characters.
Or Ctrl+D skips this step.
Successfully create an key locally!

You can share your public key with anyone.Others need it to interact with you!
You must nerver share the private key and keystore file with anyone!They can use them to make the node malicious.
You must backup your keystore file!Without the file,you may not be able to send transactions.
You must remember your password!Without the password,it’s impossible to use the keystore file!
Public Key: BBYTqmkmNksMjX/ydgnixYP1fVmd0zHQGqW1xCBo4zXNrWf3H/XXqe+NsUkvrSuZ4wtDbJqdE7NDU752gMFd5+g=
Keystore File Path: /root/Topnetwork/keystore/Lgq6CojT16wVRSCEuGcsQRPg8eRsz3auyJ
```

* Failed

Enter a password that does not meet the format.

```
Password error!
```

### Set Default Account

The system uses the newest created account to send transactions.If restart the TOPIO，you need to reset default account to send transactions.

If the new account does not exit on the blockchain,it is necessary to create the account on the blockchain via transferring TOP token to the local account through an account with balance on the blockchain.

**Request**

`wallet setDefault`

**Request Parameters**

| Parameter Name     | Required | Default Value | Parameter Type | Description                |
| ------------------ | -------- | ------------- | -------------- | -------------------------- |
| keystore file path | Yes      | -             | String         | The account keystore file. |

**Options**

| Option Name | Default Value | Type | Description           |
| ----------- | ------------- | ---- | --------------------- |
| -h,--help   | -             | -    | Help for the command. |

**Response Parameters**

None.

**Request Sample**

Caution:

> Please use the account keyStore file . Setting the default account will fail if you use the public-private key pair keystore file.

`wallet SetDefault /root/Topnetwork/keystore/T-0-LPXXAanpbjNPQ4ff4iosZYN5eNfWZWDLky`

**Response Schema**

* Successful

```
T-0-LQcciHQHgsL1dHhzN5vqQKfd8QxKhKrsbs: Set default account successfully.
```

* Failed

```
Please Input Password.
File error! You are using the keystore file, please use the account keystore file to set the default account. 
Failed to set default account.
```

### List Accounts and Public-private Key Pairs

**Request**

```
wallet list
```

**Request Parameters**

None.

**Options**

| Option     | Default Value             | Type   | Description                                                  |
| ---------- | ------------------------- | ------ | ------------------------------------------------------------ |
| -d，--dir  | System default directory. | String | List all accounts and public-private key pairs under the specified directory. |
| -u,--using | -                         | -      | List only the accounts that are being used to send the transaction. |
| -h,--help  | -                         | -      | Help for the command.                                        |

**Response Parameters**

| Parameter Name             | Parameter Type | Description                                                  |
| -------------------------- | -------------- | ------------------------------------------------------------ |
| account                    | String         | Account address.                                             |
| public key                 | String         | Base64 public key of the account.                            |
| account keystore file path | String         | Accout keystore file storage directory.                      |
| status                     | String         | Account usage status, currently in use is "Used as Default ". |
| public key                 | String         | Base64 public key of the public-private key pair.            |
| keystore file path         | String         | Public-private key pair keystore file storage directory.     |

**Request Sample**

* List all accounts and public-private key pairs under the specified directory.

```
wallet list
```

* List the "Used as Default" accounts and public-private key pairs under the specified directory.

```
wallet list -d /home/cathytest -u
```

**Response Schema**

* Successful

List all accounts and public-private key pairs under the specified directory.

```
account #0: T-0-LUGh8HG4sijgQKM14AWuka7vpLZLKwqmY7
account keystore file path: /root/Topnetwork/keystore/T-0-LUGh8HG4sijgQKM14AWuka7vpLZLKwqmY7
public key: BGU3wMJOO803a17V8R5dPtbnWKeMKDH9apltmNo2arqX2vo0sl4T21tLrO/D9NjEWHLR5iXUMFutPTaKO1wOT2I=
status: Used as default

account #1: T-0-LSKvMt2gnwmbZEgki4iZH8iJS58Hg3A9eH
account keystore file path: /root/Topnetwork/keystore/T-0-LSKvMt2gnwmbZEgki4iZH8iJS58Hg3A9eH
public key: BMPTHPnnZqLhhpQi6gBj0c1UsySMvLIwNH7TXZn6XSKbAsYOdsaaqiOm3a5uCQt0htBo/6cbkFI3QvS5SHfjo88=

account #2: T-0-LbiRyAF5M5GEBaqsjweeDZT4pG6PckL44h
account keystore file path: /root/Topnetwork/keystore/T-0-LbiRyAF5M5GEBaqsjweeDZT4pG6PckL44h
public key: BG/sXAnHU+14C+ix4YecSJS4Zr8MN2AQxvrAnZk1UA7/rmF+GWQvsfxg8TY5y3WLmyipogg5gfUmM1S0cUae0Xc=

keystore file path: /root/Topnetwork/keystore/LgjBfaWcf7cqttx4fXcxNwSVcLiNb6UTa9
public key: BBaaDVVYtAIdhQ0UgbFI4CAmx+VMrOl1Hgc89EMPjDlWKKyLzF4koAlrdqexX4z96a5qBvY0z65pm+d292vXrsk=

keystore file path: /root/Topnetwork/keystore/LP1QSHaEBPS4P8N1FhGgjxoFJ2bRnNmiCZ
public key: BCJ6IIlnKlnvSBUHDIG6nTwDiHrsQX//Ff8MmZw5FfdyT8RG+WFZmKgWYy5XaVvj85SxHbAiEMZHAfLqGnt7+tc=

keystore file path: /root/Topnetwork/keystore/LbRE8FmDyNVMyznAa2ocMgmTVDwk3w4ZfV
public key: BDqUPFJydCmyITlsW0qnNnKSdVyj0hMjf4hpJOs9P9+dEri7rJPJBWRe3+06/ruhnXgQmcXgn9L1JLOks0+5RAQ=
```

List the "Used as Default" accounts and public-private key pairs under the specified directory.

```
account #0: T-0-LWjs6udfquYRdG41hdUBbofqr6qaHFZ8Eo
account keystore file path: /home/cathytest/T-0-LWjs6udfquYRdG41hdUBbofqr6qaHFZ8Eo
public key: BP4b6tSWio8EoSKafrlF04azPMmwZASAla4lQAtShInZj1qRmiMkxhhiIVgojCQN/wHQEnZJQDt6M3sjXlODlew=
status: Used as default
```

* Failed

`No Key Pair.`

### Reset Keystore File Password

**Request**

`wallet resetPassword`

**Request Parameters**

| Parameters Name    | Required | Default Value | Parameter Type | Description                                                  |
| ------------------ | -------- | ------------- | -------------- | ------------------------------------------------------------ |
| keystore file path | 是       | -             | String         | Keystore file directory of the account or the public-privte key pair. |

**Option**

| Option Name | Default Value | Type | Description           |
| ----------- | ------------- | ---- | --------------------- |
| -h,--help   | -             | -    | Help for the command. |

**Response Parameters**

None.

**Request Sample**

```
wallet resetPassword /root/Topnetwork/keystore/LgjBfaWcf7cqttx4fXcxNwSVcLiNb6UTa9
```

**Response Schema**

* Successful

```
Reset password successfully!
```

* Failed

```
Invalid key - MAC mismatch
Password error！
Hint: 
```

