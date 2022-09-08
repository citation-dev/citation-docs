# Getting started

citationは、メッセージリンクのプレビューをEmbedで表示するDiscord Botです。

前代の [MessageQuote](https://github.com/m2en/MessageQuote) で見つかった問題点を解決するのを目的にプログラミング言語 **[Kotlin](https://kotlinlang.org/)** で再実装しています。

Dockerの環境があればすぐに用意ができるようになっています。さあ、貴方のギルドにcitationを構築しましょう!

## citationを利用するには

citationは以下の方法で利用できます。

1. ghcr.io(GitHub Packages)に用意されているビルド済みイメージを利用する (推奨)
   - アップデートのたびにイメージのPullが必要ないDocker Composeで使用することをcitationでは推奨しています。
2. 自分でビルドする


!!! warning "citationを自分でビルドする際の注意点"

    citationは以下の環境を要求します。
    Dockerで構築する場合はそもそものイメージ内に含まれているので考える必要がありませんが、自分でビルドを行う際は確認・用意が必要です。

    - Java 17

    なお、Gradleについてはデーモン(Wrapper)が付属しています。

----

=== "ghcr.io (Docker Compose)"

    ここではghcr.io(GitHub Packages)にプッシュされているビルド済みイメージを **Docker Compose** で利用する方法を紹介します。

    **1. Dockerの環境を構築する**

    [Orientation and setup - Docker Documentation](https://docs.docker.com/get-started/) を参照しながらご自身のPC、またはサーバーにDockerを構築してください。

    **2. Docker Composeをインストールする**

    [Install Docker Compose - Docker Documentation](https://docs.docker.com/compose/install/) を参照しながらご自身のPC、またはサーバーにDocker Composeをインストールしてください。

    !!! note

        `1.` で準備したものはDocker Engineです。Docker Composeは別のものです。

    **3. 環境変数を作成する**

    適当なディレクトリを作成し `.env` ファイルを作成し、 [GitHubの `.env.example` ファイル](https://github.com/m2en/citation/blob/main/.env.example) を参考に環境変数を設定してください。

    ```.env
    # citationが接続するクライアントユーザーのトークン (Required)
    CITATION_BOT_TOKEN=

    # citationのコマンドを登録するギルドのID (Optional)
    GUILD_ID=
    ```

    **4. docker-compose.ymlを作成する**

    `.env` を作ったディレクトリに `docker-compose.yml` を作成し、以下の内容を記述してください。

    ```yaml
    version: "3.9"

    services:
        citation:
            image: ghcr.io/m2en/citation:latest
            restart: always
            env_file:
              - .env
    ```

    !!! note "services.citation.image の値"

        `services.citation.image` で `latest` と指定している場所はバージョンを指定することができます。(`latest` で常に最新版を使用します。)

        citationは重大な脆弱性があったりなど、一部の条件下を除いて基本的にすべてのバージョンを使用することができます。(それでも推奨しているのは最新版です。)

        利用可能なバージョンは GitHub の [Packages](https://github.com/users/m2en/packages/container/package/citation) ページで確認できます。

    **5. Composeでcitationを実行する**

    すべて記載できたら、以下のコマンドを実行してください。

    ```bash
    docker-compose up -d
    ```

    イメージのダウンロードが始まり、しばらくするとcitationが起動します。

=== "ghcr.io (Docker Image)"

    ここではghcr.io(GitHub Packages)にプッシュされているビルド済みイメージを **Docker Image** で利用する方法を説明します。

    **1. Dockerの環境を構築する**

    [Orientation and setup - Docker Documentation](https://docs.docker.com/get-started/) を参照しながらご自身のPC、またはサーバーにDockerを構築してください。

    **2. イメージを取得する**

    以下のコマンドを実行し、イメージを取得してください。

    ```bash
    docker pull ghcr.io/m2en/citation:latest
    ```

    **3. 環境変数を作成する**

    適当なディレクトリを作成し `.env` ファイルを作成し、 [GitHubの `.env.example` ファイル](https://github.com/m2en/citation/blob/main/.env.example) を参考に環境変数を設定してください。

    ```.env
    # citationが接続するクライアントユーザーのトークン (Required)
    CITATION_BOT_TOKEN=

    # citationのコマンドを登録するギルドのID (Optional)
    GUILD_ID=
    ```

    **4. コンテナを起動する**

    以下のコマンドを実行し、citationのイメージIDを確認してください。

    ```bash
    docker images
    ```

    ![docker imagesを実行した画面](./images/005030.png)

    この場合のイメージIDは `87f14cd65aa7` です。

    `.env` を作成したディレクトリに移動し次のコマンドを実行します。

    ```bash
    docker run --rm --env-file .env -t 87f14cd65aa7
    ```

    ハッシュ値が表示されたら成功です。お疲れ様でした。

=== "自身でビルド"

    次のコマンドを実行し、citationをビルドしてください。

    ```bash
    # ssh
    git clone git@github.com:m2en/citation.git

    # https
    git clone https://github.com/m2en/citation.git

    cd citation

    ./gradlew shadowJar
    ```

    適当なディレクトリを作成し `.env` ファイルを作成し、 [GitHubの `.env.example` ファイル](https://github.com/m2en/citation/blob/main/.env.example) を参考に環境変数を設定してください。

    ```.env
    # citationが接続するクライアントユーザーのトークン (Required)
    CITATION_BOT_TOKEN=

    # citationのコマンドを登録するギルドのID (Optional)
    GUILD_ID=
    ```

    `.env` をcitationのルートディレクトリ上に設置し、次のコマンドを実行します。

    ```bash
    java -jar build/libs/citation.jar
    ```

    `citation ready!` と表示されたら成功です。お疲れ様でした。

----

## アップデートする

citationのパッチが配信されると [Releases](https://github.com/m2en/citation/releases) ページが更新されます。

以下の手順に従って更新を行ってください。

=== "ghcr.io (Docker Compose)"

    **1. `docker-compose.yml` を開き、イメージのバージョンを更新します。**

    ```yaml
    version: "3.9"

    services:
      citation:
        image: ghcr.io/m2en/citation:latest
        container_name: citation
        restart: always
        env_file: .env
    ```

    !!! note

        `services.citation.image` の値を `latest` にしている場合は変更の必要はありません。

    **2. `docker-compose.yml` を保存し、次のコマンドでイメージの削除及び、コンテナの殺害を行います。**

    ```bash
    docker-compose down
    ```

    **3. 再起動を行います。**

    ```bash
    docker-compose up -d
    ```

=== "ghcr.io (Docker Image)"

    **1. アクティブのコンテナを停止します。**

    ```bash
    docker container rm <CONTAINER_ID>
    ```

    **2. イメージを削除します。**

    ```bash
    docker image rm -f <IMAGE_ID>
    ```

    **3. 新しいイメージをPullします。**

    ```bash
    docker pull ghcr.io/m2en/citation:latest
    ```

    **4. 再起動します**

    ```bash
    docker run --rm --env-file .env -d <IMAGE_ID>
    ```

=== "自身でビルドしている場合"

    **1. 最新の状態をPullします。**

    ```bash
    git pull
    ```

    **2. ビルドします。**

    ```bash
    ./gradlew shadowJar
    ```

    **3. 再起動します。**

    ```bash
    java -jar build/libs/citation.jar
    ```

----

## イメージを削除する (Docker)

イメージを削除するには次のコマンドを実行します。

```bash
docker image rm -f <IMAGE_ID>
```

!!! warning "イメージを削除できないとき"

    イメージを削除するには予め、そのイメージが使用しているコンテナがないかどうか確認してください。

    ```bash
    docker container ls
    ```

    アクティブな状態のイメージが有る場合はForce Option `-f` を使っても削除できません。

!!! tip "コンテナなどを一括で削除する"

    次のコマンドで、コンテナなどの一括削除ができます。

    ```bash
    docker system prune
    ```

    このコマンドでは以下のものが削除されます。

    - 停止しているすべてのコンテナ
    - コンテナで使われていないすべてのネットワーク
    - すべてのイメージ
    - すべてのビルドキャッシュ

    なお、このコマンドはすべてのイメージを削除します。

    Docker Imageを自分でビルドしている場合はJava 17(JDK)のイメージも削除されるため、次回ビルド時にイメージの再インストールが行われます。

----

## コマンドを登録する

citationの引用機能は起動するだけで利用可能ですが、ヘルプコマンドなどの拡張機能を使う場合はコマンド作成が必要です。

`.env` にて `GUILD_ID` をパスとしてギルドIDを登録してください。

登録したら、 Botを起動して登録したギルドで `!register` を実行します。

!!! info "!register を実行できるメンバー"

    `!register` を実行できるメンバーは以下の権限を取得しておく必要があります。

    - `ManageServer` (サーバーの管理権限)
    
    または

    - `Administrator` (管理権限)

    サーバー所有者であっても、 上記権限を持っていないと実行できません。

## citationに必要な権限

citationが接続するクライアントユーザーに必要な権限は以下の通りです。

- `Send_Messages` (メッセージの送信権限)
- `Send_Messages_in_Threads` (スレッドでのメッセージ送信権限)
- `Embeds_Links` (リンクの埋め込み権限)
- `Add_Reactions` (リアクション付与権限)

!!! warning "Administator権限は付与しないでください"

    `Administrator` 権限は非常に強い権限です。

    サーバー所有者とほぼ同等の権限を持つため、万が一トークンが漏洩した場合、不正利用される可能性があり危険です。

    (サーバーのメンバーを全員BANし、再起不能にする荒らし 通称 `Server-Nuke` に悪用される場合があります)
