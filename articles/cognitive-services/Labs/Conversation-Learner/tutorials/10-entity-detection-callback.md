---
title: Varlık algılama geri çağırma bir konuşma öğrenen uygulaması - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Varlık algılama geri çağırma bir konuşma öğrenen uygulaması ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e41ea5930ff0c8395d0c93aa42e224ebfc894ba8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354022"
---
# <a name="how-to-use-entity-detection-callback"></a>Varlık algılama geri çağırma kullanma

Bu öğretici varlık algılama geri çağırma gösterir ve varlıkları çözmek için genel bir desen gösterir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici "tutorialEntityDetectionCallback" bot çalışıyor olması gerekir.

    npm run tutorial-entity-detection

## <a name="details"></a>Ayrıntılar
Varlık algılama geri çağırma varlıklarla ilgili iş kuralları işlemek için özel kod kullanarak sağlar. Bu gösteride geri aramalar ve programlama varlıklar "büyük apple" için "new york" çözümlenirken bir kurallı adına--Örneğin, kullanıcı tarafından girilen şehir adı çözümlemek için kullanacağız.

### <a name="open-the-demo"></a>Tanıtım açın

Uygulama listesinden Öğreticisi-10-EntityDetenctionCallback'ı tıklatın. 

### <a name="entities"></a>Varlıklar

Şu üç varlık uygulamada tanımladınız.

![](../media/tutorial10_entities.PNG)

1. Şehir kullanıcı metin giriş olarak sağlayacak özel bir varlıktır.
2. CityUnknown programlı bir varlıktır. Bu, sistem tarafından doldurulacak. Sistem bu hangi Şehir bilmiyorsa kullanıcı girişini kopyalayacak.
3. CityResolved sistem bilmeniz Şehir ' dir. Bu örneğin 'büyük apple' 'İstanbul' çözümleyecek Şehir'ın kurallı ad olacaktır.

### <a name="actions"></a>Eylemler

Üç eylem oluşturduk. 

![](../media/tutorial10_actions.PNG)

1. İlk eylemdir 'hangi Şehir, istiyor musunuz?'
2. İkinci ' $CityUknown bu Şehir bilmeniz yok. Hangi Şehir istiyor musunuz?'
3. Üçüncü ' $City denirse, t, $CityResolved için çözümlendi.' ise

### <a name="callback-code"></a>Geri çağırma kodu

Koda bakalım. C: EntityDetectionCallback yöntemi bulabilirsiniz\<installedpath > \src\demos\tutorialSessionCallbacks.ts dosya.

![](../media/tutorial10_callbackcode.PNG)

Bu işlev, varlık çözümleme gerçekleştikten sonra çağrılır.
 
- Ne yapacağını ilk Temizle $CityUknown şeydir. Her zaman başında temizlenmiş gibi $CityUknown yalnızca için tek bir bırakma korunur.
- Ardından, kabul edilmemiş Şehir listesini alın. Birinci alın ve bunu çözmeyi deneyin.
- (ResolveCity) çözümlediği işlevi tanımlanan kodunda üzerinde daha fazla. Kurallı şehir adlarının bir listesini içeriyor. Listede şehir adı bulur, onu döndürür. Aksi takdirde 'cityMap' arar ve eşlenen adını döndürür. Bir şehir bulamazsanız, null döndürür.
- Şehir için bir ad çözümlendi, son olarak, bu $CityKnown varlık depolarız. Aksi takdirde kullanıcı etti temizleyin ve $CityUknown varlığı doldurur.

### <a name="train-dialogs"></a>Tren iletişim kutuları

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'hangi Şehir, istiyor musunuz?' seçin
2. 'New york' girin.
    - Şehir bir varlık olarak tanınan unutmayın.
5. Puan Eylemler
    - Şehir ve CityResolved doldurulduğunu unutmayın.
6. '$City denirse ve ı, $CityResolved için Çözümlendi' seçin.
7. Öğretme Bitti'yi tıklatın.

Başka bir örnek iletişim ekleyin:

1. Yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'hangi Şehir, istiyor musunuz?' seçin
2. 'Büyük apple' girin.
    - Şehir bir varlık olarak tanınan unutmayın.
5. Puan Eylemler
    - CityResolved çalışan kodu etkisini gösterdiğine dikkat edin.
6. '$City denirse ve ı, $CityResolved için Çözümlendi' seçin.
7. Öğretme Bitti'yi tıklatın.

Başka bir örnek iletişim ekleyin:

1. Yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'hangi Şehir, istiyor musunuz?' seçin
2. 'Foo' girin.
    - Bu, sistem bilmediği bir şehir örneğidir. 
5. Puan Eylemler
6. Seç ' $CityUknown bu Şehir bilmeniz yok. Hangi Şehir istiyor musunuz?'.
7. 'New york' girin.
8. Puan Eylemler'i tıklatın.
    - Not CityUknown temizlenmiş ve CityResolved doldurulur.
6. '$City denirse ve ı, $CityResolved için Çözümlendi' seçin.
7. Öğretme Bitti'yi tıklatın.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturum geri aramalar](./11-session-callbacks.md)
