---
title: Nasıl iletişim kutuları log bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: İletişim kutuları bir konuşma Öğrenici modelde günlüğe kaydetme hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 4663fc377e795e603bd2484ec4cf98578408501f
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248390"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>İletişim kutuları bir konuşma Öğrenici modelde günlüğe kaydetme hakkında

Bu öğreticide, son kullanıcı konuşma Öğrenici arabiriminden test yapmak gösterilir; iletişim kutuları nasıl kaydedilir; ve nasıl düzeltme yapılacağını modelinizi iyileştirmek için iletişim kutularını günlüğe.

## <a name="video"></a>Video

[![Öğretici 9 Preview](http://aka.ms/cl-tutorial-09-preview)](http://aka.ms/blis-tutorial-09)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Günlük iletişim kutuları, gözden geçirmek ve gerçekleştirilen son kullanıcılarla iletişim kutuları düzeltmeler yapmak için kullanabilirsiniz.  Özellikle, varlık etiketleri ve eğitilen model ve genel sistem performansını artırmak için eylem seçimleri düzeltebilirsiniz. 

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. LogDialogs adı girin. Oluştur'a tıklayın.

### <a name="create-an-entity"></a>Bir varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. Varlık adı, Şehir girin.
3. Oluştur’a tıklayın.

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıtta 'Hangi şehirde?' yazın.
3. Eleyerek varlıklarda $city girin.
3. Oluştur’a tıklayın

Ardından ikinci bir eylem oluşturun:

1. Eylemler ve ardından yeni bir eylem tıklayın.
3. Yanıtta 'hava durumu $city, büyük olasılıkla güneşli ' yazın.
4. Gerekli varlıkları $city girin.
4. Oluştur’a tıklayın.

Artık iki eylem var.

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Ne olduğunu hava durumu' yazın.
3. Puan Eylemler ve "City hangi?" seçin
2. 'Seattle' girin.
3. 'Seattle' üzerinde'ne çift tıklayın ve şehir seçin.
    - Bu, bir şehir varlık olarak işaretler.
5. Puan Eylemler
6. 'Hava durumu $city, büyük olasılıkla güneşli ' seçin.
7. Öğretim Bitti'ye tıklayın.

Başka bir örnek iletişim ekleyin:

1. Train iletişim kutuları ve ardından yeni Train iletişim tıklayın.
2. 'Seattle hava durumu nedir?' yazın. Varlık olarak etiketlenmiş Seattle dikkat edin.
5. Puan Eylemler 
6. 'Hava durumu $city, büyük olasılıkla güneşli ' seçin.
7. Öğretim Bitti'ye tıklayın.

### <a name="try-the-bot-as-the-user"></a>Bot, bir kullanıcı olarak deneyin
Şimdi Biz bu bot kullanıcılara dağıttığınızdan düşünün.

1. Günlük iletişim kutuları'a tıklayın.
2. Yeni günlük iletişim tıklayın.
    - Kullanıcı kullanıcı arabiriminin sol taraftaki web Sohbeti denetimi olarak deneyimleyeceği gibi bu bot sunar. Sağ taraftaki boşluk alan yoksayabilirsiniz.
3. 'Hello' yazın.
4. Bot yanıt: "hangi city?"
4. Türü 'Boston'.
5. Bot yanıt: "hangi city?"
    - Doğru görünüyor. Bu nedenle şimdi bu iletişim kutusunu kaydedin.
2. Yapılan test tıklayın.

Yeni bir oturum başlayalım:

2. Yeni günlük iletişim tıklayın.
3. 'Boston için tahmin' türü.
4. Bot yanıt: "hangi city?"
2. Yapılan test tıklayın.

Şimdi ikinci iletişim düzeltmeleri olalım:

1. 'İçin hava durumu tahminini Boston' günlük iletişim kutuları altında tıklayın.
    - Bu konuşma açar.
    - Konuşma kullanıcı tarafında tıklatırsanız ('tahminde Boston için' burayı), varlık etiketleri değiştirebilirsiniz.
    - Sistem tarafında tıklarsanız ("city hangi" üzerinde burada), eylemi seçili değiştirebilirsiniz.
5. 'Boston tahminini' tıklayın. 
    - Sorunun temel nedenini Boston bir varlık olarak etiketlendi değil, ' dir. Bunu değiştirmek mi ihtiyacımız var.
    - 'Üzerinde Boston' ne çift tıklayın, sonra da şehir seçin.
    - Değişiklikleri Gönder'i tıklatın ve Kaydet'e tıklayın. Bu, yaptığınız değişikliklere dayalı bir eğitim iletişim kutusu oluşturmak ve eğitim iletişim kutuları, yaptığınız değişikliği noktasında içine bırak.
6. '$City, hava durumu, büyük olasılıkla güneşli değil.' seçin
7. Öğretim Bitti'ye tıklayın. Artık Git eğitimi iletişim kutuları için yeni eylem eklenir görürsünüz.

![](../media/tutorial9_logdiag1.PNG)

Şimdi diğer iletişim düzeltmeleri olalım:

1. Günlük iletişim kutuları altında 'hello' tıklayın.
    - Bu konuşma açar.
3. 'Hello' yanıta "city hangi" ' dir. Ancak daha anlamlı bir şey için değiştirmek istiyoruz. Daha iyi bir yanıt bir şey olacaktır 'Merhaba, ben hava durumu bot' ister. Ancak bir tane oluşturmanız gerekir, böylece yapan bir eylem yok.
4. Eylemini tıklatın.
    - Yanıtta türü ' hava durumu bot istiyorum. Tahminlerini yardımcı olabilir.'
6. Kaldırma onay bekleme olmayan eylem olun yanıt onay kutusu tamamlanmasını bekleyin.
7. Oluştur’a tıklayın.
8. Ardından bu yeni bir eylem seçmek için tıklayın. Daha sonra Kaydet'e tıklayın.
    - Bu, o noktaya eğitim oturumu getirir.
6. "City hangi?" seçin
7. 'Boston' yazın. Etikete Boston bir varlık olarak değilse zaten çift tıklayın.
8. Puan eylemleri tıklayın.
9. '$City, hava durumu, büyük olasılıkla güneşli değil.' seçmek için tıklayın
10. Öğretim Bitti'ye tıklayın.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık algılama geri çağırma](./10-entity-detection-callback.md)
