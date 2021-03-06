<properties
   pageTitle="Azure Security Center トラブルシューティング ガイド | Microsoft Azure"
   description="このドキュメントは、Azure Security Center で問題をトラブルシューティングするのに役立ちます。"
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="yurid"/>

# Azure Security Center トラブルシューティング ガイド
このガイドは、所属組織が Azure Security Center を使用している情報技術 (IT) プロフェッショナル、IT アーキテクト、情報セキュリティ アナリスト、クラウド管理者を対象としています。

> [AZURE.NOTE] このドキュメントの情報は、Azure セキュリティ センターのプレビュー リリースに適用されます。

## Azure セキュリティ センターとは
Azure セキュリティ センターは、Azure リソースのセキュリティを高度に視覚化し、制御することで脅威を回避、検出し、それに対応することに役立ちます。これにより、サブスクリプション全体に統合セキュリティの監視とポリシーの管理を提供し、気付かない可能性がある脅威を検出し、セキュリティ ソリューションの広範なエコシステムと連動します。

## トラブルシューティング ガイド
このガイドでは、Azure Security Center に関連する問題のトラブルシューティングの方法について説明します。Azure Security Center で行われるトラブルシューティングのほとんどは、問題の生じたコンポーネントの[監査ログ](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) レコードを確認することから始まります。監査ログを使用すると、次の内容を判断することができます。

- 実行された操作
- 操作を開始したユーザー
- 操作が発生した時間
- 操作の状態
- 操作の調査に役立つ可能性のあるその他のプロパティの値

監査ログには、リソースで実行されたすべての書き込み操作 (PUT、POST、DELETE) が含まれます。ただし、読み取り操作 (GET) は含まれません。

## Windows における監視エージェントのインストールのトラブルシューティング

Security Center 監視エージェントは、データ収集を実行するために使用されます。データ収集が有効になり、ターゲット コンピューターにエージェントが正常にインストールされた後には、次のプロセスが実行されています。

- ASMAgentLauncher.exe - Azure 監視エージェント
- ASMMonitoringAgent.exe - Azure セキュリティ監視拡張機能
- ASMSoftwareScanner.exe – Azure スキャン マネージャー

Azure セキュリティ監視拡張機能では、さまざまなセキュリティ関連の構成がスキャンされ、仮想マシンからセキュリティ ログが収集されます。スキャン マネージャーは、パッチ スキャナーとして使用されます。

インストールが正常に実行されている場合は、ターゲット VM の監査ログに次のようなエントリが示されます。

![Audit Logs](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

*%systemdrive%\\windowsazure\\logs* (C:\\WindowsAzure\\Logs など) にあるエージェント ログを確認して、インストール プロセスに関する詳しい情報を入手することもできます。

[AZURE.NOTE] Azure Security Center エージェントが正常に動作していない場合、ターゲット VM を再起動する必要があります。これは、エージェントの停止と開始のコマンドがないためです。

## Linux における監視エージェントのインストールのトラブルシューティング
Linux システムでの VM エージェントのインストールをトラブルシューティングする場合、必ず /var/lib/waagent/ に拡張機能をダウンロードしておく必要があります。次のコマンドを実行すると、拡張機能がインストールされているかどうかを確認できます。

`cat /var/log/waagent.log`

トラブルシューティングの目的で確認できるその他のログ ファイルには、次のものがあります。

- /var/log/mdsd.err
- /var/log/azure/

動作中のシステムでは、TCP 29130 での mdsd プロセスへの接続が表示されます。これは、mdsd プロセスと通信している syslog です。この動作は、次のコマンドを実行することで検証できます。

`netstat -plantu | grep 29130`

## Microsoft サポートへの問い合わせ

一部の問題は、この記事で提供されているガイドラインを基に特定できます。その他の問題も Security Center パブリック [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)で確認できます。ただし、トラブルシューティングがさらに必要な場合は、次に示すように Azure ポータルを使用して新しいサポート要求を開くことができます。

![Microsoft Support](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## 次のステップ

このドキュメントでは、Azure セキュリティ センターでのセキュリティ ポリシーの構成方法について説明しました。Azure セキュリティ センターの詳細については、次を参照してください。

- 「[Azure Security Center 計画および運用ガイド](security-center-planning-and-operations-guide.md)」 - Azure Security Center を導入するための設計上の考慮事項を計画および理解する方法についての説明
- 「[Azure セキュリティ センターでのセキュリティ ヘルスの監視](security-center-monitoring.md)」 – Azure リソースのヘルスを監視する方法についての説明
- 「[Azure セキュリティ センターでのセキュリティのアラートの管理と対応](security-center-managing-and-responding-alerts.md)」 -セキュリティの警告の管理および対応について
- 「[Azure Security Center を使用したパートナー ソリューションの監視](security-center-partner-solutions.md)」 -- パートナー ソリューションの正常性状態を監視する方法についての説明
- 「[Azure セキュリティ センターに関する FAQ](security-center-faq.md)」 – このサービスの使用に関してよく寄せられる質問
- 「[Azure セキュリティ ブログ](http://blogs.msdn.com/b/azuresecurity/)」 – Azure のセキュリティとコンプライアンスについてまとめたブログ記事の検索

<!---HONumber=AcomDC_0713_2016-->