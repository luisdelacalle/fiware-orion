# NGSIv1 オートキャスト (NGSIv1 Autocast)

デフォルトでは、パーシング技術のために、NGSIv1 属性の作成/更新オペレーションは、JSON が他のネイティブ型 (Number, Boolean など) を使用する場合でも、常に属性を文字列として格納します。さらに、DateTime 型は NGSIv1 ではサポートされていないため、この型の属性はプレーンな文字列として格納されます。これは、これらの属性値に対して高度な機能 (NGSIv2 フィルタリングなど) を使用する場合に問題になります。

NGSIv1 のオートキャスト機能により、このような状況を緩和することができます。これは、`-ngsiv1Autocast` [CLI オプション](../admin/cli.md) で有効になります。有効にすると、Orion Context Broker は NGSIv1 の作成/更新オペレーションで次の属性タイプを解釈します :

* "Number" (および、その別名 "Quantity")
* "DateTime" (および、その別名 "ISO8601")
* "Boolean"

処理は次のとおりです :

* 属性の作成時に、_リクエスト_の属性型が上記のいずれかである場合、属性を格納する前に、対応する型へのキャストが行われます
* _属性値と型の両方_ の変更を含む属性の変更時に、_リクエスト_の属性型が上記のいずれかである場合、属性を格納する前に対応する型へのキャストが行われます
* そのような変更のみの属性値を含む属性の変更時 (NGSIv1 では可能性があることに注意)、Orion によって格納された属性タイプが上記のいずれかである場合、属性値を更新する前に対応する型へのキャストが行われます

いくつかの備考 :

* `{ "value": "Yes", "type": "Number" }` または `{ "value": 29, "type": "Boolean" }` などのターゲット型への変換が不可能な場合、属性は文字列として格納されます
* これは、NGSiv1 の作成/更新のオペレーションにのみ適用されます。`-ngsiv1Autocast` が使用されているかどうかにかかわらず、NGSIv2 機能は変更されません
