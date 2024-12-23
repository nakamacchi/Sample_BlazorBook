FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
RUN apt-get update && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . /src

RUN dotnet build . -c Release -o /app/build

FROM build AS publish
RUN dotnet publish . -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# 開発環境扱いでビルドしたい場合は以下をコメントアウト
#ENV ASPNETCORE_ENVIRONMENT Development
#ENV DOTNET_ENVIRONMENT Development

# .NET 8 の既定ではポート 8080 で起動するので 80 へ変更
ENV ASPNETCORE_URLS=http://+:80/

CMD ["dotnet", "AzRefArc.AspNetBlazorServer.dll"]

# Docker ビルド
# sudo docker build -t app .
# コンテナ内ポート 80 をローカルポート 8080 へ接続
# sudo docker run -p:8080:80 app
# イメージデバッグ (コンテナ内を参照)
# sudo docker run --rm -i -t -p:8080:80 app /bin/bash

