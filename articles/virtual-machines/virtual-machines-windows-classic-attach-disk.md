<properties
	pageTitle="ディスクを VM にアタッチする | Microsoft Azure"
	description="クラシック デプロイ モデルを使用して作成された Windows 仮想マシンにデータ ディスクをアタッチし、初期化します。"
	services="virtual-machines-windows, storage"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="cynthn"/>

# クラシック デプロイ モデルを使用して作成された Windows 仮想マシンにデータ ディスクをアタッチする

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [Resource Manager モデルを使用してこれらの手順を実行する](virtual-machines-windows-attach-disk-portal.md)方法について説明します。

データ ディスクを追加する必要がある場合は、空のディスクまたはデータを含む既存のディスクを仮想マシンにアタッチできます。どちらの場合も、ディスクは Azure ストレージ アカウントにある .vhd ファイルです。新しいディスクの場合は、ディスクをアタッチした後で、Windows VM で使用できるように初期化する必要もあります。

ディスクの詳細については、[About Disks and VHDs for Virtual Machines (Virtual Machines 用のディスクと VHD について)](virtual-machines-windows-about-disks-vhds.md) を参照してください。

## ビデオ チュートリアル

[こちら](https://azure.microsoft.com/documentation/videos/attaching-a-data-disk-to-a-windows-vm/)で、このチュートリアルのビデオをご覧いただけます。


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## ディスクの初期化

1. 仮想マシンへの接続詳細については、「[Windows Server が実行されている仮想マシンにログオンする方法][logon]」を参照してください。

2. 仮想マシンにログオンした後、**サーバー マネージャー**を開きます。左側のウィンドウで、**[ファイル サービスと記憶域サービス]** を選択します。

	![サーバー マネージャーを開く](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. メニューを展開し、**[ディスク]** を選択します。

4. **[ディスク]** セクションにディスクが一覧表示されます。普通、disk 0、disk 1、disk 2 といった名前です。disk 0 はオペレーティング システム ディスク、disk 1 は一時ディスク、disk 2 は VM にアタッチしたデータ ディスクです。新しいデータ ディスクのパーティションは **[不明]** と表示されます。ディスクを右クリックし、**[初期化]** を選択します。

5.	ディスクの初期化時にすべてのデータが消去されることが通知されます。**[はい]** をクリックして警告を了解し、ディスクを初期化します。完了すると、パーティションは **[GPT]** と表示されます。もう一度ディスクを右クリックし、**[新しいボリューム]** を選択します。

6.	既定の値を使用してウィザードを完了します。ウィザードが終了すると、**[ボリューム]** セクションに新しいボリュームが表示されます。ディスクがオンラインになり、データを格納できるようになります。

	![ボリュームの初期化に成功](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] VM のサイズによって、アタッチできるディスクの数が決まります。詳細については、「[仮想マシンのサイズ](virtual-machines-linux-sizes.md)」を参照してください。

## その他のリソース

[Windows 仮想マシンからディスクを切断する方法](virtual-machines-windows-classic-detach-disk.md)

[仮想マシン用のディスクと VHD について](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md

<!---HONumber=AcomDC_0629_2016-->