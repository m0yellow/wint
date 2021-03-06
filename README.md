# WINT (browser digital signage system)

これはブラウザだけで利用できるデジタルサイネージシステムです。画面にはコンテンツ表示部とニュース表示部の二つの表示部があることで複数の情報を配信することができます。表示できるコンテンツは画像だけでなくHTMLも表示でき、表示時に逐次コンテンツを取得するため内容が動的なコンテンツの表示も可能です。コンテンツ表示部とニュース表示部で表示する内容は表示スケジュールを一巡するごとに必要に応じて更新させるため、システムを止めずに表示内容を変更することが可能です。

## デモ

[http://web.sfc.wide.ad.jp/~ema/wint/](http://web.sfc.wide.ad.jp/~ema/wint/)

## 設置方法

config.jsのcontentJsonUrlおよびnewsJsonUrlに表示スケジュールを示すJSONファイルのURLを記述し、このディレクトリをサーバ上に公開します。その後、Google Chromeで公開されたディレクトリにアクセスしてください。Google Chromeでのフルスクリーン表示はWindowsではF11キー、Macintoshではshift + command + Fで出来ます。

本システムでは、サーバサイドプログラミングを一切利用していないため、サーバ上での特殊な環境設定を必要としません。

## 設定の編集

config.jsにはスケジュールファイルのURLおよびコンテンツ部、ニュース部の表示内容切替時のアニメーションの設定などが記述されています。また、style.cssではコンテンツ部、ニュース部の文字、大きさ、余白の背景などを指定しています。環境にあわせてこれらの値を変更してください。

## スケジュールを指定する

コンテンツやニュースの表示スケジュールはconfig.jsのcontentJsonUrlとnewsJsonUrlのURLから得られるJSONの出力を元に解釈します。このJSONのフォーマットは下記のようになります。なお、本コード内ではこのJSONはファイルとして保持されているため時間経過に伴う変更はないスケジュールになっておりますが、別途、サーバサイドプログラミングでJSONを出力するプログラムを開発することでスケジュールを動的に変更できるようなシステムが完成します。

### コンテンツスケジュールのJSON仕様

    [
        {
            "duration": 10,
            "type": "iframe",
            "url": "http://localhost/wint/sample.html"
        },
        {
            "duration": 10,
            "type": "img",
            "url": "http://localhost/wint/sample.png"
        }
    ]

<dl>
<dt>duration</dt>
<dd>コンテンツの表示時間の長さ、単位は秒</dd>
<dt>type</dt>
<dd>iframeもしくはimg</dd>
<dt>url</dt>
<dd>コンテンツが存在するURL</dd>
</dl>

### ニューススケジュールのJSON仕様

    [
        "text1",
        "text2",
        "text3"
    ]

## テスト環境

本プログラムはGoogle Chrome バージョン 25.0.1364.160(Mac OS X)にて起動を確認しています。

## ライセンス

MITライセンスに準じています。利用にあたってlicense.txtを参照してください。

## 開発・運用上の仕様

* コンテンツ、ニュースのスケジュールファイルは各スケジュールの最後の表示時に再取得します。そのためスケジュールの編集が即座に反映されるわけでありません。
* コンテンツ部の表示の大きさは縦横比を保ったまま、画面を超過しないように拡大化しています。
* スケジュールを示すJSONの取得にはキャッシュ対策のため、取得時にURLの末尾に「'?timestamp='+日時」もしくは「'&timestamp='+日時」を追加してURLを変更しています。デリミタが?であるか&であるかの判定は、スケジュールファイルのURLに?の文字が含まれているか否かで判定しています。
* コンテンツを表示している際に、バックグラウンドで次に表示すべきコンテンツを取得する仕様になっています。このため、コンテンツ表示の段階でコンテンツを受信しないため、スムーズが画面の切り替えが可能になっております。稀にコンテンツの受信速度が遅いと次のコンテンツがスムーズに表示されないことがありますが、こういった場合はコンテンツを所有するサーバとの回線環境等を調整してください。
* HTMLページの表示にはHTMLタグのiframeを用いています。そのため、表示先ページにてHTTPヘッダ"X-Frame-Options"が"deny"にセットされている場合、表示することができません。
