# FS-USRP-IDL-Elements-remaining

- [FS-USRP-IDL-Elements-remaining](#fs-usrp-idl-elements-remaining)
  - [概要](#概要)
  - [機能](#機能)
  - [元のソースコードと変更したVI](#元のソースコードと変更したvi)
  - [開発環境](#開発環境)
  - [使い方](#使い方)
  - [CSVファイルを開く](#csvファイルを開く)

## 概要
このプロジェクトは、IDL環境でUSRPを使用した場合のバッファ残容量を計測します。
以下のブログの記事にあるデータはこのプロジェクトで取得しました。

USRP-RIO (IDL)で、長時間IQデータ取得を行うと一定間隔でオーバーフローが発生する不具合？か(1)  
  
http://mikioblog.dolphinsystem.jp/2021/08/usrp-rio-idliq1.html

## 機能
USRP-RIOをIDL環境で稼働させFIFO Read関数の"Elements Remainging"値をメモリに蓄積し、プログラム終了時にCSVファイルに出力します。

## 元のソースコードと変更したVI
このプロジェクトは、各LabVIEWのテンプレートプロジェクト "NI-USRP Simple Streaming"を元に作成しています。

変更したVIは以下です。

- Rx Streaming (Host).vi
グラフを表示しないようにした。  
ループが回る時間をFGVに保存するようにした。  
ログをCSVファイルに出力するようにした。
- Configure Rx.vi  
Configure Stream.viのhost FIFO depthに、グローバル変数を接続した。
- Fetch Rx Data (CBD WDT).vi  
Fetch Rx Data (CBD)の出力を波形に変換しないようにした(処理速度が落ちるので)。
- Fetch Rx Data (CBD).vi  
Fetch Rx Data (U32)の出力をCDBに変換しないようにした(処理速度が落ちるので)。
- Fetch Rx Data (U32).vi  
FIFO Read関数のElements RemaingingをFGVに保存するようにした。  
U32配列のtrimをしないようにした(処理速度が落ちるので)。

## 開発環境
LabVIEW 2018及び2020で開発しています。
それぞれのコードは、LabVIEW 2018, LabVIEW 2020フォルダ以下に含まれています。

## 使い方
LabVIEW 2018もしくは2020を起動します。
バッファ残容量のログデータをメモリ上に蓄積するため、64bitバージョンのLabVIEWを起動することを推奨します。


計測可能なUSRPは1台だけで、チャネルも１チャネルのみです。

1. FS-USRPIDL-Elements-remaining.lvprojを開きます。  
![プロジェクト](https://github.com/mikiofuku/FS-USRPIDL-Elements-remaining/blob/docimages/docimage/01.jpg)
3. Rx Streaming (Host).viを開きます。
![VI](https://github.com/mikiofuku/FS-USRPIDL-Elements-remaining/blob/docimages/docimage/02.jpg)
5. 使用するUSRPやIQレート、Number of Samples, host FIFO depthを指定します。RF0だけをEnableにします。
6. 例えば10分間計測する場合は、"Time target (s)"に"600"と入力します。
7. CSVファイルの出力先を指定する場合は、"log path"で指定します。指定が無ければデスクトップに出力します。
8. 実行します。
9. FPGAがダウンロードされて動作開始すると"Acquiring"が点灯します。
10. "Stop Rx"ボタンを押すと停止します。
11. 終了時にメモリに蓄積されたバッファ残容量のデータをCSVファイルに出力するため、出力処理完了までに時間が掛かりますのでしばらくお待ちください。

## CSVファイルを開く
計測時間が長いと、CSVファイルに出力されたログデータは大きくなるので、DIAdemで開くことを推奨します。

1. "*-elements remain.csv"をDIAdemのデータポータルにD&Dして開きます(それなりに時間が掛かるためしばらく待ちます)。
2. "*-overflow.csv"をDIAdemのデータポータルにD&Dして開きます。
3. VIEWボタンを押します。
4. データポータルの"elements remain"のCh0, Ch1の順番でクリックし、上側の2DグラフにD&Dします。バッファ残容量グラフが描画されます。
5. データポータルの"overflow"のCh0, Ch1の順番でクリックし、上側の2DグラフにD&Dします。バッファオーバーフローのグラフが描画されます。
6. 2Dグラフツールバーの"n システム [リニア]"をクリックして、グラフを２つに分割します。

![DIAdem](https://github.com/mikiofuku/FS-USRPIDL-Elements-remaining/blob/docimages/docimage/03.jpg)
