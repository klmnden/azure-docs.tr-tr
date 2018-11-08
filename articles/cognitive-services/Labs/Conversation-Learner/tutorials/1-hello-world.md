---
title: Bir "Merhaba Dünya" konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler oluşturma | Microsoft Docs
titleSuffix: Azure
description: Bir "Merhaba Dünya" konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 271141f24ff729fc99210af67ad769a5ef83a65c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242730"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Konuşma Öğrenici ile bir "Merhaba Dünya" modeli oluşturma

Bu öğreticide, konuşma Eylemler oluşturma gibi etkileşimli olarak öğretim ve son kullanıcılardan oturum iletişim kutularının düzeltmeler yapmak Öğrenici, kullanmaya başlama işlemi gösterilmektedir.

## <a name="video"></a>Video

[![Öğretici 1 Preview](https://aka.ms/cl-tutorial-01-preview)](https://aka.ms/blis-tutorial-01)


## <a name="requirements"></a>Gereksinimler
Henüz yapmadıysanız, önce tüm kurulum adımlarını tamamlandı, oluşturma dahil olun bir `.env` anahtar yazma, LUIS ile dosya.  Bkz: [hızlı](https://github.com/Microsoft/ConversationLearner-Samples) Ayrıntılar için.

Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="steps"></a>Adımlar

Giriş sayfasında Web kullanıcı arabiriminde başlatın.

### <a name="create-the-model"></a>Modeli oluşturma
1. Yeni bir modele tıklayın
2. Hello World ad alanında girin
3. Oluştur’a tıklayın

### <a name="create-an-action"></a>Bir eylem oluşturun

1. Hello World modelinde başlatmak için tıklayın
2. Eylemler ve ardından yeni bir eylem tıklayın
    - Konuşma Öğrenici kullanıcı, bir API çağrısı veya bir kart döndüren bir kısa mesaj eylem olabilir.
3. 'Hello World!' yanıt olarak, yazın
    - Bu bot döndüreceği yanıttır
4. Oluştur’a tıklayın

Bot, yani yapmak için gereken ilk şey oluşturmuş olduğunuz bir metin yanıtı döndürür.

### <a name="train-the-bot"></a>Botunuzu

#### <a name="create-the-first-dialog"></a>İlk iletişim kutusu oluşturma

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın
2. Kullanıcının konuşma, örneğin, 'hello' begging içinde Yazar örneği girin.
3. Puan Eylemler
4. 'Hello World!' seçin
    - Bu örnek bir dönüş iletişim oluşturur. 
2. 'Güle güle' girin
3. Puan Eylemler
4. Eylem Ekle tıklayın ve ardından 'Güle güle!' girin Yanıt olarak, ardından 'Oluştur' seçeneğine tıklayın
5. Öğretim Bitti'ye tıklayın. Bu, bu eğitim iletişim sona erecek.

Artık bir öğretim iletişim sisteminde vardır.

#### <a name="continue-teaching-the-bot"></a>Bot eğitiminde devam edin
Şimdi bir daha fazla eğitim yapın ve bot nasıl yanıt vereceğini bakın.

1. Yeni Train iletişim tıklayın
2. Girin ' Merhaba var. '
    - Bu ilk iletişim kutusuna benzer ve bot iyi bir puan Al bekliyoruz.
2. Puan eylemini tıklatın
    - Konum ve puan değil devam yeterince doğru ve ek öğretim gerektirir.
3. 'Hello World!' yanındaki seçin
4. 'Bye' enter
5. Puan Eylemler
6. 'Güle güle!' seçin
7. Öğretim bitti seçeneğine tıklayın

![](../media/tutorial1_actions.PNG)

Bot nasıl çalıştığını görmek için başka bir öğretim oturumu yapacağız.

'Merhaba' ve 'byebye' kullanarak yukarıdaki adımları yineleyin ve puan eylem tıkladığınızda konumu ve puan botlar yanıtın değişiklikleri not edin.

Artık 'howdy' ve 'good bye' kullanarak adımları yineleyin ve bot belirten puanları Puanlama gösterir geliştirmelerin öğrendiniz, bu etkileşim unutmayın.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Bot, bir son kullanıcı olarak test etme

1. Günlük iletişim kutuları, ardından yeni günlük iletişim tıklayın
2. 'Merhaba' yazın
3. Ardından 'bye'

'Bye' ile bir konuşma başlatmayı deneyin ve botun yanıt unutmayın.

### <a name="view-conversations-in-the-log-dialogs"></a>Görünüm konuşmalara günlük iletişim kutuları

Günlük iletişim kutularında, listeyi görüntüleyebilirsiniz konuşmaları, güncelleştirme ve etkileşimler eğitim iletişim kutuları kaydedin. Bunu yapmak için:

1. Bir konuşma oturum açma tıklayın
2. Konuşma iyi görünüyorsa, örneğin son eylemini tıklatın 'Güle güle'.
3. Önerilen yanıt seçmek için tıklayın. 
    - Ayrıca, seçin veya başka bir eylem ekleme.
4. Ardından bu eğitim iletişim kutusu olarak kaydetmek için Bitti'ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bekleyin ve eylemlerin-wait](./2-wait-vs-nonwait-actions.md)