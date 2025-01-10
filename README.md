# ロール名
ansible_role_linux_initial_setup

# 機能概要
RedHat Enterprise Linuxサーバーに対して、パラメタパターンファイルに基づき、次の設定項目にある初期設定を実施する。

**設定項目**  
| 項番 | 設定項目                 | taskファイル     | tag          | テスト用tag        | 状態出力用tag       |
| ---- | ------------------------ | ---------------- | ------------ | ------------------ | ------------------- |
| 1    | RHSM設定                 | rhsm.yml         | rhsm         | check_rhsm         | output_rhsm         |
| 2    | dnf設定                  | dnf.yml          | dnf          | check_dnf          | output_dnf          |
| 3    | ロケール設定             | locale.yml       | locale       | check_locale       | output_locale       |
| 4    | kdump設定                | kdump.yml        | kdump        | check_kdump        | output_kdump        |
| 5    | サービス自動起動設定     | services.yml     | services     | check_services     | output_services     |
| 6    | カーネル設定             | kernel.yml       | kernel       | check_kernel       | output_kernel       |
| 7    | control-alt-delete無効化 | ctrl_alt_del.yml | ctrl_alt_del | check_ctrl_alt_del | output_ctrl_alt_del |
| 8    | systemd設定              | systemd.yml      | systemd      | check_systemd      | output_systemd      |
| 9    | ホスト名設定             | hostname.yml     | hostname     | check_hostname     | output_hostname     |
| 10   | SSH設定                  | sshd.yml         | sshd         | check_sshd         | output_sshd         |
| 11   | rsyslog設定              | rsyslog.yml      | rsyslog      | check_rsyslog      | output_rsyslog      |
| 12   | ファイアウォール設定     | firewalld.yml    | firewalld    | check_firewalld    | output_firewalld    |
| 13   | ログローテート設定       | logrotate.yml    | logrotate    | check_logrotate    | output_logrotate    |
| 14   | cron設定                 | crontab.yml      | crontab      | check_crontab      | output_crontab      |
| 15   | vmtool設定               | vmtool.yml       | vmtool       | check_vmtool       | output_vmtool       |
| 16   | DNS設定                  | dns.yml          | dns          | check_dns          | output_dns          |
| 17   | 時刻同期設定             | chronyd.yml      | chronyd      | check_chronyd      | output_chronyd      |
| 18   | ユーザ/グループ設定      | users.yml        | users        | check_users        | output_users        |
| 19   | SELinux設定              | selinux.yml      | selinux      | check_selinux      | output_selinux      |

※共通tag  
- check_all　　全ての処理に関するテストを実施
- output_all　　全ての処理に関する状態出力を実施

# 変数
| 変数名                                             | 型     | 内容                                                                  | 例                                                       |
| -------------------------------------------------- | ------ | --------------------------------------------------------------------- | -------------------------------------------------------- |
| linux_initial_setup_custom_pattern_file            | string | 自作の読み込むパラメタパターンファイルパス                            | ./custome/rhel93_cloud_it.yml                            |
| linux_initial_setup_pattern_file                   | string | 選択するvars/配下に組み込まれたパラメタパターンファイル名             | rhel92_info_ot.yml                                       |
| linux_initial_setup_rhsm_conf_src                  | string | /etc/rhsm/rhsm.confに送信する設定ファイルパス                         | {{ role_path }}/test/files/rhel92/rhsm.conf              |
| linux_initial_setup_dnf_releasever                 | string | システムOSのバージョン                                                | 9.2                                                      |
| linux_initial_setup_dnf_proxy                      | string | /etc/dnf/dnf.confに設定するProxyの値                                  | http://nhkproxy.inoc.jp:8080                             |
| linux_initial_setup_dnf_packages_file              | string | インストールするパッケージのリストが記載されたファイルパス            | {{ role_path }}/files/rhel92_ot/dnf/rhel92_packages.yml  |
| linux_initial_setup_dnf_excludes                   | bool   | dnfに関するexcludepkgsの設定を有効化するか否か                        | true                                     |
| linux_initial_setup_timezone                       | string | タイムゾーン                                                          | Asia/Tokyo                                               |
| linux_initial_setup_locale                         | string | System Locale (LANG)                                                  | ja_JP.UTF-8                                              |
| linux_initial_setup_keymap                         | string | System Locale (Keymap)                                                | jp                                                       |
| linux_initial_setup_x11_keymap                     | string | System Locale (Layout)                                                | jp                                                       |
| linux_initial_setup_kdump_conf_src                 | string | /etc/kdump.confに送信する設定ファイルパス                             | {{ role_path }}/files/rhel92_ot/kdump/kdump.conf"        |
| linux_initial_setup_enabled_services               | list   | 自動起動を有効にするサービスのリスト                                  | - sssd<br> - oddjobd                                     |
| linux_initial_setup_disabled_services              | list   | 自動起動を無効にするサービスのリスト                                  | - auditd<br> - microcode                                 |
| linux_initial_setup_kernel_conf_src                | string | /etc/sysctl.confに送信する設定ファイル                                | {{ role_path }}/files/rhel92_ot/kernel/sysctl.conf       |
| linux_initial_setup_system_conf_src                | string | /etc/systemd/system.confに送信する設定ファイルパス                    | {{ role_path }}/files/rhel92_ot/systemd/system.conf      |
| linux_initial_setup_hostname                       | string | ホスト名                                                              | {{ inventory_hostname_short }}                           |
| linux_initial_setup_rsyslog_conf_src               | string | /etc/rsyslog.confに送信する設定ファイルパス                           | {{ role_path }}/files/rhel92_ot/rsyslog/rsyslog.conf     |
| linux_initial_setup_journald_conf_src              | string | /etc/systemd/journald.confに送信する設定ファイルパス                  | {{ role_path }}/files/rhel92_ot/rsyslog/journald.conf    |
| linux_initial_setup_firewall_param                 | list   | firewalldで設定するzone毎の設定値リスト                               | see [defaults](defaults/main.yml)                        |
| linux_initial_setup_firewall_param.interfaces      | list   | 実装されているNICのインターフェース名のリスト                         | {{ ansible_interfaces \| difference(['lo'])}}            |
| linux_initial_setup_firewall_param.add_services    | list   | ファイアウォールで許可するサービスのリスト                            | ssh                                                      |
| linux_initial_setup_firewall_param.remove_services | list   | ファイアウォールで許可しないサービスのリスト                          | ["cockpit","dhcpv6-client"]                              |
| linux_initial_setup_logrotate_conf_src             | string | /etc/logrotate.confに送信する設定ファイルパス                         | {{ role_path }}/files/rhel92_ot/logrotate/logrotate.conf |
| linux_initial_setup_logrotate_d_conf_src_list      | list   | /etc/logrotate.d配下に送信する設定ファイルパスのリスト                | see [defaults](defaults/main.yml)                        |
| linux_initial_setup_crontab_src                    | string | /etc/crontabに送信する設定ファイルパス                                | {{ role_path }}/files/rhel92_ot/crontab/crontab          |
| linux_initial_setup_vmware_tools_src               | string | /etc/vmware-tools/tools.confに送信する設定ファイルパス                | {{ role_path }}/files/rhel92_ot/vmtool/tools.conf        |
| linux_initial_setup_resolv_conf_src                | string | /etc/resolv.confに送信する設定ファイルパス                            | {{ role_path }}/test/files/rhel92/resolv.conf            |
| linux_initial_setup_networkmanager_conf_src        | string | /etc/NetworkManager/conf.d/90-dns-none.confに送信する設定ファイルパス | {{ role_path }}/files/rhel92_ot/dns/90-dns-none.conf     |
| linux_initial_setup_chrony_conf_src                | string | /etc/chrony.confに送信する設定ファイルパス                            | {{ role_path }}/test/files/rhel92/chrony.conf            |
| linux_initial_setup_chrony_chronyd_src             | string | /etc/sysconfig/chronydに送信する設定ファイルパス                      | {{ role_path }}/files/rhel92_ot/chronyd/chronyd          |
| linux_initial_setup_desired_groups                 | list   | 追加するグループのリストnameとidの要素を持つ                          | see [defaults](defaults/main.yml)                        |
| linux_initial_setup_desired_users                  | list   | 追加するユーザーのリストnameとgroupとidの要素を持つ                   | see [defaults](defaults/main.yml)                        |
| linux_initial_setup_desired_sudo_users             | list   | sudoersに追加するユーザーのリスト                                     |                                                          |
| linux_initial_setup_selinux_state                  | string | selinux設定値 <br> enforcing/permissive/disabled                      | disabled                                                 |

# 処理内容
以下の順で処理を実行する。  
各処理は**メイン処理**,**テスト**,**状態出力**の3ブロック構成となっており、tagによりどのブロックを実行するかを制御する。  

<details>
  <summary>[Read Pattren file] パラメタパターンファイルを読み込む</summary>
  
  - `linux_initial_setup_pattern_file`で指定されたvars/配下の組み込みパラメタパターンファイルを読み込む  
  - カスタムのパラメタパターンファイルの場合は`linux_initial_setup_custom_pattern_file`を読み込む

</details>

<details>
  <summary>[Parameter_validation] 入力パラメタのバリデーションチェック</summary>  

  パラメタパターンファイルの設定値や自動化対象ノードの状態をチェックする
</details>

<details>
  <summary>[1. Setting rhsm] RHSM設定</summary>  
  
  `linux_initial_setup_rhsm_conf_src`で指定されたファイルを/etc/rhsm/rhsm.confに上書きする  
  主にプロキシの設定。放送と一般で違いがあるため注意。  

#### メイン処理
  - `linux_initial_setup_rhsm_conf_src`で指定されたファイルを/etc/rhsm/rhsm.confに上書きする  
  - 上書き前の/etc/rhsm/rhsm.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: rhsm.conf.14279.2024-08-19@11:01:59~
#### テスト
  - /etc/rhsm/rhsm.confが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/rhsm/rhsm.confの出力
</details>

<details>
  <summary>[2. Setting dnf] dnfの設定</summary>  
  
  マイナーバージョンまでOSバージョンを固定する  
  dnf.confにてプロキシの設定をする。(放送と一般で違いがあるため注意)  
  `linux_initial_setup_dnf_packages`で指定されたパッケージをインストールする  

#### メイン処理
  - `linux_initial_setup_dnf_releasever`を/etc/dnf/vars/releaseverへ設定しバージョンを固定
  - Proxy設定を/etc/dnf/dnf.confに追加する
  - /etc/dnf/dnf.confに変更が加わる場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: `dnf.conf.4734.2024-08-28@20:16:59~`
  - dnfパッケージのインストール
  - kernelとredhat-releaseから始まるパッケージのインストール除外設定を/etc/dnf/dnf.confに追加する
  - /etc/dnf/dnf.confに変更が加わる場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: `dnf.conf.4734.2024-08-28@20:16:59~`
#### テスト
  - /etc/dnf/vars/releaseverのバージョンが設定どおりであることを確認
  - /etc/dnf/dnf.confが送信元ファイルと差分がないことを確認
  - インストールされたdnfパッケージの確認
#### 状態出力
  - /etc/dnf/dnf.confの出力
  - /etc/dnf/vars/releaseverの出力
  - インストールされているパッケージの出力
</details>  

<details>
  <summary>[3. Setting locale] ロケール設定 </summary>  
  
  timezoneを`linux_initial_setup_timezone`で指定された値に設定する  
  LANG値を`linux_initial_setup_locale`で指定された値に設定する
  Keymap値を`linux_initial_setup_keymap`で指定された値に設定する 
  Layout値を`linux_initial_setup_x11_keymap`で指定された値に設定する  
#### メイン処理
  - timezoneの設定  
  - LANGの設定
  - Keymapの設定
  - X11 Layoutの設定
#### テスト
  - timezoneの確認
  - LANG,Keymap,X11 Layoutの確認
#### 状態出力
  - timezoneの出力
  - localeの出力
</details>  

<details>
  <summary>[4. Setting kdump] kdump設定</summary>  
  
  メモリスワップのパーティションのUUIDをハイバネートのresumeの値に設定する(RHEL9系のみ)
   `linux_initial_setup_kdump_conf_src`で指定されたファイルを/etc/kdump.confに上書きする

#### メイン処理
  - swapパーティションのUUIDを取得し、/etc/default/grubのGRUB_CMDLINE_LINUXのresumeの値に追加する(RHEL9系のみ)
  - grubの設定をUEFI用として反映(RHEL9系のみ)
  - 設定有効化のためにサーバー再起動 (/etc/default/grubに変更が生じた時のみ)
  - `linux_initial_setup_kdump_conf_src`で指定されたファイルをetc/kdump.confに上書きする  
  - 上書き前の/etc/kdump.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: kdump.conf.14279.2024-08-19@11:01:59~
  - kdumpサービスの再起動
  - kdumpサービス再起動が失敗した場合は、/etc/kdump.confをバックアップから戻して、kdumpを起動
#### テスト
  - /etc/default/grubにresumeとしてメモリスワップのUUIDが指定されていることを確認
  - /etc/kdump.confが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/default/grubの出力
  - /etc/kdump.confの出力
  - kdumpサービス起動状況の出力
</details>

<details>
  <summary>[5. Setting services to enabled_disabled] サービス自動起動設定</summary>  

  `linux_initial_setup_enabled_services`のリストにあるサービスを自動起動に設定する  
  `linux_initial_setup_disabled_services`のリストにあるサービスを自動起動しないように設定する  

#### メイン処理
  - 対象サービス(`linux_initial_setup_enabled_services`)の自動起動設定  
  - 対象サービス(`linux_initial_setup_disabled_services`)の自動起動解除設定  
#### テスト
  - 自動起動設定サービスの確認
  - 自動起動解除設定サービスの確認
#### 状態出力
  - 自動起動設定サービスの出力
  - 自動起動解除設定サービスの出力
  - サービス全体の状態出力
</details>  

<details>
  <summary>[6. Setting kernel] カーネル設定</summary>  

  `linux_initial_setup_kernel_conf_src`で指定されたファイルを/etc/sysctl.confに上書きする

#### メイン処理
  - `linux_initial_setup_kernel_conf_src`で指定されたファイルを/etc/sysctl.confに上書きする  
  - 上書き前の/etc/sysctl.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: sysctl.conf.51706.2024-08-29@05:22:05~
  - sysctl -pコマンドにて設定を反映 (ファイル更新があった場合のみ)
  - 上記コマンドが失敗した場合は/etc/sysctl.confをバックアップから戻して、再反映
#### テスト
  - /etc/sysctl.confが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/sysctl.confの出力
</details>  

<details>
  <summary>[7. Setting Ctrl_Alt_Delete Key] Ctrl_Alt_Deleteキーの無効化</summary>  

  Ctrl_Alt_Deleteキーを無効化する  

#### メイン処理
  - ctrl-alt-del.targetを停止する 
  - ctrl-alt-del.targetをdisabledにする
#### テスト
  - ctrl-alt-del.targetが停止状態であることを確認
  - ctrl-alt-del.targetがmasked状態であることを確認
#### 状態出力
  - ctrl-alt-del.targetサービスのステータスを出力  

</details>  

<details>
  <summary>[8. Setting systemd] systemd設定</summary>  
  
  `linux_initial_setup_system_conf_src`で指定されたファイルを/etc/systemd/system.confに上書きする

#### メイン処理
  - `linux_initial_setup_system_conf_src`で指定されたファイルを/etc/systemd/system.confに上書きする  
  - 上書き前の/etc/systemd/system.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: system.conf.14279.2024-08-19@11:01:59~
  - ファイルの更新があった場合はHandlerのReboot serverを呼び出す
#### テスト
  - /etc/systemd/system.confが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/systemd/system.confの出力
</details>

<details>
  <summary>[9. Setting hostname] ホスト名設定</summary>  

  ホスト名を`linux_initial_setup_hostname`で指定された値に設定する  
#### メイン処理
  - 現行ホスト名の出力
  - ホスト名の設定
#### テスト
  - ホスト名の確認
#### 状態出力
  - ホスト名の出力
  - /etc/hostnameの出力
</details>

<details>
  <summary>[10. Setting sshd] SSH設定</summary>  
  
  `linux_initial_setup_sshd_conf_src`で指定されたファイルを/etc/ssh/sshd_configに上書きする

#### メイン処理
  - `linux_initial_setup_sshd_conf_src`で指定されたファイルを/etc/ssh/sshd_configに上書きする  
  - 上書き前の/etc/ssh/sshd_configとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: sshd_config.14279.2024-08-19@11:01:59~
  -   - ファイルの更新があった場合はHandlerのRestart sshdを呼び出す
#### テスト
  - /etc/ssh/sshd_configが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/ssh/sshd_configの出力
</details>

<details>
  <summary>[11. Setting rsyslog rsyslog設定</summary>  
  
  `linux_initial_setup_rsyslog_conf_src`で指定されたファイルを/etc/rsyslog.confに上書きする  
  `linux_initial_setup_journald_conf_src`で指定されたファイルを/etc/systemd/journald.confに上書きする  

#### メイン処理
  - `linux_initial_setup_rsyslog_conf_src`で指定されたファイルを/etc/rsyslog.confに上書きする
  - 上書き前の/etc/rsyslog.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: rsyslog.conf.14279.2024-08-19@11:01:59~
  - ファイルの更新があった場合はHandlerのRestart rsyslogを呼び出す
  - `linux_initial_setup_journald_conf_src`で指定されたファイルを/etc/systemd/journald.confに上書きする  
  - 上書き前の/etc/systemd/journald.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: journald.conf.63304.2024-08-29@05:30:39~
  - systemd-journaldを再起動して設定を反映 (ファイル更新があった場合のみ)
  - 再起動が失敗した場合は/etc/systemd/journald.confをバックアップから戻して、再反映
#### テスト
  - /etc/rsyslog.confが送信元ファイルと差分がないことを確認
  - /etc/systemd/journald.confが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/rsyslog.confの出力
  - /etc/systemd/journald.confの出力
</details>

<details>
  <summary>[12. Setting firewalld] ファイアウォール設定 </summary>  

  複数のzone毎にインターフェース割当て、サービスの許可、不許可設定をする  

#### メイン処理
  - firewalldサービスを起動する
  - 現行のfirewall設定を出力する
  - `linux_initial_setup_firewall_param`の各`zone`に指定されたzoneを有効化する  
  - `linux_initial_setup_firewall_param`の各`interfaces`に指定された値を各zoneに割り当てる  
  - `linux_initial_setup_firewall_param`の各`add_services`に指定されたサービスを各zoneに追加する
  - `linux_initial_setup_firewall_param`の各`remove_services`に指定されたサービスを各zoneから削除する
#### テスト
  - firewall-cmd --list-allの出力項目をzone毎にチェックし、意図通りのファイアウォール設定になっていることを確認
#### 状態出力
  - firewall-cmd --list-allの結果を出力
</details>  

<details>
  <summary>[13. Setting logrotate] ログローテート設定</summary>  

  `linux_initial_setup_logrotate_conf_src`で指定されたファイルを/etc/logrotate.confに上書きする   
  `linux_initial_setup_logrotate_d_conf_src_list`で指定されたファイル群を/etc/logrotate.d/配下に転送上書きする  

#### メイン処理
  - `linux_initial_setup_logrotate_conf_src`で指定されたファイルを/etc/logrotate.confに上書きする   
  - 上書き前の/etc/logrotate.confとの差分がある場合は同一ディレクトリにバックアップを取得する  
    - バックアップファイル名例: logrotate.conf.74825.2024-08-29@05:32:33~  
  - `linux_initial_setup_logrotate_d_conf_src_list`のファイル群の中でファイル更新が入るファイルが一つでもあれば、/etc/logrotate.d/ディレクトリをバックアップする(/etc/logrotate.d_yyyymmddHHMISS)  
    - バックアップパス例:  /etc/logrotate.d_20240829053244  
  - `linux_initial_setup_logrotate_d_conf_src_list`のファイル群を/etc/logrotate.d/配下に転送上書きする
  - バリデーションチェックを実施し、エラーの場合はバックアップディレクトリから/etc/logrotate.d/ディレクトリごと戻す  
#### テスト
  - /etc/logrotate.confが送信元ファイルと差分がないことを確認
  - /etc/logrotate.d/配下のファイル群が送信元ファイル群と差分がないことを確認

#### 状態出力
  - /etc/logrotate.confの出力
  - /etc/logrotate.d/配下のファイル群を全て出力
</details>  

<details>
  <summary>[14. Setting crontab] cron設定</summary>  
  
  `linux_initial_setup_crontab_src`で指定されたファイルを/etc/crontabに上書きする

#### メイン処理
  - `linux_initial_setup_crontab_src`で指定されたファイルを/etc/crontabに上書きする
  - 上書き前の/etc/crontabとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: crontab.79374.2024-08-29@05:32:59~
#### テスト
  - /etc/crontabが送信元ファイルと差分がないことを確認
#### 状態出力
  - /etc/crontabの出力
</details>

<details>
  <summary>[15. Setting vmtool] vmtool設定</summary>  
  
  `linux_initial_setup_vmware_tools_src`で指定されたファイルを/etc/vmware-tools/tools.confに上書きする

#### メイン処理
  - /etc/vmware-tools/を作成する
  - `linux_initial_setup_vmware_tools_src`で指定されたファイルを/etc/vmware-tools/tools.confに上書きする
  - 上書き前の/etc/crontabとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: tools.conf.79374.2024-08-29@05:32:59~
#### テスト
  - /vmware-tools/tools.confが送信元ファイルと差分がないことを確認
#### 状態出力
  - /vmware-tools/tools.confの出力
</details>

<details>
  <summary>[16. Setting dns] DNS設定</summary>  
  
  NetworkManagerによるresolv.conf更新を無効化する  
  `linux_initial_setup_resolv_conf_src`で指定されたファイルを/etc/resolv.confに上書きする  

#### メイン処理
  - `linux_initial_setup_networkmanager_conf_src`で指定されたファイルを/etc/NetworkManager/conf.d/90-dns-none.confに転送する  
  - 新規転送の場合はHandlerのReload networkmanagerを呼び出す  
  - `linux_initial_setup_resolv_conf_src`で指定されたファイルを/etc/resolv.confに上書きする  
  - 上書き前の/etc/resolv.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: resolv.conf.80150.2024-08-29@05:33:04~
#### テスト
  - /etc/NetworkManager/conf.d/90-dns-none.confが送信元ファイルと差分がないことを確認  
  - /etc/resolv.confが送信元ファイルと差分がないことを確認  
#### 状態出力
  - /etc/NetworkManager/conf.d/90-dns-none.confの出力
  - /etc/resolv.confの出力
</details>

<details>
  <summary>[17. Setting chronyd] 時刻同期設定</summary>  
  
  `linux_initial_setup_chrony_conf_src`で指定されたファイルを/etc/chrony.confに上書きする  
  (ロケーションによりnameserverのIPアドレスが違うため注意)  
  chronydデーモンに特定のコマンドラインオプションを渡さないよう設定をする  

#### メイン処理
  - `linux_initial_setup_chrony_chronyd_src`で指定されたファイルを/etc/sysconfig/chronydに上書きする  
  - 上書き前の/etc/sysconfig/chronydとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: chronyd.80150.2024-08-29@05:33:04~
  - `linux_initial_setup_chrony_conf_src`で指定されたファイルを/etc/chrony.confに上書きする 
  - 上書き前のetc/chrony.confとの差分がある場合は同一ディレクトリにバックアップを取得する
    - バックアップファイル名例: chrony.conf.80371.2024-08-29@05:33:05~
  - /etc/sysconfig/chronydもしくは/etc/chrony.confのいずれかに変更があった場合はchronydを再起動して、設定を反映する  
  - 再起動に失敗した場合は、/etc/sysconfig/chronydおよび/etc/chrony.confをバックアップから戻して、chronydを再起動する
#### テスト
  - /etc/sysconfig/chronydが送信元ファイルと差分がないことを確認  
  - /etc/chrony.confが送信元ファイルと差分がないことを確認  
#### 状態出力
  - /etc/sysconfig/chronydの出力
  - /etc/chrony.confの出力
</details>

<details>
  <summary>[18. Setting users & groups] ユーザ/グループ設定</summary>  

  指定グループ・指定ユーザーの追加およびsudoersへの追加をする  

#### メイン処理
  - `linux_initial_setup_desired_groups`のリストにあるグループの作成
  - `linux_initial_setup_desired_users`のリストにあるユーザの作成  
  - `linux_initial_setup_desired_sudo_users`のリストにあるユーザーを/etc/sudoers.d/10-nws-configに追加する
    (設定はNOPASSで全てのコマンドをroot権限で実行可)  
#### テスト
  - `linux_initial_setup_desired_groups`のグループが期待通り存在していることを確認
  - `linux_initial_setup_desired_users`ユーザーが期待通り存在していることを確認
  - `linux_initial_setup_desired_sudo_users`のリストにあるユーザーが期待通りsudo設定されていることを確認  
#### 状態出力
  - idコマンド結果の出力
  - /etc/groupの出力
  - /etc/passwdの出力
  - /etc/sudoers.d/10-nws-configの出力
</details>  

<details>
  <summary>[19. Setting selinux] SELINUX設定</summary>  

  SELinuxの状態を設定する    

#### メイン処理
  - SELinuxの状態を`linux_initial_setup_selinux_state`の値に設定する
  - 設定変更があった場合はサーバーをリブートする  
    (即時状態反映をするため、handlersの利用はせずに即時リブート)
#### テスト
  - SELinuxの状態を確認
#### 状態出力
  - `getenforce`の結果を出力
  - /etc/selinux/configの出力
  - /etc/default/grubの出力
</details>  
<br>  


# 制限事項  
1. 以下のモジュール使用時はPythonインタープリタをデフォルトを使用している  
   - firewalld  
   - selinux  
   > 上記の動作のためのpythonライブラリに不足があるが、python3.10以上ではまだ対象のライブラリが存在しないため  
2. dnfパッケージのインストールについては、
   kernelおよびredhat-releaseから始まるパッケージのインストールを制限する設定を入れている。
   そのため、初回実行後は上記対象パッケージのアップデートやダウングレードおよび再インストールはできない。  
   一時的に制限を無効化する場合はlinux_initial_setup_dnf_excludesをfalseに設定して実行する。  
3. 出力機能(output_で始まるタグの利用)については、AAP上では改行コードの解釈ができない。  
   見やすくするためには、別途エディタ等で`\n`改行させて表示させる等の処理が必要。  

# 前提条件  
- 実行環境(EE):  
  | 管理対象ノードOS | 使用する実行環境(EE) |
  | ---------------- | -------------------- |
  | RHEL8系          | ansible_ee_nws_216   |
  | RHEL9系          | ansible_ee_nws_217   |

- 管理対象ノード  
  - VM初期払出/プロビジョニングが完了していること  
  - Python:
    - python3.11がインストールされていること
    - インタプリタをpython3.11に指定できること
  - SSHによる疎通ができること
  - 全てのネットワークインターフェース設定(zone設定を含む)は完了していること
  - OS: 
    - RedHat Enterprise Linux 8.10   
    - RedHat Enterprise Linux 9.2   
    - RedHat Enterprise Linux 9.4   
  - Redhatライセンスのアクティベーションができていること
  - sudoにてroot権限で全てのコマンド実行が可能なユーザーをansibe_userとできること  
    ※ansible_user: ansible組み込み変数でplaybookを実行するユーザー名  
- 実行条件:
  - becomeをtrueにすること
  - gather_factsをtrue(default)にすること


# 使用例
### Playbook
```
- hosts: servers
  become: true
  roles:
    - name: ansible_role_linux_initial_setup
      vars:
        linux_initial_setup_pattern_file: rhel92_info_ot.yml
```

### パラメタパターンファイル指定方法  
パラメタパターンファイルはカスタム作成も可能だが、基本的には本role内のvars/配下より選択して以下のいずれかの方法により指定する。  
1. Playbookに直接書き込み  
   上記使用例のように本roleのvars/配下より、組み込まれたパラメタパターンファイルを選択して指定する
2. ジョブテンプレートの外部変数として実行時に指定  
   ジョブテンプレートにて編集画面等より「変数」欄に以下を記載する。(Playbookのvars設定よりも優先される)
   ```
   linux_initial_setup_pattern_file: rhel89_east_it.yml
   ```

> *カスタムパラメタパターンファイル作成のヒント*  
> 基本：  
> カスタムパラメタパターンファイル指定用の変数`linux_initial_setup_custom_pattern_file`に作成したカスタムパラメタパターンファイルのパスを指定することで利用可能。  
> カスタムパラメタパターンファイルはvars/配下のいずれか近いものをコピーし、必要なパラメタを変更して作成する。
> 適用方法は上記[パラメタパターンファイル指定方法]と同様だが、変数は`linux_initial_setup_custom_pattern_file`を利用し、値はファイルパスを指定する。  

### tag利用例
1. 全ての処理を実行
```
$ ansible-playbook playbook.yml
$ ansible-playbook playbook.yml -t check_all
$ ansible-playbook playbook.yml -t output_all
```

2. kernelとdnfの処理のみ実行(テスト系および出力系のタスクも実行される)  
```
$ ansible-playbook playbook.yml -t "kernel,dnf"
```

3. hostname処理のテストのみ実行
```
$ ansible-playbook playbook.yml -t check_hostname
```

4. dnf処理の状態出力のみ実行
```
$ ansible-playbook playbook.yml -t output_dnf
```

5. 2回目以降の実行でdnf.ymlを動作させる(dnf.confのexcludepkgsを無効化する)
```
$ ansible-playbook playbook.yml -t dnf -e linux_initial_setup_dnf_excludes=false
```

Author Information
------------------

This role was created by Hidekazu Iijima.
