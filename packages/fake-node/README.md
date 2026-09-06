# @gasboost/fake-node

Node.js 上で Google Apps Script の `Utilities` を利用するための fake implementation を提供します。

`@gasboost/fake-core` の共通実装と Node.js の標準 API を組み合わせて、GAS の `Utilities` に対応する処理を実装します。

## インストール

```bash
pnpm add -D @gasboost/fake-node
```

npm:

```bash
npm install -D @gasboost/fake-node
```

TypeScript で GAS の型を利用する場合は、必要に応じて型定義も追加してください。

```bash
pnpm add -D @types/google-apps-script
```

## Quick Start

```ts
import { NodeUtilities } from "@gasboost/fake-node";

const Utilities = new NodeUtilities();

const id = Utilities.getUuid();

const encoded = Utilities.base64Encode("hello");

const digest = Utilities.computeDigest(
  Utilities.DigestAlgorithm.SHA_256,
  "hello",
);
```

`NodeUtilities` は `GoogleAppsScript.Utilities.Utilities` を実装しています。

## Public API

現在 package entry point から以下を公開しています。

- `NodeUtilities`
- `Charset`
- `DigestAlgorithms`
- `MacAlgorithms`
- `RsaAlgorithm`

## 実装済み Utilities

現在の `NodeUtilities` では主に以下を実装しています。

### UUID

```ts
Utilities.getUuid();
```

Node.js の `crypto.randomUUID()` を利用可能な場合はそれを使用します。

### Base64

```ts
Utilities.base64Encode(data);
Utilities.base64EncodeWebSafe(data);

Utilities.base64Decode(encoded);
Utilities.base64DecodeWebSafe(encoded);
```

### Date

```ts
Utilities.formatDate(date, timeZone, format);
Utilities.parseDate(value, timeZone, format);
```

### String

```ts
Utilities.formatString(template, ...args);
```

### Digest

```ts
Utilities.computeDigest(Utilities.DigestAlgorithm.SHA_256, value);
```

現在以下の Digest Algorithm 定数を公開しています。

- MD2
- MD5
- SHA_1
- SHA_256
- SHA_384
- SHA_512

MD2 は `@gasboost/fake-core` の実装を利用し、それ以外は Node.js の `crypto` を利用します。

### HMAC

```ts
Utilities.computeHmacSignature(Utilities.MacAlgorithm.HMAC_SHA_256, value, key);

Utilities.computeHmacSha256Signature(value, key);
```

現在以下の Mac Algorithm 定数を公開しています。

- HMAC_MD5
- HMAC_SHA_1
- HMAC_SHA_256
- HMAC_SHA_384
- HMAC_SHA_512

### RSA Signature

```ts
Utilities.computeRsaSignature(
  Utilities.RsaAlgorithm.RSA_SHA_256,
  value,
  privateKey,
);

Utilities.computeRsaSha1Signature(value, privateKey);

Utilities.computeRsaSha256Signature(value, privateKey);
```

### Blob

```ts
const blob = Utilities.newBlob("hello", "text/plain", "hello.txt");
```

Blob の実体には `@gasboost/fake-core` の `InMemoryBlob` を利用します。

### gzip / zip

```ts
const gzipped = Utilities.gzip(blob);
const restored = Utilities.ungzip(gzipped);

const archive = Utilities.zip([blob]);
const files = Utilities.unzip(archive);
```

gzip / ungzip は Node.js の `zlib`、zip / unzip は `fflate` を利用します。

### CSV

```ts
Utilities.parseCsv(csv);
Utilities.parseCsv(csv, ";");
```

quoted field、escaped quote、改行を含む CSV を解析します。

### sleep

```ts
Utilities.sleep(1000);
```

GAS の同期 API に合わせ、指定時間ブロックする実装です。

### JSON

```ts
Utilities.jsonParse(json);
Utilities.jsonStringify(value);
```

## Node.js 依存

`@gasboost/fake-node` は以下の Node.js API / package を利用します。

- `node:crypto`
- `node:util`
- `node:zlib`
- `date-fns-tz`
- `fflate`

そのため、この package は Node.js 環境向けです。

Node.js 固有 API に依存しない fake implementation は `@gasboost/fake-core` に分離されています。

## 責務

`@gasboost/fake-node` が担当するもの:

- Node.js 上での GAS Utilities implementation
- crypto / digest / HMAC / RSA
- Base64
- date format / parse
- Blob generation
- gzip / zip
- CSV parse
- sleep
- JSON parse / stringify

Cache、Properties、Session などの in-memory implementation は `@gasboost/fake-core` が担当します。

## 制約

Google Apps Script の `Utilities` 全 API の完全な emulator ではありません。

現在 `NodeUtilities` に実装されている method のみをサポート対象とします。

## 関連パッケージ

- `@gasboost/fake-core` — 共通 in-memory implementation と Utilities 補助実装

## License

MIT
