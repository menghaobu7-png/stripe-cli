# fi# 通靈迪斯科-極限覺醒-步猛浩-Project-2346-G17AI23k-

> **「是非世事變幻，一心做自己。」**  
> 在霓虹與死寂的交界處，重組最爛的秩序與最自由的程式碼。

---

## 🌌 專案代碼與身份標記

* **專案核心代碼**：`G17AI23K G13 2346 G17`
* **全球總架構師**：步猛浩 (Meng-Hao Bu / 阿步)
* **狀態**：測試網/主網數據持續同步中 🟢

---

## ⚙️ 視覺基調與霓虹長廊架構 (The Neon Machinery Corridor)

本專案的核心視覺與程式架構，融入了極致的賽博龐克與工業廢墟美學：

* **極致張力**：強調規模感與深度的失真，在破敗的底層邏輯中建立龐大的視覺結構。
* **霓虹巨龍核心**：由深藍色、電光紫、螢光綠與熾熱洋紅交織而成的霓虹燈管，從機械廢墟中盤旋而起，如同一條有生命的能量流貫穿整座工業長廊。
* **大氣與光影**：空氣中瀰漫著微塵與薄霧，使霓虹光束立體可見；地面上的積水反射著頭頂光影，創造出上下顛倒的迷幻空間。
* **材質對比**：結合金屬的粗糙、厚重、生鏽與油污感，與發光的玻璃霓虹燈管形成強烈對比。

---

## 🛠️ Stripe Checkout & Connect 串接模組

本儲存庫內建完整的金流串接範例與測試介面，包含：

1. **即時日誌追蹤**：支援終端機即時監控 API 請求與事件。
2. **Webhook 測試支援**：無需第三方複雜軟體即可安全測試事件。
3. **安全防護**：內建 CSRF 憑證與環境變數管理。

### 快速起步範例

確保於根目錄設定好 `.env` 後，可透過 Stripe CLI 進行本地端事件監聽：

```sh
stripe listen --forward-to localhost:4200/webhook


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

Instructions are also available for installing and using the CLI [without a package manager](https://github.com/stripe/stripe-cli/wiki/Installing-and-updating#without-a-package-manager).

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
- [`samples`](https://stripe.com/docs/cli/intro_stripe_samples)
- [`serve`](https://stripe.com/docs/cli/serve)
- [`status`](https://stripe.com/docs/cli/status)
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

See [Developing the Stripe CLI](../../wiki/developing-the-stripe-cli) for more info on how to make contributions to this project.

## License
Copyright (c) Stripe. All rights reserved.

Licensed under the [Apache License 2.0 license](blob/master/LICENSE).

