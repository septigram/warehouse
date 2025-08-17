# stunnel TLS Proxy

## LICENSE

```
本資料は、GPL v2ライセンスに基づき公開された 以下のドキュメントを簡易的に抜粋翻訳したもので、内容の正確性、網羅性は保証しない。
https://www.stunnel.org/static/stunnel.html

Copyright (C) 1998-2025 Michal Trojnara

この翻訳物の配布もGPL v2に従う。
```

## 名前

stunnel - TLSオフローディングとロードバランシングプロキシ

## 概要

### Unix:
**stunnel** [FILE] | -fd N | -help | -version | -sockets | -options

### WIN32:
**stunnel** [ [ -install | -uninstall | -start | -stop | -reload | -reopen | -exit ] [-quiet] [FILE] ] | -help | -version | -sockets | -options

## 説明

**stunnel**プログラムは、リモートクライアントとローカル（**inetd**で起動可能）またはリモートサーバー間の**TLS**暗号化ラッパーとして動作するように設計されている。この概念は、システム上でTLS非対応のデーモンを実行している場合、クライアントとの安全な**TLS**チャネルでの通信を簡単に設定できることである。

**stunnel**は、POP-2、POP-3、IMAPサーバーのような一般的に使用される**Inetd**デーモン、NNTP、SMTP、HTTPのスタンドアロンデーモン、およびソースコードを変更せずにネットワークソケット上でPPPをトンネリングするために使用できる。

この製品には、Eric Young (eay@cryptsoft.com)によって書かれた暗号化ソフトウェアが含まれている。

## オプション

**FILE**
指定された設定ファイルを使用

**-fd N** (Unixのみ)
指定されたファイル記述子から設定ファイルを読み取り

**-help**
**stunnel**ヘルプメニューを表示

**-version**
**stunnel**のバージョンとコンパイル時のデフォルト値を表示

**-sockets**
デフォルトのソケットオプションを表示

**-options**
サポートされているTLSオプションを表示

**-install** (Windows NT以降のみ)
NTサービスをインストール

**-uninstall** (Windows NT以降のみ)
NTサービスをアンインストール

**-start** (Windows NT以降のみ)
NTサービスを開始

**-stop** (Windows NT以降のみ)
NTサービスを停止

**-reload** (Windows NT以降のみ)
実行中のNTサービスの設定ファイルをリロード

**-reopen** (Windows NT以降のみ)
実行中のNTサービスのログファイルを再オープン

**-exit** (Win32のみ)
既に開始されたstunnelを終了

**-quiet** (Win32のみ)
メッセージボックスを表示しない

## 設定ファイル

設定ファイルの各行は以下のいずれかである：

- 空行（無視される）
- ';'で始まるコメント（無視される）
- 'option_name = option_value'のペア
- サービス定義の開始を示す'[service_name]'

オプションのアドレスパラメータは以下のいずれかである：

- ポート番号
- IPアドレス（IPv4、IPv6、またはドメイン名）とポート番号のコロン区切りペア
- Unixソケットパス（Unixのみ）

## グローバルオプション

**chroot** = DIRECTORY (Unixのみ)
**stunnel**プロセスをchrootするディレクトリ

**chroot**は**stunnel**をchrooted jailに保持する。**CApath**、**CRLpath**、**pid**、**exec**はjail内に配置され、パッチは**chroot**で指定されたディレクトリを基準とした相対パスである必要がある。

オペレーティングシステムのいくつかの機能も、ファイルがchroot jail内に配置されている必要がある。例：

- 遅延リゾルバは通常、/etc/nsswitch.confと/etc/resolv.confが必要である
- ログファイルのローカル時間には/etc/timezoneが必要である
- その他の機能には、/dev/zeroや/dev/nullなどのデバイスが必要な場合がある

**compression** = deflate | zlib
データ圧縮アルゴリズムを選択

デフォルト：圧縮なし

DeflateはRFC 1951で説明されている標準的な圧縮方法です。

**debug** = [FACILITY.]LEVEL
デバッグレベル

レベルは、syslogレベル名または数値のいずれかである：emerg (0)、alert (1)、crit (2)、err (3)、warning (4)、notice (5)、info (6)、またはdebug (7)。指定されたレベルとそれより数値的に小さいすべてのレベルのログが表示される。

**debug = debug**（または同等の&lt;debug = 7&gt;）レベルは最も詳細なログ出力を生成する。このログレベルはstunnel開発者によって理解されることを意図しており、ユーザー向けではない。stunnel開発者から要求された場合、または混乱することを意図している場合を除き、デバッグレベルを使用すべきである。

デフォルトのログレベルはnotice (5)である。

syslogの'daemon'ファシリティが使用される（ファシリティ名が指定されていない場合）。（ファシリティはWin32ではサポートされていない。）

ファシリティとレベルの両方で大文字小文字は無視される。

**EGD** = EGD_PATH (Unixのみ)
Entropy Gathering Daemonソケットへのパス

**OpenSSL**乱数生成器に供給するために使用するEntropy Gathering Daemonソケット。

**engine** = auto | ENGINE_ID
ハードウェアまたはソフトウェア暗号化エンジンを選択

デフォルト：ソフトウェアのみの暗号化

暗号化デバイスから証明書と対応する秘密鍵を使用するエンジン設定の例については、Examplesセクションを参照すべきである。

**engineCtrl** = COMMAND[:PARAMETER]
ハードウェアエンジンを制御する

**engineDefault** = TASK_LIST
現在のエンジンに委任されるOpenSSLタスクを設定する

パラメータは、現在のエンジンに委任されるタスクのカンマ区切りリストを指定する。

以下のタスクが利用可能な場合がある（エンジンでサポートされている場合）：ALL、RSA、DSA、ECDH、ECDSA、DH、RAND、CIPHERS、DIGESTS、PKEY、PKEY_CRYPTO、PKEY_ASN1。

**fips** = yes | no
FIPS 140-2モードを有効または無効にする

このオプションにより、**stunnel**がFIPS 140-2サポートでコンパイルされている場合、FIPSモードに入ることを無効にできる。

デフォルト：no（バージョン5.00以降）

**foreground** = yes | quiet | no (Unixのみ)
フォアグラウンドモード

フォアグラウンドに留まる（フォークしない）。

**yes**パラメータを使用すると、**syslog**と**output**で指定された宛先に加えて、stderrにもログを出力する。

デフォルト：デーモンモードではバックグラウンド

**iconActive** = ICON_FILE (GUIのみ)
確立された接続がある場合に表示されるGUIアイコン

Windowsプラットフォームでは、パラメータは16x16ピクセル画像を含む.icoファイルである必要がある。

**iconError** = ICON_FILE (GUIのみ)
有効な設定が読み込まれていない場合に表示されるGUIアイコン

Windowsプラットフォームでは、パラメータは16x16ピクセル画像を含む.icoファイルである必要がある。

**iconIdle** = ICON_FILE (GUIのみ)
確立された接続がない場合に表示されるGUIアイコン

Windowsプラットフォームでは、パラメータは16x16ピクセル画像を含む.icoファイルである必要がある。

**log** = append | overwrite
ログファイルの処理

このオプションにより、ログファイル（**output**オプションで指定）を開くまたは再オープンする際に、追加するか上書きするかを選択できる。

デフォルト：append

**output** = FILE
ログメッセージをファイルに追加する

/dev/stdoutデバイスを使用して、ログメッセージを標準出力に送信できる（例：daemontools sploggerでログを記録する場合）。

**pid** = FILE (Unixのみ)
pidファイルの場所

引数が空の場合、pidファイルは作成されない。

**pid**パスは、指定されている場合は**chroot**ディレクトリを基準とした相対パスである。

**provider** = PROVIDER_ID
使用するプロバイダーの識別子を指定する。PROVIDER_IDは、特定の暗号化サービスプロバイダーを参照する一意の識別子である。

このオプションにはOpenSSL 3.0以降が必要である。

**providerParameter** = PROVIDER_ID:PARAMETER=VALUE
指定されたプロバイダーの特定のパラメータを設定する。PROVIDER_IDはプロバイダーを識別し、PARAMETERはパラメータ名、VALUEはその値である。このオプションにより、選択された暗号化サービスプロバイダーの設定をカスタマイズできる。

このオプションにはOpenSSL 3.5以降が必要である。

**RNDbytes** = BYTES
乱数シードファイルから読み取るバイト数

**RNDfile** = FILE
乱数シードデータを含むファイルへのパス

OpenSSLライブラリは、最初にこのファイルのデータを使用して乱数生成器にシードを設定する。

**RNDoverwrite** = yes | no
乱数シードファイルを新しい乱数データで上書きする

デフォルト：yes

**service** = SERVICE (Unixのみ)
stunnelサービス名

指定されたサービス名は、syslogとTCP Wrappersの**inetd**モードサービス名として使用される。このオプションは技術的にサービスセクションで指定できるが、グローバルオプションでのみ有用である。

デフォルト：stunnel

**setEnv** = VAR_NAME=VALUE
子プロセスの環境変数を変更または追加する。VAR_NAMEが既に存在する場合、その値が更新される。それ以外の場合は、新しい変数が作成される。この変更は、生成された子プロセスのみに適用され、現在の環境には影響しない。

**syslog** = yes | no (Unixのみ)
syslog経由でのログ記録を有効にする

デフォルト：yes

**taskbar** = yes | no (WIN32のみ)
タスクバーアイコンを有効にする

デフォルト：yes

## サービスレベルオプション

各設定セクションは、角括弧内のサービス名で始まる。サービス名は、libwrap（TCP Wrappers）アクセス制御と、ログファイルで**stunnel**サービスを区別するために使用される。

**inetd**モードで**stunnel**を実行したい場合（**inetd**、**xinetd**、または**tcpserver**などのサーバーによってネットワークソケットが提供される場合）は、下記の**INETD MODE**セクションを読む必要がある。

**accept** = [HOST:]PORT
指定されたアドレスで接続を受け入れ

ホストが指定されていない場合、ローカルホストのすべてのIPv4アドレスがデフォルトになる。

すべてのIPv6アドレスでリッスンするには：

```
accept = :::PORT
```

**CAengine** = ENGINE-SPECIFIC_CA_CERTIFICATE_IDENTIFIER
エンジンから信頼できるCA証明書を読み込み

読み込まれたCA証明書は、**verifyChain**と**verifyPeer**オプションで使用される。

単一のサービスセクションで複数の**CAengine**オプションが許可されている。

現在サポートされているエンジン：pkcs11、cngである。

**CApath** = CA_DIRECTORY
ディレクトリから信頼できるCA証明書を読み込み

読み込まれたCA証明書は、**verifyChain**と**verifyPeer**オプションで使用される。このディレクトリ内の証明書は、XXXXXXXX.0という名前である必要がある。ここでXXXXXXXXは証明書のDERエンコードされたサブジェクトのハッシュ値である。

このパラメータは、サーバーモードでOCSPステープリングを検証するために必要なルートCA証明書を提供するためにも使用できる。

ハッシュアルゴリズムは**OpenSSL 1.0.0**で変更された。**OpenSSL 0.x.x**から**OpenSSL 1.x.x**以降にアップグレードする際は、ディレクトリでc_rehashを実行する必要がある。

**CApath**パスは、指定されている場合は**chroot**ディレクトリを基準とした相対パスである。

**CAfile** = CA_FILE
ファイルから信頼できるCA証明書を読み込み

読み込まれたCA証明書は、**verifyChain**と**verifyPeer**オプションで使用される。

このパラメータは、サーバーモードでOCSPステープリングを検証するために必要なルートCA証明書を提供するためにも使用できる。

**CAstore** = URI_CA
URIで指定されたリソースから信頼できるCA証明書を読み込み

このオプションにより、**OSSL_STORE**フレームワークでサポートされている外部ソース（PKCS#11モジュール（例：ハードウェアトークン）、システム証明書ストア、またはリモートリソースなど）からCA証明書を読み込むことができる。

このオプションは、**CAfile**と**CAdir**とは独立して使用でき、同様に**verifyChain**または**verifyPeer**で使用される証明書、およびサーバーモードでのOCSPステープリングの検証用の証明書を提供する。

このオプションにはOpenSSL 3.0以降が必要である。

**cert** = CERT_FILE | URI
証明書チェーンファイル名

パラメータは、**stunnel**がリモートクライアントまたはサーバーに対して自身を認証するために使用する証明書を含むファイルを指定する。ファイルには、実際のサーバー/クライアント証明書から始まり、自己署名ルートCA証明書で終わる証明書チェーン全体が含まれている必要がある。ファイルはPEMまたはP12形式である必要がある。

証明書チェーンはサーバーモードで必要で、クライアントモードではオプションである。

このパラメータは、ハードウェアエンジンまたはプロバイダーが有効になっている場合の証明書識別子（PKCS#11 URI）としても使用される。

注意：プロバイダーにはOpenSSL 3.0以降が必要である

**checkEmail** = EMAIL
エンドエンティティ（リーフ）ピア証明書サブジェクトのメールアドレスを検証

証明書は、サブジェクトチェックが指定されていない場合、またはエンドエンティティ（リーフ）ピア証明書のメールアドレスが**checkEmail**で指定されたメールアドレスのいずれかと一致する場合に受け入れられる。

単一のサービスセクションで複数の**checkEmail**オプションが許可されている。

このオプションにはOpenSSL 1.0.2以降が必要である。

**checkHost** = HOST
エンドエンティティ（リーフ）ピア証明書サブジェクトのホストを検証する

証明書は、サブジェクトチェックが指定されていない場合、またはエンドエンティティ（リーフ）ピア証明書のホスト名が**checkHost**で指定されたホストのいずれかと一致する場合に受け入れられる。

単一のサービスセクションで複数の**checkHost**オプションが許可されている。

このオプションにはOpenSSL 1.0.2以降が必要である。

**checkIP** = IP
エンドエンティティ（リーフ）ピア証明書サブジェクトのIPアドレスを検証する

証明書は、サブジェクトチェックが指定されていない場合、またはエンドエンティティ（リーフ）ピア証明書のIPアドレスが**checkIP**で指定されたIPアドレスのいずれかと一致する場合に受け入れられる。

単一のサービスセクションで複数の**checkIP**オプションが許可されている。

このオプションにはOpenSSL 1.0.2以降が必要である。

**ciphers** = CIPHER_LIST
許可されるTLS暗号を選択（TLSv1.2以下）

このオプションはTLSv1.3暗号スイートには影響しない。

TLS接続で許可する暗号のコロン区切りリストである。例：DES-CBC3-SHA:IDEA-CBC-MD5。

**ciphersuites** = CIPHERSUITES_LIST
許可されるTLSv1.3暗号スイートを選択する

優先順位順のTLSv1.3暗号スイート名のコロン区切りリストである。

**ciphersuites**オプションは、**OpenSSL 3.0**以降でコンパイルされている場合、不明な暗号を無視する。

このオプションにはOpenSSL 1.1.1以降が必要である。

デフォルト：TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384:TLS_AES_128_GCM_SHA256

**client** = yes | no
クライアントモード（リモートサービスがTLSを使用）

デフォルト：no（サーバーモード）

**config** = COMMAND[:PARAMETER]
**OpenSSL**設定コマンド

**OpenSSL**設定コマンドは、指定されたパラメータで実行される。これにより、stunnel設定ファイルから任意の設定コマンドを呼び出すことができる。サポートされているコマンドは**SSL_CONF_cmd(3ssl)**マニュアルページで説明されている。

複数の**config**行を使用して、複数の設定コマンドを指定できる。

楕円曲線をサポートするには、**config = Curves:list_curves**を有効にする代わりに**curves**オプションを使用すべきである。

このオプションにはOpenSSL 1.0.2以降が必要である。

**connect** = [HOST:]PORT
リモートアドレスに接続

ホストが指定されていない場合、ホストはデフォルトでlocalhostになる。

単一のサービスセクションで複数の**connect**オプションが許可されている。ホストが複数のアドレスに解決される場合、または複数の**connect**オプションが指定されている場合、リモートアドレスはラウンドロビンアルゴリズムを使用して選択される。

**CRLpath** = DIRECTORY
証明書失効リストディレクトリ

これは、**verifyChain**と**verifyPeer**オプションを使用する際に**stunnel**がCRLを探すディレクトリである。このディレクトリ内のCRLは、XXXXXXXX.r0という名前である必要がある。ここでXXXXXXXXはCRLのハッシュ値である。

ハッシュアルゴリズムは**OpenSSL 1.0.0**で変更された。**OpenSSL 0.x.x**から**OpenSSL 1.x.x**にアップグレードする際は、c_rehashを実行する必要がある。

**CRLpath**パスは、指定されている場合は**chroot**ディレクトリを基準とした相対パスである。

**CRLfile** = CRL_FILE
証明書失効リストファイル

このファイルには複数のCRLが含まれており、**verifyChain**と**verifyPeer**オプションで使用される。

**curves** = list
ECDH曲線を':'で区切って指定する

OpenSSL 1.1.1より古いバージョンでは、単一の曲線名のみが許可されている。

サポートされている曲線のリストを取得するには：

```
openssl ecparam -list_curves
```

デフォルト：

```
X25519:P-256:X448:P-521:P-384 (OpenSSL 1.1.1以降)

prime256v1 (OpenSSL 1.1.1より古いバージョン)
```

**logId** = TYPE
接続識別子タイプ

この識別子により、各接続に対して生成されたログエントリを区別できる。

現在サポートされているタイプ：

**sequential**
数値の順次識別子は単一の**stunnel**インスタンス内でのみ一意であるが、非常にコンパクトである。手動ログ分析に最も有用である。

**unique**
この英数字識別子はグローバルに一意であるが、順次番号より長い。自動化されたログ分析に最も有用である。

**thread**
オペレーティングシステムスレッド識別子は一意ではなく（単一の**stunnel**インスタンス内でも）、短くもない。ソフトウェアまたは設定の問題のデバッグに最も有用である。

**process**
オペレーティングシステムプロセス識別子（PID）は、inetdモードで有用な場合がある。

デフォルト：sequential

**debug** = LEVEL
デバッグレベル

レベルは、syslogレベル名または数値のいずれかである：emerg (0)、alert (1)、crit (2)、err (3)、warning (4)、notice (5)、info (6)、またはdebug (7)。指定されたレベルとそれより数値的に小さいすべてのレベルのログが表示される。デフォルトはnotice (5)である。

**debug = debug**または**debug = 7**レベルは最も詳細な出力を生成するが、stunnel開発者専用である。開発者である場合、または技術サポートにログを送信することを意図している場合を除き、この値を使用すべきである。それ以外の場合、生成されたログは混乱を招く**だろう**。

**delay** = yes | no
**connect**オプションのDNSルックアップを遅延

このオプションは、動的DNS、または**stunnel**起動時にDNSが利用できない場合（ロードウォリアーVPN、ダイヤルアップ設定）に有用である。

遅延リゾルバモードは、stunnelが起動時にサービスの**connect**ターゲットのいずれかを解決できない場合に自動的に有効になる。

遅延リゾルバは**failover = prio**を強制する。

デフォルト：no

**engineId** = ENGINE_ID
サービスのエンジンIDを選択

**engineNum** = ENGINE_NUMBER
サービスのエンジン番号を選択

エンジンは1から始まる番号が付けられる。

**exec** = EXECUTABLE_PATH
ローカルのinetdタイププログラムを実行する

**exec**パスは、指定されている場合は**chroot**ディレクトリを基準とした相対パスである。

Unixプラットフォームでは、以下の環境変数が設定される：REMOTE_HOST、REMOTE_PORT、SSL_CLIENT_DN、SSL_CLIENT_I_DN。

**execArgs** = $0 $1 $2 ...
**exec**の引数（プログラム名（$0）を含む）

現在、引用符はサポートされていない。引数は任意の量の空白で区切られる。

**failover** = rr | prio
複数の"connect"ターゲットのフェイルオーバー戦略

**rr**
ラウンドロビン - 公平な負荷分散

**prio**
優先度 - 設定ファイルで指定された順序を使用する

デフォルト：prio

**ident** = USERNAME
IDENT（RFC 1413）ユーザー名チェックを使用する

**include** = DIRECTORY
DIRECTORYに配置されているすべての設定ファイル部分を含める

ファイルは名前の昇順アルファベット順で含められる。推奨されるファイル名規則は

グローバルオプション用：

```
00-global.conf
```

ローカルサービスレベルオプション用：

```
01-service.conf

02-service.conf
```

**key** = KEY_FILE | URI
**cert**オプションで指定された証明書の秘密鍵

秘密鍵は証明書所有者を認証するために必要である。このファイルは秘密に保つ必要があるため、所有者のみが読み取り可能である必要がある。Unixシステムでは、以下のコマンドを使用できる：

```
chmod 600 keyfile
```

このパラメータは、ハードウェアエンジンまたはプロバイダーが有効になっている場合の秘密鍵識別子（PKCS#11 URI）としても使用される。

注意：プロバイダーにはOpenSSL 3.0以降が必要である

デフォルト：**cert**オプションの値

**libwrap** = yes | no
/etc/hosts.allowと/etc/hosts.denyの使用を有効または無効にする

デフォルト：no（バージョン5.00以降）

**local** = HOST
デフォルトでは、発信インターフェースのIPアドレスがリモート接続のソースとして使用される。このオプションを使用して、静的ローカルIPアドレスをバインドする。

**OCSP** = URL
エンドエンティティ（リーフ）ピア証明書検証用のOCSPレスポンダを選択する

**OCSPaia** = yes | no
証明書をAIA OCSPレスポンダで検証する

このオプションにより、**stunnel**は、AIA（Authority Information Access）拡張から取得されたOCSPレスポンダURLのリストで証明書を検証できる。

**OCSPflag** = OCSP_FLAG
OCSPレスポンダフラグを指定する

複数の**OCSPflag**を使用して、複数のフラグを指定できる。

現在サポートされているフラグ：NOCERTS、NOINTERN、NOSIGS、NOCHAIN、NOVERIFY、NOEXPLICIT、NOCASIGN、NODELEGATED、NOCHECKS、TRUSTOTHER、RESPID_KEY、NOTIMEである。

**OCSPnonce** = yes | no
OCSP nonce拡張を送信および検証する

このオプションは、OCSPプロトコルをリプレイ攻撃から保護する。計算オーバーヘッドにより、nonce拡張は通常、内部（例：企業）レスポンダでのみサポートされ、パブリックOCSPレスポンダではサポートされない。

**OCSPrequire** = yes | no
決定的なOCSPレスポンスを要求する

決定的なOCSPレスポンスがステープリングとOCSPレスポンダへの直接リクエストから取得されなかった場合でも接続を許可するには、このオプションを無効にする。

デフォルト：yes

**options** = SSL_OPTIONS
**OpenSSL**ライブラリオプション

パラメータは、**SSL_CTX_set_options(3ssl)**マニュアルで説明されている**OpenSSL**オプション名であるが、**SSL_OP_**プレフィックスはない。**stunnel -options**は、現在の**stunnel**と**OpenSSL**ライブラリの組み合わせで許可されていることが判明したオプションをリストする。

複数の**option**行を使用して、複数のオプションを指定できる。オプション名の前にダッシュ（"-"）を付けることで、オプションを無効にできる。

例えば、誤ったEudora TLS実装との互換性のために、以下のオプションを使用できる：

```
options = DONT_INSERT_EMPTY_FRAGMENTS
```

デフォルト：

```
options = NO_SSLv2
options = NO_SSLv3
```

**OpenSSL 1.1.0**以降でコンパイルされている場合は、特定のTLSプロトコルバージョンを無効にする代わりに**sslVersionMax**または**sslVersionMin**オプションを使用すべきである。

**protocol** = PROTO
TLSをネゴシエートするアプリケーションプロトコル

このオプションは、TLS暗号化の初期のプロトコル固有ネゴシエーションを有効にする。**protocol**オプションは、別のポートでのTLS暗号化と一緒に使用すべきではない。

現在サポートされているプロトコル：

**cifs**
Sambaで実装されたCIFSプロトコルの独自（文書化されていない）拡張である。この拡張のサポートはSamba 3.0.0で削除された。

**capwin**
http://www.capwin.org/ アプリケーションサポート

**capwinctrl**
http://www.capwin.org/ アプリケーションサポート

このプロトコルはクライアントモードでのみサポートされている。

**connect**
RFC 2817 - **HTTP/1.1内でのTLSへのアップグレード**、セクション5.2 - **CONNECTによるトンネルの要求**に基づく

このプロトコルはクライアントモードでのみサポートされている。

**imap**
RFC 2595 - **IMAP、POP3、ACAPでのTLSの使用**に基づく

**ldap**
RFC 2830 - **軽量ディレクトリアクセスプロトコル（v3）：トランスポート層セキュリティの拡張**に基づく

**nntp**
RFC 4642 - **ネットワークニュース転送プロトコル（NNTP）でのトランスポート層セキュリティ（TLS）の使用**に基づく

このプロトコルはクライアントモードでのみサポートされている。

**pgsql**
**http://www.postgresql.org/docs/8.3/static/protocol-flow.html#AEN73982**に基づく

**pop3**
RFC 2449 - **POP3拡張メカニズム**に基づく

**proxy**
HAProxy PROXYプロトコルバージョン1 **https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt**による元のクライアントIPアドレスの受け渡し

**smtp**
RFC 2487 - **TLS経由での安全なSMTPのためのSMTPサービス拡張**に基づく

**socks**
SOCKSバージョン4、4a、5がサポートされている。SOCKSプロトコル自体は、最終的な宛先アドレスを保護するためにTLS暗号化層内にカプセル化されている。

**http://www.openssh.com/txt/socks4.protocol**

**http://www.openssh.com/txt/socks4a.protocol**

SOCKSプロトコルのBINDコマンドはサポートされていない。USERIDパラメータは無視される。

SOCKS暗号化に基づくVPNのサンプル設定ファイルについては、Examplesセクションを参照すべきである。

**protocolAuthentication** = AUTHENTICATION
プロトコルネゴシエーションの認証タイプ

現在、このオプションはクライアントサイドの'connect'と'smtp'プロトコルでのみサポートされている。

'connect'プロトコルのサポートされている認証タイプは'basic'または'ntlm'である。デフォルトの'connect'認証タイプは'basic'である。

'smtp'プロトコルのサポートされている認証タイプは'plain'または'login'である。デフォルトの'smtp'認証タイプは'plain'である。

**protocolDomain** = DOMAIN
プロトコルネゴシエーションのドメイン

現在、このオプションはクライアントサイドの'connect'プロトコルでのみサポートされている。

**protocolHeader** = HEADER
プロトコルネゴシエーションのヘッダー

現在、このオプションはクライアントサイドの'connect'プロトコルでのみサポートされている。

**protocolHost** = ADDRESS
プロトコルネゴシエーションのホストアドレス

'connect'プロトコルネゴシエーションの場合、**protocolHost**は**stunnel**によって接続されるプロキシによって接続される最終的なTLSサーバーのHOST:PORTを指定する。**stunnel**によって直接接続されるプロキシサーバーは**connect**オプションで指定する必要がある。

'smtp'プロトコルネゴシエーションの場合、**protocolHost**はクライアントSMTP HELO/EHLO値を制御する。

**protocolPassword** = PASSWORD
プロトコルネゴシエーションのパスワード

現在、このオプションはクライアントサイドの'connect'と'smtp'プロトコルでのみサポートされている。

**protocolUsername** = USERNAME
プロトコルネゴシエーションのユーザー名

現在、このオプションはクライアントサイドの'connect'と'smtp'プロトコルでのみサポートされている。

**PSKidentity** = IDENTITY
PSKクライアントのPSKアイデンティティ

**PSKidentity**は、**stunnel**クライアントで認証に使用されるPSKアイデンティティを選択するために使用できる。このオプションはサーバーセクションでは無視される。

デフォルト：**PSKsecrets**ファイルで指定された最初のアイデンティティである。

**PSKsecrets** = FILE
PSKアイデンティティと対応するキーを含むファイル

ファイルの各行は以下の形式である：

```
IDENTITY:KEY
```

16進数キーは自動的にバイナリ形式に変換される。キーは少なくとも16バイト長である必要があり、これは16進数キーの場合は少なくとも32文字を意味する。ファイルは世界読み取り可能でも世界書き込み可能でもあってはならない。

**pty** = yes | no (Unixのみ)
'exec'オプション用の疑似端末を割り当てる

**redirect** = [HOST:]PORT
証明書ベースの認証失敗時にTLSクライアント接続をリダイレクトする

このオプションはサーバーモードでのみ動作する。一部のプロトコルネゴシエーションは**redirect**オプションと互換性がない。

**renegotiation** = yes | no
TLS再ネゴシエーションをサポートする

TLS再ネゴシエーションのアプリケーションには、一部の認証シナリオや長期間の接続の再キーイングが含まれる。

一方で、この機能は些細なCPU枯渇DoS攻撃を促進する可能性がある：

**http://vincent.bernat.im/en/blog/2011-ssl-dos-mitigation.html**

TLS再ネゴシエーションを無効にしても、この問題が完全に軽減されるわけではないことに注意すべきである。

デフォルト：yes（**OpenSSL**でサポートされている場合）

**reset** = yes | no
エラーを示すためにTCP RSTフラグの使用を試行する

このオプションは一部のプラットフォームではサポートされていない。

デフォルト：yes

**retry** = yes | no | DELAY
connect+execセクションが切断された後に再接続する

DELAY値は再試行前のミリ秒数を指定する。"retry = yes"は"retry = 1000"と同じ効果がある。

デフォルト：no

**securityLevel** = LEVEL
セキュリティレベルを設定する

各レベルの意味は以下の通りである：

**level 0**
すべてが許可される。

**level 1**
セキュリティレベルは80ビットのセキュリティの最小値に対応する。80ビット未満のセキュリティを提供するパラメータはすべて除外される。その結果、1024ビット未満のRSA、DSA、DHキーと160ビット未満のECCキーは禁止される。すべてのエクスポート暗号スイートは80ビット未満のセキュリティを提供するため禁止される。SSLバージョン2は禁止される。MACにMD5を使用する暗号スイートも禁止される。さらに、OpenSSL 3.0以降では、SSLv3、TLS 1.0、TLS 1.1はすべて無効になる。

**level 2**
セキュリティレベルを112ビットのセキュリティに設定する。その結果、2048ビット未満のRSA、DSA、DHキーと224ビット未満のECCキーは禁止される。レベル1の除外に加えて、RC4を使用する暗号スイートも禁止される。圧縮は無効になる。OpenSSL 3.0より古いバージョンでは、SSLバージョン3も許可されない。

**level 3**
セキュリティレベルを128ビットのセキュリティに設定する。その結果、3072ビット未満のRSA、DSA、DHキーと256ビット未満のECCキーは禁止される。レベル2の除外に加えて、フォワードシークレシーを提供しない暗号スイートは禁止される。セッションチケットは無効になる。OpenSSL 3.0より古いバージョンでは、TLS 1.1未満のバージョンは許可されない。

**level 4**
セキュリティレベルを192ビットのセキュリティに設定する。その結果、7680ビット未満のRSA、DSA、DHキーと384ビット未満のECCキーは禁止される。MACにSHA1を使用する暗号スイートは禁止される。OpenSSL 3.0より古いバージョンでは、TLS 1.2未満のバージョンは許可されない。

**level 5**
セキュリティレベルを256ビットのセキュリティに設定する。その結果、15360ビット未満のRSA、DSA、DHキーと512ビット未満のECCキーは禁止される。

**default: 2**

**securityLevel**オプションは、**OpenSSL 1.1.0**以降でコンパイルされている場合にのみ利用可能である。

**requireCert** = yes | no
**verifyChain**または**verifyPeer**用のクライアント証明書を要求

**requireCert**が**no**に設定されている場合、**stunnel**サーバーは証明書を提示しなかったクライアント接続を受け入れる。

**verifyChain = yes**と**verifyPeer = yes**の両方が**requireCert = yes**を意味する。

デフォルト：no

**setgid** = GROUP (Unixのみ)
UnixグループID

グローバルオプションとして：デーモンモードで指定されたグループにsetgid()し、他のすべてのグループをクリアする。

サービスレベルオプションとして：**accept**で指定されたUnixソケットのグループを設定する。

**setuid** = USER (Unixのみ)
UnixユーザーID

グローバルオプションとして：デーモンモードで指定されたユーザーにsetuid()する。

サービスレベルオプションとして：**accept**で指定されたUnixソケットの所有者を設定する。

**sessionCacheSize** = NUM_ENTRIES
セッションキャッシュサイズ

**sessionCacheSize**は内部セッションキャッシュエントリの最大数を指定する。

値0は無制限サイズに使用できる。メモリ枯渇DoS攻撃のリスクにより、本番環境での使用は推奨されない。

**sessionCacheTimeout** = TIMEOUT
セッションキャッシュタイムアウト

これはキャッシュされたTLSセッションを保持する秒数である。

**sessionResume** = yes | no
セッション再開を許可または禁止する

デフォルト：yes

**sessiond** = HOST:PORT
sessiond TLSキャッシュサーバーのアドレス

**sni** = SERVICE_NAME:SERVER_NAME_PATTERN (サーバーモード)
Server Name Indication TLS拡張（RFC 3546）のセカンダリサービス（名前ベースの仮想サーバー）としてサービスを使用する。

**SERVICE_NAME**は**accept**オプションでクライアント接続を受け入れるプライマリサービスを指定する。**SERVER_NAME_PATTERN**はリダイレクトされるホスト名を指定する。パターンは'*'文字で始まる場合がある（例：'*.example.com'）。通常、単一のプライマリサービスに対して複数のセカンダリサービスが指定される。**sni**オプションは、単一のセカンダリサービス内で複数回指定することもできる。

このサービスとプライマリサービスは、クライアントモードで設定できない。

セカンダリサービスの**connect**オプションは、**protocol**オプションが指定されている場合は無視される。これは**protocol**がTLSハンドシェイク前にリモートホストに接続するためである。

Libwrapチェック（Unixのみ）は2回実行される：TCP接続が受け入れられた後にプライマリサービス名で、TLSハンドシェイク中にセカンダリサービス名で。

**sni**オプションは、**OpenSSL 1.0.0**以降でコンパイルされている場合にのみ利用可能である。

**sni** = SERVER_NAME (クライアントモード)
パラメータをTLS Server Name Indication（RFC 3546）拡張の値として使用。

空のSERVER_NAMEはSNI拡張の送信を無効にする。

**sni**オプションは、**OpenSSL 1.0.0**以降でコンパイルされている場合にのみ利用可能である。

**socket** = a|l|r:OPTION=VALUE[:VALUE]
accept/local/remoteソケットにオプションを設定する

lingerオプションの値はl_onof:l_lingerである。timeの値はtv_sec:tv_usecである。

例：

```
socket = l:SO_LINGER=1:60
    ローカルソケットのクローズに1分のタイムアウトを設定
socket = r:SO_OOBINLINE=yes
    リモートソケットの受信データストリームに
    アウトオブバンドデータを直接配置
socket = a:SO_REUSEADDR=no
    アドレス再利用を無効にする（デフォルトで有効）
socket = a:SO_BINDTODEVICE=lo
    ループバックインターフェースでのみ接続を受け入れ
```

**sslVersion** = SSL_VERSION
TLSプロトコルバージョンを選択

サポートされているバージョン：all、SSLv2、SSLv3、TLSv1、TLSv1.1、TLSv1.2、TLSv1.3

特定のプロトコルの可用性は、リンクされたOpenSSLライブラリに依存する。OpenSSLの古いバージョンはTLSv1.1、TLSv1.2、TLSv1.3をサポートしていない。OpenSSLの新しいバージョンはSSLv2をサポートしていない。

古いSSLv2とSSLv3は現在デフォルトで無効になっている。

オプションを設定

```
sslVersion = SSL_VERSION
```

は、**OpenSSL 1.1.0**以降でコンパイルされている場合、オプション

```
sslVersionMax = SSL_VERSION
sslVersionMin = SSL_VERSION
```

と同等である。

**sslVersionMax** = SSL_VERSION
サポートされているプロトコルバージョンの最大値

サポートされているバージョン：all、SSLv3、TLSv1、TLSv1.1、TLSv1.2、TLSv1.3

**all**は、リンクされたOpenSSLライブラリでサポートされている最高バージョンまでのプロトコルバージョンを有効にする。

特定のプロトコルの可用性は、リンクされたOpenSSLライブラリに依存する。

**sslVersionMax**オプションは、**OpenSSL 1.1.0**以降でコンパイルされている場合にのみ利用可能である。

デフォルト：all

**sslVersionMin** = SSL_VERSION
サポートされているプロトコルバージョンの最小値

サポートされているバージョン：all、SSLv3、TLSv1、TLSv1.1、TLSv1.2、TLSv1.3

**all**は、リンクされたOpenSSLライブラリでサポートされている最低バージョンまでのプロトコルバージョンを有効にする。

特定のプロトコルの可用性は、リンクされたOpenSSLライブラリに依存する。

**sslVersionMin**オプションは、**OpenSSL 1.1.0**以降でコンパイルされている場合にのみ利用可能である。

デフォルト：TLSv1

**stack** = BYTES (FORKモデルを除く)
作成されたスレッドのCPUスタックサイズ

過度なスレッドスタックサイズは仮想メモリ使用量を増加させる。不十分なスレッドスタックサイズはアプリケーションクラッシュを引き起こす可能性がある。

デフォルト：65536バイト（テストしたすべてのプラットフォームで十分）

**ticketKeySecret** = SECRET
セッションチケット機密性保護に使用される16進対称キー

RFC 5077で定義されたセッションチケットは、サーバーサイドキャッシュがセッションごとの状態を維持する必要がない拡張セッション再開機能を提供する。

**ticketKeySecret**と**ticketMacSecret**オプションを組み合わせることで、他のクラスタノードでネゴシエートされたセッションを再開したり、サーバー再起動後にネゴシエートされたセッションを再開したりできる。

キーは16または32バイト長である必要があり、これは正確に32または64の16進数字を意味する。オプションで、2文字の16進バイト間にコロンを使用できる。

このオプションはサーバーモードでのみ動作する。

**ticketKeySecret**オプションは、**OpenSSL 1.0.0**以降でコンパイルされている場合にのみ利用可能である。

**NO_TICKET**オプションを無効にすることは、OpenSSL 1.1.1より古いバージョンでのチケットサポートに必要であるが、このオプションは**redirect**オプションと互換性がないことに注意すべきである。

**ticketMacSecret** = SECRET
セッションチケット整合性保護に使用される16進対称キー

キーは16または32バイト長である必要があり、これは正確に32または64の16進数字を意味する。オプションで、2文字の16進バイト間にコロンを使用できる。

このオプションはサーバーモードでのみ動作する。

**ticketMacSecret**オプションは、**OpenSSL 1.0.0**以降でコンパイルされている場合にのみ利用可能である。

**TIMEOUTbusy** = SECONDS
期待されるデータを待つ時間

**TIMEOUTclose** = SECONDS
close_notifyを待つ時間（バグのあるMSIEの場合は0に設定）

**TIMEOUTconnect** = SECONDS
リモートホストへの接続を待つ時間

**TIMEOUTidle** = SECONDS
アイドル接続を保持する時間

**TIMEOUTocsp** = SECONDS
OCSPレスポンダへの接続を待つ時間

**transparent** = none | source | destination | both (Unixのみ)
選択されたプラットフォームで透過プロキシサポートを有効にする

サポートされている値：

**none**
透過プロキシサポートを無効にする。これがデフォルトである。

**source**
アドレスを書き換えて、ラップされたデーモンが**stunnel**を実行しているマシンではなく、TLSクライアントマシンから接続しているように見せる。

このオプションは現在以下で利用可能である：

**Linux &gt;=2.6.28でのリモートモード（**connect**オプション）**
この設定には、**stunnel**をrootとして実行し、**setuid**オプションなしで実行する必要がある。

この設定には、iptablesとルーティングの以下の設定が必要である（/etc/rc.localまたは同等のファイルで）：

```
iptables -t mangle -N DIVERT
iptables -t mangle -A PREROUTING -p tcp -m socket -j DIVERT
iptables -t mangle -A DIVERT -j MARK --set-mark 1
iptables -t mangle -A DIVERT -j ACCEPT
ip rule add fwmark 1 lookup 100
ip route add local 0.0.0.0/0 dev lo table 100
echo 0 &gt;/proc/sys/net/ipv4/conf/lo/rp_filter
```

**stunnel**もrootとして実行し、**setuid**オプションなしで実行する必要がある。

**Linux 2.2.xでのリモートモード（**connect**オプション）**
この設定には、カーネルが**透過プロキシ**オプションでコンパイルされている必要がある。接続されたサービスは別のホストにインストールされている必要がある。クライアントへのルーティングは**stunnel**ボックスを経由する必要がある。

**stunnel**もrootとして実行し、**setuid**オプションなしで実行する必要がある。

**FreeBSD &gt;=8.0でのリモートモード（**connect**オプション）**
この設定には追加のファイアウォールとルーティング設定が必要である。**stunnel**もrootとして実行し、**setuid**オプションなしで実行する必要がある。

**ローカルモード（**exec**オプション）**
この設定は**libstunnel.so**共有ライブラリをプリロードすることで動作する。_RLD_LIST環境変数はTru64で使用され、LD_PRELOAD変数は他のプラットフォームで使用される。

**destination**
元の宛先が**connect**オプションの代わりに使用される。

透過宛先のサービスセクションは以下のようになる：

```
[transparent]
client = yes
accept = &lt;stunnel_port&gt;
transparent = destination
```

この設定にはiptables設定が必要で、/etc/rc.localまたは同等のファイルで実行される可能性がある。

同じホストにインストールされた接続ターゲット用：

```
/sbin/iptables -t nat -I OUTPUT -p tcp --dport &lt;redirected_port&gt; \
    -m ! --uid-owner &lt;stunnel_user_id&gt; \
    -j DNAT --to-destination &lt;local_ip&gt;:&lt;stunnel_port&gt;
```

リモートホストにインストールされた接続ターゲット用：

```
/sbin/iptables -I INPUT -i eth0 -p tcp --dport &lt;stunnel_port&gt; -j ACCEPT
/sbin/iptables -t nat -I PREROUTING -p tcp --dport &lt;redirected_port&gt; \
    -i eth0 -j DNAT --to-destination &lt;local_ip&gt;:&lt;stunnel_port&gt;
```

透過宛先オプションは現在Linuxでのみサポートされている。

**both**
**source**と**destination**の両方の透過プロキシを使用する。

後方互換性のために2つのレガシーオプションもサポートされている：

**yes**
このオプションは**source**に名前が変更された。

**no**
このオプションは**none**に名前が変更された。

**verify** = LEVEL
ピア証明書を検証する

このオプションは廃止されており、**verifyChain**と**verifyPeer**オプションに置き換える必要がある。

**level 0**
ピア証明書チェーンを要求して無視する。

**level 1**
存在する場合、ピア証明書チェーンを検証する。

**level 2**
ピア証明書チェーンを検証する。

**level 3**
ピア証明書チェーンとエンドエンティティ（リーフ）ピア証明書をローカルにインストールされた証明書に対して検証する。

**level 4**
ピア証明書チェーンを無視し、エンドエンティティ（リーフ）ピア証明書のみをローカルにインストールされた証明書に対して検証する。

**default**
検証なし。

**verifyChain** = yes | no
ルートCAから開始してピア証明書チェーンを検証

サーバー証明書検証では、**checkHost**または**checkIP**で特定の証明書を要求することも重要である。

自己署名ルートCA証明書は、**CAfile**で指定されたファイル、または**CApath**で指定されたディレクトリに保存されている必要がある。

デフォルト：no

**verifyPeer** = yes | no
エンドエンティティ（リーフ）ピア証明書を検証する

エンドエンティティ（リーフ）ピア証明書は、**CAfile**で指定されたファイル、または**CApath**で指定されたディレクトリに保存されている必要がある。

デフォルト：no

## 制限事項

**stunnel**は、FTPプロトコルの性質上、FTPデーモンには使用できません。FTPプロトコルはデータ転送に複数のポートを利用するためです。ただし、TLS対応のFTPとtelnetデーモンのバージョンが利用可能です。

## INETDモード

**stunnel**の最も一般的な使用法は、ネットワークポートでリッスンし、connectオプションを介して新しいポート、または**exec**オプションを介して新しいプログラムとの通信を確立することである。ただし、**inetd**、**xinetd**、または**tcpserver**などで他のプログラムが着信接続を受け入れ、**stunnel**を起動することを望む特別な場合がある。

例えば、**inetd.conf**に以下の行がある場合：

```
imaps stream tcp nowait root /usr/local/bin/stunnel stunnel /usr/local/etc/stunnel/imaps.conf
```

これらの場合、**inetd**スタイルのプログラムは、ネットワークソケット（上記の**imaps**）をバインドし、接続が受信されたときに**stunnel**に渡す責任がある。したがって、**stunnel**に**accept**オプションを持たせたくない。すべての**サービスレベルオプション**はグローバルオプションセクションに配置され、**[service_name]**セクションは存在しない。設定例については**EXAMPLES**セクションを参照すべきである。

## 証明書

TLS対応の各デーモンは、ピアに対して有効なX.509証明書を提示する必要がある。また、着信データを復号化するための秘密鍵も必要である。証明書とキーを取得する最も簡単な方法は、無料の**OpenSSL**パッケージで生成することである。証明書生成に関する詳細情報は、以下にリストされているページで見つけることができる。

**.pem**ファイルには、暗号化されていない秘密鍵と署名された証明書（証明書リクエストではない）が含まれている必要がある。したがって、ファイルは以下のようになる：

```
-----BEGIN RSA PRIVATE KEY-----
[エンコードされたキー]
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
[エンコードされた証明書]
-----END CERTIFICATE-----
```

## ランダム性

**stunnel**は、TLSが良好なランダム性を使用するために、PRNG（疑似乱数生成器）にシードを設定する必要があります。以下のソースが、十分なランダムデータが収集されるまで順番に読み込まれます：

- **RNDfile**フラグで指定されたファイル
- 設定されている場合、RANDFILE環境変数で指定されたファイル
- RANDFILEが設定されていない場合、ホームディレクトリの.rndファイル
- コンパイル時に'--with-random'で指定されたファイル
- Windowsで実行されている場合の画面の内容
- **EGD**フラグで指定されたegdソケット
- コンパイル時に'--with-egd-sock'で指定されたegdソケット
- /dev/urandomデバイス

コンソールユーザーインタラクション（マウスの移動、ウィンドウの作成など）がないWindowsマシンでは、画面の内容は十分に可変ではないため、**RNDfile**フラグで使用するランダムファイルを提供する必要があることに注意してください。

**RNDfile**フラグで指定されたファイルにはランダムデータが含まれている必要があることに注意すべきである。これは、**stunnel**が実行されるたびに異なる情報が含まれている必要があることを意味する。これは**RNDoverwrite**フラグが使用されない限り、自動的に処理される。このファイルを手動で更新したい場合、**OpenSSL**の最近のバージョンの**openssl rand**コマンドが有用である。

重要な注意：/dev/urandomが利用可能な場合、**OpenSSL**はしばしばランダム状態をチェックしながらPRNGにシードを設定する。/dev/urandomがあるシステムでは、上記のリストの最下部に記載されているにもかかわらず、**OpenSSL**がそれを使用する可能性がある。これは**stunnel**ではなく**OpenSSL**の動作である。

## DHパラメータ

**stunnel** 4.40以降には、ハードコードされた2048ビットDHパラメータが含まれている。**stunnel** 5.18以降では、これらのハードコードされたDHパラメータは、自動生成された一時的なDHパラメータで24時間ごとに置き換えられる。DHパラメータの生成には数分かかる場合がある。

代替として、証明書ファイルで静的DHパラメータを指定することも可能で、これにより一時的なDHパラメータの生成が無効になる：

```
openssl dhparam 2048 >> stunnel.pem
```

## ファイル

**/usr/local/etc/stunnel/stunnel.conf**
**stunnel**設定ファイル

## バグ

**execArgs**オプションとWin32コマンドラインは引用符をサポートしていない。

## 関連項目

**tcpd(8)**
インターネットサービスのアクセス制御機能

**inetd(8)**
インターネット「スーパーサーバー」

**http://www.stunnel.org/**
**stunnel**ホームページ

**http://www.openssl.org/**
**OpenSSL**プロジェクトウェブサイト

## 著者

**Michał Trojnara**
<**Michal.Trojnara@stunnel.org**>