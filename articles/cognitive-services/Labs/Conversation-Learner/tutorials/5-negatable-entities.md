---
title: Değişkeni yoksayılamaz varlıklar bir konuşma öğrenen uygulaması - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Değişkeni yoksayılamaz varlıklar bir konuşma öğrenen uygulaması ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3d65376c9c43ee1407468f3e8bf3e058048bd556
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354065"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-application"></a>Değişkeni yoksayılamaz varlıklar bir konuşma öğrenen uygulaması ile kullanma

Bu öğretici varlıkların "değişkeni yoksayılamaz" özelliğini gösterir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Kullanıcı temizleyebilirsiniz""Hayır, $entity istemiyorum"olduğu gibi bir varlık değeri," eylem "değişkeni yoksayılamaz" olarak işaretlemek veya "Hayır, $entity değil." Örneğin, "Hayır, İzmir ' çıkmak istemiyorum."

Concretely, bir varlığın "değişkeni yoksayılamaz" özellik ayarlanırsa:

- Varlık belirtilenlerden etiketleme olduğunda, bir varlığın normal (pozitif) örnekleri hem varlığın "negatif" örneklerini etiketlemek izin verir
- HALUK öğrenir iki varlık modelleri: biri pozitif örnekleri ve negatif örnekleri için ikinci bir
- Bu varlık değişkeninin değerini (varsa) temizlemek için bir varlık negatif bir örneğini etkisini olduğu

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. NegatableEntity adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-an-entity"></a>Bir varlık oluşturun

1. Varlıklar, yeni varlık tıklatın.
2. Varlık adı ad girin.
3. Onay değişkeni yoksayılamaz.
    - Bu kullanıcı varlık için bir değer sağlayabilir veya deyin şeydir belirtir *değil* varlık değeri. İkinci durumda, bu varlık eşleşen bir değer silme neden olacaktır.
3. Oluştur’a tıklayın.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta 'I adınız bilmiyorsanız' yazın.
3. Adayını varlıklarda adı girin.
3. Oluştur’a tıklayın

Daha sonra ikinci bir eylem oluşturun.

1. Eylemler, sonra yeni eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta, türü ' adınızı bildirin. $Name olduğundan '.
4. Oluştur’a tıklayın

Şimdi iki eylemlere sahiptir.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'I adınız bilmiyorsanız' seçin
    - Yalnızca geçerli eylem olduğundan, % 100 puan alma dikkat edin.
2. ' Adımın david' girin
3. 'David' seçin ve etiketi seçin ' + ad '
    - 'Name' iki örneğini dikkat edin: '+ adı' ve '-adı '.  Ayrıca bu değeri sağlanmaktadır anlamına gelir. Anlamına gelir bir değer olmadığını biz sistem söylemiş olursunuz.
5. Puan Eylemler
    - Not ad değer bot's bellekte sunulmuştur.
    - ' Adınızı bildirin. $Name olduğundan ' yalnızca yanıt. 
6. Seç ' adınızı bildirin. $Name olduğundan '.

Değişkeni yoksayılamaz varlık temizleme deneyelim:

7. 'Adımın david değil' girin.
    - Bildirim önceki deseni temel alınarak adı olarak seçilen 'not'. Yanlış olmasıdır.
2. 'Not ''ı tıklatın, sonra kırmızı x. 
3. 'David' üzerinde'ı tıklatın.
    - Şimdi bu değerin adı varlık olmadığını iletişim negatif bir varlık budur.
2. Seç '-adı '.
3. Puan Eylemler'i tıklatın.
    - Değer bellekten temizlenmiş dikkat edin.
2. 'Adınızı bilmeniz yok', tek eylem olduğu seçin.

Sonraki nasıl adı için yeni bir değer girilebilir gösterir.

3. 'John' adı olarak girin. Ardından 'john' seçin ve adına tıklayın.
4. Puan Eylemler'i tıklatın.
5. Seç ' adınızı bildirin. $Name olduğundan '.

Şimdi girilen ad değiştirmeyi deneyin.

6. ' Adımın Çiğdem ' girin.
7. Seç ' adınızı bildirin. $Name olduğundan '.
7. Puan Eylemler'i tıklatın.
8. Bildirim **Çiğdem** geçersiz kıldı **john** varlık değerleri.
9. 'Adımın Çiğdem değil' girin.
    - Sistem bu negatif bir örneği olarak etiketli dikkat edin.
2. Puan Eylemler'i tıklatın.
3. 'Adınızı bilmeniz yok', tek eylem olduğu seçin.
7. Öğretme Bitti'yi tıklatın.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok değerli varlıklar](./6-multi-value-entities.md)
