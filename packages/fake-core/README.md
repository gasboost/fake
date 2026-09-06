# @gasboost/fake-core

Google Apps Script の組み込み API に対応する in-memory fake implementation を提供します。

Node.js 固有 API には依存せず、GAS API に依存するコードをテスト可能にするための基礎実装を担当します。

## インストール

```bash
pnpm add -D @gasboost/fake-core
```

npm:

```bash
npm install -D @gasboost/fake-core
```

TypeScript で GAS の型を利用する場合は、必要に応じて型定義も追加してください。

```bash
pnpm add -D @types/google-apps-script
```

## Public API

現在の package entry point からは以下を公開しています。

### Blob

- `InMemoryBlob`

`GoogleAppsScript.Base.Blob` を実装する in-memory Blob です。

```ts
import { InMemoryBlob } from "@gasboost/fake-core";

const blob = new InMemoryBlob("hello", "text/plain", "hello.txt");

blob.getDataAsString();
blob.getBytes();
blob.getName();
```

### Cache

- `InMemoryCache`
- `InMemoryCacheService`

`GoogleAppsScript.Cache.Cache` と `CacheService` に対応する in-memory 実装です。

```ts
import { InMemoryCacheService } from "@gasboost/fake-core";

const CacheService = new InMemoryCacheService();
const cache = CacheService.getScriptCache();

cache.put("key", "value");

console.log(cache.get("key"));
```

`InMemoryCache` は expiration、key length、value size、保存件数など、現在実装されている GAS Cache の制約も再現します。

### Properties Service

- `InMemoryProperties`
- `InMemoryPropertiesService`

`PropertiesService` の Document / User / Script Properties に対応する in-memory 実装です。

```ts
import { InMemoryPropertiesService } from "@gasboost/fake-core";

const PropertiesService = new InMemoryPropertiesService();
const properties = PropertiesService.getScriptProperties();

properties.setProperty("key", "value");

console.log(properties.getProperty("key"));
```

### Session

- `InMemoryContext`
- `InMemorySession`
- `SecurityPolicy`
- `OAuthScope`

GAS の Session を、実行コンテキストと OAuth Scope を明示して再現するための実装です。

```ts
import {
  InMemoryContext,
  InMemorySession,
  OAuthScope,
  SecurityPolicy,
} from "@gasboost/fake-core";

const context = new InMemoryContext(
  "owner@example.com",
  "user@example.com",
  {
    type: "WEB_APP",
    executeAs: "USER",
  },
  new SecurityPolicy([
    OAuthScope.USERINFO_EMAIL,
  ]),
  "ja",
  "Asia/Tokyo",
);

const Session = new InMemorySession(context);

Session.getActiveUser();
Session.getEffectiveUser();
Session.getActiveUserLocale();
Session.getScriptTimeZone();
```

現在の `InMemoryContext` は以下の実行形態を扱います。

- Web App
- Trigger
- Custom Function

実行形態、実行ユーザー、owner、domain、OAuth Scope に基づいて Session の user 情報を判定します。

### Utilities support

package entry point から Utilities 実装を支える補助クラスも公開しています。

- `AppsScriptByte`
- `BinaryData`
- `DateFormatPattern`
- `Md2`
- `PartedDate`
- `StringCode`

これらは主に `@gasboost/fake-node` の `NodeUtilities` から利用されます。

通常の利用では、Node.js 上の Utilities fake として `@gasboost/fake-node` を利用してください。

## 責務

`@gasboost/fake-core` が担当するもの:

- GAS API の in-memory implementation
- Node.js 固有 API に依存しない fake
- Blob
- Cache / CacheService
- Properties / PropertiesService
- Session と実行コンテキスト
- Utilities 実装を支える共通処理

Node.js の crypto / zlib などに依存する `Utilities` の実装は `@gasboost/fake-node` が担当します。

## 制約

Google Apps Script ランタイム全体を再現する package ではありません。

現在実装されている class / method のみをサポート対象とします。

## 関連パッケージ

- `@gasboost/fake-node` — Node.js 上で動作する Utilities implementation

## License

MIT
