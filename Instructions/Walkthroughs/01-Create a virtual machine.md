﻿---
wts:
    title: '01 - ポータルで仮想マシンを作成します'
    module: 'モジュール 02 - コア Azure サービス'
---
# 01 - ポータルで仮想マシンを作成します

このチュートリアルでは、Azure Portal で仮想マシンを作成し、仮想マシンに接続し、Web サーバー ロールをインストールしてテストします。 

推定時間: 45 分

**注記**: このチュートリアルでは、情報アイコンをクリックして読む時間を設けてください。 

# タスク 1: 仮想マシンを作成する

このタスクでは、Windows Server 2016 Datacenter 仮想マシンを作成します。 

1. [Azure Portal](https://portal.azure.com) にサインインします。

2. **仮想マシン** を検索し、[**追加**] をクリックします。

3. [**基本**] タブで、情報を入力します。それ以外の場合はデフォルトのままにします。

	|設定 | 値 |
	|---|---|
	|サブスクリプション | **サブスクリプションを選択する**|
	|リソース グループ | **myRGVM** (新規作成) |
	|仮想マシン名 | **myVm** |
	|場所 | **(US) 米国東部**|
	|イメージ | **Windows サーバー 2016 データセンター**|
	|管理者アカウントのユーザー名 | **azureuser** |
	|管理者アカウントのパスワード | **Pa$$w0rd1234**|
	|受信ポートの規則 - 選択ポートを許可する | **RDP (3389)** と **HTTP (80)**|
	|||

4. [管理] タブと [**監視**] セクションに移動します。

	|設定 | 値 |
	|---|---|
	|ブート診断 | **Off**|
	|||

5. 残りの既定値をそのままにして、ページの下部にある [**確認および作成**] ボタンを選択します。

6. 検証に成功したら、[**作成**] ボタンをクリックします。仮想マシンをデプロイするには、5?7分かかります。

7. 展開ページと [**通知**] アイコン (トップ メニュー) に更新を受信します。

# タスク 2: 仮想マシンに接続する

このタスクでは、RDP を使用して新しい仮想マシンに接続します。 

1. **myVM** を検索し、新しい仮想マシンを選択します。

**注記**: [**通知**] の [**リソースに移動**] リンクを使用することもできます。 

2. 仮想マシンの [**概要**] ページで、[**接続**] ボタンをクリック します。

    ![[接続] ボタンが強調表示された仮想マシンのプロパティのスクリーンショット。](../images/0101.png)

    **注記**: 次の指示では、Windows コンピューターから VM に接続する方法を説明しています。Mac では、Mac App Store からのリモート デスクトップ クライアントなどの RDP クライアントが必要であり、Linux 仮想マシンでは `ssh` を使用して bash シェルから直接接続できます。

2. [**仮想マシンに接続**] ページで、ポート 3389 経由で Public IP Addresses によって接続する既定のオプションを保持し、[**RDP ファイルのダウンロード**] をクリックします。

3. ダウンロードした RDP ファイルを **開き**、指示されたら [**接続**] をクリックします。 

    ![[接続] ボタンが強調表示された仮想マシンのプロパティのスクリーンショット。 ](../images/0102.png)

4. [**Windows セキュリティ**] ウィンドウで [**その他**] を選択し、[**別のアカウントを使用する**] を選択します。ユーザー名 (.\azureuser) とパスワード (Pa$$w0rd1234) を提供します。[**OK**] をクリックして接続します。

    ![別のアカウントを選択して使用した Windows セキュリティ ダイアログと、azure ユーザーが入力したユーザー名とパスワードのスクリーンショット。](../images/0103.png)


5. サインイン プロセス中に証明書の警告が表示されることがあります。[**はい**] をクリックするか、接続を作成してデプロイした VM に接続します。正常に接続されるはずです。

    ![「はい」ボタンが強調表示された、信頼できない証明書をユーザーに通知する証明書警告ダイアログのスクリーンショット。 ](../images/0104.png)

お疲れさまでした。Azure で Windows Server 仮想マシンをデプロイして接続しました

# タスク 3: Web サーバーの役割をインストールしてテストする

このタスクでは、サーバーに Web サーバーの役割をインストールし、既定の IIS ようこそページが表示されることを確認します。

1. 仮想マシンで PowerShell コマンド プロンプトを開くには、[**スタート**] ボタンをクリックし、[**PowerShell**] 」と入力し、メニューの [**Windows PowerShell**] を右クリックして、[**管理者として実行**] を選択します。

    ![[スタート] ボタンがクリックされ、管理者として実行された PowerShell が強調表示された仮想マシン デスクトップのスクリーンショット。](../images/0105.png)

2. PowerShell コマンド プロンプトで次のコマンドを実行して、仮想マシンに **Web-Server** 機能をインストールします。このコマンドをコピーして貼り付けることができます。

```PowerShell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```
  
3. 完了すると、値が **True** の **Success** を示すプロンプトが表示されます。インストールを完了するために仮想マシンを再起動する必要はありません。VM への RDP 接続を閉じます。

    ![コマンド Install-WindowsFeature -name Web-Server -IncludeManagementTools が正常に完了し、成功したことを示す出力がある Windows PowerShell コマンド プロンプトのスクリーンショット。](../images/0106.png)

4. ポータルに戻り、VM を選択し、VM の [**概要**] ウィンドウで、IP アドレスの右側にある [**コピーするにはクリックします**] ボタンを使用してコピーし、ブラウザー タブに貼り付けます。

    ![IP アドレスがコピーされた Azure Portal 仮想マシンのプロパティ ウィンドウのスクリーンショット。](../images/0107.png)

5. 既定の IIS Web Server ウェルカム ページが開き、この IP アドレスを使用して、または完全修飾ドメイン名を使用してパブリックに接続するために使用できます。

    ![Web ブラウザのパブリック IP アドレスを介してアクセスされるデフォルトの IIS Web サーバーのウェルカム ページのスクリーンショット。](../images/0108.png)

お疲れさまでした。この IP アドレスを使用して、または完全修飾ドメイン名を使用して、パブリックに接続できる Web サーバーを作成しました。ホストする Web ページがある場合、それらのソース ファイルを仮想マシンにデプロイし、デプロイされた仮想マシンでパブリック アクセス用にそれらをホストできます。


**注記**: 追加コストを回避するには、このリソース グループを削除します。リソース グループを検索し、リソース グループをクリックして、[**リソース グループの削除**] をクリックします。リソース グループの名前を確認し、[**削除**] をクリックします。**通知** を監視して、削除の進行状況を確認します。
