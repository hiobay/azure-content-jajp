<properties 
	pageTitle="Azure でのアプリのスケールアップ" 
	description="Azure App Service のアプリをスケールアップして容量と機能を追加する方法について説明します。" 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="mollybos"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/05/2016" 
	ms.author="cephalin"/>

# Azure でのアプリのスケールアップ #

この記事では、Azure App Service でアプリのスケールを変更する方法について説明します。スケール変更には 2 つのワークフローがありますが、この記事では一方のワークフロー (スケールアップ) について取り上げます。

- [スケールアップ](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): CPU、メモリ、ディスク領域を増やしたり、専用 VM、カスタム ドメインと証明書、ステージング スロット、自動スケールのような拡張機能を追加したりします。スケールアウトするには、アプリが属している App Service プランの価格レベルを変更します。
- [スケールアウト](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): アプリを実行する VM インスタンス数を増やします。価格レベルに応じて、20 個までのインスタンスにスケールアウトすることができます。**Premium** レベルの [App Service Environment](../app-service/app-service-app-service-environments-readme.md) を使用すると、スケールアウト カウントが 50 インスタンスに増えます。スケールアウトの詳細については、「[手動または自動によるインスタンス数のスケール変更](../azure-portal/insights-how-to-scale.md)」を参照してください。あらかじめ定義されている規則とスケジュールに基づいてインスタンス数を自動的に変更する自動スケールの使い方が説明されています。

スケール設定は適用に数秒を要するのみで、[App Service プラン](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)に含まれるすべてのアプリに反映されます。コードを変更したりアプリケーションを再デプロイしたりする必要はありません。

App Service の個々のプランの価格と機能の詳細については、[App Service の価格の詳細](/pricing/details/web-sites/)に関するページを参照してください。

> [AZURE.NOTE] App Service プランを **Free** レベルから切り替える前にまず、ご利用の Azure サブスクリプションに設定されている[使用制限](/pricing/spending-limits/)を削除する必要があります。Microsoft Azure App Service サブスクリプションのオプションを表示または変更するには、「[Microsoft Azure サブスクリプション][azuresubscriptions]」を参照してください。

<a name="scalingsharedorbasic"></a> <a name="scalingstandard"></a>

## 価格レベルのスケールアップ

1. ブラウザーで、[Azure ポータル][portal]を開きます。
	
2. ご使用のアプリのブレードで、**[すべての設定]**、**[スケールアップする]** の順にクリックします。
	
	![Navigate to scale up your Azure app.][ChooseWHP]
	
4. スクロールしてレベルを選び、**[選択]** をクリックします。

	操作が完了すると、**[通知]** タブに緑色で "**成功**" が点滅します。
	
<a name="ScalingSQLServer"></a>
##スケーリングに関連するリソース
Azure SQL Database や Azure Storage などの他のサービスにアプリが依存している場合は、これらのリソースもニーズに合わせてスケールアップできます。これらのリソースのスケールは、App Service プランでは変更されません。個別に変更する必要があります。

1. **[Essentials]** で、**[リソース グループ]** リンクをクリックします。

	![Scale up your Azure app's related resources](./media/web-sites-scale/RGEssentialsLink.png)

2. 次に、リソース グループ ブレードの **[概要]** パートで、スケールするいずれかのリソースをクリックします。以下のスクリーンショットは、SQL Database リソースと Azure Storage リソースを示しています。

	![Navigate to resource group blade to scale up your Azure app](./media/web-sites-scale/ResourceGroup.png)

3. SQL Database リソースの場合は、**[設定]**、**[価格レベル]** の順にクリックして価格レベルを変更します。

	![Scale up the SQL Database backend for your Azure app](./media/web-sites-scale/ScaleDatabase.png)

	SQL Database の場合は [[geo レプリケーション]](../sql-database/sql-database-geo-replication-overview.md) を有効にすることもできます。

    Azure Storage リソースの場合は、**[設定]**、**[構成]** の順にクリックしてストレージ オプションをスケールアップします。

    ![Scale up the Azure Storage account used by your Azure app](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>
## 開発者機能
価格レベルに応じて、次の開発者向け機能を使用できます。

### ビット ###

- **Basic**、**Standard**、および **Premium** レベルでは、64 ビットおよび 32 ビットのアプリケーションがサポートされます。
- **Free** プラン レベルと **Shared** プラン レベルでは、32 ビットのアプリケーションのみがサポートされます。

### デバッガー サポート ###

- **Free**、**Shared**、および **Basic** モードでは、デバッガー サポートを利用する場合、App Service プランあたりの同時接続数は 1 です。
- **Standard** および **Premium** モードでは、デバッガー サポートを利用する場合、App Service プランあたりの同時接続数は 5 です。

<a name="OtherFeatures"></a>
## その他の機能

- すべてのユーザー (開発者を含む) が関心を持つ料金や機能など、App Service プランのその他すべての機能の詳細については、[App Service の料金の詳細](/pricing/details/web-sites/)に関するページを参照してください。

>[AZURE.NOTE] Azure アカウントにサインアップする前に Azure App Service の使用を開始する場合は、[App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751)に関するページにアクセスしてください。App Service で有効期間の短いスターター Web アプリをすぐに作成できます。このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<a name="Next Steps"></a>
## 次のステップ

- Azure を利用し始めるには、「[Microsoft Azure の無料評価版サイト](/pricing/free-trial/)」を参照してください。
- 料金、サポート、および SLA については、次のリンクを参照してください。

	[データ転送の料金詳細](/pricing/details/data-transfers/)

	[Microsoft Azure サポート プラン](/support/plans/)

	[サービス レベル アグリーメント](/support/legal/sla/)

	[SQL Database の料金詳細](/pricing/details/sql-database/)

	[Microsoft Azure の仮想マシンおよびクラウド サービスのサイズ][vmsizes]

	[App Service の料金の詳細](/pricing/details/app-service/)

	[App Service の料金の詳細 - SSL 接続](/pricing/details/web-sites/#ssl-connections)

- 拡張性と回復力に優れたアーキテクチャの構築など、Azure App Service のベスト プラクティスについては、「[Azure App Service Web Apps のベスト プラクティス](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx)」を参照してください。

- App Service アプリの拡張に関するビデオ:

	- [Azure Websites のスケールを設定するタイミング - Stefan Schackow 共演](/documentation/videos/azure-web-sites-free-vs-standard-scaling/)
	- [Azure Websites、CPU、またはスケジュールの自動スケール - Stefan Schackow 共演](/documentation/videos/auto-scaling-azure-web-sites/)
	- [Azure Websites のスケールを設定する方法 - Stefan Schackow 共演](/documentation/videos/how-azure-web-sites-scale/)


<!-- LINKS -->
[vmsizes]: /pricing/details/app-service/
[SQLaccountsbilling]: http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]: http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
 

<!---HONumber=AcomDC_0706_2016-->