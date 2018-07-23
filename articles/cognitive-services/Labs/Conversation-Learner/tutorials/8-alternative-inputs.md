---
title: Konuşma Öğrenici - Microsoft Bilişsel hizmetler ile diğer girişler kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici diğer girişler kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8d3b3f419ceacbb9a6fe2b19cf68ea6873de536f
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171026"
---
# <a name="how-to-use-alternative-inputs"></a>Nasıl alternatif girişlerini kullan

Bu öğreticide, öğretim arabiriminde kullanıcı girişi için "diğer girişler" alanının kullanmayı gösterir.

## <a name="video"></a>Video

[![Öğretici 8 Preview](http://aka.ms/cl-tutorial-08-preview)](http://aka.ms/blis-tutorial-08)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
"Diğer", kullanıcı bir eğitim iletişim kutusunda belirli bir noktada etti ve farklı kullanıcı konuşma girişleri. Alternatif girişleri daha sıkı bir şekilde ne bir kullanıcı, her değişim eğitim iletişim kutusunda listelemede zorunda kalmadan diyebilirsiniz, değişimleri belirtmenizi sağlar.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. AlternativeInputs adı girin. Oluştur'a tıklayın.

### <a name="create-an-entity"></a>Bir varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. Varlık adı, Şehir girin.
3. Oluştur’a tıklayın.

### <a name="create-three-actions"></a>Üç eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıtta 'hangi şehirde istiyorsunuz?' yazın.
3. Eleyerek varlıklarda $city girin.
3. Oluştur’a tıklayın

Ardından ikinci bir eylem oluşturun:

1. Eylemler ve ardından yeni bir eylem tıklayın.
3. Yanıtta 'hava durumu $city, büyük olasılıkla güneşli ' yazın.
4. Gerekli varlıkları $city girin.
4. Oluştur’a tıklayın.

Üçüncü bir eylem oluşturun:

1. Eylemler ve ardından yeni bir eylem tıklayın.
3. Yanıtta 'için hava durumu sorma' yazın.
    - Bu kullanıcının sorusuna yanıt gibi olacaktır 'sistem neler yapabilirsiniz?'
4. Eleyerek varlıklarda $city girin.
4. Oluştur’a tıklayın

Artık üç eylem var.

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Ne olduğunu hava durumu' yazın.
3. Puan Eylemler ve 'hangi şehirde istiyorsunuz?' seçin
2. 'Denver' girin.
3. 'Üzerinde denver' ne çift tıklayın ve şehir seçin.
    - Bu, bir şehir varlık olarak işaretler.
5. Puan Eylemler
    - 'denver' Şehir varlık içinde artık yok. 
6. 'Hava durumu $city, büyük olasılıkla güneşli ' seçin.
7. Öğretim Bitti'ye tıklayın.

Başka bir örnek iletişim ekleyin:

1. Yeni Eylem ve ardından yeni Train iletişim tıklayın.
2. 'Ne yapabilirsiniz?' yazın.
3. Puan Eylemler ve 'için hava durumu sorma' seçin
2. 'Ne olduğunu hava durumu Seattle' girin.
3. 'Seattle' üzerinde'ne çift tıklayın ve şehir seçin.
    - Bu, bir şehir varlık olarak işaretler.
5. Puan Eylemler
    - 'seattle' Şehir varlık içinde artık yok. 
6. 'Hava durumu $city, büyük olasılıkla güneşli ' seçin.
7. Öğretim Bitti'ye tıklayın.

Ne olacağını görelim kullanıcı için yukarıdaki anlamsal olarak benzer bir şey diyorsa:

1. Yeni Eylem ve ardından yeni Train iletişim tıklayın.
2. 'Yardım' yazın.
3. Puan eylemleri tıklayın.
    - İki olası yanıtların puanları çok yakın. Bu bize modeli iki Eylemler arasındaki sınırı hakkında karıştırılır bildirir.
6. Gönderilemeyen tıklayın öğretim ve onaylayın.

![](../media/tutorial8_closescores.png)

Bu durumda, iletişim kutuları için alternatif bir giriş eklemek yardımcı olacaktır. Öğretim yaptığınız gibi bunları ekleyebilirsiniz. Ayrıca, geri dönün ve daha sonra ekleyebilirsiniz.

2. 'Yapabilecekleriniz hakkında?' tıklayın Train iletişim kutularında.
2. İletişim kutusunda 'yapabilecekleriniz hakkında?' tıklayın. seçmek için.
    1. Sağ bölmede, varlık algılama birkaç alternatifleri Ekle alternatif giriş girin:
    1. 'My seçenekleri nelerdir?' girin
    2. 'My seçenekleri bildir' girin.
    3. 'Yardım' girin
    1. Değişiklikleri Gönder'e tıklayın.


![](../media/tutorial8_helpalternates.png)

2. Şimdi 'Seattle hava durumu nedir ' tıklayın.
    1. Ekle alternatif girişinde 'seattle için tahmini' girin.
    2. 'Seattle' üzerinde'ne çift tıklayın ve şehir seçin. Varlıklar için alternatif bir giriş, mevcut olması ve aynı varlık kümelerine sahiptir. Varlık içeriği farklı ise uygundur.
    3. Ekle alternatif girişinde ', bugün beyoğlu Yağmur ' girin.
    4. 'Üzerinde denver' tıklayın ve şehir seçin.
    5. Değişiklikleri Gönder'i tıklatın ve bitti.


İlk iletişim için alternatif girişleri ekleyelim:

1. Train iletişim kutuları'a tıklayın.
2. İletişim kutusunda ' ne hava durumu ile ' başlatılıyor'a tıklayın.
2. Sol bölmede 'ne olduğunu hava durumu' seçmek için tıklayın:
    1. Ekle alternatif girişinde 'hava durumu tahminini' girin.
    2. 'Da Yağmur?' girin
    3. Değişiklikleri Gönder'e tıklayın.
4. Sol bölmede 'denver' seçmek için tıklayın:
    1. Ekle alternatif girişinde 'denver için' girin.
    2. 'Austin için tahmini' girin.
        - Tam tümceciği vurgulanır. İfade, sonra kırmızı x tıklayın. Ardından austin seçip üzerinde Şehir'i tıklatın.
        - Değişiklikleri Gönder'e tıklayın
    1. Modeli yeniden eğitme neden olacak Bitti'ye tıklayın.

![](../media/tutorial8_altcities.png)

Çeşitlemeleri deneyelim:

1. Yeni Train iletişim tıklayın.
2. 'Hangi özellikleri kullanıyorsunuz' yazın.
3. Puan eylemleri tıklayın.
    - Puanları modelinin kesin gösteren sonraki eylemi daha belirleyici sunulmuştur.
2. 'İçin hava durumu sorma' seçin.
6. Öğretim bitti seçeneğine tıklayın

Artık görülen nasıl alternatif girişleri kullanıcı söylediğiniz başka şeyler belirtmek için kullanılabilir. Bunlar, birçok yolla aynı, bunları tek bir iletişim kutusuna daraltma ve hangi kullanıcı diyebilirsiniz numaralandırma olan birçok iletişim kutuları oluşturma önlemenize yardımcı.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Günlük iletişim kutuları](./9-log-dialogs.md)
