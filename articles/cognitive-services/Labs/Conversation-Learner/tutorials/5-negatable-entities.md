---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler değişkeni yoksayılamaz varlıklar kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle değişkeni yoksayılamaz varlıkları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2fd00d53755e44e3a3d86782c40aa6a53ff4d378
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171410"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeliyle değişkeni yoksayılamaz varlıklar kullanma

Bu öğreticide, varlıkların "değişkeni yoksayılamaz" özelliğini gösterir.

## <a name="video"></a>Video

[![Öğretici 5 Preview](http://aka.ms/cl-tutorial-05-preview)](http://aka.ms/blis-tutorial-05)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar
Kullanıcı temizleyebilirsiniz""Hayır, $entity istemiyorum"olduğu gibi bir varlık değeri ise" eylem "değişkeni yoksayılamaz" olarak işaretlemek veya "no $entity değil." Örneğin, "Hayır, İzmir ' çıkmak istemiyorum."

Concretely, bir varlığın "değişkeni yoksayılamaz" özelliği ayarlanmışsa:

- Varlık bahsetmeleri etiketleme, hem normal (pozitif) bir varlığın örneklerinin hem de "negatif" bir varlık örneklerini etiket olanak sağlar
- LUIS iki varlık modeli öğrenir: biri örnekleri pozitif ve negatif örnekleri için ikinci bir
- Bir varlığın negatif bir örneğinin (varsa), bu varlık değişkeninin değerini temizlemek için etkisidir

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. NegatableEntity adı girin. Oluştur'a tıklayın.

### <a name="create-an-entity"></a>Bir varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. Varlık adı, ad girin.
3. Denetim değişkeninin tersi.
    - Bu özellik, kullanıcı varlığı için bir değer sağlayabilir veya örneğin bir şeydir gösterir *değil* varlık değeri. İkinci durumda, bu varlık eşleşen bir değer silme neden olur.
3. Oluştur’a tıklayın.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıtta 'Adınızı bilmiyorum' yazın.
3. Eleyerek varlıklarda adı girin.
3. Oluştur’a tıklayın

Ardından ikinci bir eylem oluşturun.

1. Eylemler ve ardından yeni bir eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıt olarak, türü ' adınızı biliyorum. $Name olan '.
4. Oluştur’a tıklayın

Artık iki eylem var.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'Adınızı bilmiyorum' seçin
    - Yalnızca geçerli eylem olduğundan puan % 100'dür.
2. ' Adımı david' girin
3. 'David','ı seçin ve etiket ' + ad '
    - 'Name', iki durum vardır: '+ name' ve '-adı '.  (+) Artı ekler veya değerin üzerine yazar. (-) Eksi değer kaldırır.
5. Puan Eylemler
    - Ad değeri botun bellekte sunulmuştur.
    - ' Adınızı biliyorum. $Name olan ' yalnızca yanıt. 
6. Seç ' adınızı biliyorum. $Name olan '.

Şimdi değişkeni yoksayılamaz varlık temizlemeyi deneyin:

7. 'Adımı david değil' girin.
    - Bildirim adı önceki deseni temel alınarak olarak seçilen 'not'. Bu etiketi geçersiz.
2. 'Not' tıklayın ve ardından kırmızı bir x işareti. 
3. 'David' üzerinde'a tıklayın.
    - Artık bu adı varlık değeri değil, iletişim negatif bir varlık budur.
2. Seç '-adı '.
3. Puan eylemleri tıklayın.
    - Değer bellekten temizlenmiş dikkat edin.
2. 'Adınızı bilmiyorum', yalnızca eylem olduğu seçin.

Sonraki nasıl adı için yeni bir değer girilebilir gösterir.

3. 'John' adını girin. Ardından, 'john ''nı seçip adına tıklayın.
4. Puan eylemleri tıklayın.
5. Seç ' adınızı biliyorum. $Name olan '.

Şimdi, girilen ad değiştirmeyi deneyin.

6. ' Adımı susan' girin.
7. Seç ' adınızı biliyorum. $Name olan '.
7. Puan eylemleri tıklayın.
8. Bildirim **susan** geçersiz kıldı **john** varlık değerleri.
9. 'Adımı susan değil' girin.
    - Sistem bu negatif bir örneği olarak etiketli dikkat edin.
2. Puan eylemleri tıklayın.
3. 'Adınızı bilmiyorum', yalnızca eylem olduğu seçin.
7. Öğretim Bitti'ye tıklayın.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok değerli varlıklar](./6-multi-value-entities.md)
