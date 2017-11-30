# 運用自動化ハンズオン に関するご案内
* このページは、Internet Week 2017 にて実施される、`運用自動化ハンズオン ～StackStormで実践するインフラ運用革命～` の資料及び情報掲載ページです

# プログラム・スケジュール
## 09:30 - 09:30 (30min) 参加者受付・事前準備と諸注意
### 09:00 参加者受付開始
- 会場に入りましたら、事前に資料を座席においていますので、紙の置いてある席にご着席下さい

### 09:05 - 開場諸注意
- プログラム実施にあたりましての注意事項をご案内いたします
- また、Slack への join, VM へのログイン(SSH 秘密鍵含む) を試行可能な方は、この時間を利用してご確認下さい

## 09:30 - 10:30 (60min) StackStormとは
### (10min) ハンズオンの説明 (担当: 杉本)
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-01-intro.pdf)

### (20min) ハンズオン Part 0. ハンズオン環境へのログイン・動作確認
- [ハンズオン](https://github.com/internetweek2017-st2/handson_documents/wiki/Handson-Part-0)

### (25min) StackStorm 概要 (担当:大山)
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-02-st2_abstraction.pdf)

### (5min) 休憩

## 10:30 - 12:00 (90min) ケーススタディ基本編 (午前の部)(担当: 大山)
### (45min) ハンズオン Part 1. st2 コマンドチュートリアル
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-03-part1-st2_tutorial.pdf)
- [ハンズオン](https://github.com/internetweek2017-st2/handson_documents/wiki/Handson-Part-1)

### (5min) 休憩 

### (40min) ハンズオン Part2. 障害自動検知と自動修復(AR) Sensu を活用した監視と障害検知
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-04-part2.pdf)
- [ハンズオン](https://github.com/internetweek2017-st2/handson_documents/wiki/Handson-Part-2)

## 12:00 - 13:15 休憩 (75min)
- ランチセッションにもご参加いただけます

## 13:15 - 14:55 (100min) ケーススタディ基本編 (午後の部)(担当:萬治)
### (50min) ハンズオン Part 3. 障害自動検知と自動修復(AR) 障害検知からの vSRX の AR
- [スライド1](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-05-part3-1.pdf)
- [スライド2](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-06-part3-2.pdf)
- [ハンズオン](https://github.com/internetweek2017-st2/handson_documents/wiki/Handson-Part-3)

### (5min) 休憩

### (40min) ハンズオン Part4. WorkFlow による複雑なオペレーション
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-07-part4.pdf)
- [ハンズオン](https://github.com/internetweek2017-st2/handson_documents/wiki/Handson-Part-4)

## 14:50 - 15:40 (50min) ケーススタディ応用編
### (35min) 実運用例の紹介(AD 認証, 冗長構成, バージョンアップ) (担当: 大山)
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-09-st2_advanced.pdf)

### (15min) 有償版 WorkflowComposer の紹介 (担当：管野)
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-10-st2_enterprise.pdf)

### (当日発表なし・資料のみ) Mistral の使用 (担当: 萬治)
- [スライド](https://github.com/internetweek2017-st2/handson_documents/blob/master/slide/iw2017-st2-08-mistral.pdf)

## アンケート回答(15:40-15:45)
- [こちら](https://questant.jp/q/iw2017-h3329)からアンケート回答をお願いします

# Vagrant を用いた環境構築方法について
本ハンズオンでご利用頂いた環境は Vagrant を用いて作成されているため、以下の手順により同じ環境を構築可能です。

## Vagrant インストール
[Vagrant のサイト](https://www.vagrantup.com/) から最新版（2.0.1）をダウンロード・インストールして下さい。  
※  `apt-get` 等でインストールすると version が古いため、サイトからダウンロード・インストールを行って下さい。  
その後、以下のコマンドで Vagrant plugin のインストールを行って下さい。
```
$ vagrant plugin install vagrant-host-shell
$ vagrant plugin install vagrant-junos
```

## VirtualBox のインストール
[VirtualBox のサイト](https://www.virtualbox.org/wiki/Downloads)からダウンロード・インストールを行って下さい。  
ubuntu 等の場合は `sudo apt-get install virtualbox` でインストール可能です。

## Vagrant で VM を作成する
```
$ git clone https://github.com/internetweek2017-st2/handson_documents.git
$ cd handson_documents
$ vagrant up
```
