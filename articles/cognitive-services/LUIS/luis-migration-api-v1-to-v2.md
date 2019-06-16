---
title: v1-v2 API'si geçişi
titleSuffix: Azure Cognitive Services
description: Sürüm 1 uç noktası ve konuşma tanıma API'leri yazma kullanım dışı bırakılmıştır. Sürüm 2 uç nokta ve yazma API'leri geçirme anlamak için bu kılavuzu kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: a4e176552fcbab9d54da696439c7e46750be3944
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65522357"
---
# <a name="api-v1-to-v2-migration-guide-for-luis-apps"></a>API v1 için LUIS uygulamaları v2 Geçiş Kılavuzu
Sürüm 1 [uç nokta](https://aka.ms/v1-endpoint-api-docs) ve [yazma](https://aka.ms/v1-authoring-api-docs) API'leri kullanım dışı bırakılmıştır. 2\. sürüme geçirme anlamak için bu kılavuzu kullanın [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) ve [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) API'leri. 

## <a name="new-azure-regions"></a>Yeni Azure bölgeleri
LUIS sahip yeni [bölgeleri](https://aka.ms/LUIS-regions) LUIS API'leri için sağlanan. LUIS, bölge grupları için farklı bir portal sağlar. Uygulama sorgu beklediğiniz aynı bölgede yazılması gerekir. Uygulamaları bölgeleri otomatik olarak geçirilmez. Tek bir bölge sonra almak üzere başka bir yeni bölgede kullanılabilir olması için uygulamanın verilecek.

## <a name="authoring-route-changes"></a>Rota değişiklikleri yazma
Yazma API rota değiştirilen kullanımından **prog** kullanımına yol **API** rota.


| version | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps|
|2|/luis/**API**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Uç nokta rota değişiklikleri
API uç noktası, farklı bir yanıt yanı sıra yeni sorgu dizesi parametreleri vardır. Ayrıntılı bayrağı true ise, puanı, bağımsız olarak tüm hedefleri topScoringIntent yanı sıra bir ıntents adlı bir dizi döndürülür.

| version | Rota Al |
|--|--|
|1|/luis/v1/Application? kimliği {AppID} = & q = {q}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& timezoneOffset] [& ayrıntılı] [& Yazım denetimi] [& hazırlama] [& bing-yazım-onay-subscription-key] [& günlük]|


V1 uç noktası başarılı yanıt:
```json
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 uç noktası başarılı yanıt:
```json
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Artık API anahtarı yönetimi
Abonelik uç noktası anahtarı API'leri bırakılmıştır 410 GONE döndürüyor.

| version | yol |
|--|--|
|1|/luis/V1.0/prog/Subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [uç nokta anahtarları](luis-how-to-azure-subscription.md) Azure portalında oluşturulur. Anahtara bir LUIS uygulaması üzerinde atadığınız **[Yayımla](luis-how-to-azure-subscription.md)** sayfası. Gerçek bir anahtar değer kattığını bilmek gerekmez. LUIS, atama yapmak için abonelik adını kullanır. 

## <a name="new-versioning-route"></a>Yeni sürüm oluşturma yolu
V2 modeli şimdi bulunan bir [sürüm](luis-how-to-manage-versions.md). Sürüm 10 karakter rota adıdır. Varsayılan "0.1" sürümüdür.

| version | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps/ {AppID} / varlıklar|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} / varlıklar|

## <a name="metadata-renamed"></a>Meta veri olarak yeniden adlandırıldı
LUIS meta verileri döndürmek API'leri yeni adları vardır.

| V1 rota adı | v2 rota adı |
|--|--|
|PersonalAssistantApps |Yardımcıları|
|applicationcultures|kültürler|
|applicationdomains|etki alanları|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Önermek için" "Örnek" olarak yeniden adlandırıldı
LUIS önerir mevcut konuşma [konuşma uç noktası](luis-how-to-review-endpoint-utterances.md) modeli geliştirmek. Önceki sürümde bu taşıyordu **örnek**. Yeni sürüm için örnek adı değiştirilirse **Öner**. Bu adlandırılır **[gözden geçirin, konuşma uç noktası](luis-how-to-review-endpoint-utterances.md)** LUIS Web sitesinde.

| version | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps/ {AppID} /entities/ {Entityıd} /**örnek**|
|1|/luis/V1.0/**prog**/apps/ {AppID} /intents/ {intentId} /**örnek**|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} /entities/ {Entityıd} /**önerin**|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} /intents/ {intentId} /**önerin**|


## <a name="create-app-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından uygulaması oluşturma
[Önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) bir önceden tanımlanmış bir etki alanı modeli sağlar. Önceden oluşturulmuş etki alanları ortak etki alanları için LUIS uygulamanızı hızla geliştirmenizi sağlar. Bu API, önceden oluşturulmuş bir etki alanını temel alarak yeni bir uygulama oluşturmanıza olanak sağlar. Yeni AppID yanıttır.

|v2 yol|Fiili|
|--|--|
|/luis/api/v2.0/Apps/customprebuiltdomains  |GET, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Al|

## <a name="importing-1x-app-into-2x"></a>1\.x uygulama 2.x alma
Dışarı aktarılan 1.x uygulamanın JSON içine aktarmadan önce değiştirmeniz gereken bazı alanlar vardır [LUIS] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar 
[Önceden oluşturulmuş varlıklarla](luis-prebuilt-entities.md) değiştirilmiştir. V2 kullandığınızdan emin olun önceden oluşturulmuş varlıklar. Bu kullanarak içerir [datetimeV2](luis-reference-prebuilt-datetimev2.md), datetime yerine. 

### <a name="actions"></a>Eylemler
Actions özelliği artık geçerli değil. Boş olmalıdır 

### <a name="labeled-utterances"></a>Etiketli konuşma
V1 boşluk başlangıcında veya sonunda bir sözcük veya tümcecik içeren etiketli konuşma izin verilir. Boşluklar kaldırıldı. 

## <a name="common-reasons-for-http-response-status-codes"></a>HTTP yanıtı durum kodları için yaygın nedenler
Bkz: [LUIS API'si yanıt kodları](luis-reference-response-codes.md).

## <a name="next-steps"></a>Sonraki adımlar

Var olan geri KALAN güncelleştirilecek v2 API'si belgeleri çağırır LUIS için kullanım [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) ve [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) API'leri. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
