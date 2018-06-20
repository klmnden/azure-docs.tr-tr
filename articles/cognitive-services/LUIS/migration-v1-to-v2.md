---
title: API geçirme Kılavuzu v1'den v2 | Microsoft Docs
titleSuffix: Azure
description: Bilgi nasıl geçiş için en son API ayarlayın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 060baa6578f8a31234a1a667e2d591b92c39a06f
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264293"
---
# <a name="api-v2-migration-guide"></a>API v2 Geçiş Kılavuzu
Sürüm 1 [endpoint](https://aka.ms/v1-endpoint-api-docs) ve [yazma](https://aka.ms/v1-authoring-api-docs) API'leri kullanım dışı. 2. sürüme geçirmek nasıl anlamak için bu kılavuzu kullanın [endpoint](https://aka.ms/luis-endpoint-apis) ve [yazma](https://aka.ms/luis-authoring-apis) API'leri. 

## <a name="new-azure-regions"></a>Yeni Azure bölgeleri
HALUK sahip yeni [bölgeleri](https://aka.ms/LUIS-regions) HALUK API için sağlanan. HALUK bölge grupları için farklı bir Web sitesi sağlar. Uygulama sorgulamak için beklediğiniz aynı bölgede yazılmış olabilir. Uygulamaları bölgeleri otomatik olarak geçirilmez. Uygulama, tek bir bölge silip yeni bir bölgede kullanılabilir olması için başka bir içe aktarın.

## <a name="authoring-route-changes"></a>Rota değişiklikleri yazma
Geliştirme API rota değiştirilen kullanımından **prog** kullanmanın rota **API** rota.


| sürüm | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps|
|2|/luis/**API**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Uç nokta yol değişiklikleri
API uç noktası, yeni sorgu dizesi parametreleri gibi farklı bir yanıt vardır. Ayrıntılı bayrağı true ise, puanı bağımsız olarak tüm hedefleri topScoringIntent yanı sıra hedefleri adlı bir dizi döndürülür.

| sürüm | Rota Al |
|--|--|
|1|/luis/v1/Application? kimliği {AppID} = & q = {q}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& timezoneOffset] [& ayrıntılı] [& Yazım denetimi] [& hazırlama] [& bing yazım-onay-abonelik-anahtar] [& günlük]|


V1 uç nokta başarılı yanıtı:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 uç nokta başarılı yanıtı:
```JSON
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

## <a name="key-management-no-longer-in-api"></a>Artık API'sindeki anahtar yönetimi
Abonelik anahtarı 410 GONE döndüren API'leri bırakılmıştır.

| sürüm | yol |
|--|--|
|1|/luis/V1.0/prog/Subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [Abonelik anahtarları](luis-how-to-azure-subscription.md) Azure portalında oluşturulur. Üzerinde HALUK uygulama anahtarına atamak **[Yayımla](manage-keys.md)** sayfası. Gerçek anahtar değeri bilmeniz gerek yoktur. HALUK atama yapmak için abonelik adını kullanır. 

## <a name="new-versioning-route"></a>Yeni sürüm yol
V2 modeli şimdi bulunan bir [sürüm](luis-how-to-manage-versions.md). Sürüm adı rotadaki 10 karakterdir. "0,1" varsayılan sürümdür.

| sürüm | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps/ {AppID} / varlıklar|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} / varlıklar|

## <a name="metadata-renamed"></a>Meta veri yeniden adlandırıldı
HALUK döndürmenizi API'leri yeni adlara sahip.

| V1 rota adı | v2 rota adı |
|--|--|
|PersonalAssistantApps |Yardımcıları|
|applicationcultures|kültürler|
|applicationdomains|etki alanları|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Sample" "önermek için" yeniden adlandırıldı
HALUK öneren varolan utterances [endpoint utterances](label-suggested-utterances.md) modeli geliştirmek. Önceki sürümde bu taşıyordu **örnek**. Yeni sürümde örnek adı değiştirilirse **önermek**. Bu adlı **[gözden geçirin, uç nokta utterances](https://docs.microsoft.com/azure/cognitive-services/LUIS/label-suggested-utterances)** HALUK Web sitesinde.

| sürüm | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps/ {AppID} /entities/ {Entityıd} /**örnek**|
|1|/luis/V1.0/**prog**/apps/ {AppID} /intents/ {intentId} /**örnek**|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} /entities/ {Entityıd} /**önerisi**|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} /intents/ {intentId} /**önerisi**|


## <a name="create-app-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından uygulaması oluşturma
[Önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) önceden tanımlanmış etki alanı modeli sağlar. Önceden oluşturulmuş etki alanları HALUK uygulamanız ortak etki alanları için çabucak geliştirmelerine olanak verir. Bu API, önceden oluşturulmuş bir etki alanına dayalı yeni bir uygulama oluşturmanıza olanak sağlar. Yanıt yeni AppID ' dir.

|v2 yol|Fiil|
|--|--|
|/luis/api/v2.0/Apps/customprebuiltdomains  |GET, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Al|

## <a name="importing-1x-app-into-2x"></a>1.x uygulama 2.x içeri aktarma
Dışarı aktarılan 1.x uygulamanın JSON içine almadan önce değiştirmek için gereken bazı alanlar vardır [HALUK] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar 
[Önceden oluşturulmuş varlıklar](Pre-builtEntities.md) değiştirilmiştir. V2 kullandığınızdan emin olun önceden oluşturulmuş varlıklar. Bu kullanmayı da içeren [datetimeV2](pre-builtentities.md?#use-a-prebuilt-datetimev2-entity), datetime yerine. 

### <a name="actions"></a>Eylemler
Eylemler özelliği artık geçerli değil. Boş olmalıdır 

### <a name="labeled-utterances"></a>Etiketli utterances
V1 başına veya bir sözcük veya tümcecik sonunda boşluklar etiketli utterances izin verilir. Boşluk kaldırıldı. 

## <a name="common-reasons-for-http-response-status-codes"></a>HTTP yanıtı durum kodları yaygın nedenler
Bkz: [HALUK API yanıt kodları](luis-reference-response-codes.md).

## <a name="next-steps"></a>Sonraki adımlar

Varolan REST güncelleştirmek için v2 API belgelerine çağırır LIUS için kullanım [endpoint](https://aka.ms/luis-endpoint-apis) ve [yazma](https://aka.ms/luis-authoring-apis) API'leri. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions