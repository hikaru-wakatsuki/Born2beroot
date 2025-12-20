*This project has been created as part of the 42 curriculum by hwakatsu.*

# Born2beroot

## Description
このプロジェクトは、仮想マシン上に安全なサーバー環境を構築することを目的とする。パスワードポリシーの設定、sudoの導入、SSHサービスやUFW/AppArmorといったセキュリティ対策、そしてMariaDBやWordPressといったサービスの展開（選択課題）まで、システム管理の基礎を網羅する。

## Project description
### Operating System Choice
本プロジェクトでは **Debian** を選択した。

### Debian vs Rocky Linux
**Linuxの配布パッケージ（ディストリビューション）**:

※**ディストリビューション**:Linuxカーネルに、日常的に使うための部品を一式まとめて、ひとつのOSとして使える形にして配られている“完成品セット”

**Rocky**:有料の商用版である「RHEL（Red Hat Enterprise Linux）」と同じ動きを目指して作られた無料のOS

特徴:**企業運用・長期安定・互換性重視**

    →新機能よりもAPI/ABI互換とバックポート修正を重視しているから

**Debian**:企業ではなくコミュニティ（Debian Project）が非営利で作っている“元祖的”なOS

特徴:**サーバー・デスクトップ・組み込みまで幅広い**

    →幅広いアーキテクチャと膨大なパッケージで汎用性が高いから

### AppArmor vs SELinux
**Linuxのセキュリティ機構（LSM）**:プロセスが何にアクセスできるかを細かく制御。

※**プロセス**:実行中のプログラム1つの“インスタンス”

**SELinux（Rocky）**

各プロセスは“ドメイン（タイプ）”というラベルを持つ（例: Apacheは httpd_t）。ポリシーが「そのドメインがどのファイルタイプに何の操作をできるか」を厳密に制御。

**AppArmor（Debian）**

各プロセスは“実行ファイルのパス”に紐づくプロファイルで制限される（例: /usr/sbin/nginx 用プロファイル）。許可されたファイルパスや操作だけが可能。

### UFW vs firewalld
**ファイアウォール管理ツール**:人間にとって扱いやすい形で実際のパケットフィルタ向けのルールに変換・適用してくれる上位ツール
**UFW（Uncomplicated Firewall）**:Ubuntu で標準的に提供される、シンプル志向のCLIツール。
**firewalld**:RedHat系で標準の常駐デーモン型ファイアウォール。NICや送信元に応じて異なるポリシーを適用できる。

### VirtualBox vs UTM
**ハイパーバイザー**:仮想化技術であり、必要なコンピューティング・リソースを分離し、仮想マシン(VM)の作成、実行、管理を可能にするソフトウェア

**ホストOS型=Type2**:既存のホストOSの上でアプリとして動く

**Virtual Box**:Oracle社が開発している、OSSの仮想化ソフト。ホストOSを選ばない。

**UTM**:Macユーザーのための仮想化ソフト。ホストOSがmacOSであると軽快に動作する。

### apt vs aptitude
**パッケージ管理**:OSがソフトウェア（プログラムやライブラリ）を「まとめ（パッケージ）」として扱い、インストール・更新・削除・依存関係の解決・署名検証などを一元管理する仕組み

**apt**:CLI。インストールするパッケージ名を指定してコマンドラインから起動すると、'/etc/apt/sources.list'に指定されたソースリストとそのパッケージの依存関係リストからそのパッケージを検索し、それらを並べ替えて、現在のパッケージと一緒に自動的にインストールする。

機能:apt-get（パッケージのインストール、アップグレード、システムのアップグレード、パッケージの削除、依存関係の解決）

**aptitude**:aptより高レベルのテキストベースの対話型のGUI。高度なパッケージングツールのフロントエンドであり、ユーザーが対話的にパッケージを検索し、インストールまたは削除できるようにします。

機能:apt-get,apt-mark(自動または手動でインストールされるようにパッケージをマーク),apt-cache(アップグレードできないようにパッケージを保留など)

### SSH
**SSH（Secure Shell）**:ネットワーク越しに安全（暗号化）にリモートのコンピュータへログインしたり、コマンド実行・ファイル転送・ポート転送を行うための仕組み（プロトコルとツール群）
sshのユーザー認証方法
・パスワード認証: ユーザー名とパスワードを使用して認証
・公開鍵認証: 事前に公開鍵と秘密鍵をペアで作成し認証側が公開鍵を持ち、ユーザーが対応する秘密鍵を持っているかどうかで認証

### LVM
**LVM（Logical Volume Manager）**:複数の物理ディスクやパーティションを仮想化し、柔軟に区切ったり拡張できる仕組み（ストレージ管理のレイヤ）。物理を意識せずに「論理ボリューム」を作成・拡張・移動・スナップショット化できる

**PV（Physical Volume）**:1デバイス等が保有する領域

**PE（Physical Extent）**:PVを一定サイズのブロックに区切った「最小割り当て単位」

**VG（Volume Group）**:複数のPVをまとめた容量プール。各PVのPEサイズは同じである必要がある。

**LV（Logical Volume）**:VG（ボリュームグループ）から切り出して提供する「論理的なディスク領域」

### sudo
**sudo（superuser do）**:「通常ユーザーが一時的に権限を昇格して、特定のコマンドを別ユーザー（既定はroot）として実行する」ための仕組み・コマンド
メリット:
・普段は一般ユーザーで作業し、必要な瞬間だけ権限を上げて安全に操作できる
・ユーザーがどのコマンドを実行可能かを細かく制御できる
・実行履歴がログに残る（監査・追跡に有用）
・既定では「ユーザー自身のパスワード」を尋ねるため、rootパスワードを配らずに管理できる

### cron
決まった時間・周期でコマンドやスクリプトを自動実行する仕組み

*/10 * * * * /usr/local/bin/monitoring.sh

 分　時　日　月　曜日　コマンド

### monitoring.sh

**uname -a**:OSの基本情報

**/proc/cpuinfo**:CPUの詳細情報を持つファイル

**free --mega**:メモリ使用状況を表示（MB）

**df -m**:ディスク使用量を表示（MB）

**vmstat 1 2**:CPU・メモリ・IOの状態をリアルタイム表示（1秒間隔・2回表示）

**who -b**:最後に起動した時刻（ブート時刻）

**lsblk**:ブロックデバイス（ディスク構成）表示

**ss -ta**:TCPソケット一覧表示（TCP・ALL）

**users**:現在ログインしているユーザー一覧

**ip link**:ネットワークインターフェース情報

**journalctl _COMM=sudo**:systemdのログ管理（_COMM=sudo = sudo実行記録のみ）

## Instructions
### 1. Installation and Partitioning (Bonus)

ボーナス要件に従い、以下の構造でLVMパーティションを構築した。 すべてのパーティションは、暗号化されたLVM（Logical Volume Management）内に作成されている。

/	ルートディレクトリ。システムの基本ファイルを格納する。
/home	ユーザーデータ。各ユーザーの個人ファイルを保持する。
/var	変動データ。ログやデータベースなど、サイズが変化するデータを格納する。
/var/log	システムログ。ログ専用の領域。ログ溢れによるシステム停止を防止するために独立させている。
/tmp	一時ファイル。一時的な作業データを格納する。
/srv	サービスデータ。WordPress等のサービスが提供するデータを格納する。

### 2. Execution

仮想マシンを起動し、以下の方法でアクセスします。

    VirtualBox で仮想マシンを起動。

### 3. Security Settings

    Password Policy: /etc/pam.d/common-password および /etc/login.defs を編集し、パスワードの複雑さ、有効期限、再利用制限を設定した。

    Sudo Configuration: /etc/sudoers.d/ 内に独自設定ファイルを作成。試行回数制限、カスタムエラーメッセージ、入力内容のログ保存、TTYモードの強制を適用した。

    SSH: セキュリティ向上のため、デフォルトポートを4242に変更し、rootでのログインを禁止した。

### Bonus Services
1. WordPress Stack

以下の軽量な構成（LLMPスタック）でWordPressを構築した。

    Web Server: **lighttpd**:軽量OSSWebサーバ

    AP: **WordPress**:PHP製アプリケーション

    Database: **MariaDB**:OSSデータベース

    UFW: 80番ポート（HTTP）を許可し、外部からのアクセスを可能にした。

2. Extra Service (Service of your choice)

    Selected Service: **OpenLiteSpeed**

    Reason:「Apache互換の超高速Webサーバ（商用）」であるため。Apache互換を強く意識して作られた 高速・高性能な商用Webサーバ

### HASH
 shasum <filename>

### SSH
 ssh [username]@[IP_ADDRESS] -p 4242

### UFW
 sudo ufw status

 sudo ufw allow [ポート番号]

 sudo utf deny [ポート番号]

### GROUP
 sudo addgroup <groupname>

 getent group <groupname>

### USER
 sudo adduser <login>

 sudo adduser <username> <groupname>

　passwd <username>
### PASSWD
 /etc/login.defs

 /etc/pam.d/common-password

 chage -l hwakatsu
### SUDO
/etc/sudoers.d/sudo_config

### HOST
 hostnamectl set-hostname <hostname>

 hostnamectl

 /etc/hosts

### cron
 crontab -e   # 編集

 crontab -l   # 表示

 crontab -r   # 削除
### WordPress
http://localhost:8080/
### OpenLiteSpeed
http://localhost:7080

## Resources
**ハイパーバイザー**:
https://www.redhat.com/ja/topics/virtualization/what-is-a-hypervisor

**パッケージ管理**:
https://www.tecmint.com/difference-between-apt-and-aptitude/
