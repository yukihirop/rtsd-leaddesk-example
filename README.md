# leaddeskのAPIドキュメントの分解例

## 確認環境

- rails(Rails 4.2.5.1)
- ruby(ruby 2.3.3p222 (2016-11-21 revision 56859) [x86_64-darwin18])

## 準備

SwaggerEditor(UI)で開いたりするので以下の準備をしてください。

```bash
$ brew cask install chromedriver
$ docker pull swaggerapi/swagger-ui:latest
$ docker pull swaggerapi/swagger-editor:latest
```

## 試し方

`routes_to_swagger_docs` の設定に関しては、 `config/environments/development.rb` をご覧ください。
(leaddeskの場合は必須な設定はありません。)

OpenAPI(V3)形式に変換したAPIドキュメントが `leaddesk.json` として用意してあるのでそれを使います。

### 分析・分解

```bash
$ SWAGGER_FILE=./leaddesk.json bundle exec rake routes:swagger:analyze
```

### SwaggerUIで表示

```bash
$ # 全体を表示する
$ bundle exec routes:swagger:ui
$ # 特定のpathsファイル(単体)だけ表示する
$ PATHS_FILE=swagger_docs/src/paths/users.yml bundle exec routes:swagger:ui
$ # 特定のpathsファイル(複数)だけ表示する
$ echo 'users.yml' >> swagger_docs/.paths
$ echo 'activities.yml' >> swagger_docs/.paths
$ echo 'calls.yml' >> swagger_docs/.paths
$ bundle exec routes:swagger:ui
```

### SwaggerEditorで編集

```bash
$ # 全体を編集する
$ bundle exec routes:swagger:editor
$ # 特定のpathsファイル(単数)だけを編集する
$ PATHS_FILE=swagger_docs/src/paths/users.yml bundle exec routes:swagger:editor
$ # 特定のpathsファイル(複数)だけ編集する
$ echo 'users.yml' >> swagger_docs/.paths
$ echo 'activities.yml' >> swagger_docs/.paths
$ echo 'calls.yml' >> swagger_docs/.paths
$ bundle exec routes:swagger:editor
```

### テキストエディタで編集する場合

git管理しない `swagger_docs/swagger_doc.yml` を `monitor` コマンドで管理する事で差分を検知します。

vscodeを使っている場合は、[SwaggerViewer](https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer)プラグインが便利

```bash
$ # 全体を編集する
$ bundle exec routes:swagger:monitor   # swagger_docs/swagger_doc.ymlファイルを編集する。
$ # 特定のpathsファイル(単数)だけを編集する
$ PATHS_FILE=swagger_docs/src/paths/users.yml bundle exec routes:swagger:monitor
$ # 特定のpathsファイル(複数)だけ編集する
$ echo 'users.yml' >> swagger_docs/.paths
$ echo 'activities.yml' >> swagger_docs/.paths
$ echo 'calls.yml' >> swagger_docs/.paths
$ bundle exec routes:swagger:monitor
```

### 書いたドキュメントを配布する

必要な分だけ配布用のドキュメントを生成する事が出来ます。

```bash
$ # 全体を配布する
$ bundle exec routes:swagger:dist
$ # 特定のpathsファイル(単数)だけを配布する
$ PATHS_FILE=swagger_docs/src/paths/users.yml bundle exec routes:swagger:dist
$ # 特定のpathsファイル(複数)だけ配布する
$ echo 'users.yml' >> swagger_docs/.paths
$ echo 'activities.yml' >> swagger_docs/.paths
$ echo 'calls.yml' >> swagger_docs/.paths
$ bundle exec routes:swagger:dist
```

### pathsファイル一覧を取得する

```bash
$ bundle exec routes:swagger:paths_ls
```