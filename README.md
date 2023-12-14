## 目的
next.jsを使用してCMSプラットフォームサービスを構築する技術選定における検証である。

MUIは制御コンポーネント、RHF(React Hook Form)は非制御コンポーネントである為、制御／非制御の実装による再レンダリングを検証する。

## app概要
create-next-appで構築されたNext.jsとfirabaseのfirestoreとauthenticationを利用したCMSプロジェクトです。

* フォームバリデーションはzodを使用する

[ベースappはコチラ](https://github.com/k-gitest/next-ts-fire-auth-store-tailwind-withMUI)

## 開発環境

* next 13.4.2
* typescript 5.0.4
* firebase 9.22.0
* firebase-admin 11.9.0
* tailwind 3.3.2
* mui/material 5.13.5
* swr 2.1.5
* zod 3.21.4

## ディレクトリ構成

<pre>
myapp...プロジェクトディレクトリ
  ├── components ...呼び出し用コンポーネントファイル
  │     ├── Private ...ログインユーザー向けコンポーネント
  │     ├── Public ...非ログインユーザー向けコンポーネント
  │     ├── layout ...メインレイアウト
  │     └── provider ...ユーザー認証チェック
  ├── lib ...firebaseなど外部設定ファイル
  ├── pages ...初期生成されるメインファイル
  │     ├── [uid] ...一般向け画面
  │     │     └── [pid] ... 投稿表示画面
  │     ├── api ...サーバー側処理
  │     │     └── admin ... adminSDK使用ファイル
  │     ├── login ...ログイン画面
  │     ├── signup ...登録画面
  │     └── user ...会員向け画面
  ├── public ...画像ファイル
  ├── styles ...css設定ファイル
  └── types ...型定義ファイル
</pre>

* components/AuthFormではMUIのTextFieldコンポーネントを使用してRHFのregisterと併用してフォームを実装している
* components/Private/PostFormとUserFormではMUIのフォームコンポーネント各種を使用しRHFのControllerを併用してフォームを実装している

## 注意点

RHFはregisterを使用すると非制御であるが、MUIなどの外部UIコンポーネントを使用する場合はControlerを使用し制御コンポーネントとして扱う。

しかしMUIのTextFieldコンポーネントはregisterが使用でき非制御コンポーネントとして扱う事ができる。

## 結論

RHFのregisterを使用すれば再レンダリングが抑えられ表示パフォーマンスが抑えられる。MUIであってもTextFieldコンポーネントのみのフォームであれば非制御として扱う事ができパフォーマンスも期待できるので有用である。

その一方でTextFieldコンポーネント以外だとControllerを使用しないと連携できず再レンダリングもしてしまう。

結論としては無理に制御／非制御を混在させるよりも、使用するフォームパーツによって統一した方が使いやすい。
