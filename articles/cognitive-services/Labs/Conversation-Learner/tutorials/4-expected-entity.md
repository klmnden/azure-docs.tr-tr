---
title: Konuşma öğrenen eylemlerin - Microsoft Bilişsel Hizmetler "varlık beklenen" özelliğinin nasıl kullanılacağını | Microsoft Docs
titleSuffix: Azure
description: Bir konuşma öğrenen uygulamasının "varlık beklenen" özelliğinin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 83911eba8112cf356af8c4cd562a87f746fbabc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354058"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Eylemler "varlık beklenen" özelliğini kullanma

Bu öğretici Eylemler "varlık beklenen" alanının gösterir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Bir eylem kullanıcının yanıta bir varlık durumu olacak beklediğiniz sisteme iletişim kurmak için bir eylem "varlık beklenen" alanını kullanın.

Bir eylem "varlık beklenen" alanının $entity için ardından İleri kullanıcı utterance üzerinde ayarlarsanız concretely, sistem olur:

1. İlk olarak, her zamanki gibi makine öğrenme tabanlı varlık ayıklama modeli kullanarak varlıkları bulma girişimi
2. Adım 1 ' de varlık bulunursa, ardından--buluşsal yöntem--olarak tam kullanıcı utterance $entity için atayın.
3. Çağrı `EntityDetectionCallback` her zamanki gibi ve eylem seçimi için devam edin.

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. ExpectedEntities adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-an-entity"></a>Bir varlık oluşturun

1. Varlıklar, yeni varlık tıklatın.
2. Varlık adı ad girin.
3. Oluştur’a tıklayın

Not varlık türü 'custom'--Bu varlık eğitilmiş anlamına gelir.  Davranışlarını düzeltilemez--bu başka bir öğreticide kapsanan önceden derlenmiş varlıkları vardır.

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta ', adı nedir?' yazın.
3. Beklenen varlıklarda $name girin. Kaydet'i tıklatın.
    - Başka bir deyişle, bu soruyu sorular ve kullanıcı yanıtı algılanan herhangi bir varlık yok, bot kullanıcının yanıtını bütün bu varlıktır varsayılmaktadır.
2. Eylemler, sonra yeni eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta 'Hello $name' yazın.
    - Varlık disqualifying bir varlık olarak otomatik olarak eklendiğine dikkat edin. 
4. Kaydet’e tıklayın.

Şimdi iki eylemlere sahiptir.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve ', adı nedir?' seçin
    - Yanıt '$name Merhaba,' Not seçilemez, çünkü bu requies tanımlanacak varlık $name ve $name değil bot'ın bellek.
2. 'David' girin. 
    - Adı bir varlık olarak vurgulanır unutmayın. Yukarıdaki yanıtı bir varlık olarak seçmek için ayarlar yapılıyor buluşsal yöntem nedeniyle budur.
5. Puan Eylemler
    - Not ad değer bot's bellekte sunulmuştur.
    - 'Hello $name' yanıt olarak kullanıma sunulmuştur. 
6. 'Hello $name' seçin.
7. Öğretme Bitti'yi tıklatın.

İşte iki örnekler burada "varlık beklenen" buluşsal yöntem tetiklenen değil için makine öğrenme varlık ayıklama modeli bir ad tanımlar.

1. Yeni tren iletişim'ı tıklatın.
2. ' Adımın david' girin.
    - Önce bu word görüldüğü çünkü david adı varlık olarak tanımlamak olduğunu unutmayın.
2. Puan Eylemler
3. 'Hello $name' seçin.
4. ' Adımın Çiğdem ' girin.
    - Bu desen zaten görüldüğü beri bu adı olarak Çiğdem tanımlar unutmayın.
2. Puan Eylemler'i tıklatın.
2. 'Hello Çiğdem' seçin.
3. Öğretme Bitti'yi tıklatın.

İşte burada "varlık beklenen" buluşsal yöntem tetikler, ancak yanlış iki başka örnekler ve nasıl biz düzeltme yapabilirsiniz.

1. 'Beni Ara jose' yazın.
    - Bu adı bir varlık olarak tanımadığı unutmayın.
2. Jose üzerinde tıklayın ve ad seçin.
3. Puan Eylemler'i tıklatın.
4. Merhaba $name seçin.
5. Öğretme Bitti'yi tıklatın.
1. Yeni tren iletişim'ı tıklatın.
2. 'Hello' girin.
3. 'Adınızı nedir' için yanıt olarak 'I frank olarak adlandırılan ' girin.
    - Deyimin tamamını vurgulanır unutmayın. İstatistiksel modeli buluşsal yöntem harekete ve tüm yanıt adı varlık olarak seçilen bir ad bulamadığından budur.
2. Bunu düzeltmek için vurgulanan deyimini tıklatın ardından üzerinde kırmızı x simgesini tıklatın. 
3. Frank seçmek için tıklatın, sonra adına tıklayın.
2. Puan Eylemler
3. 'Hello $name' seçin.
4. Öğretme Bitti'yi tıklatın.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Değişkeni yoksayılamaz varlıklar](./5-negatable-entities.md)
