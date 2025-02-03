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
- 代わりに以下のような記述を行うことで、プリレンダリングを無効化した各レンダリングモードを利用することができます。
  - @rendermode @(new InteractiveServerRenderMode(prerender: false));
  - @rendermode @(new InteractiveWebAssemblyRenderMode(prerender: false));
  - @rendermode @(new InteractiveAutoRenderMode(prerender: false));
- が、かなり面倒な記述になるため、プリレンダリングを無効にしたレンダリングモードを利用したい場合には、ご自身のソリューションファイルに p.295 リスト 11.8 に記載されているモード指定クラスを追加していただき、これを利用するようにしてください。

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

## 正誤表（2024/08/16 追記）

さらに読者の方から以下の指摘をいただきました。ご指摘いただきました読者の方にこの場を借りてお礼申し上げます。ありがとうございました。

### p.53 OnInitializedAsync() 処理について

リスト 3.8 に示されているコードサンプルですが、このページを開く際に、

- まずホームページ（例えば https://localhost:7224/ など）を開いたのち、リンクによって遷移してきた場合には問題なく動作しますが、
- 直接このページ（例えば https://localhost:7224/Counter など）を開くと、以下の例外が発生し、正しく動作しません。

```C#
InvalidOperationException: JavaScript interop calls cannot be issued at this time. This is because the component is being statically rendered. When prerendering is enabled, JavaScript interop calls can only be performed during the OnAfterRenderAsync lifecycle method.
```

これは以下の理由によるものです。

- .NET 8 以降の Blazor には、後半の第11章で解説しているプリレンダリング機能が備わっており、既定で有効化されています。
- この機能が有効化されている場合、OnInitializedAsync() 処理では JavaScript 関連の処理を記述することができません。（詳細 → https://learn.microsoft.com/ja-jp/aspnet/core/blazor/components/lifecycle?view=aspnetcore-8.0）

解決方法としては 2 通りあります。

- ① プリレンダリング機能を無効化した上で、OnInitializedAsync() 処理に記述する。（11.7.1 節にて解説）
- ② プリレンダリング機能はそのままで、OnAfterRenderAsync(bool firstRender) 処理に記述する。

ただし②の方法を採る場合、レンダリング処理がすでに完了しているため、このメソッドの中でデータ変数を更新しても UI 側に反映されません（再バインドされない）。このため、例えば リスト3.8 のケースで②の方法を使う場合には、以下のように、メソッドの最後に this.StateHasChanged(); 命令を入れる必要があります。

```C#
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        var result = await storage.GetAsync<int>("CurrentCount");
        currentCount = result.Success ? result.Value : 0;
        this.StateHasChanged();
    }
```

状況に応じて最適な方法を選択していただくことになりますが、第 11 章で解説しているように、プリレンダリング機能がそもそも不要なケースも多いと思います。プリレンダリング機能が不要な場合には、①の方法の方が素直な解決策になりますので、そちらをオススメします。

### p.48, p.203 パラメータのチェックに関して

p.48 及び p.203 の解説では、引き渡されたパラメータのチェックに OnParametersSet() メソッドを使うように書かれていますが、これは誤りで、OnInitializedAsync()メソッドの先頭で引き渡されたパラメータのチェックを行う必要があります。以下に詳細を解説します。

p.203 の解説では「OnParametersSet()メソッドの処理後、OnInitializedAsync()メソッドが呼び出されます」とありますがこれは誤りで、正しくは「OnInitializedAsync()メソッドの処理後、OnParametersSet()メソッドが呼び出されます」になります。このため、以下のように記述すると、不正なパラメータを使ったクエリが動作してしまうことになり、問題があります。

```C#
    [Parameter]
    public required string AuthorId { get; init; }

    // このメソッドは OnInitializedAsync() の後に動作する
    protected override void OnParametersSet()
    {
        // セキュリティ対策
        if (String.IsNullOrEmpty(AuthorId)) throw new ArgumentNullException("AuthorId");
        if (System.Text.RegularExpressions.Regex.IsMatch(AuthorId, @"^\d{3}-\d{2}-\d{4}$") == false) throw new ArgumentException("AuthorId");
    }

    protected override async Task OnInitializedAsync()
    {
        // 不正な URL パラメータが与えられた場合でも、この処理は動いてしまう
        await using (var pubs = await DbFactory.CreateDbContextAsync())
        {
            Titles = (await pubs.Titles.Where(t => t.TitleAuthors
                .Where(ta => ta.AuthorId == this.AuthorId).Count() > 0) // Count() > 0 は Any() にできない？
                .Include(t => t.Publisher)
                .ToListAsync()).AsQueryable();

            AuthorToDisplay = await pubs.Authors
                .Include(a => a.TitleAuthors)
                .FirstOrDefaultAsync(a => a.AuthorId == this.AuthorId);
        }

        Message = AuthorToDisplay is null ?
            null :
            $"{AuthorToDisplay.AuthorFirstName} {AuthorToDisplay.AuthorLastName} さんの情報は以下の通りです。";
    }
```

こちらに関しての解決方法は 2 通りあります。

① OnInitializedAsync() 内で、パラメータを使った処理を行う前にチェックを行う。
② SetParametersAsync(ParameterView parameters) メソッドでパラメータチェックを行う。

①の方法の場合には、以下のような記述になります。シンプルでわかりやすいので、基本的にはこの方法がオススメです。

```C#
    [Parameter]
    public required string AuthorId { get; init; }

    protected override async Task OnInitializedAsync()
    {
        // セキュリティ対策
        if (String.IsNullOrEmpty(AuthorId)) throw new ArgumentNullException("AuthorId");
        if (System.Text.RegularExpressions.Regex.IsMatch(AuthorId, @"^\d{3}-\d{2}-\d{4}$") == false) throw new ArgumentException("AuthorId");

        await using (var pubs = await DbFactory.CreateDbContextAsync())
        {
            Titles = (await pubs.Titles.Where(t => t.TitleAuthors
                .Where(ta => ta.AuthorId == this.AuthorId).Count() > 0) // Count() > 0 は Any() にできない？
                .Include(t => t.Publisher)
                .ToListAsync()).AsQueryable();

            AuthorToDisplay = await pubs.Authors
                .Include(a => a.TitleAuthors)
                .FirstOrDefaultAsync(a => a.AuthorId == this.AuthorId);
        }

        Message = AuthorToDisplay is null ?
            null :
            $"{AuthorToDisplay.AuthorFirstName} {AuthorToDisplay.AuthorLastName} さんの情報は以下の通りです。";
    }
```

①の方法の難点は、実際にこの値が使われないとはいえ、不正な値が AuthorId パラメータに一時的とはいえ設定されてしまうことです。もし、そもそも AuthorId パラメータに不正な値がセットされること自体を防ぎたいのであれば、OnParametersSet() メソッドではなく、SetParametersAsync() メソッドを利用してください。この方法は、実際に [Parameter] 属性つき変数に値がセットされる前に処理を挿し込むものです。具体的な記述例としては以下になります。

```C#
    [Parameter]
    public required string AuthorId { get; init; }

    public override async Task SetParametersAsync(ParameterView parameters)
    {
        if (parameters.TryGetValue<string>(nameof(AuthorId), out var value))
        {
            // セキュリティ対策
            if (String.IsNullOrEmpty(value)) throw new ArgumentNullException("AuthorId : " + value);
            if (System.Text.RegularExpressions.Regex.IsMatch(value, @"^\d{3}-\d{2}-\d{4}$") == false) throw new ArgumentException("AuthorId : " + value);
        }
        await base.SetParametersAsync(parameters);
    }
```

理想論的には②が望ましいですが、コードがやや複雑になりますので、①の実装で十分なことが多いと思います。ケースバイケースで使い分けてください。

なお、それなら OnParametersSet() メソッドは何のためにあるのか？ という話になりますが、こちらはパラメータ値に依存するデータや UI 表示項目を更新する目的で利用します。具体例については以下のドキュメントを見てください。

- https://learn.microsoft.com/ja-jp/aspnet/core/blazor/components/lifecycle?view=aspnetcore-8.0#after-parameters-are-set-onparameterssetasync

## その他

本書ではクラウドサービスに関しては深く記載しておりませんが、以下のサイトでマイクロソフトのメンバーが整理した様々な技術資料をご参照いただけます。アプリケーションをクラウド上で運用する際に参考にしてください。

- [Azure 技術資料インデックス (jp-techdocs)](https://github.com/Azure/jp-techdocs)
