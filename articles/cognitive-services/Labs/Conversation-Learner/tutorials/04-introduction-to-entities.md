---
title: Konuşma Öğrenici sahip bir Model - Microsoft Bilişsel hizmetler varlık kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle varlıkları kullanmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 3d9e2498a23ad49eb014cb0f81c819f3f63eef5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387819"
---
# <a name="introduction-to-entities"></a>Varlıkları giriş

Bu öğretici, varlıklar, varlıkların eleyerek, gerekli varlıkları ve kullanımlarını konuşma Öğrenici içinde tanıtır.

## <a name="video"></a>Video

[![Varlıkları öğretici Önizleme giriş](https://aka.ms/cl_Tutorial_v3_IntroEntities_Preview)](https://aka.ms/cl_Tutorial_v3_IntroEntities)

## <a name="requirements"></a>Gereksinimler

Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Varlıkları kendi görevi, ayıklama kullanıcı konuşma veya özel kod tarafından atama yoluyla gerçekleştirmek için robot gereken bilgileri yakalayın. Varlıkları kendileri de olan açıkça sınıflandırıldığı "Gerekli" veya "Eleyerek." olarak tarafından eylemi kullanılabilirliği kısıtlayabilirsiniz

- Modelin bellekte eylemin kullanılabilir çalışması için gerekli varlıkları bulunması gerekir
- Varlıkları eleyerek gerekir *değil* modelin bellek kullanılabilir olması eylem için sırada bulunması

Bu öğreticide, özel varlıklar üzerinde odaklanır. Birden çok değerli, değişkeni yoksayılamaz varlıkları ve programlı varlıkları önceden eğitilmiş, diğer öğreticileri, sunulmuştur.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "IntroToEntities" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

### <a name="entity-creation"></a>Varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Özel olarak eğitilen" seçin "Varlık türü."
3. "City", "Varlık adı." yazın
4. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> 'Özel eğitilen' varlık türü bu varlık, diğer varlık türlerinden farklı olarak düşünürler anlamına gelir.

### <a name="create-the-actions"></a>Eylem oluşturma

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "İstediğiniz hangi şehirde bilmiyorum." yazın
3. "City" "Varlıkları eleyerek" alanına yazın
4. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> "City" varlık "Eleyerek varlıklara" ekleme "city" varlık Botun bellekte tanımlandığında Botun göz önünde bulundurarak bu eylemi elemek.

Şimdi, ikinci bir eylem oluşturun.

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "$city, hava durumu, büyük olasılıkla güneşli sağlıyor." yazın
3. "Oluştur" düğmesine tıklayın.

> [!NOTE]
> "city" varlık yanıt başvuruya göre gerekli varlıkları listesinde otomatik olarak eklendi.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "hello" yazın
    - Bu, konuşma kullanıcı tarafı benzetimini yapar.
3. "Puan Eylemler" düğmesine tıklayın.
4. Yanıtı seçin, "İstediğiniz hangi şehirde bilmiyorum."
5. "Seattle", kullanıcı olarak yanıtlayın.
6. "Puan Eylemler" düğmesine tıklayın.
7. Yanıtı seçin, "$city, hava durumu, büyük olasılıkla güneşli sağlıyor."
8. "Kaydet" düğmesine tıklayın.

![](../media/tutorial3_entities.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Beklenen varlık](./05-expected-entity.md)
