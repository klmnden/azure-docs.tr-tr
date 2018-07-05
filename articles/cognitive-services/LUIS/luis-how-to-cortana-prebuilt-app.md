---
title: Kullanım Cortana önceden derlenmiş uygulama luıs'den | Microsoft Docs
description: Cortana kişisel yardımcı, önceden oluşturulmuş bir uygulama gelen Language Understanding Intelligent Services (LUIS) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/03/2018
ms.author: v-geberr
ms.openlocfilehash: 27900068d4420b3e70dacc6d1bdfdf0c52053ceb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767313"
---
# <a name="cortana-prebuilt-app"></a>Önceden oluşturulmuş Cortana uygulamasını

> [!IMPORTANT]
> Kullanmanızı öneririz [önceden oluşturulmuş etki alanları](./luis-how-to-use-prebuilt-domains.md), önceden oluşturulmuş Cortana uygulamasını yerine. Örneğin, yerine, **builtin.intent.calendar.create_calendar_entry**, kullanın **Calendar.Add** gelen **Takvim** önceden oluşturulmuş etki alanı.
> Önceden oluşturulmuş etki alanları, aşağıdaki avantajları sağlar: 
> * Önceden oluşturulmuş ve kullanan hedefleri ve birbiriyle iyi çalışmak üzere tasarlanmış varlıkları içeren sağlarlar. Önceden oluşturulmuş bir etki alanı doğrudan uygulamanızın içine tümleştirebilirsiniz. Uygunluk İzleyicisi derliyorsanız, örneğin, ekleyebilirsiniz **uygunluk** etki alanı ve hedefleri ve uygunluk etkinlikleri ağırlık ve yemek planlama, kalan süre İzleme amaçları dahil olmak üzere, izleme için varlık kümesinin tamamını veya uzaklığı ve kaydetme uygunluk etkinlik notları.
> * Önceden oluşturulmuş etki alanı hedefleri özelleştirilebilir. Hotels gözden geçirmeler sağlamak istiyorsanız, örneğin, eğitim ve özelleştirebileceğiniz **Places.GetReviews** gelen hedefi **yerler** otel incelemeleri istekleri tanımak için etki alanı.
> * Önceden oluşturulmuş etki alanları genişletilebilir. Örneğin, kullanmak istediğiniz **yerler** önceden oluşturulmuş etki alanında restoranlar ve cuisine türünü almak için bir gereksinim için arayan bir bot oluşturun ve eğitme bir **Places.GetCuisine** hedefi.

Kendi uygulamalarınızı oluşturmanıza olanak tanıyan ek olarak, LUIS de amaç ve varlıkları Microsoft Cortana kişisel yardımcı'dan önceden oluşturulmuş bir uygulama olarak sağlar. Bu genel kullanıma açık LUIS uygulaması davranışını değiştirilemez. Hedefleri ve bu uygulamayı varlıklarda düzenlenemez veya başka LUIS uygulamalara tümleşiktir. İstemci uygulamanızı hem önceden oluşturulmuş bu uygulama hem de kendi LUIS uygulamanızı erişmesini isterseniz, istemci uygulamanızın her iki LUIS uygulama başvuru gerekir.

Önceden oluşturulmuş kişisel yardımcı uygulama (yerel) bu kültürler tarafından kullanılabilir: İngilizce, Fransızca, İtalyanca, İspanyolca ve Çince.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Uç nokta için önceden oluşturulmuş Cortana uygulamasını edinin

Aşağıdaki uç noktaların kullanarak önceden oluşturulmuş Cortana uygulamasını erişebilirsiniz: 

| Dil | Uç Nokta|
|--------| ------------------|
| Türkçe| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Çince| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Fransızca | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    İspanyolca | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    İtalyanca| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


Uç nokta URL'leri web'da ayrıca [uygulamalar - uygulamalar kişisel yardımcı alma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>Kişisel yardımcı uygulamayı inceleyin
Utterance yorumlamak isterseniz bu utterance uç nokta URL'si ekleme sonra Örneğin, "ekip toplantısı için bir randevuya", oluşturmaktır. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key=YOUR-SUBSCRIPTION-KEY&q=create%20an%20appointment%20for%20team%20meeting
```

URL'yi bir web tarayıcısına yapıştırın ve uç nokta anahtarınızı yerine `YOUR-SUBSCRIPTION-KEY` alan.

Tarayıcıda önceden oluşturulmuş Cortana uygulamasını tanımladığını görebilirsiniz `builtin.intent.calendar.create_calendar_entry` hedefi olarak ve `builtin.calendar.title` utterance yanı sıra, varlık türü olarak `create an appointment for team meeting`.

```JSON
{
  "query": "create an appointment for team meeting",
  "topScoringIntent": {
    "intent": "builtin.intent.calendar.create_calendar_entry"
  },
  "entities": [
    {
      "entity": "team meeting",
      "type": "builtin.calendar.title"
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş Cortana uygulamasını başvurusu](./luis-reference-cortana-prebuilt.md)

