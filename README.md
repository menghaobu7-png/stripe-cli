# Stripe CLI

![GitHub release (latest by date)](https://img.shields.io/github/v/release/stripe/stripe-cli)
![Build Status](https://github.com/stripe/stripe-cli/actions/workflows/release.yml/badge.svg)

The Stripe CLI helps you build, test, and manage your Stripe integration right from the terminal.

**With the CLI, you can:**

- Securely test webhooks without relying on 3rd party software
- Trigger webhook events or resend events for easy testing
- Tail your API request logs in real-time
- Create, retrieve, update, or delete API objects.

![demo](docs/demo.gif)

## Installation

Stripe CLI is available for macOS, Windows, and Linux for distros like Ubuntu, Debian, RedHat and CentOS.

### Installation script (recommended)

Use the installation script for your operating system to enable automatic updates by default.

**macOS or Linux:**

```sh
curl -fsSL https://cli.stripe.com/install.sh | sh
```

**Windows (PowerShell):**

```powershell
irm https://cli.stripe.com/install.ps1 | iex
```

After installation, open a new terminal to use `stripe`.

### Package managers

Package-manager installations don't support automatic updates. Use your package manager to upgrade the CLI, as described in [Upgrading](#upgrading).

### npm (macOS, Linux, Windows)

If you have Node.js >= 18 installed, you can install via `npm`:

```sh
npm install -g @stripe/cli
```

You can also directly execute commands via `npx`, although this won't add `stripe` to your `PATH`:

```sh
npx @stripe/cli login
```

### macOS

**Homebrew:**

```sh
brew install stripe
```

### Linux

**apt (Debian, Ubuntu):**

```sh
curl -s https://packages.stripe.dev/api/security/keypair/stripe-cli-gpg/public | gpg --dearmor | sudo tee /usr/share/keyrings/stripe.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/stripe.gpg] https://packages.stripe.dev/stripe-cli-debian-local stable main" | sudo tee -a /etc/apt/sources.list.d/stripe.list
sudo apt update
sudo apt install stripe
```

**yum/dnf (RedHat, Fedora, CentOS):**

```sh
echo -e "[Stripe]\nname=stripe\nbaseurl=https://packages.stripe.dev/stripe-cli-rpm-local/\nenabled=1\ngpgcheck=0" | sudo tee -a /etc/yum.repos.d/stripe.repo
sudo yum install stripe
```

### Windows

**WinGet:**

```sh
winget install Stripe.StripeCLI
```

**Scoop:**

```sh
scoop bucket add stripe https://github.com/stripe/scoop-stripe-cli.git
scoop install stripe
```

### Docker

The CLI is also available as a Docker image: [`stripe/stripe-cli`](https://hub.docker.com/r/stripe/stripe-cli).

```sh
docker run --rm -it stripe/stripe-cli version
stripe version x.y.z (beta)
```

**Password Store Setup with Docker**

While test mode doesn’t require password store, you will need to set it up if you wish to perform live mode requests.

> You can also make live mode requests on a per command basis by attaching the `--api-key` flag.

1. Create `entrypoint.sh`

```sh
#!/bin/sh
if ! [ -f ~/.gnupg/trustdb.gpg ] ; then
  chmod 700 ~/.gnupg/
  gpg --quick-generate-key stripe-live # This will generate a gpg key called "stripe-live"
fi
if ! [ -f ~/.password-store/.gpg-id ] ; then
  pass init stripe-live # This will initialize a password store record named "stripe-live", using the gpg key above
  pass insert stripe-live # This will insert value for the password store "stripe-live", which we will put Stripe Live Secret Key in
fi

string="$@"
liveflag="--live"

if [ -z "${string##*$liveflag*}" ] ;then
  OPTS="--api-key $(pass show stripe-live)" # This will use the content of the password store "stripe-live" which was inserted in line 8
fi

#pass insert stripe-live
/bin/stripe  $@ $OPTS
```

2. Create a docker file `Dockerfile-cli`

```sh
FROM  stripe/stripe-cli:vx.x.x
RUN  apk  add  pass  gpg-agent
COPY  ./entrypoint.sh  /entrypoint.sh
ENTRYPOINT  [ "/entrypoint.sh" ]
```

3. Build the docker image

```sh
docker build -t stripe-cli -f Dockerfile-cli .
```

4. Run the docker image with password volumes, replacing `$command` with the appropraite Stripe CLI command (i.e `customers list`)

```sh
docker run --rm -it -v stripe-config://root/.config/stripe/ -v stripe-gpg://root/.gnupg/ -v stripe-pass://root/.password-store/ stripe-cli $command
```

> For live mode requests append `--live` after `$command`.

### Without package managers

Download the latest release for your platform from the [GitHub Releases page](https://github.com/stripe/stripe-cli/releases/latest).

**macOS:**

```sh
tar -xvf stripe_X.X.X_mac-os_ARCH.tar.gz
```

Optionally move the `stripe` binary to `/usr/local/bin` for global access.

**Linux:**

```sh
tar -xvf stripe_X.X.X_linux_x86_64.tar.gz
```

Move the `stripe` binary to a directory on your `PATH`.

**Windows:**

Unzip `stripe_X.X.X_windows_x86_64.zip` and add the path containing `stripe.exe` to your `Path` environment variable.

> **Note:** Anti-virus software may flag the binary as unsafe. This is a false positive; see [issue #692](https://github.com/stripe/stripe-cli/issues/692) for details.

## Upgrading

### Automatic updates

When you install using a script, the CLI checks for updates as you use it and automatically installs updates within the same major version.

To disable automatic updates, set `STRIPE_NO_AUTO_UPDATE=1` or add `auto_update = false` to `~/.config/stripe/config.toml`.

### Manual upgrades

If you installed using a package manager, use the matching upgrade command.

### npm (macOS, Linux, Windows)

```sh
npm install -g @stripe/cli
```

### macOS

**Homebrew:**

```sh
brew upgrade stripe
```

### Linux

**apt (Debian, Ubuntu):**

```sh
sudo apt update && sudo apt upgrade stripe
```

**yum/dnf (RedHat, Fedora, CentOS):**

```sh
yum update stripe
```

### Windows

**WinGet:**

```sh
winget upgrade Stripe.StripeCLI
```

**Scoop:**

```sh
scoop update stripe
```

### Docker

```sh
docker pull stripe/stripe-cli:latest
```
Because Docker containers are ephemeral, the `stripe login` command isn't supported. Use the [`--api-key` flag](https://docs.stripe.com/cli/api_keys) instead.

### Without package managers

Download the latest release for your platform from the [GitHub Releases page](https://github.com/stripe/stripe-cli/releases/latest) and replace your existing binary.

## Uninstalling

### 1. Remove your plugins

Package managers only remove the `stripe` binary; they leave installed plugins behind. Remove plugins first, while the `stripe` binary is still available:

```sh
stripe plugin uninstall --all
```

### 2. Remove the CLI

**npm (macOS, Linux, Windows):**

```sh
npm uninstall -g @stripe/cli
```

**Homebrew (macOS):**

```sh
brew uninstall stripe
```

**apt (Debian, Ubuntu):**

```sh
sudo apt remove stripe
```

**yum/dnf (RedHat, Fedora, CentOS):**

```sh
sudo yum remove stripe
```

**WinGet (Windows):**

```sh
winget uninstall Stripe.StripeCLI
```

**Scoop (Windows):**

```sh
scoop uninstall stripe
```

**Docker:**

```sh
docker rmi stripe/stripe-cli
```

**Without package managers:**

Delete the `stripe` binary you downloaded.

### 3. Optionally remove configuration and credentials

Uninstalling the CLI intentionally keeps your configuration, so reinstalling preserves your projects and settings. It is only removed if you remove it yourself:

```sh
# Clear stored credentials for every project you are logged into
stripe logout --all

# Remove all remaining CLI configuration
rm -rf "${XDG_CONFIG_HOME:-$HOME/.config}/stripe"
```

## Usage

Installing the CLI provides access to the `stripe` command.

```sh-session
stripe [command]

# Run `--help` for detailed information about CLI commands
stripe [command] help
```

## Commands

The Stripe CLI supports a broad range of commands. Below are some of the most used ones:
- [`login`](https://stripe.com/docs/cli/login)
- [`listen`](https://stripe.com/docs/cli/listen)
- [`trigger`](https://stripe.com/docs/cli/trigger)
- [`logs tail`](https://stripe.com/docs/cli/logs/tail)
- [`events resend`](https://stripe.com/docs/cli/events/resend)
- [`config`](https://stripe.com/docs/cli/config)
- [`open`](https://stripe.com/docs/cli/open)
- [`get`, `post` & `delete` commands](https://stripe.com/docs/cli/get)
- [`resource` commands](https://stripe.com/docs/cli/resources)

## Documentation

For a full reference, see the [CLI reference site](https://stripe.com/docs/cli)

## Telemetry

The Stripe CLI includes a telemetry feature that collects some usage data. See our [telemetry reference](https://stripe.com/docs/cli/telemetry) for details.

## Feedback

Got feedback for us? Please don't hesitate to tell us on [feedback](https://stri.pe/cli-feedback).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for details on developing the Stripe CLI. All contributions are governed by the [code of conduct](CODE_OF_CONDUCT.md).

## License
Copyright (c) Stripe. All rights reserved.

Licensed under the [Apache License 2.0 license](blob/master/LICENSE).
​環境變數範本（.env.example）
​說明專案執行所需的環境變數（如 RPC 節點 URL、私鑰存放變數名稱、API 金鑰等），方便未來部署或協作時對照，但切記敏感金鑰絕不上傳。
​依賴套件清單（requirements.txt 或 package.json）
​標註腳本運作所需的相依套件版本（例如 web3==6.x），確保在不同環境下執行時相容性穩定。
​錯誤處理與防呆機制
​在程式碼中加入 Gas 費上限預警、交易逾時重試，或是餘額不足時的例外處理（Try-Catch），提升自主腳本執行的穩健度。​資產的絕對自主權：
​不依賴中心化交易所（如 OKEx 等）或傳統銀行作為資金中介，透過私鑰直接掌握錢包與合約。
​所有交易簽署與廣播都在本地端完成，不經手任何第三方代管平台。
​程式碼即規則（Code is Law）：
​利用 Python (web3.py) 與標準 RPC 節點，直接與區塊鏈底層及智能合約（如解鎖、提領、流動性調度）互動。
​將日常的資金需求、出金或結帳通道（如串接 Banxa API 等）整合在同一個技術框架內，維持乾淨、正規且自動化的開發者思維。# 欲解鎖數量與合約互動邏輯
nonce = w3.eth.get_transaction_count(my_address)

tx = contract.functions.withdraw(amount_to_unlock).build_transaction({
    'from': my_address,
    'nonce': nonce,
    'gas': 250000,
    'maxFeePerGas': w3.to_wei('30', 'gwei'),
    'maxPriorityFeePerGas': w3.to_wei('2', 'gwei'),
    'chainId': 1
})

signed_tx = w3.eth.account.sign_transaction(tx, private_key=PRIVATE_KEY)
tx_hash = w3.eth.send_raw_transaction(signed_tx.raw_transaction)

print(f"自主解鎖交易已送出，Tx Hash: {w3.to_hex(tx_hash)}")

Sovereign Stack & Code is Law 實戰架構總結
​本地端私鑰隔離：所有區塊鏈互動與交易簽署皆於本機端（Local Environment）完成，私鑰與敏感金鑰絕不上傳至任何伺服器或第三方介面。  
​無中介直接調度：透過標準 RPC 節點直接與底層智能合約及區塊鏈網路互動，確保資產處於完全自主、不可被任意凍結或攔截的主權狀態。  
​合規入口與防呆機制：結合鏈上智能合約解鎖與標準通道（如 Banxa API/SDK 藍圖），並落實環境變數防呆與異常處理，維持乾淨、正規且自動化的開發思維G17AI23kG13 2346 13G17Meng-HaoBu步孟豪
