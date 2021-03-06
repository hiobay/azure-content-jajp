<properties
	pageTitle="Azure Active Directory ドメイン サービス プレビュー: 管理ガイド | Microsoft Azure"
	description="Red Hat Enterprise Linux 仮想マシンのAzure AD ドメイン サービスへの参加"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="maheshu"/>

# Red Hat Enterprise Linux 7 仮想マシンの管理対象ドメインへの参加
この記事では、Red Hat Enterprise Linux (RHEL) 7 仮想マシンを Azure AD ドメイン サービスの管理対象ドメインに参加させる方法について説明します。

## Red Hat Enterprise Linux 仮想マシンのプロビジョニング
Azure ポータルを使用して RHEL 7 仮想マシンをプロビジョニングするには、次の手順を実行します。

1. [Azure ポータル](https://portal.azure.com)にサインインします。

    ![Azure ポータル ダッシュボード](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. 次のスクリーンショットのように、左側のウィンドウの**[新規]**をクリックして、検索バーに**「Red Hat」**と入力します。検索結果に Red Hat Enterprise Linux の項目が表示されます。**Red Hat Enterprise Linux 7.2** をクリックします。

    ![検索結果から RHEL を選択](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. **[すべて]** ウインドウの検索結果に、Red Hat Enterprise Linux 7.2 のイメージが表示されます。**[Red Hat Enterprise Linux 7.2]** をクリックして、仮想マシン イメージの詳細情報を表示します。

    ![検索結果から RHEL を選択](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. **[Red Hat Enterprise Linux 7.2]** ウインドウに、仮想マシン イメージの詳細情報が表示されます。**[デプロイ モデルの選択]** ボックスの一覧で **[クラシック]** を選択します。次に **[作成]** ボタンをクリックします。

    ![イメージの詳細を表示](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. **[VM の作成]** ウインドウで、新しい仮想マシンの**ホスト名**を入力します。ローカル管理者のユーザー名を **[ユーザー名]** フィールドで指定し、**パスワード**を入力します。ローカル管理者ユーザーの認証に SSH キーを使用することもできます。仮想マシンの**価格レベル**を選択します。

    ![VM の作成 - 基本情報](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. **[オプションの構成]** をクリックします。**[オプションの構成]** ウインドウが開きます。次のスクリーンショットのように、**[オプションの構成]** ウインドウの **[ネットワーク]** をクリックします。

    ![VM の作成 - 仮想ネットワークの構成](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. **[ネットワーク]** ウインドウが開きます。**[ネットワーク]** ウインドウの **[仮想ネットワーク]** をクリックして、Linux VM をデプロイする仮想ネットワークを選択します。**[仮想ネットワーク]** ウインドウが開きます。**[仮想ネットワーク]** ウインドウで、**[既存の仮想ネットワークの使用]** オプションを選択します。Azure AD ドメイン サービスを利用できる仮想ネットワークを選択します。この例では、”MyPreviewVNet” 仮想ネットワークを選択しています。

    ![VM の作成 - 仮想ネットワークの選択](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. **[オプションの構成]** ウインドウで、**[OK]** ボタンをクリックします。

    ![VM の作成 - 仮想ネットワークが選択されました](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. これで、仮想マシンを作成する準備が整いました。**[VM の作成]** ウインドウで、**[作成]** ボタンをクリックします。

    ![VM の作成 - 準備完了](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. RHEL 7.2 のイメージに基づき、新しい仮想マシンのデプロイが開始されます。

  ![VM の作成 - デプロイが開始されました](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. 数分後に仮想マシンのデプロイが完了し、使用する準備が整います。

  ![VM の作成 - 展開済み](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## 新しくプロビジョニングされた Linux 仮想マシンへのリモート接続
RHEL 7.2 仮想マシンの Azure でのプロビジョニングが完了しました。続いて、仮想マシンへのリモート接続を行います。

**RHEL 7.2 仮想マシンへの接続** 「[Linux を実行する仮想マシンにログオンする方法](../virtual-machines/virtual-machines-linux-classic-log-on.md)」の記事に記載の手順に従ってください。

次の手順では、SSH クライアントである PuTTY を使用して、RHEL 仮想マシンに接続することを想定しています。詳細については、「[PuTTY Download Page (PuTTY のダウンロード ページ)](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)」を参照してください。

1. PuTTY プログラムを開きます。

2. 新たに作成された RHEL 仮想マシンの**ホスト名**を入力します。 この例では、仮想マシンのホスト名は ”contoso rhel.cloudapp .net” です。仮想マシンのホスト名が不明な場合は、Azure ポータルの VM ダッシュボードを参照してください。

    ![PuTTY 接続](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. 仮想マシンの作成時に指定したローカル管理者の資格情報を使用して、仮想マシンにログオンします。この例では、ローカル管理者アカウント ”mahesh” を使用しました。

    ![PuTTY ログイン](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## Linux 仮想マシンに必要なパッケージのインストール
仮想マシンへの接続が完了したら、ドメイン参加に必要なパッケージを仮想マシンにインストールします。次の手順に従います。

1. **realmd のインストール:** ドメイン参加には realmd パッケージを使用します。PuTTY ターミナルで、次のコマンドを入力します。

    sudo yum install realmd

    ![realmd のインストール](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    数分後に realmd パッケージが仮想マシンにインストールされます。

    ![realmd がインストールされました](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **sssd のインストール:** realmd パッケージは、ドメイン参加操作を実行するために sssd に依存します。PuTTY ターミナルで、次のコマンドを入力します。

    sudo yum install sssd

	![sssd のインストール](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    数分後に sssd パッケージが仮想マシンにインストールされます。

    ![realmd がインストールされました](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **kerberos のインストール:** PuTTY ターミナルで、次のコマンドを入力します。

    sudo yum install krb5-workstation krb5-libs

	![kerberos のインストール](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

	数分後に realmd パッケージが仮想マシンにインストールされます。

	![Kerberos がインストールされました](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## Linux 仮想マシンの管理対象ドメインへの参加
Linux 仮想マシンに必要なパッケージがインストールされたら、続いて仮想マシンを管理対象ドメインに参加させます。

1. AAD ドメイン サービスの管理対象ドメインを探します。PuTTY ターミナルで、次のコマンドを入力します。

    sudo realm discover CONTOSO100.COM

	![領域の検出](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

	**「realm discover」**で管理対象ドメインが見つからない場合は、ドメインが仮想マシンから到達可能か確認します (ping をお試しください)。また、仮想マシンが、管理対象ドメインが利用可能な同じ仮想ネットワークにデプロイされているかも確認します。

2. kerberos を初期化します。PuTTY ターミナルで、次のコマンドを入力します。”AAD DC 管理者” グループに所属するユーザーを指定します。指定されたユーザーのみが、管理対象ドメインにコンピューターを参加させることができます。

    kinit bob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    kinit のエラーを防ぐため、ドメイン名は必ず大文字で指定します。

3. コンピューターをドメインに参加させます。PuTTY ターミナルで、次のコマンドを入力します。上記の手順で指定したのと同じユーザー (”kinit”) を指定します。

    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'

	![領域の結合](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

コンピューターの管理対象ドメインへの参加が完了すると、「Successfully enrolled machine in realm (コンピューターは領域に正常に登録されました)」という旨のメッセージが表示されます。


## ドメイン参加の確認
コンピューターが管理対象ドメインに参加できたか確認してみましょう。そのためには、ドメインに新しく参加した RHEL VM に、ssh とドメイン ユーザー アカウントを使用して接続し、 ユーザー アカウントが正しく解決済みかを確認します。

1. PuTTY ターミナルで次のコマンドを入力し、ドメインに新しく参加した RHEL 仮想マシンに、SSH を使用して接続します。管理アカウントに所属するドメイン アカウントを使用します (ここでは ”bob@CONTOSO100.COM”)。

    ssh-l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. PuTTY ターミナルで次のコマンドを入力し、ユーザーのホーム ディレクトリが正しく初期化されているかを確認します。

	pwd

3. PuTTY ターミナルで次のコマンドを入力し、ユーザーの指定したグループ メンバーが正しく解決済みかを確認します。

    id

コマンドのサンプル出力は次のとおりです。

![ドメイン参加の確認](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## ドメイン参加のトラブルシューティング
「[Troubleshooting domain join (ドメイン参加のトラブルシューティング)](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join)」を参照してください。


## 詳細情報
- [Linux が実行されている仮想マシンにログオンする方法](../virtual-machines/virtual-machines-linux-classic-log-on.md)。
- [Installing Kerberos (Kerberos のインストール)](https://access.redhat.com/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
- [Red Hat Enterprise Linux 7 - Windows Integration Guide (Red Hat Enterprise Linux 7 - Windows 統合ガイド)](https://access.redhat.com/documentation/ja-JP/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)

<!---HONumber=AcomDC_0706_2016-->