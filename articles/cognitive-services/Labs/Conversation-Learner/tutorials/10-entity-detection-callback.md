---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler varlık algılama geri çağırma kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle varlık algılama geri çağırma kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e9a3b0a2be58313266b949b907e4eb49a318a6dc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260083"
---
# <a name="how-to-use-entity-detection-callback"></a>Varlık algılama geri çağırma kullanma

Bu öğretici, varlık algılama geri çağırma gösterir ve varlıkları çözmek için yaygın bir düzen gösterilmektedir.

## <a name="video"></a>Video

[![Öğretici 10 Önizleme](https://aka.ms/cl-tutorial-10-preview)](https://aka.ms/blis-tutorial-10)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide gerektiren `tutorialEntityDetectionCallback` bot çalışıyor.

    npm run tutorial-entity-detection

## <a name="details"></a>Ayrıntılar
Varlıklarla ilgili iş kurallarını işlemek için özel kod kullanarak varlık algılama geri çağırma sağlar. Bu Tanıtım "büyük apple'a" "new york" Çözümleme bir kurallı ad için--örneğin, kullanıcı tarafından girilen şehir adı çözümlemek için geri aramaları ve programlı varlıklar'ı kullanır.

### <a name="open-the-demo"></a>Tanıtım açın

Öğretici-10-EntityDetectionCallback modeli listesinde tıklayın. 

### <a name="entities"></a>Varlıklar

Üç varlığı, modelde tanımlanmıştır.

![](../media/tutorial10_entities.PNG)

1. Şehir kullanıcı metin girişi sunan özel bir varlıktır.
2. CityUnknown programlı bir varlıktır. Bu varlık, sistem tarafından doldurulacak. Sistem bu hangi şehirde emin değilseniz, kullanıcı girişini kopyalar.
3. Sistem hakkında bilmeniz Şehir CityResolved olur. Bu varlık, örneğin 'büyük apple' için 'new york' çözülecektir city'nin kurallı ad olacaktır.

### <a name="actions"></a>Eylemler

Üç eylem modelde tanımlanmıştır.

![](../media/tutorial10_actions.PNG)

1. İlk eylemdir 'hangi şehirde istiyorsunuz?'
2. İkincisi ise ' Bu Şehir $CityUknown bilmiyorum. Hangi şehirde istiyor musunuz?'
3. Üçüncü '$City söylediğiniz ve ben, $CityResolved için çözümlenir.' olan

### <a name="callback-code"></a>Geri çağırma kodu

Koda göz atalım. C: EntityDetectionCallback yöntemi bulabilirsiniz\<installedpath > \src\demos\tutorialSessionCallbacks.ts dosya.

![](../media/tutorial10_callbackcode.PNG)

Varlık çözümleme işlemi gerçekleştirildikten sonra bu işlev çağrılır.
 
- Bunun yapacağı ilk şey, NET $CityUknown olur. Her zaman başında temizlenmiş gibi $CityUknown yalnızca tek bir bırakma için açık kalır.
- Ardından, kabul edilmemiş şehirlerin listesini alın. İlk yararlanın ve bu sorunu çözmek çalışır.
- (ResolveCity) çözümler işlevin tanımlı olduğu ayrıntılı kodu olarak yukarıda. Kurallı Şehir adları listesi var. Listede şehir adı bulur, bunu döndürür. Aksi takdirde 'cityMap' içinde arar ve eşleşen adı döndürür. Bir şehir bulamazsanız, null döndürür.
- Şehir için bir ad çözümlendi, son olarak, bu $CityKnown varlıkta depolarız. Aksi takdirde, hangi kullanıcı söylediği temizleyin ve $CityUknown varlık doldurun.

### <a name="train-dialogs"></a>Train iletişim kutuları

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'hangi şehirde istiyorsunuz?' seçin
2. 'New york' girin.
    - Metin bir şehir kurumu olarak kabul edilir.
5. Puan Eylemler
    - `City` ve `CityResolved` doldurulduğunu.
6. '$City söylediğiniz ve ben, $CityResolved için çözümlenen' seçin.
7. Öğretim Bitti'ye tıklayın.

Başka bir örnek iletişim ekleyin:

1. Yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'hangi şehirde istiyorsunuz?' seçin
2. 'Büyük apple' girin.
    - Metin bir şehir kurumu olarak kabul edilir.
5. Puan Eylemler
    - `CityResolved` çalışan kod etkisini gösterir.
6. '$City söylediğiniz ve ben, $CityResolved için çözümlenen' seçin.
7. Öğretim Bitti'ye tıklayın.

Başka bir örnek iletişim ekleyin:

1. Yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'hangi şehirde istiyorsunuz?' seçin
2. 'Foo' girin.
    - Bu, sistem bilmediği bir şehir örneğidir. 
5. Puan Eylemler
6. Seç ' Bu Şehir $CityUknown bilmiyorum. Hangi şehirde istiyor musunuz?'.
7. 'New york' girin.
8. Puan eylemleri tıklayın.
    - `CityUknown` temizlenmiş, ve `CityResolved` doldurulur.
6. '$City söylediğiniz ve ben, $CityResolved için çözümlenen' seçin.
7. Öğretim Bitti'ye tıklayın.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturumun geri çağırmaları](./11-session-callbacks.md)
