[![Build Status](https://travis-ci.org/gnosis/safe-cli.svg?branch=master)](https://travis-ci.org/gnosis/safe-cli)
[![Coverage Status](https://coveralls.io/repos/github/gnosis/safe-cli/badge.svg?branch=master)](https://coveralls.io/github/gnosis/safe-cli?branch=master)
![Python 3.8](https://img.shields.io/badge/Python-3.8-blue.svg)

# Safe-CLI
Command line utility for **Gnosis Safe** contracts. Use it to manage your **Gnosis Safe** easily from the command line

## Installing
**Python >= 3.7** is required. **Python 3.8** is recommended.

```bash
stat venv 2>/dev/null || python -m venv venv
source venv/bin/activate && pip install -r requirements.txt
```

## Using
```bash
source venv/bin/activate
python safe_cli.py <checksummed_safe_address> <ethereum_node_url>
```

Then you should be on the prompt and see information about the Safe, like the owners, version, etc.
Next step would be loading some owners for the Safe. At least `threshold` owners need to be loaded to do operations
on the Safe and at least one of them should have funds for sending transactions.

Loading owners is not needed if you just want to do `read-only` operations.

To load owners:
```
> load_cli_owners <account_private_key>
Loaded account 0xab...cd with balance=123 ether
Set account 0xab..cd as default sender of txs
```

You can also load owners from an environment variable. Before running the `safe-cli`:
```bash
export MY_PRIVATE_KEY=YOUR_EOA_PRIVATE_KEY
```
Then:
```
> load_cli_owners MY_PRIVATE_KEY
Loaded account 0xab...cd with balance=123 ether
Set account 0xab..cd as default sender of txs
```

To check the loaded owners:
```
> show_cli_owners
```

To unload an owner:
```
> unload_cli_owners <ethereum_checksummed_address>
```

Operations currently supported:
- `send_custom <address> <value-wei> <data-hex-str> [--delegate]`: Sends a custom transaction from the Gnosis Safe 
to a contract. If `--delegate` is set a `delegatecall` will be triggered.
- `send_ether <address> <value-wei>`: Sends ether from the Gnosis Safe to another account
- `send_erc20 <address> <token_address> <value>`: Send ERC20 token from the Gnosis Safe to another account
- `add_owner <address>`: Adds a new owner `address` to the Safe.
- `remove_owner <address>`: Removes an owner `address` from the Safe.
- `change_threshold <integer>`: Changes the `threshold` of the Safe.
- `enable_module <address>`: Enable module `address`
- `disable_module <address>`: Disable module `address`
- `change_fallback_handler <address>`: Updates the fallback handler to be `address`. Supported by Safes with `version >= v1.1.0`
- `change_master_copy <address>`: Updates the master copy to be `address`. It's used to update the Safe.  **WARNING: DON'T USE 
THIS IF YOU DON'T KNOW WHAT YOU ARE DOING. ALL YOUR FUNDS COULD BE LOST**
- `update`: Updates the Safe to the latest version (if you are on a known network like `Rinkeby` or `Mainnet`).
**WARNING: DON'T USE THIS IF YOU DON'T KNOW WHAT YOU ARE DOING. ALL YOUR FUNDS COULD BE LOST**

Operations currently supported with transaction service (mainnet, rinkeby):
- `balances`: Returns a list of balances for ERC20 tokens and ether.
- `history`: History of multisig transactions (including pending).

If the information in the information bar is outdated or there's any problem you can force the `safe-cli` to update
the information about the Safe using:
```
> refresh
```

## Creating a new Safe (WIP)
Use `safe_creator.py <node_url> <private_key> --owners <checksummed_address_1> <checksummed_address_2> --threshold <positive-int>`.

Example:
```
python safe_creator.py https://rinkeby.infura.io/v3/token $PRIVATE_KEY --owners 0x848EF06Bb9d1bc79Bb3B04b7Ea0e251C6E788d7c --threshold 1
```

## Demo
For this demo, `PRIVATE_KEY` environment variable was set to a _EOA_ private key (owner of a a previously created and outdated Safe)
and `ETHEREUM_NODE_URL` to a http rinkeby node.
At first, Safe is updated to the last version and then `123 Wei` are sent to the owner of the Safe (it could be any other address).

**Don't use `update` command in mainnet, as it can leave your Gnosis Safe funds stuck. Safe CLI is still a beta**

[![asciicast](https://asciinema.org/a/c7ekQpCl2WJIYEmYYYgdfcfjv.png)](https://asciinema.org/a/c7ekQpCl2WJIYEmYYYgdfcfjv)

Contributors
------------
- [Pedro Arias Ruiz](https://github.com/AsiganTheSunk)
- [Uxío Fuentefría](https://github.com/uxio0) (uxio@gnosis.pm)
