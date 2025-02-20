# NuxtJS

## プロジェクト作成

```
npx nuxt init  プロジェクト名
```

```
docker compose up -d --build
```

```
nuxt-tutorial/
├── .nuxt/            --自動生成されたNuxtアプリケーションが格納されたフォルダ
├── node_module/      -- パッケージが格納されたフォルダ
├── public/           -- Webに公開するファイル群を格納するフォルダ
├──  server/          -- エンドポイントAPIサーバ処理を行うファイル群を格納するフォルダ
├── app.vue           -- メインの単一コンポーネントファイル
├── nuxt.config.js    -- Nuxtアプリケーションの設定ファイル
├── package-lock.json -- npmの依存関係に関する設定ファイル
├── package.json      -- npmに関する設定ファイル
├── tsconfig.json     -- TypeScriptに関する設定ファイル
└── README.md
```