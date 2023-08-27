# webapiとは

まずはAPIから。

>API（Application Programming Interface）とは、プログラムの機能の一部を別のプログラム上で利用できるように共有する仕組みです。

これにwebという単語を合体させていることから

>Web APIとは、httpやhttpsなどWeb技術を用いて実現されるAPIの一類型となる


例としてgoogleapiやsns認証その他諸々

# apiの仕組みについて

railsの仕組みはクライアントがhttpリクエストを送り、controllerへ送信その後レスポンスをhtmlとして返すのが一般的になっている(MVC)

それに対してAPIはクライアントがAPIプログラムのサーバーにリクエストを送り、`レスポンスとしてAPIプログラムの機能をレスポンスする`形になっている。

