# C#ユーザーのためのWebアプリ開発パターン ASP.NET Core Blazorによるエンタープライズアプリ開発

本ページでは、インプレス様から出版している「[C#ユーザーのためのWebアプリ開発パターン ASP.NET Core Blazorによるエンタープライズアプリ開発](https://book.impress.co.jp/books/1122101173)」のサンプルコードを掲載しております。

※ 本ページ末尾に、本書の正誤表を掲載しております。併せてご確認ください。

## サンプルコードについて

第7～8章で作成するサンプルコードをこちらのサイトで共有しています。以下からご参照ください。

- 完成したソースコード ([BlazorApp1 フォルダ下](BlazorApp1/))
- サンプルデータベース作成スクリプト ([pubs_azure_timestampつき.txt](pubs_azure_timestampつき.txt))
- Entity Framework Core 用の O/R マップファイル ([Pubs.cs](Pubs.cs))

なお、実行したい場合には下記のサンプルデータベースを作成することに加え、データベースに接続するための文字列を設定として追加する必要があります。（接続文字列の管理方法については本書 第 5 章 5.4 節をご参照ください。）

```接続文字列
{
  "ConnectionStrings": {
    "PubsEntities": "Server=tcp:xxxxxx.database.windows.net,1433;Initial Catalog=pubs;Persist Security Info=False;User ID=azrefadmin;Password=XXXXX;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
  }
}
```

## サンプルデータベースの作成方法

テスト用データベース pubs は、付録 01 に掲載した方法の他に、いくつかの作成方法があります。以下のサイトでご紹介しておりますので、参考にしてください。

- [テスト用 pubs データベースの作成方法について](https://github.com/nakamacchi/AzRefArc.SqlDb)

## その他の ASP.NET Core Blazor のサンプルコードについて

書籍で解説している 3 つの開発方式に沿ったサンプルを以下のサイトで示しております。こちらも併せてご参照ください。

| 開発するアプリのタイプ | 開発スタイル | プロジェクトテンプレート | プロジェクトオプション | 利用するレンダリングモード | サンプルの置き場所 |
| --- | --- | --- | --- | --- | --- |
| イントラネット業務アプリ | Blazor Server 型 (従来型) | Blazor Web App | Interactivity Type = Server, Intaractivity Location = Global | InteractiveServer (prerender: false) | [Source](https://github.com/nakamacchi/AzRefArc.AspNetBlazorServer) [Web](https://azrefarc-aspnetblazorserver.azurewebsites.net/) |
| 随時切断型 Web アプリ | Blazor WASM 型 (従来型) | Blazor Web Assembly アプリ | (指定なし) | InteractiveWebAssembly (prerender: false) | [Source](https://github.com/nakamacchi/AzRefArc.AspNetBlazorWasm) [Web](https://azrefarc-aspnetblazorwasm.azurewebsites.net/) |
| インターネット B2C アプリ | Blazor United 型 (.NET 8) | Blazor Web App | Interactivity Type = Server and WebAssembly, Intaractivity Location = per page/component | Static SSR, InteractiveServer, WASM, Auto | [Source](https://github.com/nakamacchi/AzRefArc.AspNetBlazorUnited) [Web](https://azrefarc-aspnetblazorunited.azurewebsites.net/) |

## 正誤表

本書の一部に謝りがありました。大変申し訳ありませんが、以下の通り訂正させてください。（ご指摘いただきました [草場 友光](https://x.com/tomo_kusaba) 様、大変ありがとうございました。この場を借りてお礼申し上げます。m(_ _)m）

### p.346 A2.6 様々な本番環境への配置方法

- In-Process 型ホスティングを利用するケースとして、「IIS の Windows 統合認証機能を利用したい」というケースを挙げていますが、現在では Out-Process 型ホスティングでも IIS から認証情報を引き継ぐことができるようになりました。([参考](https://learn.microsoft.com/ja-jp/aspnet/core/host-and-deploy/iis/out-of-process-hosting?view=aspnetcore-8.0#application-configuration))
- Out-Process 型ホスティングを利用したい場合には、プロジェクトファイルに <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel> を追加する必要があります。（[参考](https://learn.microsoft.com/ja-jp/aspnet/core/host-and-deploy/iis/out-of-process-hosting?view=aspnetcore-8.0#out-of-process-hosting-model)）

### p.131 表6.1 主なアノテーション

- "複合キーの場合は複数のカラムに [Key] を付与" としていますが、この方法では複合キーの設定はできません。
- アノテーションで指定する場合には、POCO クラスの属性として、[PrimaryKey(nameof(キー1), nameof(キー2), nameof(キー3), ...)] と指定する必要があります。

## 正誤表（2024/06/27 追記）

さらに読者の方から以下の指摘をいただきました。.NET 8 製品版リリース時の修正を取り込み切れていなかったものが多く、大変失礼しました。ご指摘いただきました読者の方にこの場を借りてお礼申し上げます。ありがとうございました。

### p.280～p.295 属性によるレンダリングモードの指定

- .NET 8 RC 時点では、属性を用いたレンダリングモードの指定として以下が利用できましたが、.NET 8 製品版ではこれらの指定ができなくなりました。
  - @attribute [RenderModeInteractiveServer(true/false)]
  - @attribute [RenderModeInteractiveWebAssembly(true/false)]
  - @attribute [RenderModeInteractiveAuto(true/false)]
- 一方、図11.7 に記載している通り、@rendermode を用いた指定方法の場合、既定では、サーバ側プリレンダリングを無効化するレンダリングモードの選択肢が用意されていません。このため、プリレンダリングを無効にしたレンダリングモードを利用したい場合には、ご自身のソリューションファイルに p.295 リスト 11.8 に記載されているモード指定クラスを追加していただき、これを利用するようにしてください。

### p.89 表5.1 4 行目 HttpClient について

- 表に誤りがありました。4 行目に関しては以下の通りです。
  - サービス : HttpClient, IHttpClientFactory
  - 内容 : HTTP 呼び出しを行うサービス
  - 既定で利用できるか？ : ×（要自力登録）
  - 備考 : IHttpClientFactory を利用したほうがよい

### .NET 8 でのファイルレイアウト変更が反映されていなかった点、および誤植箇所

- p.49 \<Router\> オブジェクトは App.razor ファイルではなく Routes.razor ファイル内にあります。
  - なお Blazor WASM 型プロジェクトの場合には、\<Router\> オブジェクトは App.razor ファイル内にあります。（＝Blazor Server 型プロジェクトと Blazor WASM 型プロジェクトとで若干ファイルレイアウトが異なります。）
- p.83 図5.2 の Routers.razor は Routes.razor の誤植です。
- p.171 Index.razor ではなく Home.razor が正しいです。

## その他

本書ではクラウドサービスに関しては深く記載しておりませんが、以下のサイトでマイクロソフトのメンバーが整理した様々な技術資料をご参照いただけます。アプリケーションをクラウド上で運用する際に参考にしてください。

- [Azure 技術資料インデックス (jp-techdocs)](https://github.com/Azure/jp-techdocs)
