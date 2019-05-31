---
title: Bir "Merhaba Dünya" konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler oluşturma | Microsoft Docs
titleSuffix: Azure
description: Bir "Merhaba Dünya" konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: fe5d21fadef8f4452ba36259dbf89cefc78230de
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388053"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Konuşma Öğrenici ile bir "Merhaba Dünya" modeli oluşturma

Bu öğreticide, Eylemler oluşturma, robot etkileşimli olarak öğretim ve son kullanıcılardan gelen oturum iletişim kutularının düzeltme yapma gibi konuşma Öğrenici kullanmaya başlamak gösterilir.

## <a name="video"></a>Video

[![Hello World öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_HelloWorld_Preview)](https://aka.ms/cl_tutorial_v3_helloworld)


## <a name="requirements"></a>Gereksinimler
Henüz yapmadıysanız, önce tüm kurulum adımlarını tamamlandı, oluşturma dahil olun bir `.env` anahtar yazma, LUIS ile dosya.  Bkz: [hızlı](../quickstart.md) Ayrıntılar için.

Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="steps"></a>Adımlar

Giriş sayfasında Web kullanıcı arabiriminde başlatın.

### <a name="create-the-model"></a>Model oluşturma
1. "Yeni modeli" düğmesine tıklayın.
2. "Hello World" "Name" alanına girin.
3. "Oluştur" düğmesine tıklayın.

Artık oluşturduğunuz modeli görünümünü görmeniz gerekir.

### <a name="create-an-action"></a>Bir eylem oluşturun
1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
    - Konuşma Öğrenici kullanıcı, bir API çağrısı veya bir kart döndüren bir kısa mesaj eylem olabilir.
2. "Botun yanıtta..." alan türü "Hello".
    - Bot döndüreceği yanıt budur.
3. "Oluştur" düğmesine tıklayın.

Bot yapabilmek için ilk eylem oluşturduğunuz yani metin yanıt verin.

### <a name="train-dialogs"></a>Train iletişim kutuları
Burada, kullanıcı konuşma yanıtlamayla ilgili modeli eğitme budur.

#### <a name="first-training-dialog"></a>İlk eğitim iletişim

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Tür "Hi" ENTER tuşuna basın.
    - Hangi kullanıcının bir örnek bir konuşma başında diyebilirsiniz gibi.
3. "Puan Eylemler" düğmesine tıklayın.
4. "Hello" seçin.
    - Bu örnek iletişim kutusunda bir tam Aç tamamladığınız. 
5. Kullanıcı yanıtı "Goodbye" yazın.
6. "Puan Eylemler" düğmesine tıklayın.
7. Tıklayın "+ eylem" düğmesi.
8. Tür "güle güle!" "... Botun yanıt" alanına ve "Oluştur" düğmesine tıklayın.
    - Bot ile eylem yeni oluşturduğunuz yanıt verdiğini dikkat edin.
9. "Kaydet" düğmesine tıklayın. 
    - Bitirmek ve bu eğitim iletişim kaydedin.

Artık bir eğitim iletişim birlikte tek bir varlık modeli ve iki eylem var.

#### <a name="second-training-dialog"></a>İkinci eğitim iletişim
Şimdi bir daha fazla eğitim yapın ve Bot nasıl yanıt verdiğini görebilirsiniz.

1. "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. "Hi" yazın
    - Bu ilk iletişim kutusuna benzer ve Bot iyi bir puan Al bekliyoruz.
3. "Puan Eylemler" düğmesine tıklayın.
    - Konum ve puan yine de yeterli doğru olmayabilir ve ek eğitim gerektirebilir.
4. "Hello" seçin.
5. Kullanıcı yanıtı "bye" yazın.
6. "Puan Eylemler" düğmesine tıklayın.
7. "Güle güle!" seçin
8. "Kaydet" düğmesine tıklayın.

### <a name="log-dialogs"></a>Günlük iletişim kutuları
Bu, Test, görüntülemek ve düzeltin, ya da gerçek kullanıcıları ile Botunuza çalıştırılmış konuşmaları yerdir.

#### <a name="test-the-model-as-an-end-user"></a>Model bir son kullanıcı olarak test etme
1. Sol panelde, "Günlük iletişim kutuları" sonra "Yeni günlük iletişim kutusu" düğmesine tıklayın.
2. "Merhaba" yazın.
3. Bot, otomatik olarak "Hello" ile yanıt vermelidir kısa bir süre bekleyin
4. 'Byebye' girin
5. Kısa bir süre bekleyin, yeniden Bot otomatik olarak "Hello" ile yanıt vermelidir.
6. "Test Bitti" düğmesine tıklayın.

#### <a name="view-and-correct-a-user-conversation"></a>Görüntüleme ve kullanıcı konuşma düzeltin
Günlük iletişim kutuları kullanma, konuşmalar listesini görüntüleyebilirsiniz kullanıcılar ile Botunuza tutulan. Ayrıca, bunları Botun yanıtları düzeltin ve etkileşimler eğitim iletişim kutuları kaydetmek için de düzenleyebilirsiniz. Bunu yapmak için:
1. Kılavuzda konuşma oturum açma seçeneğini tıklatın.
2. Örneğin son Bot eylemini tıklatın "Hello".
3. "Güle güle!" seçin Bot düzeltmek için.
4. "Kaydet olarak Train iletişim" düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Eğitim giriş](./02-intro-to-training.md)
