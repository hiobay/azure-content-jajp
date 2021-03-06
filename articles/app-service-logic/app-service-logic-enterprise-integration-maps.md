<properties 
	pageTitle="マップと Enterprise Integration Pack の概要 | Microsoft Azure App Service" 
	description="Enterprise Integration Pack と Logic Apps でマップを使用する方法について説明します。" 
	services="app-service\logic" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="app-service-logic" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/29/2016" 
	ms.author="deonhe"/>

# マップと Enterprise Integration Pack についての詳細情報

## 概要
Enterprise Integration では、ある形式から別の形式に XML データを変換するのにマップを使用します。

## マップとは
マップとは、ドキュメント内のどのデータを別の形式に変換するかを定義する XML ドキュメントです。

## マップを使用する理由
たとえば、B2B の注文または請求書を顧客から定期的に受け取るとします。顧客が使う日付の形式は YYYMMDD です。しかし自分の組織では、MMDDYYY の形式で日付を保存しています。マップを使用することで、注文または請求書の詳細を顧客活動データベースに保存する前に、日付の形式を YYYMMDD から MMDDYYY に*変換*できます。

## マップをアップロードする方法
Azure ポータルで次の手順を実行します。
1. **[参照]** を選択します。 ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. フィルター検索ボックスに「**integration**」と入力し、結果リストから **[Integration Accounts (統合アカウント)]** を選択します。 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. マップの追加先となる**統合アカウント**を選択します。 ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  **[Maps (マップ)]** タイルを選択します。 ![](./media/app-service-logic-enterprise-integration-maps/map-1.png)
5. 開いた [Maps (マップ)] ブレードの **[追加]** ボタンを選択します。 ![](./media/app-service-logic-enterprise-integration-maps/map-2.png)
6. マップの**名前**を入力し、**[マップ]** ボックスの右側にあるフォルダー アイコンを選択して、マップ ファイルをアップロードします。アップロード プロセスが完了したら、**[OK]** ボタンを選択します。 ![](./media/app-service-logic-enterprise-integration-maps/map-3.png)
7. 対象の統合アカウントにマップが追加されています。マップ ファイルの追加が成功または失敗したことを示す通知が画面に表示されます。通知の表示後、**[Maps (マップ)]** タイルを選択すると、[Maps (マップ)] ブレードで新しく追加されたマップを確認できます。 ![](./media/app-service-logic-enterprise-integration-maps/map-4.png)

## マップを編集する方法
マップを編集するには、必要な変更を施した新しいマップ ファイルをアップロードする必要があります。最初にマップをダウンロードして編集できます。

既存のマップに替わる新しいマップをアップロードするには、次の手順に従います。
1. **[Maps (マップ)]** タイルを選択します。
2. [Maps (マップ)] ブレードが開いたら、編集するマップを選択します。
3. **[Maps (マップ)]** ブレードの **[更新]** リンクを選択します。 ![](./media/app-service-logic-enterprise-integration-maps/edit-1.png)
4. 開いたファイル ピッカー ダイアログを使用して、アップロードするマップ ファイルを選択してから、ファイル ピッカーの **[開く]** を選択します。 ![](./media/app-service-logic-enterprise-integration-maps/edit-2.png)
5. マップがアップロードされると、通知のポップアップが表示されます。


## マップを削除する方法
1. **[Maps (マップ)]** タイルを選択します。
2. [Maps (マップ)] ブレードが開いたら、削除するマップを選択します。
3. **[削除]** リンクを選択します。 ![](./media/app-service-logic-enterprise-integration-maps/delete.png)
4. マップを本当に削除するか確認されます。 ![](./media/app-service-logic-enterprise-integration-maps/delete-confirmation-1.png)


## 次のステップ
- [契約についての詳細情報](./app-service-logic-enterprise-integration-agreements.md "Enterprise Integration の契約についての詳細情報")

<!---HONumber=AcomDC_0706_2016-->