---
title: İletişim kutuları konuşma öğrenen uygulamada - Microsoft Bilişsel hizmetler günlüğe kaydetme hakkında | Microsoft Docs
titleSuffix: Azure
description: İletişim kutuları bir konuşma öğrenen uygulamasında oturum öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 477545c48aeca05d56fdae28ac65a8f381a482fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354113"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-application"></a>İletişim kutuları bir konuşma öğrenen uygulamasında oturum

Bu öğretici, son kullanıcı konuşma öğrenen arabirimde test yapmak gösterilmektedir; iletişim kutuları nasıl kaydedilir; ve modelinizi geliştirmek için iletişim kutuları için düzeltmelerin nasıl yapılacağını günlüğe.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Günlük iletişim kutuları gözden geçirin ve son kullanıcılar ile gerçekleştirilen iletişim kutuları düzeltmeler yapmak için kullanabilirsiniz.  Özellikle, varlık etiketleri ve eğitilen model ve genel sistem performansını artırmak için eylem seçimleri düzeltebilirsiniz. 

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. LogDialogs adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-an-entity"></a>Bir varlık oluşturun

1. Varlıklar, yeni varlık tıklatın.
2. Varlık adı Şehir girin.
3. Oluştur’a tıklayın.

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta 'Hangi Şehir?' yazın.
3. Adayını varlıklarda $city girin.
3. Oluştur’a tıklayın

Sonra ikinci bir eylem oluşturun:

1. Eylemler, yeni bir eylem tıklatın.
3. Yanıtta ' $city hava büyük olasılıkla güneşli ' yazın.
4. Gerekli varlıkları $city girin.
4. Oluştur’a tıklayın.

Artık iki eylemler vardır.

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hava durumu nedir' yazın.
3. Puan Eylemler'i tıklatın ve 'Hangi Şehir?' seçin
2. 'Seattle' girin.
3. 'Üzerinde Seattle' çift tıklatın ve şehir seçin.
    - Bu, bir şehir varlık olarak işaretler.
5. Puan Eylemler
6. ' $City hava büyük olasılıkla güneşli ' seçin.
7. Öğretme Bitti'yi tıklatın.

Başka bir örnek iletişim ekleyin:

1. Yeni eylemi ve ardından yeni tren iletişim'ı tıklatın.
2. 'Seattle'da hava durumu nedir?' yazın. Seattle bir varlık olarak etiketlenir dikkat edin.
5. Puan Eylemler 
6. ' $City hava büyük olasılıkla güneşli ' seçin.
7. Öğretme Bitti'yi tıklatın.

### <a name="try-the-bot-as-the-user"></a>Kullanıcı olarak bot deneyin
Şimdi Biz bu bot kullanıcılara dağıttıysanız düşünün.

1. Günlük iletişim kutuları'ı tıklatın.
2. Yeni sohbet oturumu'ı tıklatın.
    - Kullanıcı, web Sohbeti denetimi UI soldaki deneyimini gibi bu bot gösterir. Sağdaki boşluk alanı yoksayabilirsiniz.
3. 'Hello' yazın.
4. Bot yanıt: 'hangi Şehir?'
4. Türü 'İzmir'.
5. Bot yanıt: 'hangi Şehir?'
    - Bu doğru olmadığı görülüyor. Bu iletişim kutusunu sağlandığından kaydedin.
2. Done sınama'ı tıklatın.

Yeni bir oturum başlayalım:

2. Yeni sohbet oturumu'ı tıklatın.
3. 'Boston için tahmin' türü.
4. Bot yanıt: 'hangi Şehir?'
2. Öğretme Bitti'yi tıklatın.

Şimdi ikinci iletişim düzeltmeleri olalım:

1. 'İçin tahmini İzmir' günlük iletişim kutuları altında tıklayın.
    - Bu, konuşma açar.
    - Konuşma kullanıcı tarafında tıklatırsanız ('Boston için tahminde' burada), varlık etiketleri değiştirebilirsiniz.
    - Sistem tarafında tıklatırsanız ('Şehir hangi' üzerinde burada), hangi eylemini seçili değiştirebilirsiniz.
5. 'Boston için tahmini' tıklayın. 
    - Burada kök nedeni Boston bir varlık olarak etiketlenmiş değil, oluşturur. Biz, değiştirmeniz gerekebilir.
    - 'Üzerinde İzmir' çift tıklatın, sonra şehir seçin.
    - Değişiklikleri Gönder'i tıklatın ve Kaydet'i tıklatın. Bu işlem yaptığınız değişikliklere dayalı bir eğitim iletişim oluşturacak ve eğitim iletişim kutuları yaptığınız değişikliği noktasında içine bırakın.
6. '$City, hava durumu, büyük olasılıkla güneşli değil.' seçin
7. Öğretme Bitti'yi tıklatın. Tren iletişim kutuları için şimdi giderseniz, yeni eylemi eklenen görürsünüz.

![](../media/tutorial9_logdiag1.PNG)

Şimdi diğer iletişim düzeltmeleri olalım:

1. Günlük iletişim kutuları altında 'hello' tıklayın.
    - Bu, konuşma açar.
3. 'Hello' yanıta 'Şehir hangi' olduğuna dikkat edin. Ancak, daha fazla anlamlı bir şey için değiştirmek istiyoruz. Daha iyi bir yanıt bir şey olacaktır 'hello, ben hava durumu bot' ister. Ancak biz oluşturmak zorunda böylece yapan eylem yok.
4. Eylemini tıklatın.
    - Yanıtta yazın ' hava durumu bot istiyorum. Tahminlerini yardımcı olabilir.'
6. Kaldırma onay bekleme olmayan eylem olun yanıt onay kutusu tamamlanmasını bekleyin.
7. Oluştur’a tıklayın.
8. Sonra bu yeni eylem seçmek için tıklatın. Daha sonra Kaydet'e tıklayın.
    - Bu, bu noktaya kadar geri eğitim oturumunda getirir.
6. 'Hangi Şehir?' seçin
7. 'İzmir' yazın. Etikete Boston bir varlık olarak değilse zaten çift tıklayın.
8. Puan Eylemler'i tıklatın.
9. '$City, hava durumu, büyük olasılıkla güneşli değil.' seçmek için tıklatın
10. Öğretme Bitti'yi tıklatın.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık algılama geri çağırma](./10-entity-detection-callback.md)
