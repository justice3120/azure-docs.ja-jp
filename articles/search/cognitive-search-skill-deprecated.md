---
title: 非推奨のコグニティブ スキル - Azure Search
description: このページには、近い将来サポートされなくなり、非推奨と見なされるコグニティブ検索スキルの一覧が含まれています。
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.subservice: cognitive-search
ms.openlocfilehash: 57dd8e47d44248270920e44268b99c98d399504d
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2019
ms.locfileid: "69635908"
---
# <a name="deprecated-cognitive-search-skills"></a>非推奨の認知検索スキル

このドキュメントでは、非推奨と見なされるコグニティブ スキルについて説明します。 以下の内容について、次のガイドを使用してください。

* スキル名: 非推奨となるスキルの名前です。@odata.type 属性にマッピングされます。
* 利用可能な最終 API バージョン: 対応する非推奨スキルを含むスキルセットを作成および更新できる Azure Search パブリック API の最終バージョンです。
* サポート終了: この日を過ぎると、対応するスキルはサポートされていないと見なされます。 以前に作成したスキルセットは引き続き機能するはずですが、ユーザーには非推奨のスキルから移行することが推奨されています。
* 推奨事項:サポートされているスキルを使用するための移行パスです。 ユーザーは、引き続きサポートを受けるため、推奨事項に従うことをお勧めします。

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>利用可能な最終 API バージョン

2017-11-11-Preview

### <a name="end-of-support"></a>サポート終了

2019 年 2 月 15 日

### <a name="recommendations"></a>Recommendations 

代わりに [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) を使用します。 NamedEntityRecognitionSkill のほとんどの機能を提供しており、品質も向上しています。 さらに、複合出力フィールドに詳細な情報が格納されています。

[エンティティ認識スキル](cognitive-search-skill-entity-recognition.md)に移行するには、スキル定義で以下の 1 つ以上の変更を行う必要があります。 スキル定義は、[スキルセット更新 API](https://docs.microsoft.com/rest/api/searchservice/update-skillset) を使用して更新することができます。

> [!NOTE]
> 現時点では、概念としての信頼度スコアはサポートされていません。 `EntityRecognitionSkill` には、将来的な使用と旧バージョンとの互換性のため、`minimumPrecision` パラメーターが存在しています。

1. *(必須)* `@odata.type` を `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` から `"#Microsoft.Skills.Text.EntityRecognitionSkill"` に変更します。

2. *(省略可能)* `entities` 出力を使用する場合は、`EntityRecognitionSkill` からの `namedEntities` 複合コレクション出力を使用します。 スキル定義の `targetName` を使用すると、これを `entities` という注釈にマップすることができます。

3. *(省略可能)* `categories` を明示的に指定しない場合、`EntityRecognitionSkill` は `NamedEntityRecognitionSkill` でサポートされているカテゴリものだけでなく、別の種類のカテゴリも返すことがあります。 この動作が望ましくない場合は、明示的に `categories` パラメーターを `["Person", "Location", "Organization"]` に設定します。

    _移行定義サンプル_

    * 単純な移行

        _(変更前) NamedEntityRecognition スキルの定義_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(変更後) EntityRecognition スキルの定義_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * 少し複雑な移行

        _(変更前) NamedEntityRecognition スキルの定義_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(変更後) EntityRecognition スキルの定義_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>関連項目

+ [定義済みのスキル](cognitive-search-predefined-skills.md)
+ [スキルセットの定義方法](cognitive-search-defining-skillset.md)
+ [エンティティ認識スキル](cognitive-search-skill-entity-recognition.md)
