# gasboost/fake

Google Apps Script の組み込み API に依存するコードを、Node.js 上のローカル開発や自動テストで扱いやすくするための fake implementation を提供するリポジトリです。

Google Apps Script の `CacheService`、`PropertiesService`、`Session`、`Utilities` などは GAS ランタイム上で提供されるため、そのままでは Node.js 上で実行できません。

gasboost/fake は、用途に応じて in-memory 実装と Node.js 実装を分離して提供します。

## Packages

### @gasboost/fake-core

Node.js 固有 API に依存しない、Google Apps Script API の fake implementation を提供します。

主な公開 API:

- `InMemoryBlob`
- `InMemoryCache`
- `InMemoryCacheService`
- `InMemoryProperties`
- `InMemoryPropertiesService`
- `InMemoryContext`
- `InMemorySession`
- `SecurityPolicy`
- `OAuthScope`

加えて、`@gasboost/fake-node` から利用される Utilities 関連の補助クラスも公開しています。

詳細は [@gasboost/fake-core README](./packages/fake-core/README.md) を参照してください。

### @gasboost/fake-node

Node.js の標準機能や Node.js 向けライブラリを利用して、Google Apps Script の `Utilities` を再現する実装を提供します。

主な公開 API:

- `NodeUtilities`
- `Charset`
- `DigestAlgorithms`
- `MacAlgorithms`
- `RsaAlgorithm`

`@gasboost/fake-node` は `@gasboost/fake-core` に依存します。

詳細は [@gasboost/fake-node README](./packages/fake-node/README.md) を参照してください。

## 用途

- Unit Test
- Integration Test
- GAS API に依存するライブラリのローカル検証
- Google Apps Script と Node.js の両方を対象とする開発

gasboost/fake は Google Apps Script ランタイム全体を再現する emulator ではありません。

実装されている API のみをサポート対象とします。

## License

MIT
