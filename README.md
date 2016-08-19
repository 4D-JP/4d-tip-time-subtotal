# 4d-tip-time-subtotal
時間の小計を出力するサンプルです。

###概要

[Subtotal](http://doc.4d.com/4dv15r/help/command/ja/page97.html)で集計できる値は，実数・整数・倍長整数に限られます。

しかし，コマンドはフィールドだけでなく，変数にも対応しているので，時間を整数に変換（``0``を加算）する手法を組み合わせれば，時間の小計を出力することができます。

###例題

小計に使用する変数（リストフォームに配置されている必要はありませんが，型はしっかり宣言しておきます）および小計を出力するための変数を用意します。

```
C_TIME(TIMESUBTOTAL)
C_LONGINT(TIMEVALUE)

ALL RECORDS([Table_1])
ORDER BY([Table_1];[Table_1]ブレークレベル)
BREAK LEVEL(1)
ACCUMULATE(TIMEVALUE)

SET PRINT PREVIEW(True)
OPEN PRINTING JOB
PRINT SELECTION([Table_1])
CLOSE PRINTING JOB
```

出力フォームのメソッドは下記のようになります。

```
$event:=Form event

Case of 
	: ($event=On Load)
		
		TIMEVALUE:=0x0000
		TIMESUBTOTAL:=?00:00:00?
		
	: ($event=On Printing Detail)
		
		TIMEVALUE:=[Table_1]時間+0
		
	: ($event=On Printing Break)
		
		TIMESUBTOTAL:=Subtotal(TIMEVALUE)
		
End case 
```
