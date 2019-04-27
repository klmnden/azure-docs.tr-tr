---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler varlık algılama geri çağırma kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle varlık algılama geri çağırma kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 576dc6bd44360f73c4133907233e59e5f51837b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707589"
---
# <a name="how-to-use-entity-detection-callback"></a>Varlık algılama geri çağırma kullanma

Bu öğretici, varlık algılama geri çağırma gösterir ve varlıkları çözmek için yaygın bir düzen gösterilmektedir.

## <a name="video"></a>Video

[![Varlık algılama geri çağırma öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_EntityDetection_Preview)](https://aka.ms/cl_Tutorial_v3_EntityDetection)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide gerektiren `tutorialEntityDetectionCallback` bot çalışıyor.

    npm run tutorial-entity-detection

## <a name="details"></a>Ayrıntılar
Varlık algılama geri çağırmaları varlık tanıma davranışlarını ve varlık düzenleme kod aracılığıyla değiştirilmesini sağlar. Biz bu işlev bir tipik varlık algılama geri çağırma tasarım modeli aracılığıyla walking tarafından gösterilecektir. Örneğin, "büyük apple'a" "new york" çözümleniyor.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "EntityDetectionCallback" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

Bu örnekte üç varlıkları gereklidir, üç şimdi oluşturun.

### <a name="create-the-custom-trained-entity"></a>Eğitilen özel varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Özel olarak eğitilen" seçin "Varlık türü."
3. Tür "Şehir" için "Varlık adı."
4. "Oluştur" düğmesine tıklayın.

### <a name="create-the-first-programmatic-entity"></a>İlk programlı varlık oluşturma

1. "Yeni varlık" düğmesine tıklayın.
2. "Varlık türü." "Programlı" seçin
3. Tür "CityUnknown" için "Varlık adı."
4. "Oluştur" düğmesine tıklayın.

### <a name="create-the-first-programmatic-entity"></a>İlk programlı varlık oluşturma

1. "Yeni varlık" düğmesine tıklayın.
2. "Varlık türü." "Programlı" seçin
3. Tür "CityResolved" için "Varlık adı."
4. "Oluştur" düğmesine tıklayın.

Şimdi, üç eylem oluşturun.

### <a name="action-creation"></a>Eylem oluşturma

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "hangi şehirde istiyorsunuz?" yazın
3. "City" "Kullanıcı yanıtı beklenen varlık" alanına yazın
4. "City" "Varlıkları eleyerek" alanına yazın
5. "Oluştur" düğmesine tıklayın.
6. "Yeni Eylem" düğmesine tıklayın.
7. "Botun yanıt..." alanı, "hangi şehirde istiyorsunuz?" yazın
8. "Bu Şehir bilmiyorum." "Kullanıcı yanıtı beklenen varlık" alanına yazın
9. "Oluştur" düğmesine tıklayın.
10. "Yeni Eylem" düğmesine tıklayın.
11. "Botun yanıt..." alanı, "$CityResolved çok güzel." yazın
12. "Oluştur" düğmesine tıklayın.

Geri çağırma kod aşağıdaki gibidir:

![](../media/tutorial10_callbackcode.PNG)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde türü "hi"
3. "Puan Eylemler" düğmesine tıklayın.
4. "Hangi şehirde istiyorsunuz?" yanıt seçin
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "büyük apple" yazın
6. "Puan Eylemler" düğmesine tıklayın.
    - Varlık algılama geri düğmesine tıklayarak tetikler
    - Geri çağırma kod CityResolved varlık değeri doğru "new york" olarak ayarlar.
7. Yanıtı seçin, "new york, çok güzel."

Bu düzen, birçok bot senaryoları normaldir. İş mantığınızı kullanıcı konuşma ve ayıklanan varlıkları sağlanır ve bu mantığı programlı varlıklarının içinde için iletişim kutusunun sonraki kapatır kaydedilmişse kurallı biçimi içine utterance dönüştürür.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturumun geri çağırmaları](./13-session-callbacks.md)
