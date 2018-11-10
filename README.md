# 徳丸さんのセキュリティ問題

[問題：間違ったCSRF対策～初級編～](https://blog.tokumaru.org/2018/11/csrf.html)

## 実証コード

[attack.html](attack.html)

## 原因

[chgmail.php](chgmail.php)においてtokenが払い出されてない時を考慮してない
tokenが作られてない状態かつPOSTデータにtokenがセットされてない状態だとnull同士を比較するのでif文が真になってしまい
処理が続行されてしまう

## 対策

[chgmail-fix.php](chgmail-fix.php)みたいに未定義なら処理を進めない

    if (!isset($_SESSION['token']) or $_POST['token'] !== $_SESSION['token']) { // ワンタイムトークン確認
        die('正規の画面からご使用ください');
    }
