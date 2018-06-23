---
title: Alternatif girişleri konuşma öğrenen - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Alternatif girişleri konuşma öğrenen ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2214436b193932e5b3b80c190f7754a0436b7ed8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354119"
---
# <a name="how-to-use-alternative-inputs"></a>Alternatif girişleri kullanma

Bu öğreticide "alternatif girişler" alanının öğretme arabiriminde kullanıcı girişi için nasıl kullanılacağı gösterilmiştir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
"Diğer" kullanıcı eğitimi iletişim belirli bir noktada etti diğer kullanıcı utterances girdileridir. Alternatif girişleri ne bir kullanıcı, bir eğitim iletişim kutusunda her bir çeşidine listesinde gerek kalmadan diyebilirsiniz, Çeşitlemeler daha sıkı şekilde belirtmenizi sağlar.

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. AlternativeInputs adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-an-entity"></a>Bir varlık oluşturun

1. Varlıklar, yeni varlık tıklatın.
2. Varlık adı Şehir girin.
3. Oluştur’a tıklayın.

### <a name="create-three-actions"></a>Üç eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta 'hangi Şehir, istiyor musunuz?' yazın.
3. Adayını varlıklarda $city girin.
3. Oluştur’a tıklayın

Sonra ikinci bir eylem oluşturun:

1. Eylemler, yeni bir eylem tıklatın.
3. Yanıtta ' $city hava büyük olasılıkla güneşli ' yazın.
4. Gerekli varlıkları $city girin.
4. Oluştur’a tıklayın.

Üçüncü eylem oluşturun:

1. Eylemler, yeni bir eylem tıklatın.
3. Yanıtta 'hava durumu için isteyen deneyin' yazın.
    - Bu kullanıcının soruya yanıt gibi olacaktır 'sistem ne?'
4. Adayını varlıklarda $city girin.
4. Oluştur’a tıklayın

Artık üç eylem vardır.

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hava durumu nedir' yazın.
3. Puan Eylemler'i tıklatın ve 'hangi Şehir, istiyor musunuz?' seçin
2. 'Beyoğlu' girin.
3. 'Beyoğlu' üzerinde çift tıklayın ve şehir seçin.
    - Bu, bir şehir varlık olarak işaretler.
5. Puan Eylemler
    - Beyoğlu Şehir varlıkta mevcut olduğunu unutmayın. 
6. ' $City hava büyük olasılıkla güneşli ' seçin.
7. Öğretme Bitti'yi tıklatın.

Başka bir örnek iletişim ekleyin:

1. Yeni eylemi ve ardından yeni tren iletişim'ı tıklatın.
2. 'Ne yapabilirsiniz?' yazın.
3. Puan Eylemler'i tıklatın ve 'hava durumu için isteyen deneyin' seçin
2. 'Seattle'da hava durumu nedir' girin.
3. 'Üzerinde seattle' çift tıklatın ve şehir seçin.
    - Bu, bir şehir varlık olarak işaretler.
5. Puan Eylemler
    - Seattle Şehir varlıkta mevcut olduğunu unutmayın. 
6. ' $City hava büyük olasılıkla güneşli ' seçin.
7. Öğretme Bitti'yi tıklatın.

Neler görelim kullanıcı için yukarıdaki anlamsal olarak benzer bir şey diyorsa:

1. Yeni eylemi ve ardından yeni tren iletişim'ı tıklatın.
2. 'Help' yazın.
3. Puan Eylemler'i tıklatın.
    - İki olası yanıtları puanlarını çok yakın olduğunu unutmayın. Bu bize modeli iki eylemler arasında sınır hakkında karıştırılır bildirir.
6. Abandon tıklatın eğitme ve onaylayın.

![](../media/tutorial8_closescores.png)

Bu durumda, iletişim kutuları için alternatif girişleri eklemek yardımcı olacaktır. Öğretme yapmakta olduğunuz gibi bunları ekleyebilirsiniz. Ayrıca, geri dönün ve daha sonra ekleyebilirsiniz.

2. 'Neler yapabileceğiniz hakkında?' tıklayın Tren iletişim kutularında.
2. İletişim kutusunda, 'neler yapabileceğiniz hakkında? ''ı tıklatın. seçmek için.
    1. Sağ bölmede, varlık algılama, birkaç alternatifleri Ekle alternatif girişinde girin:
    1. 'My seçenekleri nelerdir?' girin
    2. 'My seçimler ver' girin.
    3. 'Yardımcı' girin
    1. Değişiklikleri Gönder'i tıklatın.


![](../media/tutorial8_helpalternates.png)

2. Şimdi 'Seattle'da hava durumu nedir ' seçeneğini tıklatın.
    1. Ekle alternatif girişinde 'seattle için tahmini' girin.
    2. 'Üzerinde seattle' çift tıklatın ve şehir seçin. Varlıklar için alternatif girişleri olmalı ve aynı varlık kümesine sahip olduğunu unutmayın. Varlıklarına genel içeriğini farklı olması durumunda sorun yoktur.
    3. Ekle alternatif girişinde ', bugün beyoğlu'sizi ' girin.
    4. 'Beyoğlu' üzerinde'ı tıklatın ve şehir seçin.
    5. Değişiklikleri Gönder'i tıklatın ve yapılır.


İlk iletişim için alternatif girişleri ekleyelim:

1. Tren iletişim kutuları'ı tıklatın.
2. ' Ne hava durumu ile ' başlatılıyor iletişim tıklayın.
2. Sol bölmede 'hava durumu nedir' seçmek için tıklatın:
    1. Ekle alternatif girişinde 'hava tahmini' girin.
    2. 'Da sizi?' girin
    3. Gönderme değişiklikleri'i tıklatın.
4. Sol bölmede 'beyoğlu' seçmek için tıklatın:
    1. Ekle alternatif girişinde 'için beyoğlu' girin.
    2. 'Ankara'da için tahmini' girin.
        - Tam tümcecik vurgulanır unutmayın. Tümcecik sonra kırmızı x'i tıklatın. Ardından Ankara'da seçin ve üzerinde Şehir'i tıklatın.
        - Gönderme değişiklikler'e tıklayın
    1. Yeniden eğitme modelini neden olacak Bitti'yi tıklatın.

![](../media/tutorial8_altcities.png)

Çeşitlemeleri deneyelim:

1. Yeni tren iletişim'ı tıklatın.
2. 'Ne yetenekleri olduğunuz' yazın.
3. Puan Eylemler'i tıklatın.
    - Puanları model çalışılarak gösteren sonraki eylemini daha belirleyici olduğunu unutmayın.
2. 'Hava durumu için isteyen deneyin' seçin.
6. Öğretme Bitti'yi tıklatın

Artık görülen nasıl alternatif girişleri kullanıcı etti başka şeyler belirtmek için kullanılabilir. Birçok yolla için aynı olan, tek bir iletişim kutusuna daraltma ve hangi kullanıcı diyebilirsiniz numaralandırma birçok iletişim kutuları oluşturma önlemenize yardımcı olurlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Günlük iletişim kutuları](./9-log-dialogs.md)
