---
title: "Selenium IDEで「はじめよう自動化」ver.2019"
emoji: "📹"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["selenium", "test", "automation", "adventcalendar", "beginner"]
published: true
published_at: 2019-12-23 09:00
---

この記事は [Selenium/Appium Advent Calendar 2019](https://qiita.com/advent-calendar/2019/selenium_and_appium) の23日目の記事です。

また、この記事は [Selenium/Appium Advent Calendar 2018の14日目の記事](https://zenn.dev/takeyaqa/articles/selenium-ide-tutorial) を2019年版としてアップデートしたものです。

この記事ではツールを使ったブラウザの自動化をやってみたい！　という自動化ビギナーの皆さんに向けて、Selenium IDEを使った自動テストの作り方を（ほぼ）ステップバイステップで解説します。

<!--more-->

## Selenium IDE

Selenium IDEはブラウザの拡張機能として提供されているキャプチャ&リプレイツールです。
ブラウザでの操作を記録（**キャプチャ**）して同じ操作を再生（**リプレイ**）する機能を持っています。

インストールは以下のリンクから。この記事ではChrome版を使用しますが、Firefox版もたぶんいっしょです。

* [Chrome拡張機能](https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd)
* [Firefox拡張機能](https://addons.mozilla.org/en-GB/firefox/addon/selenium-ide/)

インストールするとブラウザのツールバーにアイコンが追加されます。起動するにはこのアイコンをクリックしてください。

![selenium-ide-icon](/images/2019/12/23/01_selenium-ide-icon.png)

## テスト対象サイト

今回はテストの対象として日本Seleniumユーザコミュニティのテスト用サイトを使います。ホテルの予約フォームを模したデモサイトです。

http://example.selenium.jp/reserveApp_Renewal/

## テストケース

自動テストでもまずはテストケースです。今回は入力フォームでの選択と宿泊金額の組み合わせをテストすることにします。（デフォルト値のままの項目は割愛しています）

| 手順                               |       入力値/選択値 |    期待結果   |
|:----------------------------------|:------------|:-------------|
| 「宿泊日」の入力欄に日付を入力する        | 2019/12/31 |               |
| 「宿泊日」の宿泊日数を選択する           | 2          |               |
| 「朝食バイキング」のラジオボタンを選択する   | なし        |               |
| 「お名前」の入力欄に名前を入力する        | 山田一郎 |                   |
| 「利用規約に同意して次へ」ボタンをクリックする |             | 合計金額が「14000」となること              |

## 操作を記録する

実際に操作を記録していきましょう。アイコンをクリックしてSelenium IDEを起動してください。

ウェルカムページが表示されます。「Record a new test in a new project」を選択します。

![selenium-ide-01](/images/2019/12/23/02_selenium-ide.png)

つづいて、このテストプロジェクトの名前を入力します。

![selenium-ide-02](/images/2019/12/23/03_selenium-ide.png)

次に「base URL」を入力します。これはこのテストプロジェクトのスタート地点となるURLです。ここでは「http://example.selenium.jp/reserveApp_Renewal/ 」を入力します。

![selenium-ide-03](/images/2019/12/23/04_selenium-ide.png)

「Start Recording」をクリックすると新しいブラウザのウィンドウが立ち上がり「Selenium IDE is recoding...」と表示されます。今このウィンドウは操作が記録されている状態です。ではテストケースの通りに操作をしていきましょう。

1. 宿泊日に「2019/12/31」を入力する
2. 宿泊日数から「2」を選択する
3. 朝食バイキング「なし」をクリックする
4. お名前に「山田一郎」を入力する
5. 「利用規約に同意して次へ」ボタンをクリックする

ボタンをクリックすると画面遷移して合計金額が表示されると思います。ここで期待結果の確認のためにassertを追加します。Selenium IDEの`assertText`は画面上の表示テキストと期待値のテキストが一致しているかどうかを検証することができます。

表示されている金額をドラッグして選択し、右クリックメニューを開きます。「Selenium IDE」から「Assert Text」を選択します。これで今表示されている値を期待値としたassertが作成されます。期待値を変えたい場合は後から画面で変更することができます。

期待値の記録が終わったらブラウザのウィンドウを閉じて、Selenium IDEの停止ボタン（赤くて点滅してるやつ）をクリックして記録を止めます。

<!-- ![selenium-ide-04](/images/2019/12/23/05_selenium-ide.gif) -->

テストケースの名称を入力されるように促されるので、任意の名前を入力します。テストの内容やパターンがわかりやすい名前にするのがいいでしょう。これで記録は完了です。

![selenium-ide-05](/images/2019/12/23/06_selenium-ide.png)


## テストの実行

![selenium-ide-06](/images/2019/12/23/07_selenium-ide.png)


出来上がりはおおむねこんな感じになると思います。余分なクリックなどが記録されている場合はその行を選択してDELETEキーを押せば削除することができます。

さて、テストの実行です。といってもボタンを一つ押すだけです。
Run all testsボタンをクリックしてください。新しいウィンドウが立ち上がり、自動で入力やクリックが進んでいくと思います。もし速すぎてよくわからないという場合は時計のアイコンをクリックすると速さを調節することができます。

<!-- ![selenium-ide-07](/images/2019/12/23/08_selenium-ide.gif) -->

実行後にはこのように結果がログとして記録されます。

動作確認が完了したら忘れずに保存しておきましょう。Save projectボタンから名前を指定して保存ができます。ファイル形式は `.side`という独自の形式[^1]になります。

以上がSelenium IDEのキャプチャ＆リプレイの基本的な使い方となります。

## 最後に

駆け足ですが、キャプチャ＆リプレイでのテスト自動化を解説していきました。
このサイトは入力値を変えることで様々なテストが可能です。例えば宿泊日付を過去の日付にすると金額ではなくエラーメッセージが表示されます。テストケースを追加してエラー系の自動テストを作成してみてもいいですね。

この記事が自動化をはじめる最初の一歩の助けになれば幸いです。

---

[^1]: 実体はJSON形式みたいです
