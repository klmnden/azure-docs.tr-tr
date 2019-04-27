---
title: Konuşma Öğrenici Eylemler - Microsoft Bilişsel hizmetler, "Beklenen varlık" özelliğinin nasıl kullanılacağını | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modelin "Beklenen varlık" özelliği kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 01d991cff9b7f7a66740f86e537833ffe4e862c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707941"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Eylemler "Beklenen varlık" özelliğini kullanma

Bu öğreticide "Beklenen varlık" özelliğini eylemleri gösterir.

## <a name="video"></a>Video

[![Beklenen varlık öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_ExpectedEntity_Preview)](https://aka.ms/cl_Tutorial_v3_ExpectedEntity)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Bu eylem bir varlık için kullanıcının yanıt kaydetmek için eylem "bekleniyor varlık" özelliğini kullanın.

Varlıklar eylemi "Beklenen varlık" özelliğine eklerken, sistem olur:

1. Varlık ayıklama modeli tabanlı makine öğrenimi kullanarak varlıkları eşleşen çalışılıyor tarafından başlama
2. Tüm kullanıcı utterance için buluşsal yöntemler üzerinde hiçbir varlıkların bulunması durumunda temel $entity atayın
3. Çağrı `EntityDetectionCallback`ve eylem seçimi için devam edin.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "ExpectedEntities" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

### <a name="entity-creation"></a>Varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Özel olarak eğitilen" seçin "Varlık türü."
3. "Varlık adı." "name" yazın
4. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> 'Özel eğitilen' varlık türü bu varlık, diğer varlık türlerinden farklı olarak düşünürler anlamına gelir.

![](../media/tutorial4_entities.PNG)

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "Adınız ne?" yazın
3. "Name" "Varlıklar bekleniyor" alanına yazın
4. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> Bu eylem seçilirse algılandı ve kullanıcının yanıt ayıklanır varlıkları "name" varlığı için kaydedilir. Varlık yok algılanmazsa, tüm yanıt bu varlığa kaydedilir.

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. Alan "Botun yanıtta...", "Hi $name!" yazın
3. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> "name" varlık, bir "gerekli varlıkları" başvuruya göre yanıt olarak otomatik olarak eklendi.

Artık iki eylem var.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde türü "hi."
    - Bu, konuşma kullanıcı tarafı benzetimini yapar.
3. "Puan Eylemler" düğmesine tıklayın.
4. "Adınız ne?" yanıt seçin
    - "Hi $name!" Bu yanıt "name" modelin bellekte artık tanımlanması için varlık gerektirdiği yanıt seçilemez.
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Ferdi" yazın
    - Varlık olarak yanıt kaydetmek için önceki ayarlar buluşsal göre bir varlık olarak "Ferdi" vurgulanır.
6. "Puan Eylemler" düğmesine tıklayın.
    - "Hello $name" eylemi bir eylem olarak seçilebilir, bu nedenle "name" varlığı şimdi modelin bellekte "Ferdi" tanımlanır.
7. "Hi $name!" yanıt seçin
8. "Kaydet" düğmesine tıklayın.

Alternatif girişleri başka trenler Model ekleme.

1. "Jose istiyorum." "alternatif giriş Ekle" alanına yazın.
    - Varlığın değeri olarak tüm metin bloğunu seçer şekilde Model adı bir varlık olarak algılamaz
2. "Jose istiyorum" deyimini tıklatın ve sonra çöp kutusu simgesine tıklayın.
3. "Jose üzerinde", ardından varlık listesinde "name"'ı tıklatın.
4. Puan eylemleri tıklayın.
5. "Merhaba Frank!" yanıt seçin
6. "Kaydet" düğmesine tıklayın.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Değişkeni yoksayılamaz varlıklar](./06-negatable-entities.md)
