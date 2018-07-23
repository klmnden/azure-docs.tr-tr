---
title: Konuşma Öğrenici Eylemler - Microsoft Bilişsel hizmetler, "beklenen varlık" özelliğinin nasıl kullanılacağını | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modelin "varlık bekleniyor" özelliği kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e013e237a996d722d958920a1310e3aaea36c52
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170917"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Eylemler "varlık bekleniyor" özelliğini kullanma

Bu öğreticide, "beklenen varlık" alanın Eylemler gösterilmektedir.

## <a name="video"></a>Video

[![Öğretici 4 Preview](http://aka.ms/cl-tutorial-04-preview)](http://aka.ms/blis-tutorial-04)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Kullanıcının yanıt eylemi için bir varlık durumu olacak beklediğiniz sisteme iletişim kurmak için bir eylem "varlık bekleniyor" alanını kullanın.

Bir eylemin "varlık bekleniyor" alanı $entity ve ardından İleri kullanıcı utterance üzerinde ayarlarsanız concretely, sistem olur:

1. İlk olarak, her zamanki şekilde makine öğrenimi tabanlı varlık ayıklama modeli kullanarak varlıklarını bulmaya
2. Adım 1, varlık yok bulunursa, ardından--bir buluşsal yöntem--olarak tam kullanıcı utterance $entity için atayın.
3. Çağrı `EntityDetectionCallback` zamanki ve eylem seçimi için devam edin.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. ExpectedEntities adı girin. Oluştur'a tıklayın.

### <a name="create-an-entity"></a>Bir varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. Varlık adı, ad girin.
3. Oluştur’a tıklayın

> [!NOTE]
> Varlık türü 'custom' dir. Bu değeri, varlık eğitilmiş anlamına gelir.  Önceden derlenmiş varlıklar, davranışları değiştirilemez, yani vardır.  Bu varlıklar ele alınmaktadır [Pre-Built varlıkları öğretici](./7-built-in-entities.md).

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıtta 'Adınız ne?' yazın.
3. Beklenen varlıklarda $name girin. Kaydet’e tıklayın.
    - Bu değer, bu soruyu sormak ve kullanıcı yanıtı algılanan herhangi bir varlık yok, robot kullanıcının yanıtın tamamını varlıktır varsayacaktır anlamına gelir.
2. Eylemler ve ardından yeni bir eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta 'Hello $name' yazın.
    - Varlık disqualifying bir varlık olarak otomatik olarak eklenir. 
4. Kaydet’e tıklayın.

Artık iki eylem var.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'Adınız ne?' seçin
    - Yanıt 'Hello $name' $name varlık tanımlanmasını gerektirir ve $name botun bellekte olmadığından seçilemez.
2. 'David' girin. 
    - Varlık olarak adı vurgulanır. Yukarıda yanıt varlık olarak seçmek için ayarlar buluşsal yöntem nedeniyle budur.
5. Puan Eylemler
    - Ad değeri botun bellekte sunulmuştur.
    - 'Hello $name' yanıt olarak kullanıma sunulmuştur. 
6. 'Hello $name' seçin.
7. Öğretim Bitti'ye tıklayın.

Burada "varlık bekleniyor" buluşsal tetiklenmez için makine öğrenimi varlık ayıklama modeli bir ad tanımlar iki örnek aşağıdadır.

1. Yeni Train iletişim tıklayın.
2. ' Adımı david' girin.
    - Önce bu word yakaladı olduğundan model david adı varlık olarak tanımlar.
2. Puan Eylemler
3. 'Hello $name' seçin.
4. ' Adımı susan' girin.
    - Bu düzen zaten görüldü olduğundan model susan adı olarak tanımlar.
2. Puan eylemleri tıklayın.
2. 'Hello susan' seçin.
3. Öğretim Bitti'ye tıklayın.

Aşağıdaki örneklerde, "beklenen varlık" buluşsal tetikler, ancak hatalı. Örnekler, ardından nasıl düzeltme yapılacağını gösterir.

1. 'Beni Ara jose' yazın.
    - Model adı bir varlık olarak algılamaz.
2. Jose üzerinde tıklayın ve adı'nı seçin.
3. Puan eylemleri tıklayın.
4. Merhaba $name'ı seçin.
5. Öğretim Bitti'ye tıklayın.
1. Yeni Train iletişim tıklayın.
2. 'Hello' girin.
3. Yanıtta, için 'adınız ne', 'I frank olarak adlandırılan ' girin.
    - Deyimin tamamını vurgulanır. Bu durum, buluşsal yöntem harekete ve tüm yanıt adı entity seçili istatistiksel modeli bir ad bulamadı çünkü.
2. Bunu düzeltmek için vurgulanan deyimini tıklayın ardından üzerinde kırmızı x simgesini tıklatın. 
3. Frank seçmek için tıklayın ve ardından adına tıklayın.
2. Puan Eylemler
3. 'Hello $name' seçin.
4. Öğretim Bitti'ye tıklayın.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Değişkeni yoksayılamaz varlıklar](./5-negatable-entities.md)
