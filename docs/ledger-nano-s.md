# Using SCRT with Ledger

Note: This guide is for Ledger Nano S but according to community members it also works for Ledger Nano X.

## Prerequisites

- This guide assumes you have a verified, genuine Ledger Nano S device.
- If you don't, or you using your Ledger device for the first time, you should check Ledger's [Getting Started](https://support.ledger.com/hc/en-us/sections/360001415213-Getting-started) guide.
- We also advise you to check your Ledger's genuineness and upgrade your firmware to the newest one available (`v1.6.0+`).
- Have a machine with [Ledger Live](https://www.ledger.com/ledger-live) installed.
- Have the latest version of our latest binaries installed. You can get it [here](https://github.com/enigmampc/SecretNetwork/releases/latest).

## Install Cosmos Ledger App

- Open Ledger Live and go to Settings (gear icon on the top right corner):
  ![](https://raw.githubusercontent.com/cosmos/ledger-cosmos/master/docs/img/cosmos_app1.png)

- Enable developer mode:  
  ![](https://raw.githubusercontent.com/cosmos/ledger-cosmos/master/docs/img/cosmos_app2.png)

- Now go to Manager and search "Cosmos":
  ![](https://raw.githubusercontent.com/cosmos/ledger-cosmos/master/docs/img/cosmos_app3.png)

- Our binaries require Cosmos App Version `1.5.1` (if you only see a lower version available, like `1.0.0`, then you need to upgrade your Ledger firmware).

- Hit "Install" and wait for the process to complete.

_Ref: https://github.com/cosmos/ledger-cosmos_

## Common commands

These are some basic examples of commands you can use with your Ledger. You may notice that most commands stay the same, you just need to add the `--ledger` flag.

**Note: To run these commands below, or any command that requires signing with your Ledger device, you need your Ledger to be opened on the Cosmos App:**  
![](https://miro.medium.com/max/1536/1*Xfi5_ScAiFn6rr9YBjgFFw.jpeg)
_Ref: https://medium.com/cryptium-cosmos/how-to-store-your-cosmos-atoms-on-your-ledger-and-delegate-with-the-command-line-929eb29705f_

### Prepare your Linux host to work with ledger

Some users may not have their ledger recognized by their Linux host. To fix this issue implement the fix for connection issues on Linux from the [ledger support page](https://support.ledger.com/hc/en-us/articles/115005165269-Connection-issues-with-Windows-or-Linux)

```bash
wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
```

# Install the secretcli Secret Network light client

Get the latest release of `secretcli` for your OS: https://github.com/secretnodes/TheRomulusUpgrade/releases/tag/v0.2.0.

2) Install:

   - Mac/Windows: Rename it from `secretcli-${VERSION}-${OS}` to `secretcli` or `secretcli.exe` and put it in your path.
   - Ubuntu/Debian: `sudo dpkg -i enigma*.deb`

2) Setup

On OSX : Open terminal, and navigate to the directory with the binary you downloaded. You can do this by running the following command.

```bash
cd <type directory where binary is located>
```

If you are on OSX run this to give secretcli permissions to run. Be sure you renamed the downloaded file to secretcli.
```
sudo chmod +x secretcli
```

On Windows : Open CMD, and navigate to the directory with the exe you downloaded. You can do this by running the following command.

```bash
cd <type directory where binary is located>
```

3) Configure:

Note: On OSX add the following before "secretcli"

```bash
./
```

The result should appear as such.

```bash
./secretcli status
```

   ```bash
   # Set the mainnet chain-id
   secretcli config chain-id secret-1
   ```

   ```bash
   secretcli config output json
   ```

   ```bash
   secretcli config indent true
   ```

   ```bash
   # Set the full node address
   secretcli config node tcp://client.secretnodes.org:26657
   ```

   ```bash
   # Verify everything you receive from the full node
   secretcli config trust-node false
   ```

4) Check the installation:

   ```bash
   secretcli status
   ```

### Create an account

> Note: You can use any number you'd like for your account number. Be sure to remember the number you used, so you can recover if needed.

```bash
secretcli keys add <account name> --ledger --account <account number on your Ledger>
```

:warning::warning::warning:

**Please backup the mnemonics!**

### Display your account address

```bash
secretcli keys show -a <account name>
```

### Add an account to `secretcli` that already exists on your Ledger

_You'll use this when you, say, using a different machine._

```bash
secretcli keys add <account name> --ledger --account <account number on your Ledger> --recover
```

**Note! If you run the above command without the `--ledger` flag, the CLI will prompt you to enter your BIP39 mnemonic, which is your Ledger recovery phrase. YOU DO NOT WANT TO DO THIS. This will essentially save your private key locally.**

### Send tokens

```bash
secretcli tx send <account name or address> <to_address> <amount> --ledger
```

### Delegate SCRT to a validator

```bash
secretcli tx staking delegate <validator address> <amount to bond> --from <account key> --gas auto --gas-prices <gasPrice> --ledger
```

### Collect rewards and commission

```bash
secretcli tx distribution withdraw-all-rewards --from <account name> --gas auto --commission --ledger
```

### Vote on proposals

```bash
secretcli tx gov vote <proposal-id> <vote> --from <account name> --ledger
```
