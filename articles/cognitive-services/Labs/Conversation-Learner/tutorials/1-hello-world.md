---
title: "\"Hello World\" konuşma öğrenen uygulama - Microsoft Bilişsel hizmetler oluşturma | Microsoft Docs"
titleSuffix: Azure
description: "\"Hello World\" konuşma öğrenen uygulama oluşturmayı öğrenin."
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 30026285ac6dda45d2f5e3718aae741b928cf242
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354100"
---
# <a name="how-to-create-a-hello-world-application-with-conversation-learner"></a>Konuşma öğrenen ile bir "Hello World" uygulaması oluşturma

Bu öğretici, konuşma Eylemler oluşturulurken dahil olmak üzere etkileşimli olarak eğitme ve son kullanıcıların oturum iletişim kutularının düzeltme yapma öğrenen, kullanmaya başlama gösterilmektedir.

## <a name="requirements"></a>Gereksinimler
Henüz yapmadıysanız, önce tüm kurulum adımlarını tamamlanmıştır, oluşturma dahil olmak bir `.env` anahtar yazma, HALUK dosyasıyla.  Bkz: [Hızlı Başlangıç](https://github.com/Microsoft/ConversationLearner-Samples) Ayrıntılar için.

Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="steps"></a>Adımlar

Web kullanıcı arabirimini giriş sayfasında başlatın.

### <a name="create-the-app"></a>Uygulama oluşturma
1. Yeni uygulama'ı tıklatın
2. Hello World ad alanına girin
3. Oluştur’a tıklayın

### <a name="create-an-action"></a>Bir eylem oluşturun

1. Hello World uygulamasını başlatmak için tıklatın
2. Eylemler tıklatın, yeni eylem
    - Eylem kullanıcı, bir API çağrısı veya bir kart konuşma öğrenen döndüren bir kısa mesaj olabilir.
3. 'Yanıtta Merhaba Dünya!' yazın
    - Bot döndürülecek yanıt budur
4. Oluştur’a tıklayın

Bot yani yapabilirsiniz ilk şey oluşturduğunuz bir metin yanıt döndürür.

### <a name="train-the-bot"></a>Bot eğitme

#### <a name="create-the-first-dialog"></a>İlk iletişim kutusu oluşturma

1. Tren iletişim kutuları, ardından yeni tren iletişim tıklatın
2. Konuşma, örneğin, begging 'hello' ın kullanıcı söyleyin örneği girin.
3. Puan Eylemler
4. 'Hello World!' seçin
    - Bu, bir tane Aç örnek iletişim kutusu oluşturur. 
2. 'Goodbye' girin
3. Puan Eylemler
4. Eylem Ekle'yi tıklatın, sonra 'Güle güle!' girin Yanıtta, ardından 'Oluştur' u
5. Öğretme Bitti'yi tıklatın. Bu, bu eğitim iletişim sona erer.

Şimdi bir öğretme iletişim sistemi sahip.

#### <a name="continue-teaching-the-bot"></a>Bot eğitme devam
Şimdi bir daha fazla eğitim yapın ve bot nasıl yanıt vereceğini bakın.

1. Yeni tren iletişim'ı tıklatın
2. Girin ' yüksek var '
    - Bu ilk iletişim için benzer ve bot iyi puanı almak isteriz.
2. Puan eylemini tıklatın
    - Konum ve puanı değil hala yeterince doğru ve ek öğretme gerektirir.
3. 'Hello World!' yanındaki seçin
4. 'Bye' girin
5. Puan Eylemler
6. 'Güle güle!' seçin
7. Öğretme Bitti'yi tıklatın

![](../media/tutorial1_actions.PNG)

Bot nasıl çalıştığını görmek için başka bir öğretme oturumu gerçekleştiririz.

'Merhaba' ve 'byebye' kullanarak yukarıdaki adımları yineleyin ve puanı eylem tıklattığınızda konumu ve puanı aracılarını yanıtının değişiklikleri not edin.

Şimdi 'howdy' ve 'good bye' kullanma adımları yineleyin ve bot belirten puanları Puanlama gösterir yenilikleri öğrendiğinize göre bu etkileşimi unutmayın.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Son kullanıcı olarak bot test

1. Günlük iletişim kutuları, ardından yeni günlük iletişim tıklatın
2. Türü 'Merhaba'
3. Ardından 'bye'

'Bye' konuşma başlatmayı deneyin ve bot's yanıtı not alın.

### <a name="view-conversations-in-the-log-dialogs"></a>Görünüm görüşmeleri günlük iletişim kutuları

Günlük iletişim kutularında, listeyi görüntüleyebilirsiniz görüşmeleri, güncelleştirme ve etkileşimleri eğitim iletişim kutuları kaydedin. Bunu yapmak için:

1. Bir konuşma Oturum Aç'ı tıklatın
2. Konuşma iyi görünüyorsa, örn. son eylemini tıklatın 'Goodbye'.
3. Önerilen yanıt seçmek için tıklatın. 
    - Ayrıca, seçin veya başka bir eylem ekleyebilirsiniz.
4. Ardından bu bir eğitim Farklı Kaydet iletişim kutusu için Bitti'yi tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bekleyin ve Eylemler bekleyin](./2-wait-vs-nonwait-actions.md)