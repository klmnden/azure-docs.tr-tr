---
title: Kullanım Cortana önceden oluşturulmuş uygulamadan HALUK | Microsoft Docs
description: Cortana kişisel Yardımcısı, önceden oluşturulmuş bir uygulama gelen dil anlama akıllı Hizmetleri (HALUK) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: b792d090d037ef180258a1634d4bd063c0a71b9a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353320"
---
# <a name="cortana-prebuilt-app"></a>Cortana önceden oluşturulmuş uygulama

> [!IMPORTANT]
> Kullanmanızı öneririz [önceden oluşturulmuş etki alanları](./luis-how-to-use-prebuilt-domains.md), Cortana önceden oluşturulmuş uygulama yerine. Örneğin, yerine, **builtin.intent.calendar.create_calendar_entry**, kullanın **Calendar.Add** gelen **Takvim** önceden oluşturulmuş bir etki alanı.
> Önceden oluşturulmuş etki alanları aşağıdaki avantajları sağlar: 
> * Önceden oluşturulmuş ve pretrained hedefleri ve iyi birbirleri ile çalışmak üzere tasarlanmış varlıkları paketlerini sağlarlar. Önceden oluşturulmuş bir etki alanı doğrudan uygulamanızın içine tümleştirebilirsiniz. Örneğin, bir formda kalma İzleyicisi oluşturuyorsanız ekleyebileceğiniz **uygunluk** etki alanı ve amaçları ve uygunluk etkinlikleri amaçlar ağırlığını ve yemek planlama, kalan süre izleme de dahil olmak üzere, izleme için varlık kümesinin tamamını veya uzaklığı ve kaydetme uygunluk etkinlik notları.
> * Önceden oluşturulmuş bir etki alanı hedefleri özelleştirilebilir. Oteller değerlendirmelerini sağlamak istiyorsanız, örneğin, eğitmek özelleştirmek ve **Places.GetReviews** gelen hedefi **yerler** otel incelemeler isteklerinde tanımak için etki alanı.
> * Önceden oluşturulmuş etki alanları genişletilebilir. Kullanmak istiyorsanız, örneğin, **yerlerde** önceden oluşturulmuş bir etki alanında Restoran ve cuisine türü alınırken amacına gereksinimini arar bir bot, yapı ve eğitim bir **Places.GetCuisine** hedefi.

Kendi uygulamalar oluşturmanıza olanak tanıyan ek olarak, HALUK ayrıca hedefleri ve varlıkları Microsoft Cortana kişisel yardımcı'dan önceden oluşturulmuş bir uygulama olarak sağlar. Bu genel kullanıma açık HALUK uygulamanın davranışı değiştirilemez. Hedefleri ve bu uygulamada varlıkları düzenlenemez veya diğer HALUK uygulamaları tümleştirilmiştir. Önceden oluşturulmuş bu uygulama ve kendi HALUK uygulama erişimi için istemci uygulamanız istediğinde, istemci uygulamanız, hem HALUK uygulamaları başvuru gerekir.

Önceden oluşturulmuş kişisel yardımcı uygulama bu kültürler (yerel) kullanılamaz: İngilizce, Fransızca, İtalyanca, İspanyolca ve Çince.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Uç nokta için Cortana önceden oluşturulmuş uygulamayı Al

Cortana önceden oluşturulmuş uygulamayı şu uç noktalar kullanarak erişebilirsiniz: 

| Dil | Uç Nokta|
|--------| ------------------|
| Türkçe| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Çince| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Fransızca | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    İspanyolca | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    İtalyanca| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


> [!NOTE]
> URL'leri kullanılabilir de uç nokta [uygulamalar - uygulamalar kişisel Yardımcısı almak](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>Kişisel yardımcı uygulama ölçeğinizi deneyin
Uç noktasını çağırmak için uç noktaya abonelik anahtar bağımsız değişkeni ve sorgu dizenizi ekleyebilirsiniz. 

Yorumlamak istediğiniz utterance uç nokta URL'si bu utterance ekleyebilirsiniz sonra Örneğin, "team toplantı randevu oluşturma" ise. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key={YOUR-SUBSCRIPTION-KEY}&q=create an appointment for team meeting
```

Bir web tarayıcısına URL'sini yapıştırın ve için abonelik anahtarınızı yerine `{YOUR-SUBSCRIPTION-KEY}` alan.

Tarayıcıda Cortana önceden oluşturulmuş uygulama tanımlayan görebilirsiniz `builtin.intent.calendar.create_calendar_entry` hedefi olarak ve `builtin.calendar.title` utterance yanı sıra, varlık türü olarak `create an appointment for team meeting`.

![Cortana önceden oluşturulmuş bir uygulama kullanma](./media/luis-how-to-prebuilt-cortana/luis-prebuilt-cortana-browser.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Cortana önceden oluşturulmuş uygulama başvurusu](./luis-reference-cortana-prebuilt.md)

