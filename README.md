# COSMAC MicroBoard Computer

## Project Overview

COSMACの評価ボードであるCDP18S020の主要機能を製作し、その上でRCA UT4 ROMという小さなモニタプログラムを動かすことを最初の目標としました。可能であればCDP18S020の主要機能を再現したいと考えました。

以下のドキュメントを参考にし、現在入手可能なパーツに置き換え、COSMAC評価ボード CDP18S020の主要部分を10cmｘ10cmの基板上に製作しています。合わせて、外部バスに拡張基板を接続し、評価ボードに実装されているLED表示やGPIO(CDP1852)を付加しました。

- [Evaluation Kit Manual for the RCA CDP1802 COSMAC Microprocessor](http://bitsavers.trailing-edge.com/components/rca/cosmac/MPM-203_CDP1802_Evaluation_Kit_Manual_Sep76.pdf)

その後、UT4の代わりに[The 1802 Membership Card](http://www.sunrise-ev.com/1802.htm)のモニタプログラムMCSMP20を載せることでより強力なシステムとなり、その上でPixie(CDP1861)互換のデバイスを接続することで、COSMAC VIP OSやCHIP-8といったCOSMAC VIPのアプリケーションが動作しています。

ビデオ出力回路は以下の3種類を試しています。

- TV DISPLAY Boardはつくるシリーズ７に掲載されている回路です。これはCDP1861互換ではなく独自回路になっています。
- STG1861は- [Spare Time Gizmos' projects.](http://www.sparetimegizmos.com/Hardware/Elf2K_Accessories.htm#STG1681%20Pixie%20Graphics%20Replacement)で公開されているCDP1861 Pixie互換の回路です。HEX KeyboardはCOSMAC VIPのマニュアルに掲載されている回路です。
- [Teensy Pixie Video](https://github.com/fourstix/MCard1802TeensyPixieVideo)はGaston Williamsさんが開発したCDP1861互換のOLED表示回路です。

![image](/docs/images/coamac_stg1861_rev02_pcb3.jpg)

## Schematics

- [CPU Board Rev 0.4 (2020/5/7)](https://kanpapa.com/cosmac/download/cdp18s020_cpu_rev04_scr.pdf)
- [Extention Board Rev 0.3 (2020/2/2)](https://kanpapa.com/cosmac/download/cdp18s020_ex_rev03_scr.pdf)
- [TV DISPLAY Board Rev 0.2 (2020/1/3)](https://kanpapa.com/cosmac/document/cosmac_tv_rev0_2_sch.pdf) ... Not CDP1861
- [STG1861 display/Hex keyboard Rev 0.2 (2020/4/30)](https://kanpapa.com/cosmac/download/cosmac_stg1861_rev0_2_scr.pdf)
- [COSMAC MBC BUS Rev. 0.2 (2020/6/21)](https://kanpapa.com/cosmac/download/cosmac_mbcbus_rev0_2_sch.pdf)
- [Teensy Pixie Video/HEX Keyboard/Sound Board Rev. 0.1 (2020/08/03)](https://kanpapa.com/cosmac/download/cosmac_teensy_pixie_rev0_1_sch.pdf)

## BOM

- [CPU Board Rev 0.4](/bom/cdp18s020_cpu_rev04_bom.md)
- [Extention Board Rev 0.3](/bom/cdp18s020_ex_rev03_bom.md)
- [TV DISPLAY Board Rev 0.2](/bom/cosmac_tv_rev02_bom.md)
- [STG1861 display/Hex keyboard Rev 0.2](/bom/cosmac_stg1861_rev02_bom.md)
- [Teensy Pixie Video/HEX Keyboard/Sound Rev. 0.1](/bom/cosmac_teensy_pixie_rev01_bom.md)

## Memory Map

CPU Board Rev 0.1では以下のようにしました。

|Address|Device|Notes|
|:--|:--|:--|
|0000 - 1FFF| 6264 RAM (U3) |USER AREA 8KByte |
|2000 - 7FFF| 6264 RAM (U3)のイメージ | 使用不可 |
|8000 - 87FF| 28C256 EEPROM (U2) | UT4 MONITOR |
|8800 - 8BFF| 28C256 EEPROM (U2)のイメージ | 使用不可 |
|8C00 - 8FFF| 6264 RAM (U4) | UT4 REGISTER STORAGE 無くてもUT4は動きます|
|9000 - FFFF| 28C256 EEPROM(U2)と6264 RAM (U4)のイメージ|使用不可|

CPU Board Rev 0.2ではシンプルにしました。UT4 REGISTER STORAGEは省略しました。

|Address|Device|Notes|
|:--|:--|:--|
|0000 - 7FFF| 62256 RAM (U3 or U5)|USER AREA 32KByte|
|8000 - 81FF| 28C256 EEPROM (U2) |UT4 MONITOR|
|8200 - FFFF| 28C256 EEPROM (U2) |未使用|

CPU Board Rev 0.2以降で、UT4の代わりにMCSMP20モニタとCOSMAC VIP OSをROMに焼いています。

|Address|Device|Notes|
|:--|:--|:--|
|0000 - 7FFF| 62256 RAM (U3 or U5) |USER AREA 32KByte|
|8000 - CFFF| 28C256 EEPROM (U2) |MCSMP20 MONITOR|
|D000 - FFFF| 28C256 EEPROM (U2) |COSMAC VIP OS|

## 操作方法

RESET後にRUN U(Utirity)ボタンを押すと$8000のROMが$0000に一時的に配置され、UT4モニタが起動します。
RESET後にRUN P(Program)ボタンを押すと$0000に配置されたアプリケーションが実行されます。
S5をSTEPにして、RESET後にRUN Pを押すことで１ステップ実行されます。
UT4モニタの使いかたはCDP18S020のマニュアルを参照してください。具体的な操作例は[ブログ](https://kanpapa.com/cosmac/blog/2019/10/cosmac-mbc-sample1-run.html)にあります。

## PCB Gerber view

### CPU Board(Rev 0.4)

2020/5/7にRev 0.4基板を発注しました。COVID-19の影響で大幅な配送遅延があり2020/6/23に到着しました。Rev. 0.3との違いは27C256対応です。

![image](/docs/images/cdp18s020_cpu_rev04_gerber_view.jpg)

※組み立て時にはS2, S3,S4のプッシュスイッチの取り付け向きに注意してください。導通状態になっているピンが1番ピンなので事前にテスターで確認してください。また、今回S5はトグルスイッチにしましたのでご注意ください。

### Extention Board(Rev 0.3)

2020/2/2に基板を発注し2020/2/13に納品されました。現時点では機能的には問題ありません。バスコネクタの位置が旧仕様のためBUS基板での接続はできません。

![image](/docs/images/cosmac_ex_rev03_gerber_view.jpg)

### TV DISPLAY Board(Rev 0.2)

2020/1/4に発注し1/9に納品されました。現時点では機能的には問題ありません。バスコネクタの位置が旧仕様のためBUS基板での接続はできません。これは実験的な回路であり、CDP1861互換ではありません。

![image](/docs/images/cosmac_tv_rev02_gerver_viewer.png)

### STG1861 Display/Hexkey Board(Rev 0.2)

2020/4/30に発注しました。2020/5/16に納品されました。

![image](/docs/images/coamac_stg1861_rev02_pcb2.jpg)

### COSMAC MBC BUS Board Rev. 0.2

2020/6/21に発注し、2020/7/8に到着しました。

![image](/docs/images/cosmac_mbc_bus_rev02_gerber_viewer.jpg)

### Teensy Pixie Video/HEX Keyboard/Sound Board Rev. 0.1

2020/8/4に発注し2020/8/13に到着しました。

![image](/docs/images/teensy32_pixie_hexkey_sound_pcb1.jpg)

## PCB Gerber data

- COSMAC MBC CPUボード Rev 0.4 [cdp18s020_cpu_rev04.zip](/gerber/cdp18s020_cpu_rev04.zip) (2020/05/07)
- COSMAC MBC 拡張ボード Rev 0.3 [cdp18s020_ex_rev03.zip](/gerber/cdp18s020_ex_rev03.zip) (2020/02/02)
- COSMAC MBC TVディスプレイボード Rev 0.2 (Not CDP1861) [cosmac_tv_rev0_2.zip](/gerber/cosmac_tv_rev0_2.zip) (2020/1/3)
- STG1861 Display/Hexkey Board(Rev 0.2) Under construction
- COSMAC MBC BUS Rev. 0.2 [cosmac_mbcbus_rev0_2.zip](/gerber/cosmac_mbcbus_rev0_2.zip) (2020/06/21)
- Teensy Pixie Video/HEX Keyboard/Sound board Rev. 0.1 [cosmac_teensy_pixie_rev0_1.zip](/gerber/cosmac_teensy_pixie_rev0_1.zip) (2020/08/03)

## PCB Errata

プリント基板の修正情報です。

### CPU Board Rev 0.4

特に問題は見つかっておりません。

### Extention Board Rev. 0.3

CPUバスの配置を左側に変更しましたが、その際に1番ピンの位置が1.27mm上側にずれていました。フラットケーブル接続では支障はありませんが、次回改版する機会があれば修正します。
LEDの種類によっては1KΩでもまだ明るすぎると感じるかもしれません。事前に輝度を確認の上抵抗値を決定してください。

### STG1861 Display/Hexkey board Rev. 0.2

ジャンパー部分のシルク印刷がまちがっていました。yanakata様ご指摘ありがとうございました。  
誤）JP1 INT, JP2 EF1  
正）JP1 EF1, JP2 INT  

### COSMAC MBC BUS board Rev. 0.2

特に問題は見つかっておりません。

### Teensy Pixie Video/HEX Keyboard/Soundボード Rev. 0.1

特に問題は見つかっておりません。

## 動作確認済アプリケーション

1. 文字列を表示するプログラム

    - アセンブラソース [sample1.asm](https://github.com/kanpapa/cosmac/blob/master/ut4/sample1.asm)
    - アセンブルリスト [sample1.lst](https://github.com/kanpapa/cosmac/blob/master/ut4/sample1.lst)

1. TINY BASIC
    - [Evaluation Kit Manual for the RCA CDP1802 COSMAC Microprocessor](http://bitsavers.trailing-edge.com/components/rca/cosmac/MPM-203_CDP1802_Evaluation_Kit_Manual_Sep76.pdf)に掲載されているダンプリストを入力して動作しました。

1. MCSMP20

    - [The 1802 Membership Card](http://www.sunrise-ev.com/1802.htm)のモニタプログラム

1. COSMAC VIP Operating System

    - [RCA COSMAC VIP CDP18S711 Instruction Manual](http://bitsavers.trailing-edge.com/components/rca/cosmac/COSMAC_VIP_Instruction_Manual_1978.pdf)に掲載されているもの。

1. COSMAC VIP CHIP-8 Interpreter

    - [RCA COSMAC VIP CDP18S711 Instruction Manual](http://bitsavers.trailing-edge.com/components/rca/cosmac/COSMAC_VIP_Instruction_Manual_1978.pdf)に掲載されているもの。

## テスターの皆様

3名のかたにテスターとしてご協力いただいています。ありがとうございます。

- @DAI_2040 様  
- @Sabotenboy 様  
- @yanatoku 様  

## 参考・引用文献

- トランジスタ技術別冊　つくるシリーズ７　手作りコンピュータ入門 CQ出版社, 1981
- [KiCad 5.0 / 5.1 入門実習テキスト『KiCad Basics for 5.x』 　Kosaka.Lab.出版掛 マッハ新書](https://booth.pm/ja/items/941963)
- [intersil CDP1802AC/3データシート](https://www.renesas.com/jp/ja/www/doc/datasheet/cdp1802ac-3.pdf)
- [SB-Assembler](https://www.sbprojects.net/sbasm/)
- [COSMAC ELF - RCA CDP1802 Computing](http://www.cosmacelf.com/)
- [Compute II Issue 3: The 1802 Instruction Set](https://www.atarimagazines.com/computeii/issue3/page52.php)
- [Evaluation Kit Manual for the RCA CDP1802 COSMAC Microprocessor](http://bitsavers.trailing-edge.com/components/rca/cosmac/MPM-203_CDP1802_Evaluation_Kit_Manual_Sep76.pdf)
- [RCA COSMAC VIP CDP18S711 Instruction Manual](http://bitsavers.trailing-edge.com/components/rca/cosmac/COSMAC_VIP_Instruction_Manual_1978.pdf)
- [The 1802 Membership Card](http://www.sunrise-ev.com/1802.htm)
- [Spare Time Gizmos: STG1861 Pixie Graphics Replacement](http://www.sparetimegizmos.com/Hardware/Elf2K_Accessories.htm#STG1681%20Pixie%20Graphics%20Replacement)
- [fourstix/MCard1802TeensyPixieVideo](https://github.com/fourstix/MCard1802TeensyPixieVideo)

## 利用上の注意

本サイトに掲載している回路、技術、プログラムなどは無保証です。  
これらの利用によって発生したトラブルには責任を負いかねます。自己責任にてご利用ください。  
KiCADデータおよびガーバーデータはCC BY-SA 4.0 (Copyright (C) 2019 Kazuhiro Ouchi)です。  
記事中のサンプルプログラムはMITライセンスです。  
