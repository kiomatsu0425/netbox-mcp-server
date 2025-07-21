# NetBox MCP サーバー

これは NetBox 用のシンプルな読み取り専用 [Model Context Protocol](https://modelcontextprotocol.io/) サーバーです。MCP をサポートする LLM を通じて NetBox のデータに直接アクセスできます。

## ツール

| ツール | 説明 |
|------|------|
| get_objects | 種類やフィルターに基づいて NetBox のコアオブジェクトを取得します |
| get_object_by_id | 特定の ID を持つ NetBox オブジェクトの詳細情報を取得します |
| get_changelogs | フィルターに基づいて変更履歴 (監査トレイル) を取得します |

> 注: 現時点ではサポートされるオブジェクトタイプは明示的に定義されており NetBox のコアオブジェクトに限定されています。プラグインのオブジェクトタイプには対応していません。

## 使い方

1. NetBox で読み取り専用 API トークンを作成し、MCP 経由で公開したいデータへアクセスできる権限を与えます。
2. 依存関係をインストールします: `uv add -r requirements.txt`
3. サーバーが実行できることを確認します: `NETBOX_URL=https://netbox.example.com/ NETBOX_TOKEN=<your-api-token> uv run server.py`
3. MCP サーバーの設定を LLM クライアントに追加します。例として Claude Desktop (Mac) の場合:

```json
{
  "mcpServers": {
        "netbox": {
            "command": "uv",
            "args": [
                "--directory",
                "/path/to/netbox-mcp-server",
                "run",
                "server.py"
            ],
            "env": {
                "NETBOX_URL": "https://netbox.example.com/",
                "NETBOX_TOKEN": "<your-api-token>"
            }
        }
}
```
> Windows では `C:\\Users\\myuser\\.local\\bin\\uv` や `C:\\Users\\myuser\\netbox-mcp-server` のように完全なエスケープされたパスを使用してください。
> 詳しいトラブルシューティングについては [MCP クイックスタート](https://modelcontextprotocol.io/quickstart/user) を参照してください。

4. LLM クライアントでツールを利用します。例:

```text
> 'Equinix DC14' サイトにあるすべてのデバイスを取得
...
> IPAM の利用状況を教えて
...
> ネットワークにある Cisco デバイスは?
...
> 先週 NYC サイトを変更したのは誰?
...
> 過去 1 か月間のコアルーターへの構成変更をすべて表示
```

## 開発

貢献は歓迎します! Issue を立てるか PR を送ってください。

## ライセンス

このプロジェクトは Apache 2.0 ライセンスの下で提供されています。詳細は LICENSE ファイルを参照してください。
