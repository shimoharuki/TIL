# hotwireを使用した経緯

現在作成しているアプリで 非同期通信を行いたかったけど別に非同期にこだわる必要はなかったので使ってみようとおもっただけ

本来非同期にするには

レスポンスされたJSONに対してDOMを構築してとフロントとバックエンドでコードを書く必要があるが

hotwireはいらないレスポンスされるのがhtmlなのでフロントでコードを書く機会がすごい減る

# hotwireの構成


hotwireの中身は現在基本的には

`turbo`と`Stimulus`で構成

turboはここから3分割でき

>Turbo Driveページ全体を非同期っぽしたい時に使用

>Turbo Frames部分的なHtmlタグのみの際に使用

>Turbo Streams複数の部分的なHtmlタグの際に使用


となっている

またturboをrailsで使用する際には

`turbo-rails`というgemを使用すると便利


Stimulusは

`turboではできないようなJSを使用したコードなどを出来るだけ書きやすく記述する`

機能。こちらもgemは存在している(stimulus-rails)
