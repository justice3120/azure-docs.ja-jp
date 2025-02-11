---
title: CloudSimple による Azure VMware ソリューション - 概要
description: CloudSimple サービスによる Azure VMware ソリューションの機能、シナリオ、および利点について説明します。
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 01fd132b3e43d15c5f2eee0114ef09dbac6df2a6
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812457"
---
# <a name="what-is-vmware-solution-on-azure-by-cloudsimple"></a>CloudSimple による Azure VMware ソリューションとは

**CloudSimple による Azure VMware ソリューション**は VMware プラットフォームを Azure で実行できるように完全に管理されたサービスです。 このソリューションには、vSphere、vCenter、vSAN、NSX T、および対応するツールが含まれています。
Azure クラウド上の場所に、Azure ベア メタルのインフラストラクチャで VMware 環境がネイティブに実行します。 サービスには、効率的かつ安全に VMware プラットフォームを使用するために必要なすべての機能が含まれています。

![CloudSimple による Azure VMware ソリューションの概要](media/azure-vmware-solution-by-cloudsimple.png)

## <a name="features"></a>機能

* オンデマンドでは、VMware クラウド環境のセルフ サービス プロビジョニング。 必要に応じて容量を追加し、削除する機能
* VMware プラットフォームの展開、アップグレード、管理プレーンのバックアップ、正常性と容量の監視、アラート、トラブルシューティング、および修復。
* L2 と L3 サービスとファイアウォール ルールの管理など VMware を有効にするために必要な、土台となるネットワーク サービス。
* VPN、パブリック IP、およびインターネット ゲートウェイを含むエッジ型のネットワーク サービス。 これらのエッジ サービスは、Azure 上で実行され、対応するセキュリティと Azure の DDoS 保護を実行します。
* コスト削減のための容量の予約。
* Azure とオンプレミスへ高速で短い待機時間で接続。
* Azure サービスを統合された形式で利用するお客様向けのソリューション アーキテクチャでは、独自の「パブリック クラウドに VMware クラウド」アーキテクチャを活用しています。 これらの Azure サービスには、Azure AD 、ストレージ、アプリケーション ゲートウェイなどが含まれます。
* インフラストラクチャは、完全に個々のユーザー専用となり、他のお客様のインフラストラクチャから物理的に分離されます。
* アクティビティの管理、使用状況、課金/測定、およびユーザー管理などの管理機能。
* 1 日 24 時間週 7 日体制のカスタマー サポート

## <a name="benefits"></a>メリット

* **運用の継続性:** CloudSimple は、VMware プラットフォームへのネイティブ アクセスを提供しています。 CloudSimple アーキテクチャは、既存との互換性があります。
  * [アプリケーション]
  * Operations
  * ネットワーク
  * セキュリティ
  * バックアップ
  * 障害復旧
  * Audit
  * コンプライアンス ツール
  * 処理
* **再トレーニングなし:** VMware プラットフォームの互換性により、既存のスキルと知識を使用できます。
* **インフラストラクチャの機敏性:** すべての容量ニーズを予測し、その結果、容量を無駄遣いしたりインフラストラクチャが足りなくなるということがなくなります。 CloudSimple は、クラウド サービスとして提供され、いつでも容量を追加または削減できます。
* **セキュリティ:** Azure を通した CloudSimple 環境へのアクセスにより、組み込みの DDoS を保護しセキュリティを監視することになります。
* **低コスト:** CloudSimple プラットフォームは高度に設計されており、高レベルの自動化、運用効率、およびスケールの経済性を実現します。 さらに、CloudSimple はコストを削減するためにパブリック クラウドに VMware が存在することを利用するソリューション アーキテクチャを公開します。 例として、Azure AD、Azure ストレージへのバックアップ、アプリケーション ゲートウェイ、ロード バランサーなどがあります。
* **新しいハイブリッド プラットフォーム:** このサービスにより、Azure の残りの部分に高速、短い待機時間でアクセスできます。 さらにCloudSimple 管理では、同じ UI と API を使用してVMware 仮想マシンと Azure の残りの部分を統合管理できます。 開発チームは、統合された一貫性のある方法でパブリックとプライベートの両方のプラットフォームを活用できます。
* **インフラストラクチャの監視やトラブルシューティング、およびサポート:** CloudSimple では、基になるインフラストラクチャをサービスとして動作します。 故障したハードウェアは自動的に置き換えられます。 CloudSimple により、環境がスムーズに実行されるようにする間、使用に集中できます。
* **ポリシーの互換性:** VMware ベースのツール、セキュリティ プロシージャ、監査プラクティス、およびコンプライアンス認定資格を維持します。

## <a name="scenarios"></a>シナリオ

* **データ センターの廃止または移行:** 既存のデータ センターの制限に達するかハードウェアを更新するときに、容量を追加します。 クラウドで必要な容量を追加したりハードウェアの更新管理する手間を排除できて簡単です。 時間のかかる変換や再設計と比較して、クラウド移行のリスクやコストが削減されます。 使い慣れた VMware の使ツールとスキルを使用して、クラウド移行を促進します。 いったんクラウドになれば、 Azure サービスを使用して自分のペースでアプリケーションを今の時代に合わせられます。
* **必要に応じて拡張:** 新しい開発環境または季節ごとのキャパシティ バーストなどの予期しないニーズに合わせてクラウドを拡張します。 ニーズに応じて新しい容量を簡単に作成し、必要がある限り維持のみができます。 先行投資を削減し、プロビジョニング速度を加速させ、オンプレミスとクラウドの両方でアーキテクチャとポリシーを同じにして複雑さを軽減します。
* **Azure クラウドでのディザスター リカバリーおよび仮想デスクトップ:** Azure クラウド内のデータ、アプリ、およびデスクトップへのリモート アクセスを確立します。 高帯域幅接続により、データを高速にアップロード/ダウンロードしてインシデントから復旧します。 ネットワークの待機時間が短いことにより、デスクトップ アプリからの応答時間が短くなることを期待できます。 CloudSimple では、CloudSimple portal と使い慣れた VMware ツールを使用してクラウド内のポリシーとネットワークのすべてを簡単に複製できます。 回復およびレプリケーションの手軽さにより、DR と VDI の実装の作成と管理の作業量とリスクが大幅に削減されます。
* **高性能アプリケーションとデータベース:** CloudSimple には、もっとも骨の折れる VMware ワークロードを実行するように設計ハイパーコンバージド アーキテクチャが用意されています。 Oracle、Microsoft SQL サーバー、ミドルウェアのシステム、および高パフォーマンスの非 SQL データベースを実行します。 オンプレミス、Azure 上の VMware および Azure プライベート ワークロードにまたがるハイブリッド アプリを性能に妥協することなく実行できるようにする高速 25 Gbps のネットワーク接続で、自社のデータ センターとしてクラウドを体験してください。
* **真のハイブリッド:** VMware と Azure で DevOps が統合されます。 すべてのワークロードに適用できる Azure のサービスとソリューション向け VMware 管理を最適化します。 データ センターを展開したり、アプリケーションを再設計することなくパブリック クラウド サービスにアクセスします。 Azure 上での VMware アプリケーションの ID、アクセス制御ポリシー、ログと監視を一元管理します。

![シナリオ](media/cloudsimple-scenarios.png)

## <a name="next-steps"></a>次の手順

* [CloudSimple サービスの作成](quickstart-create-cloudsimple-service.md)
* [プライベート クラウドの作成](quickstart-create-private-cloud.md)