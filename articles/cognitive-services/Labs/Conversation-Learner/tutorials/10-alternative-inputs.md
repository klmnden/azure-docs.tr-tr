---
title: Konuşma Öğrenici - Microsoft Bilişsel hizmetler ile diğer girişler kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici diğer girişler kullanmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 383c063e1ba7a29986e4b1c48024072799234414
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387939"
---
# <a name="how-to-use-alternative-inputs"></a>Nasıl alternatif girişlerini kullan

Bu öğretici, kullanıcı konuşma öğretim arabiriminde için alternatif giriş alanı kullanmayı gösterir.

## <a name="video"></a>Video

[![Alternatif girişleri öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Hangi kullanıcı eğitimi iletişim kutusunda belirli bir noktada etti alternatif, anlamsal olarak eşdeğer kullanıcı konuşma alternatif girişlerdir. Bu alternatif girişleri daha sıkı bir şekilde konuşma çeşitleri eğitim iletişim kutuları her varyasyonu adres gerek kalmadan belirtmenizi sağlar.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Model oluşturma

1. Web kullanıcı Arabiriminde "Yeni modeli."'a tıklayın.
2. "AlternativeInputs" "Name" alanına yazın ve ENTER tuşuna basın.
3. "Oluştur" düğmesine tıklayın.

### <a name="entity-creation"></a>Varlık oluşturma

1. Sol panelde, "Varlık" sonra "Yeni varlık" düğmesine tıklayın.
2. "Özel olarak eğitilen" seçin "Varlık türü."
3. "City", "Varlık adı." yazın
4. "Oluştur" düğmesine tıklayın.

![](../media/T10_actions.png)

Şimdi, üç eylem oluşturalım.

### <a name="create-the-first-action"></a>İlk Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "City hangi?" yazın
3. "City" "Kullanıcı yanıtı beklenen varlık" alanına yazın
4. "City" "Eleyerek sağlar" alanına yazın
5. "Oluştur" düğmesine tıklayın.

![](../media/T10_action_create_1.png)

### <a name="create-the-second-action"></a>İkinci Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "$city, hava durumu, büyük olasılıkla güneşli sağlıyor." yazın
3. "Oluştur" düğmesine tıklayın.

![](../media/T10_action_create_2.png)

### <a name="create-the-third-action"></a>Üçüncü Eylem oluştur

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "için hava durumu sormayı deneyin." yazın
3. "City" "Eleyerek sağlar" alanına yazın
4. "Oluştur" düğmesine tıklayın.

![](../media/T10_action_create_3.png)

Artık üç eylem var.

![](../media/T10_actions.png)

### <a name="train-the-model"></a>Modeli eğitme

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "hava durumu nedir?" türü
3. "Puan Eylemler" düğmesine tıklayın.
4. "City hangi?" yanıt seçin
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Denver" yazın
6. "Puan Eylemler" düğmesine tıklayın.
7. Yanıtı seçin, "hava durumu, Denver, büyük olasılıkla güneşli sağlıyor."
8. "Değişiklikleri Gönder" düğmesine tıklayın.

![](../media/T10_training_1.png)

Şimdi başka bir eğitimi iletişim oluşturarak daha fazla modeli eğitin.

### <a name="second-model-train-dialog"></a>İkinci modeli eğitme iletişim

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Ne yapabilirsiniz?" türü
3. "Puan Eylemler" düğmesine tıklayın.
4. Yanıtı seçin, "için hava durumu sormayı deneyin."
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Seattle hava durumu nedir?" türü
6. "Seattle" ı "city" varlık listesinden tıklatın.
7. "Puan Eylemler" düğmesine tıklayın.
8. Yanıtı seçin, "Seattle hava durumu, büyük olasılıkla güneşli sağlıyor."
9. "Değişiklikleri Gönder" düğmesine tıklayın.

![](../media/T10_training_2.png)

### <a name="third-model-train-dialog-using-alternative-input"></a>Alternatif girişini kullanarak üçüncü modeli eğitme iletişim

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "help" yazın
3. "Puan Eylemler" düğmesine tıklayın.
    - Model için en iyi seçenek, belirsiz olduğundan varsayılan olarak yüksek yüzdelik dilim seçmelisiniz.
4. "Öğretim İptal" düğmesine ve ardından "Onayla" düğmesine tıklayın.

![](../media/T10_training_3.png)

Şimdi daha iyi alternatif girişlerini kullanarak sistem ayarlayın. Öğretim ya da üzeri, alternatif bir giriş ekleyebilirsiniz.

1. Sol paneldeki "Train iletişim kutuları"'a tıklayın ve ardından "Ne yapabilirsiniz?" Train iletişim kutuları listesinden.
1. "Ne yapabilirsiniz?" tıklayın Sohbet panelinde utterance.
1. "İçinde alternatif Girişi Ekle..." alanı, "help" yazın ve isabet girin.
1. "Değişiklikleri Kaydet" düğmesine tıklayın.

![](../media/T10_training_4.png)

Houston işlemek için başka bir alternatif giriş ekleyelim.

1. "Seattle hava durumu nedir?" tıklayın Sohbet panelinde utterance.
1. Alan "alternatif giriş Ekle", "Tahmin için Houston" türü ve ENTER tuşuna basın.
   - Hata iletisi vurgular olgu alternatif girişleri, anlamsal olarak eşdeğer ve özgün utterance olarak aynı varlıkları içerir; yalnızca aynı değerleri varlık. Aynı varlıkları varlığını gereklidir.
1. "Houston üzerinde"'a tıklayın ve "city" varlık listesinden seçin.
1. Alan "alternatif giriş Ekle", "Tahmin için Seattle" türü ve ENTER tuşuna basın.
1. "Seattle" üzerinde tıklayın ve "city" varlıkları listeden seçin.
1. "Değişiklikleri Kaydet" düğmesine tıklayın.
1. "Kaydet Düzenle" düğmesine tıklayın.

![](../media/T10_training_5.png)

### <a name="testing-the-model"></a>Model test etme

1. Sol panelde "Günlük iletişim kutuları" sonra "Yeni günlük iletişim kutusu." tıklayın
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "bana help" yazın
3. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Denver tahminini" yazın

![](../media/T10_logdialog.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Günlük iletişim kutuları](./11-log-dialogs.md)
