---
title: ナレッジ ストアでのプロジェクションの操作 (プレビュー) - Azure Search
description: 検索以外のシナリオで使用するために AI のインデックス作成パイプラインから強化されたデータを保存して整形します
manager: eladz
author: vkurpad
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: vikurpad
ms.subservice: cognitive-search
ms.openlocfilehash: 4fa669eaf96c08d7a2e1e7255ff6b1f6ff7b4f14
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639833"
---
# <a name="working-with-projections-in-a-knowledge-store-in-azure-search"></a>ナレッジ ストアでのプロジェクションの操作

> [!Note]
> ナレッジ ストアはプレビュー段階にあり、運用環境での使用は意図していません。 [REST API バージョン 2019-05-06-Preview](search-api-preview.md) でこの機能を提供します。 現時点で .NET SDK のサポートはありません。
>

Azure Search を使用すると、インデックス作成の一環として AI 認知技術とカスタム技術を介してコンテンツを強化することができます。 強化によってドキュメントに構造が追加され、検索がより効果的になります。 多くの場合、強化されたドキュメントは、ナレッジ マイニングなど、検索以外のシナリオに役立ちます。

[ナレッジ ストア](knowledge-store-concept-intro.md)のコンポーネントであるプロジェクションは、ナレッジ マイニングの目的で物理ストレージに保存できる強化されたドキュメントのビューです。 プロジェクションを使用すると、Power BI などのツールで追加の作業なしでデータを読み取ることができるように、関係を維持しながら、ニーズに合った形状にデータを "射影" できます。 

プロジェクションは、Azure Table ストレージの行と列に格納されたデータ、または Azure Blob ストレージに格納された JSON オブジェクトを使用して表形式にすることができます。 データが強化されると、データの複数のプロジェクションを定義できます。 これは、個々のユース ケースで同じデータを異なる方法で整形する場合に便利です。 

ナレッジ ストアは 2 種類のプロジェクションをサポートします。

+ **テーブル**:行と列として最も適切に表現されているデータの場合、テーブルのプロジェクションを使用すると、Table ストレージでスキーマ化された形状またはプロジェクションを定義できます。 

+ **オブジェクト**:データと強化の JSON 表現が必要な場合は、オブジェクトのプロジェクションを BLOB として保存します。

コンテキスト内の定義済みプロジェクションを確認する方法については、[ナレッジ ストアを使い始める方法](knowledge-store-howto.md)に関する手順を参照してください。

## <a name="projection-groups"></a>プロジェクション グループ

場合によっては、強化されたデータをさまざまな目的に合わせて異なる形状で射影する必要があります。 ナレッジ ストアを使用すると、複数のプロジェクション グループを定義できます。 プロジェクション グループには、次のように相互排他性と関連性の重要な特性があります。

### <a name="mutually-exclusivity"></a>相互排他性

1 つのグループに射影されるすべてのコンテンツは、他のプロジェクション グループに射影されるデータとは無関係です。 これは、同じデータを異なる形状で持ち、各プロジェクション グループで繰り返すことができることを意味します。 

プロジェクション グループに適用される 1 つの制約は、プロジェクションの種類とプロジェクション グループの相互排他性です。 1 つのグループ内に定義できるのは、テーブル プロジェクションとオブジェクト プロジェクションのいずれか一方のみです。 テーブルとオブジェクトの両方が必要な場合は、テーブル用に 1 つのプロジェクション グループを定義し、オブジェクト用にもう 1 つのプロジェクション グループを定義します。

### <a name="relatedness"></a>関連性

1 つのプロジェクション グループ内に射影されたすべてのコンテンツは、データ内の関係を維持します。 関係は生成されたキーに基づいており、各子ノードは親ノードへの参照を保持します。 関係は複数のプロジェクション グループにまたがりません。また、あるプロジェクション グループで作成されたテーブルやオブジェクトは、他のプロジェクション グループで生成されたデータとは関係がありません。

## <a name="input-shaping"></a>入力の整形
データを正しい形状や構造にすることは、テーブルでもオブジェクトでも、効果的に使用するために重要です。 予定するアクセス方法と使用方法に基づいてデータを整形または構造化する機能は、スキルセット内で **Shaper** スキルとして公開されている重要な機能です。  

プロジェクションのスキーマに一致するオブジェクトが強化ツリーにあると、プロジェクションの定義が簡単になります。 新しくなった [Shaper スキル](cognitive-search-skill-shaper.md) を使用すると、強化ツリーのさまざまなノードからオブジェクトを構成し、それらを新しいノード以下の子にすることができます。 **Shaper** スキルを使用すると、入れ子になったオブジェクトを含む複合型を定義することができます。

射影する必要があるすべての要素を含む新しい形状を定義すると、プロジェクションのソースとして、または別のスキルへの入力としてその形状を使用できます。

## <a name="table-projections"></a>テーブル プロジェクション

インポートが容易になるため、Power BI を使用したデータ探索にはテーブル プロジェクションをお勧めします。 さらに、テーブル プロジェクションによって、テーブルの関係間のカーディナリティを変更することができます。 

関係を維持しながら、インデックス内の 1 つのドキュメントを複数のテーブルに射影できます。 複数のテーブルに射影する場合、子ノードが同じグループ内の別のテーブルのソースでない限り、完全な形状が各テーブルに射影されます。

### <a name="defining-a-table-projection"></a>テーブル プロジェクションの定義

スキルセットの `knowledgeStore` 要素内にテーブル プロジェクションを定義するときは、まず強化ツリーのノードをテーブル ソースにマップします。 通常、このノードは、テーブルに射影するために必要な特定の形状を生成するスキルの一覧に追加した **Shaper** スキルの出力です。 射影対象として選択したノードは、スライスして、複数のテーブルに射影することができます。 テーブル定義は、射影するテーブルの一覧です。 

各テーブルには 3 つのプロパティが必要です。

+ tableName:Azure Storage 内のテーブルの名前。

+ generatedKeyName:この行を一意に識別するキーの列名。

+ source:強化のソースとする強化ツリーのノード。 通常、これは shaper の出力ですが、スキルのいずれかの出力の可能性があります。

テーブル プロジェクションの例を次に示します。

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [
            { "tableName": "MainTable", "generatedKeyName": "SomeId", "source": "/document/EnrichedShape" },
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/EnrichedShape/*/KeyPhrases/*" },
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/EnrichedShape/*/Entities/*" }
          ]
        },
        {
          "objects": [
            
          ]
        }
      ]
    }
}
```
この例で示すように、キー フレーズとエンティティは異なるテーブルにモデル化されており、各行の親 (MainTable) への参照が含まれます。 

次の図は、「[How to get started with knowledge store (ナレッジ ストアの使用を開始する方法)](knowledge-store-howto.md)」の Caselaw 演習を参照しています。 ケースに複数の意見があり、各意見がその中に含まれるエンティティを識別することで強化されるシナリオでは、ここに示すようにプロジェクションをモデル化できます。

![テーブルのエンティティと関係](media/knowledge-store-projection-overview/TableRelationships.png "テーブル プロジェクションの関係のモデル化")

## <a name="object-projections"></a>オブジェクト プロジェクション

オブジェクト プロジェクションは、任意のノードをソースにすることができる強化ツリーの JSON 表現です。 多くのケースでは、テーブル プロジェクションの作成と同じ **Shaper** スキルを使用してオブジェクト プロジェクションを生成できます。 

```json
{
 
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
          "objects": [
            {
              "storageContainer": "Reviews", 
              "format": "json", 
              "source": "/document/Review", 
              "key": "/document/Review/Id" 
            }
          ]
        }
      ]
    }
}
```

オブジェクト プロジェクションを生成するには、いくつかのオブジェクト固有の属性が必要です。

+ storageContainer:オブジェクトが保存されるコンテナー
+ source:プロジェクションのルートとなる強化ツリーのノードのパス
+ key:格納するオブジェクトの一意のキーを表すパス。 コンテナー内の BLOB 名を作成するために使用されます。

## <a name="projection-lifecycle"></a>プロジェクションのライフサイクル

プロジェクションには、データ ソース内のソース データに関連付けられているライフサイクルがあります。 データが更新され、インデックスが再作成されると、強化の結果と共にプロジェクションが更新され、最終的にプロジェクションがデータ ソースのデータと一致するようになります。 プロジェクションは、インデックスに構成した削除ポリシーを継承します。 

## <a name="using-projections"></a>プロジェクションの使用

インデクサーの実行後に、プロジェクションによって指定したコンテナーまたはテーブル内の射影されたデータを読み取ることができます。 

分析に関しては、Power BI での探索は、Azure Table ストレージをデータ ソースとして設定する場合と同じくらい簡単です。 内部の関係を利用すると、とても簡単に一連の視覚化をデータ上に作成できます。

また、データ サイエンス パイプラインで強化されたデータを使用する必要がある場合は、[BLOB から Pandas DataFrame にデータを読み込む](../machine-learning/team-data-science-process/explore-data-blob.md)こともできます。

最後に、ナレッジ ストアからデータをエクスポートする必要がある場合、Azure Data Factory には、データをエクスポートして任意のデータベースに格納するコネクタがあります。 

## <a name="next-steps"></a>次の手順

次の手順として、サンプル データと手順を使用して最初のナレッジ ストアを作成します。

> [!div class="nextstepaction"]
> [ナレッジ ストアを作成する方法](knowledge-store-howto.md)。