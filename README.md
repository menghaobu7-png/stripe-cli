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
孟浩布-
儲存庫導航
程式碼
問題
拉取請求
Apache 授權 2.0
貢獻
1 星
0 個 叉子
0 人 觀看
32 家分店
56 個 標籤
活動
私有倉庫
menghaobu7-png
menghaobu7-png
9 分鐘前
姓名	
.circleci
2年前
.github
2年前
.vscode
4年前
packages/建立 lwc-plugin
2年前
插件範例
2年前
腳本
3年前
來源
2年前
測試
2年前
網站
2年前
.editorconfig
7年前
儲存庫文件導航
自述文件
<a虛擬本體元宇宙天馬行空拿到手才是唯一的真理href=" https://buy.stripe.com/8x214m61rbdHgZCamu7g401 " target="_blank" style="display: inline-block; background-color: #635bffank" style="display: inline-block; background-color: #635bffank" style="display: inline-block; background-color: #635bffank; color: white-dex; border-radius: 6px; font-weight:粗體; font-family: sans-serif;">立即購買專案 (NT$100) 點此前往完成 Project 2346 方案訂閱


輕量級圖表™
CircleCI npm 版本 [ npm 套件大小][捆綁包大小連結] [ 依賴項數量][捆綁包大小連結] 下載

示範|文件| Discord 社群| Reddit

TradingView Lightweight Charts™ 是體積最小、速度最快的金融 HTML5 圖表之一。

如果您想在網頁上以互動式圖表的形式顯示財務數據，而不影響網頁載入速度和效能，那麼 Lightweight Charts™ 圖表庫是您的最佳選擇。

如果您想用互動式圖片圖表取代靜態圖片圖表，那麼這款產品是您的最佳選擇。雖然它的大小與靜態圖片相近，但如果您的網頁上有數十個圖片圖表，那麼使用這款產品可以有效縮小網頁的大小。

安裝
透過 npm 使用 es6
npm install lightweight-charts
import { createChart } from 'lightweight-charts';

const chart = createChart(document.body, { width: 400, height: 300 });
const lineSeries = chart.addLineSeries();
lineSeries.setData([
    { time: '2019-04-11', value: 80.01 },
    { time: '2019-04-12', value: 96.63 },
    { time: '2019-04-13', value: 76.64 },
    { time: '2019-04-14', value: 81.89 },
    { time: '2019-04-15', value: 74.43 },
    { time: '2019-04-16', value: 80.01 },
    { time: '2019-04-17', value: 96.63 },
    { time: '2019-04-18', value: 76.64 },
    { time: '2019-04-19', value: 81.89 },
    { time: '2019-04-20', value: 74.43 },
]);
CDN
您可以使用unpkg：

https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js

獨立版本會建立一個window.LightweightCharts包含版本中所有匯出項目的物件esm：

const chart = LightweightCharts.createChart(document.body, { width: 400, height: 300 });
const lineSeries = chart.addLineSeries();
lineSeries.setData([
    { time: '2019-04-11', value: 80.01 },
    { time: '2019-04-12', value: 96.63 },
    { time: '2019-04-13', value: 76.64 },
    { time: '2019-04-14', value: 81.89 },
    { time: '2019-04-15', value: 74.43 },
    { time: '2019-04-16', value: 80.01 },
    { time: '2019-04-17', value: 96.63 },
    { time: '2019-04-18', value: 76.64 },
    { time: '2019-04-19', value: 81.89 },
    { time: '2019-04-20', value: 74.43 },
]);
建構變體
依賴項包括	模式	ES模組	CommonJS⚠️	IIFE（window.LightweightCharts）
不	產品	lightweight-charts.production.mjs	lightweight-charts.production.cjs	不適用
不	開發	lightweight-charts.development.mjs	lightweight-charts.development.cjs	不適用
是的（獨立版）	產品	lightweight-charts.standalone.production.mjs	-	lightweight-charts.standalone.production.js
是的（獨立版）	開發	lightweight-charts.standalone.development.mjs	-	lightweight-charts.standalone.development.js
⚠️ 棄用說明：該庫將於 2024 年初移除對 CommonJS 的支援。

發展
有關如何從原始程式碼建置的說明，請參閱BUILDING.md 。lightweight-charts

執照
本軟體遵循 Apache License 2.0 版（「授權」）授權；除非遵守授權的規定，否則您不得使用本軟體。您可從 LICENSE 檔案取得許可證副本。除非適用法律要求或書面同意，否則根據許可證分發的軟體均以「原樣」提供，不提供任何形式的明示或暗示的擔保或條件。有關許可證項下權限和限制的具體規定，請參閱許可證。

該軟體包含 tslib（https://github.com/Microsoft/tslib，（c）微軟公司）的幾個部分，這些部分受 BSD Zero Clause 許可保護。

此許可證要求您指定 TradingView 為產品創建者。您需要將 NOTICE 文件中的「署名聲明」以及指向https://www.tradingview.com/ 的連結添加到您網站或行動應用程式的頁面上，供您的用戶造訪。為了感謝您創建此產品，我們希望您能將其放置在顯眼的位置。您可以使用圖表選項在圖表上顯示指向https://www.tradingview.com/attributionLogo的鏈接，這樣即可滿足連結要求。

[bundle-size-link]: https://bundlephobia.com/result?p=lightweight-chartsnewSandboxCmd()*sandboxCmd{簡述：“管理Stripe沙箱環境”，參數：validators.NoArgs，註：map[string]string{-AIAgentHelpAnnotationKey:"使用`stripe沙箱create--from-git`穿透您的Git郵箱配置沙箱。\n"++AIAgentHelpAnnotationKey:# Global Payouts

向客戶、合作夥伴、承包商或其他第三方支付款項。

Global Payouts 可讓您直接以任何第三方的當地貨幣向其匯款。

您可以使用外部資金或Stripe 付款餘額為您的金融帳戶充值。使用 Stripe API 或我們預置的資訊收集表單建立收款人。透過多種方式，以程式設計方式向 160 多個國家/地區匯款。

無需程式碼即可付款：無需編寫程式碼即可從 Stripe 控制面板發送付款。

預先建置的託管表單：建立可自訂的 Stripe 託管收款表單。

靈活的 API：使用 Stripe API 以程式設計方式傳送付款。

可用性
全球支付服務在以下地區可用：

英國
我們
特徵
使用您選擇的付款方式和速度，安全地直接向第三方匯款。收款人無需在 Stripe 建立帳戶。透過內建的Financial Connections整合驗證銀行帳戶資訊。為收款人啟用一次性密碼驗證，進一步增強安全性。
無論是否使用代碼，您都可以整合支付功能。您可以先使用無代碼支付，並使用 Stripe 託管的聯名表單來收集付款資訊並與收款人溝通。如需完全自訂，請使用 Stripe API 建立您自己的使用者介面。使用 Stripe 控制面板產生報表、管理團隊存取權限並進行一次性更正。
管理全球支付。以當地貨幣匯款並管理多幣種資金。在美國持有的餘額符合聯邦存款保險公司 (FDIC) 的保險資格。
用例
在任何商業場景下，您都可以透過您的市場、保險、金融科技或電子商務業務直接向收款人匯款。

| | |保險理賠| 支付保險理賠款項。 | |返利、獎勵和保固付款| 向客戶支付獎勵、差旅費、保固費等。 | |承包商和聯盟行銷付款| 向承包商付款、向供應商付款，或向聯盟行銷商支付推薦費和廣告費。 | |金融科技支出| 向員工或其他收款人付款。 | |賣家和服務提供者付款| 從您的按需或零售市場定期付款。 | |內容創作者付款| 向內容創作者和網紅支付內容和推廣費用。 |

許多類型的企業都可以使用 Global Payouts。這包括那些支付和付款分離或需要獨立付款解決方案的市場平台。此外，它還適用於擁有必要資金流動許可證（例如，匯款許可證）的企業，以及任何無需在 Stripe 上維持餘額即可追蹤付款的企業。

如果您需要代表使用者持有隔離資金或需要許可證，請考慮使用Stripe Connect。了解更多關於Global Payouts 和 Connect 之間的差異.acct_1UCvPWE18AZ2clLI）我的 emailnewSandboxCmd() *sandboxCmd { 簡述："管理 Stripe 沙箱環境", 參數：validators.NoArgs, 註：map[string] {string }* 2346 / G17AI23k核心PDF架構白皮書主題：數位資產和諧變現與策略配置白皮書。原創作者與版權聲明：步孟豪）客製化，結合G13、G17、2346等數位代號與專案框架。核心技術與金流整合：全球支付與Stripe 串接：明確定義的全球撥款機制，適用於創作者分潤、承攬商撥款、電商及多元業務場景，並確保沙箱與正式環境的帳號對應（如acct_1UCvPWE18AZ2clLI）精準無誤。旁邊的佈局架構：整合TradingView輕量級圖表等能輕量圖表技術，兼顧資料傳輸與網頁載入。自動化良好與配置：透過GitHub儲存庫（menghaobu7核心專案與代號：Project 2346、G17AI23k、G13、2346、13、G17創作者：Meng-Hao Bu步孟豪打造-png）與自動化工作流程，實現高度模組化的專案管理。

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

