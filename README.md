# アプリの設定
### 1. アプリのひな型作成(全てコンテナ内で完結させるため、結構パワープレイ)
##### 1. 下記コマンドを実行し、アプリのひな型を作成する
`npx create-react-app ./temp`

##### 2. 下記コマンドを実行し、カレントディレクトリにすべて移動させる
`mv temp/node_modules/* ./node_modules/ && rm -rf temp/node_modules/`\
`mv temp/* ./ && rm -rf temp/`\
※正直ここまでやるなら、Dockerfile内でcreate-react-appしたほうがいい気がしなくもないが、いい案が見つからない

### 2. ホットリロードを有効にする
##### 1. package.jsonのscriptsプロパティ内のstartコマンドを下記のように変更する
`"start": "WATCHPACK_POLLING=true react-scripts start",`

##### 2. package.jsonのscriptsプロパティ内のtestコマンドを下記のように変更する
`"test": "WATCHPACK_POLLING=true npx cross-env PORT=3001 react-scripts test",`

##### 3. 下記コマンドでReactアプリケーションを起動すると、ホットリロードが効くようになる
`npm start`

##### 4. 下記コマンドでテストを実行すると、テストコードに対してホットリロードが効くようになる
`npm test`

***

# ESLintとPrettierのインストール
##### 1. ESLintとPrettierのインストール
`npm i -D eslint prettier eslint-config-prettier`

##### 2. ESLintの初期化
```npx eslint --init```

##### 3. Prettierの設定
1. .prettierrcを作成
2. 下記のように記述
    ```
    {
        "arrowParens": "always",
        "bracketSpacing": true,
        "endOfLine": "lf",
        "htmlWhitespaceSensitivity": "css",
        "jsxBracketSameLine": false,
        "jsxSingleQuote": false,
        "printWidth": 120,
        "semi": true,
        "singleQuote": false,
        "tabWidth": 4,
        "useTabs": false
    }
    ```
| 設定名                    | 説明                                                                       |
|---------------------------|----------------------------------------------------------------------------|
| `arrowParens`: "always"   | アロー関数の引数を常に括弧()で囲む                                         |
| `bracketSpacing`: true    | オブジェクトリテラルの中括弧の内側にスペースを入れる。例: { obj: "val" }  |
| `endOfLine`: "lf"         | 改行コードをLFに設定                                                       |
| `htmlWhitespaceSensitivity`: "css" | HTMLの空白の扱いをCSSの表示プロパティに従って決定する                     |
| `jsxBracketSameLine`: false | JSXの複数行要素の場合、最後の > を次の行に置く                             |
| `jsxSingleQuote`: false  | JSX内でシングルクォートではなくダブルクォートを使用する                     |
| `printWidth`: 120        | 1行当たりの最大文字数を120文字に設定                                       |
| `semi`: true             | 文の最後にセミコロンをつける                                               |
| `singleQuote`: false     | 文字列をシングルクォートではなくダブルクォートで囲む                       |
| `tabWidth`: 4            | タブ幅を4スペースに設定                                                    |
| `useTabs`: false         | スペースを使用してインデントする（タブを使用しない）                       |


##### 4. ESLintの設定
1. .eslintrc.jsonを下記のようにする
    ```
    {
        "env": {
            "browser": true,
            "es2021": true
        },
        "extends": ["eslint:recommended", "plugin:react/recommended", "prettier"],
        "parserOptions": {
            "ecmaVersion": "latest",
            "sourceType": "module"
        },
        "plugins": ["react"],
        "rules": {
            "react/react-in-jsx-scope": "off",
            "react/prop-types": "off"
        }
    }
    ```
2. .eslintignoreを作成
3. 下記のように記述
    ```
    build/
    public/
    node_modules/
    **/*.min.js
    *.config.js
    .*lintrc.js
    ```

##### 5. ESLintとPrettierを保存時に自動実行させる設定
1. settings.jsonに下記設定を追記
    ```
    // ファイル保存時、コードを整形する
    "editor.formatOnSave": true,
    // ファイル保存時のアクション
    "editor.codeActionsOnSave": {
        // ESLintのチェックを走らせる
        "source.fixAll.eslint": "explicit"
    },
    // 入力時、コードを自動的に整形をする
    "editor.formatOnType": true,
    // ペースト時、コードを自動的に整形をする
    "editor.formatOnPaste": true,
    // デフォルトのフォーマッターをPrettierにする
    "editor.defaultFormatter": "esbenp.prettier-vscode"
    ```
---

#### デバッグ(任意)
1. ファイル作成
    ```
    mkdir -p .vscode && touch .vscode/launch.json
    ```
2. ファイルの中身を下記のように記述
    ```
    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Chrome",
                "type": "chrome",
                "request": "launch",
                "url": "http://localhost:5173",
                "webRoot": "${workspaceFolder}/frontend"
            }
        ]
    }
    ```