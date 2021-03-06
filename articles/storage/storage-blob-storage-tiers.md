<properties
    pageTitle="BLOB 用 Azure クール ストレージ | Microsoft Azure"
    description="Azure Blob Storage のストレージ層では、アクセス パターンに基づいてオブジェクト データをコスト効率の高い方法で格納できるストレージを提供しています。クール ストレージ層は、アクセス頻度の低いデータ向けに最適化されています。"
    services="storage"
    documentationCenter=""
    authors="sribhat-msft"
    manager=""
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="sribhat"/>


# Azure Blob Storage: ホット ストレージ層とクール ストレージ層

## 概要

Azure Storage では現在、Blob Storage (オブジェクト ストレージ) 用に 2 つのストレージ層が提供されています。そのため、使い方しだいで、コスト効率の高い方法でデータを格納することができます。Azure **ホット ストレージ層**は、頻繁にアクセスされるデータの格納に適しています。Azure **クール ストレージ層**は、アクセスされる頻度は低いものの、保管期間が長いデータの格納に適しています。クール ストレージ層に格納されるデータについては、可用性が若干低くても許容できますが、高い持続性は必要で、アクセスにかかる時間とスループット特性もホット データと同程度である必要があります。クール データの場合、可用性の SLA が若干低く、アクセス コストが高めであっても、ストレージ コストが大幅に低ければ許容できます。

最近では、クラウドに格納されるデータが急激に増加しています。ストレージのニーズが拡大する中でコストを管理するには、アクセスの頻度や予定保有期間などの属性に基づいてデータを整理する方法が効果的です。クラウドに格納されるデータは、有効期間を通じてどのように生成、処理、およびアクセスされるかという点で、まったく異なる場合があります。有効期間を通じて活発にアクセスおよび変更されるデータもあれば、有効期間の初期に頻繁にアクセスされ、古くなるにつれて大幅にアクセスが減るデータもあります。また、クラウド内でアイドル状態のままとなり、格納されてからはほとんどアクセスされないデータもあります。

前の各データ アクセス シナリオは、特定のアクセス パターン用に最適化されたストレージの分化された層の利点を利用しています。Azure Blob Storage は現在、ホットとクールの各ストレージ層を導入し、分化されたストレージ層に対するニーズに異なる価格モデルで対応しています。

## BLOB ストレージ アカウント

**BLOB ストレージ アカウント**とは、Azure Storage に BLOB (オブジェクト) として非構造化データを格納するための特殊なストレージ アカウントです。BLOB ストレージ アカウントでは現在、ストレージ層としてホット ストレージまたはクール ストレージを選択できます。アクセス頻度が低いクール データは、より低いストレージ コストで格納し、アクセス頻度が高いホット データは、より低いアクセス コストで格納することができます。BLOB ストレージ アカウントは、既存の汎用ストレージ アカウントと同様で、現在使用されているすべての優れた耐久性、可用性、スケーラビリティ、およびパフォーマンス機能を共有します。たとえば、ブロック BLOB と追加 BLOB の 100% の API 整合性などです。

> [AZURE.NOTE] BLOB ストレージ アカウントは、ブロック BLOB と追加 BLOB のみをサポートします。ページ BLOB はサポートしません。

BLOB ストレージ アカウントは、**アクセス層**属性を公開します。この属性を使用すると、アカウントに格納されているデータに応じてストレージ層を**ホット**または**クール**として指定できます。データの使用パターンが変化した場合は、いつでもこれらのストレージ層を切り替えることができます。

> [AZURE.NOTE] ストレージ層を変更すると、追加料金が発生することがあります。詳細については、「[価格と課金](storage-blob-storage-tiers.md#pricing-and-billing)」セクションを参照してください。

ホット ストレージ層の使用シナリオの例には、次のようなものがあります。

- 活発に使用されたり、頻繁にアクセス (読み取りまたは書き込み) されると予想されるデータ。
- 処理段階にあり、最終的にはクール ストレージ層に移行されるデータ。

クール ストレージ層の使用シナリオの例には、次のようなものがあります。

- バックアップ、アーカイブ、および障害復旧のデータセット。
- 既に頻繁に参照されなくなっているものの、アクセスされたときにはすぐに利用できることが期待されている、古いメディア コンテンツ。
- 今後処理するためにさらにデータが収集されている場合に、コスト効率の高い方法で格納する必要がある、大規模なデータ セット (たとえば、長期保存する科学データや、製造施設からの未加工のテレメトリ データ)。
- 最終的に使用可能な形式に処理した後も保持する必要がある、元の (未加工の) データ (たとえば、他の形式にコード変換した後の未加工のメディア ファイル)。
- 長期間格納しておく必要があり、ほとんどアクセスされることがない、コンプライアンスおよびアーカイブ データ (たとえば、監視カメラ映像、医療機関の古い X 線/MRI 画像、金融サービスの顧客電話の音声録音や会話記録)。

ストレージ アカウントの詳細については、「[Azure ストレージ アカウントについて](storage-create-storage-account.md)」を参照してください。

ブロックまたは追加 Blob Storage だけを必要とするアプリケーションでは、階層化されたストレージの分化された料金モデルの利点を活用するために、BLOB ストレージ アカウントを使用することをお勧めします。ただし、状況によってはそのようにできず、汎用ストレージ アカウントを使用するしかない場合もあります。たとえば、次のような場合です。

- テーブル、キュー、またはファイルを使用する必要があり、BLOB を同じストレージ アカウントに格納したい。同じ共有キーを持つこと以外に、これらを同じアカウント内に格納する技術的な利点はないことに注意してください。
- まだクラシック デプロイ モデルを使用する必要がある。BLOB ストレージ アカウントは、Azure Resource Manager デプロイメント モデルだけで利用できます。
- ページ BLOB を使用する必要がある。BLOB ストレージ アカウントはページ BLOB をサポートしていません。一般に、ページ BLOB が必要な特別な理由がなければ、ブロック BLOB を使用することをお勧めします。
- [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) の 2014-02-14 より前のバージョンか、クライアント ライブラリの 4.x より前のバージョンを使用していて、アプリケーションをアップグレードできない。

> [AZURE.NOTE] BLOB ストレージ アカウントは、現在、ほとんどの Azure リージョンでサポートされており、残りのリージョンでも順次サポートされていきます。利用できるリージョンの更新された一覧については、「[Azure のリージョン](https://azure.microsoft.com/regions/#services)」を参照してください。

## ストレージ層の比較

次の表では、2 つのストレージ層を比較しています。

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250"> <col width="250"> <col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>ホット ストレージ層</center></strong></td>
    <td><strong><center>クール ストレージ層</center></strong>&lt;/td
</tr>
<tr>
    <td><strong><center>可用性</center></strong></td>
    <td><center>99.9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>可用性<br>(RA-GRS 読み取り)</center></strong></td>
    <td><center>99.99%</center></td>
    <td><center>99.9%</center></td>
</tr>
<tr>
    <td><strong><center>利用料金</center></strong></td>
    <td><center>より高いストレージ コスト<br>より低いアクセスおよびトランザクション コスト</center></td>
    <td><center>より低いストレージ コスト<br>より高いアクセスおよびトランザクション コスト</center></td>
</tr>
<tr>
    <td><strong><center>最小オブジェクト サイズ<center></strong></td>
    <td colspan="2"><center>該当なし</center></td>
</tr>
<tr>
    <td><strong><center>最小ストレージ存続期間<center></strong></td>
    <td colspan="2"><center>該当なし</center></td>
</tr>
<tr>
    <td><strong><center>待機時間<br>(1 バイト目にかかる時間)<center></strong></td>
    <td colspan="2"><center>ミリ秒</center></td>
</tr>
<tr>
    <td><strong><center>スケーラビリティとパフォーマンスのターゲット<center></strong></td>
    <td colspan="2"><center>汎用ストレージ アカウントと同じ</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB ストレージ アカウントは、汎用ストレージ アカウントと同じパフォーマンスとスケーラビリティ ターゲットをサポートしています。詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット](storage-scalability-targets.md)」を参照してください。

## 価格と課金

BLOB ストレージ アカウントでは、ストレージ層に基づいて、Blob Storage の新しい価格モデルを採用しています。BLOB ストレージ アカウントを使用するときには、課金に関して次の点を考慮してください。

- **ストレージ コスト**: データの格納のコストは、格納されているデータの量だけでなく、ストレージ層にも左右されます。ギガバイトあたりのコストは、クール ストレージ層の方がホット ストレージ層よりも低くなります。
- **データ アクセス コスト**: クール ストレージ層のデータの場合、読み取りと書き込みに対して、ギガバイト単位のデータ アクセス料金が課金されます。
- **トランザクション コスト**: どちらの層も、トランザクションごとの料金が発生します。ただし、クール ストレージ層のトランザクション単位のコストは、ホット ストレージ層の場合よりも高くなります。
- **geo レプリケーション データ転送コスト**: GRS と RA-GRS を含む geo レプリケーションが構成されているアカウントだけに適用されます。geo レプリケーション データ転送には、ギガバイトあたりの料金がかかります。
- **送信データ転送コスト**: 送信データ転送 (Azure リージョン外に転送されるデータ) では、帯域幅使用量に対する課金がギガバイトあたりで発生します。これは、汎用ストレージ アカウントと同じです。
- **ストレージ層の変更**: ストレージ層をクールからホットに変更すると、切り替えごとに、ストレージ アカウントに存在するすべてのデータの読み取りと同等の課金が発生します。一方、ホットからクールへのストレージ層の変更では、コストは発生しません。

> [AZURE.NOTE] ユーザーが新しいストレージ層を試し、サービス開始後の機能を検証できるように、クールからホットへのストレージ層の変更に対する課金は 2016 年 6 月 30 日までは免除されます。2016 年 7 月 1 日からは、クールからホットへのすべての切り替えに課金が適用されます。BLOB ストレージ アカウントの価格モデルの詳細については、「[Azure Storage 料金](https://azure.microsoft.com/pricing/details/storage/)」ページを参照してください。送信データ転送の価格の詳細については、「[Data Transfers (データ転送) の料金詳細](https://azure.microsoft.com/pricing/details/data-transfers/)」を参照してください。

## クイック スタート

このセクションでは、Azure ポータルを使用して、次のシナリオについて説明します。

- BLOB ストレージ アカウントの作成方法。
- BLOB ストレージ アカウントの管理方法。

### Azure ポータルの使用

#### Azure ポータルを使用した BLOB ストレージ アカウントの作成

1. [Azure ポータル](https://portal.azure.com)にサインインします。

2. ハブ メニューで、**[新規]**、**[データ + ストレージ]**、**[ストレージ アカウント]** の順にクリックします。

3. ストレージ アカウントの名前を入力します。

	この名前は、グローバルに一意である必要があります。この名前は、ストレージ アカウントのオブジェクトにアクセスするための URL の一部として使用されます。

4. デプロイ モデルとして **[Resource Manager]** を選択します。

	階層型ストレージは、Resource Manager ストレージ アカウントでのみ使用できます。これは、新しいリソースに推奨されるデプロイ モデルです。詳細については、「[Azure Resource Manager の概要](../resource-group-overview.md)」を参照してください。

5. [Account Kind (アカウントの種類)] ボックスの一覧の **[Blob Storage (Blob Storage)]** を選択します。

	ここで、ストレージ アカウントの種類を選択します。階層型ストレージは汎用ストレージでは利用できません。種類が Blob Storage のアカウントでのみ利用できます。

	これを選択すると、パフォーマンス レベルが [Standard] に設定されます。階層型ストレージは、Premium パフォーマンス レベルでは利用できません。

6. ストレージ アカウントのレプリケーション オプション (**[LRS]**、**[GRS]**、または **[RA-GRS]**) を選択します。既定値は **[RA-GRS]** です。

	LRS はローカル冗長ストレージ、GRS は geo 冗長ストレージ (2 つのリージョン)、RA-GRS は読み取りアクセス geo 冗長ストレージ (2 つのリージョン。セカンダリに対する読み取りアクセス権が付与される) です。

	Azure Storage のレプリケーション オプションの詳細については、[Azure Storage のレプリケーション](storage-redundancy.md)に関するページを参照してください。

7. ニーズに応じた適切なストレージ層を選択します。**[アクセス レベル]** を **[クール]** と **[ホット]** のいずれかに設定します。既定値は **[ホット]** です。

8. 新しいストレージ アカウントを作成するサブスクリプションを選択します。

9. 新しいリソース グループを指定するか、既定のリソース グループを選択します。リソース グループの詳細については、[Azure ポータルを使用した Azure リソースの管理](../azure-portal/resource-group-portal.md)に関するページを参照してください。

10. ストレージ アカウントのリージョンを選択します。

11. **[作成]** をクリックしてストレージ アカウントを作成します。

#### Azure ポータルを使用した BLOB ストレージ アカウントのストレージ層の変更

1. [Azure ポータル](https://portal.azure.com)にサインインします。

2. ストレージ アカウントに移動するには、[すべてのリソース] を選択し、ストレージ アカウントを選択します。

3. [設定] ブレードで **[構成]** をクリックし、アカウント構成を表示または変更します。

4. ニーズに応じた適切なストレージ層を選択します。**[アクセス レベル]** を **[クール]** と **[ホット]** のいずれかに設定します。

5. ブレードの上部にある [保存] をクリックします。

> [AZURE.NOTE] ストレージ層を変更すると、追加料金が発生することがあります。詳細については、「[価格と課金](storage-blob-storage-tiers.md#pricing-and-billing)」セクションを参照してください。

## BLOB ストレージ アカウントの評価と移行

このセクションの目的は、ユーザーが BLOB ストレージ アカウントの使用にスムーズに移行できるようにお手伝いすることです。2 つのユーザー シナリオがあります。

- 汎用ストレージ アカウントを既に持っており、適切なストレージ層の BLOB ストレージ アカウントへの変更を評価したい。
- BLOB ストレージ アカウントの使用を決定しているか、既に所有しており、ホット ストレージ層とクール ストレージ層のどちらを使用すべきかを評価したい。

いずれの場合も、最初にすべきことは、BLOB ストレージ アカウントに格納されたデータの格納とアクセスにかかるコストを見積もり、そのコストを現在のコストと比較することです。

### BLOB ストレージ アカウント レベルの評価

BLOB ストレージ アカウントに格納されたデータの格納とアクセスにかかるコストを見積もるためには、既存の使用パターンを評価するか、予想される使用パターンを概算する必要があります。一般に、以下のことを調べる必要があります。

- ストレージの使用量 - どのくらいの量のデータが格納され、その量は月ごとにどのように変化するか。
- ストレージ アクセス パターン - アカウントに対してどのくらいの量のデータの読み取りや書き込みがあるか (新規データも含めて)。 データ アクセスにはどのくらいのトランザクションが使用されるか。また、そのトランザクションの種類は何か。

#### 既存のストレージ アカウントの監視

既存のストレージ アカウントを監視し、そのデータを収集するには、Azure Storage Analytics を利用できます。これにより、ログ記録が実行され、ストレージ アカウントのメトリック データが得られます。Storage Analytics では、汎用ストレージ アカウントと BLOB ストレージ アカウントの両方について、Blob Storage サービスへの要求に関して集計されたトランザクション統計情報と容量データを含むメトリックを格納できます。このデータは、同じストレージ アカウント内の既知のテーブルに格納されます。

詳細については、「[About Storage Analytics Metrics (Storage Analytics Metrics について)](https://msdn.microsoft.com/library/azure/hh343258.aspx)」と「[Storage Analytics Metrics Table Schema (Storage Analytics Metrics のテーブル スキーマ)](https://msdn.microsoft.com/library/azure/hh343264.aspx)」を参照してください。

> [AZURE.NOTE] BLOB ストレージ アカウントは、そのアカウントのメトリック データの格納とアクセスのためだけに Table サービス エンドポイントを公開します。

Blob Storage サービスのストレージ使用量を監視するには、容量メトリックを有効にする必要があります。これを有効にすると、ストレージ アカウントの BLOB サービスに関する容量データが毎日記録されます。これは、同じストレージ アカウント内の *$MetricsCapacityBlob* テーブルに書き込まれるテーブル エントリとして記録されます。

Blob Storage サービスのデータ アクセス パターンを監視するには、API レベルで時間単位のトランザクション メトリックを有効にする必要があります。これを有効にすると、API あたりのトランザクションが 1 時間ごとに集計され、同じストレージ アカウント内の *$MetricsHourPrimaryTransactionsBlob* テーブルに書き込まれるテーブル エントリとして記録されます。*$MetricsHourSecondaryTransactionsBlob* テーブルには、RA-GRS ストレージ アカウントを使用している場合のセカンダリ エンドポイントに対するトランザクションが記録されます。

> [AZURE.NOTE] 汎用ストレージ アカウントがあり、そのアカウントに、ページ BLOB のほか、ブロック BLOB データと追加 BLOB データと共に仮想マシン ディスクを格納している場合、この見積もりプロセスは適用できません。これは、BLOB ストレージ アカウントに移行され得るブロック BLOB と追加 BLOB のみ、BLOB の種類に基づいて容量とトランザクションのメトリックを区別できないためです。

データ使用量とアクセス パターンを正確に見積もるには、通常の使用状況を表すメトリックのリテンション期間を選択したうえで推定することをお勧めします。その方法の 1 つとして、メトリック データを 7 日間保持し、そのデータを毎週収集して、月末に分析を行う方法があります。そのほかに、過去 30 日間のメトリック データを保持し、30 日の期間の最後にそのデータを収集、分析する方法もあります。

メトリック データの有効化、収集、表示の詳細については、「[Azure のストレージ メトリックの有効化とメトリック データの表示](storage-enable-and-view-metrics.md)」をご覧ください。

> [AZURE.NOTE] 分析データの格納、アクセス、ダウンロードについても、通常のユーザー データと同様に課金されます。

#### コストを見積もるための使用状況メトリックの利用

##### ストレージ コスト

行キー *"data"* がある容量メトリック テーブル *$MetricsCapacityBlob* の最新のエントリは、ユーザー データによって使用されたストレージ容量を示します。行キー *"analytics"* がある容量メトリック テーブル *$MetricsCapacityBlob* の最新のエントリは、分析ログによって使用されたストレージ容量を示します。

ユーザー データと分析ログ (有効な場合) の両方によって使用されたこの合計容量は、ストレージ アカウントにデータを格納するコストを見積もるために使用できます。また、同じ方法を使用して、汎用ストレージ アカウントでのブロック BLOB と追加 BLOB のストレージ コストを見積もることができます。

##### トランザクション料金

*"TotalBillableRequests"* の合計は、トランザクション メトリック テーブル内の API のすべてのエントリを対象とし、その特定の API のトランザクションの総数を示すものです。"*例*" として、特定期間の *"GetBlob"* トランザクションの合計は、行キー *"user;GetBlob"* を持つすべてのエントリに対する課金対象の要求をすべて加算することによって計算できます。

BLOB ストレージ アカウントのトランザクション コストを見積もるには、トランザクションを 3 つのグループに分類する必要があります (それぞれ価格が異なるため)。

- *"PutBlob"*、*"PutBlock"*、*"PutBlockList"*、*"AppendBlock"*、*"ListBlobs"*、*"ListContainers"*、*"CreateContainer"*、*"SnapshotBlob"*、*"CopyBlob"* などの書き込みトランザクション。
- *"DeleteBlob"*、*"DeleteContainer"* などの削除トランザクション。
- その他すべてのトランザクション。

汎用ストレージ アカウントのトランザクション コストを見積もるには、操作と API に関係なく、すべてのトランザクションを集計する必要があります。

##### データ アクセスと geo レプリケーション データ転送のコスト

Storage Analytics では、ストレージ アカウントに対する読み取りと書き込みのデータ量は示されませんが、トランザクション メトリック テーブルを確認することで大まかに見積もることは可能です。トランザクション メトリック テーブル内の API の全エントリを対象とした *"TotalIngress"* の合計は、その特定の API の受信データの総量をバイトで示すものです。同様に、*"TotalEgress"* の合計は、送信データの総量をバイトで示します。

BLOB ストレージ アカウントのデータ アクセス コストを見積もるには、トランザクションを 2 つのグループに分類する必要があります。

- ストレージ アカウントから取得されたデータの量は、主に *"GetBlob"* 操作と *"CopyBlob"* 操作の *"TotalEgress"* の合計を確認することで見積もることができます。
- ストレージ アカウントに書き込まれたデータの量は、主に *"PutBlob"* 操作、*"PutBlock"* 操作、*"CopyBlob"* 操作、*"AppendBlock"* 操作の *"TotalIngress"* の合計を確認することで見積もることができます。

また、BLOB ストレージ アカウントの geo レプリケーション データ転送のコストは、GRS ストレージ アカウントまたは RA-GRS ストレージ アカウントの場合に書き込まれるデータ量の見積もりを使用することによって計算できます。

> [AZURE.NOTE] ホット ストレージ層またはクール ストレージ層を使用する場合のコストの計算に関する詳細な例については、「[Azure Storage 料金](https://azure.microsoft.com/pricing/details/storage/)」というページにある、*"ホットおよびクール アクセス層とはどのようなものですか? また、どちらを使用すればよいのでしょうか?"* というタイトルの FAQ を参照してください。

### 既存のデータの移行

BLOB ストレージ アカウントは、ブロック BLOB と追加 BLOB の格納に特化しています。テーブル、キュー、ファイル、ディスク、および BLOB を格納できる既存の汎用ストレージ アカウントは、BLOB ストレージ アカウントに変換することはできません。つまり、ストレージ層を使用するには、新しい BLOB ストレージ アカウントを作成し、既存のデータを新たに作成したアカウントに移行する必要があります。オンプレミス ストレージ デバイス、サード パーティのクラウド ストレージ プロバイダー、または Azure の既存の汎用ストレージ アカウントから、既存のデータを BLOB ストレージ アカウントに移行するには、以下の方法を使用できます。

#### AzCopy

AzCopy は、Azure Storage との間で高パフォーマンスのデータ コピーを行うように設計された Windows コマンドライン ユーティリティです。AzCopy を使用して、既存の汎用ストレージ アカウントから BLOB ストレージ アカウントにデータをコピーできます。またオンプレミス ストレージ デバイスから BLOB ストレージ アカウントにデータをアップロードすることもできます。

詳細については、「[AzCopy コマンド ライン ユーティリティを使用してデータを転送する](storage-use-azcopy.md)」を参照してください。

#### データ移動ライブラリ

Azure Storage Data Movement Library for .NET は、AzCopy を動作させているコア データ移動フレームワークに基づいています。ライブラリは、AzCopy と同じように、高パフォーマンスで信頼性が高く簡単なデータ転送操作ができるように設計されています。そのため、AzCopy によって提供される機能の利点をアプリケーションでネイティブに活用でき、AzCopy の外部インスタンスを実行したり監視したりする必要はありません。

詳細については、[Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement) に関するページを参照してください。

#### REST API またはクライアント ライブラリ

Azure クライアント ライブラリのいずれかまたは Azure ストレージ サービス REST API を使用して、データを BLOB ストレージ アカウントに移行するためのカスタム アプリケーションを作成することができます。Azure Storage には、.NET、Java、C++、Node.js、PHP、Ruby、Python などの複数の言語とプラットフォーム用の豊富なクライアント ライブラリが用意されています。クライアント ライブラリは、再試行ロジック、ログ、並列アップロードといった高度な機能を提供します。また、REST API を直接使用して開発することもでき、HTTP/HTTPS 要求を行うどの言語からでも呼び出すことができます。

詳細については、[Azure Blob Storage の概要](storage-dotnet-how-to-use-blobs.md)に関するページを参照してください。

> [AZURE.NOTE] クライアント側の暗号化を使用して暗号化された BLOB には、その BLOB と共に格納される暗号化関連メタデータが格納されます。すべてのコピー メカニズムで、BLOB メタデータと、特に暗号化に関連するメタデータが必ず保持されることがきわめて重要です。このメタデータなしで BLOB をコピーした場合、BLOB コンテンツを再度取得できなくなります。暗号化関連メタデータの詳細については、[Azure Storage のクライアント側の暗号化](storage-client-side-encryption.md)に関するページを参照してください。

## FAQ

1. **既存のストレージ アカウントを引き続き使用できますか。**

    はい。既存のストレージ アカウントはまだ使用可能であり、料金や機能も変更されていません。それらにはストレージ層を選択する機能がなく、今後も階層化機能は導入されません。

2. **BLOB ストレージ アカウントは、いつ、どのようなときに使用し始めればよいですか。**

    BLOB ストレージ アカウントは、BLOB を格納するために特化しており、新しい BLOB 主体の機能を導入できます。今後、BLOB ストレージ アカウントは、このアカウントの種類に基づいて階層型ストレージや階層化などの新たな機能が導入されるため、BLOB を格納する方法として推奨されるようになります。ただし、いつ移行するかは、ビジネス要件に応じてお客様が判断する必要があります。

3. **既存のストレージ アカウントを BLOB ストレージ アカウントに変換できますか。**

    いいえ。BLOB ストレージ アカウントは別種のストレージ アカウントであり、上で説明したように、新規に作成してデータを移行する必要があります。

4. **同じアカウントの両方のストレージ層にオブジェクトを格納することはできますか。**

    ストレージ層を示す "*アクセス層*" 属性はアカウント レベルで設定され、そのアカウント内のすべてのオブジェクトに適用されます。アクセス層属性はオブジェクト レベルでは設定できません。

5. **自分の BLOB ストレージ アカウントのストレージ層を変更することはできますか。**

    はい。ストレージ アカウントの "*アクセス層*" 属性を設定して、ストレージ層を変更することができます。ストレージ層の変更は、アカウントに格納されているすべてのオブジェクトに適用されます。ホットからクールへのストレージ層の変更では、料金は発生しません。一方、クールからホットへの変更では、アカウント内のすべてのデータの読み取りに対して GB あたりのコストが発生します。

6. **自分の BLOB ストレージ アカウントのストレージ層は、どの程度の頻度で変更できますか。**

    ストレージ層の変更頻度に対して制限は設けられていませんが、ストレージ層をクールからホットに変更すると、かなりの料金が発生することに注意してください。ストレージ層を頻繁に変更することは、お勧めしません。

7. **クール ストレージ層内の BLOB の動作は、ホット ストレージ層内の BLOB とは異なりますか。**

    ホット ストレージ層内の BLOB の待機時間は、汎用ストレージ アカウントの BLOB と同じになります。クール ストレージ層内の BLOB の待機時間は、汎用ストレージ アカウントの BLOB と類似しています (ミリ秒)。

    クール ストレージ層内の BLOB は、ホット ストレージ層に格納された BLOB よりも可用性サービス レベル (SLA) が若干低くなります。詳細については、「[Storage の SLA](https://azure.microsoft.com/support/legal/sla/storage)」を参照してください。

8. **BLOB ストレージ アカウントにページ BLOB と仮想マシンのディスクを保存できますか。**

    BLOB ストレージ アカウントは、ブロック BLOB と追加 BLOB のみをサポートします。ページ BLOB はサポートしません。Azure の仮想マシンのディスクではページ BLOB が使用されるため、BLOB ストレージ アカウントは仮想マシンのディスクの格納に使用できません。ただし、仮想マシンのディスクのバックアップを BLOB ストレージ アカウント内にブロック BLOB として格納することはできます。

9. **BLOB ストレージ アカウントを使用するには、既存のアプリケーションを変更する必要がありますか。**

    BLOB ストレージ アカウントは、ブロック BLOB と追加 BLOB に関して、汎用ストレージ アカウントとの100% の API 整合性を備えています。アプリケーションでブロック BLOB または追加 BLOB が使用されており、[Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) の 2014-02-14 バージョン以降を使用している限り、アプリケーションは機能します。プロトコルの古いバージョンを使用している場合は、新しいバージョンを使用して両方の種類のストレージ アカウントとシームレスに動作するように、アプリケーションを更新する必要があります。一般に、どの種類のストレージ アカウントを使用するかにかかわらず、常に最新バージョンを使用することをお勧めしています。

10. **ユーザー エクスペリエンスに変更はありますか。**

    BLOB ストレージ アカウントは、ブロック BLOB と追加 BLOB の格納に関しては汎用ストレージ アカウントとよく似ており、高い持続性、可用性、スケーラビリティ、パフォーマンス、セキュリティなどの Azure Storage のすべての主要機能をサポートしています。BLOB ストレージ アカウントに固有の機能および制限と、上に記載したストレージ層を除き、いずれも同じままです。

## 次のステップ

### BLOB ストレージ アカウントを評価する

[リージョン別に BLOB ストレージ アカウントの可用性を確認する](https://azure.microsoft.com/regions/#services)

[Azure Storage のメトリックを有効にして現在のストレージ アカウントの使用状況を評価する](storage-enable-and-view-metrics.md)

[リージョン別に Blob Storage の価格を確認する](https://azure.microsoft.com/pricing/details/storage/)

[データ転送の価格を確認する](https://azure.microsoft.com/pricing/details/data-transfers/)

### BLOB ストレージ アカウントの利用を開始する

[Azure Blob Storage を使用する](storage-dotnet-how-to-use-blobs.md)

[Azure Storage との間でのデータの移動](storage-moving-data.md)

[AzCopy コマンド ライン ユーティリティを使用してデータを転送する](storage-use-azcopy.md)

[自分のストレージ アカウントを調べる](http://storageexplorer.com/)

<!---HONumber=AcomDC_0713_2016-->