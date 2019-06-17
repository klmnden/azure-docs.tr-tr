---
title: Önceden oluşturulmuş varlıklar
titleSuffix: Azure Cognitive Services
description: LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık destek LUIS uygulamanızı kültüre göre değişir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 0cfc4ff58cfeb65f80f9ac5ce2dd532defde5ef8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60596100"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Ortak veri türleri tanımak için önceden oluşturulmuş varlıklar

LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. 

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

1. Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **varlıkları** sol tarafındaki. 

1. Üzerinde **varlıkları** sayfasında **önceden oluşturulmuş bir varlık ekleme**.

1. İçinde **önceden oluşturulmuş varlıklar ekleme** iletişim kutusunda, datetimeV2 seçin önceden oluşturulmuş varlık. 

    ![Önceden oluşturulmuş varlık Ekle iletişim kutusu](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. **Done** (Bitti) öğesini seçin.

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Önceden oluşturulmuş bir varlık değerini görüntülemek için en kolay yolu sorgu yayımlanan uç noktasından. 

1. Üst araç çubuğunda, seçin **Yayımla**. Yayımlama **üretim**. 

1. Yeşil bir başarı bildirimi görüntülendiğinde seçin **uç noktalar listesine bakın** bağlantı uç noktaları görmek için.

1. Bir uç nokta seçin. Bu uç noktasına yeni bir tarayıcı sekmesi açılır. Tarayıcı sekmesini açık tutun ve devam **Test** bölümü.

## <a name="test"></a>Test etme
Varlık eklendikten sonra uygulamayı eğitme gerekmez. 

Test uç noktasında yeni hedefi için bir değer olarak eklenen **q** parametresi. İçin önerilen konuşma için aşağıdaki tabloyu kullanın **q**:

|Test utterance|Varlık değeri|
|--|:--|
|Kitap bir uçuş yarın|2018-10-19|
|randevuyu Mart 3 iptal et|LUIS döndürülen en son Mart 3 Geçmiş (2018-03-03) ve Mart gelecekte 3 (2019-03-03) utterance bir yıl belirttiğinden alamadık.|
|10'da bir toplantısı zamanlayın|10:00:00|

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Önceden oluşturulmuş varlık belirteci içeren varlıklar işaretleme
 Metin gibi varsa `HH-1234`, özel bir varlık olarak işaretlemek istediğiniz _ve_ sahip olduğunuz [önceden oluşturulmuş numarası](luis-reference-prebuilt-number.md) modele eklenir, siz LUIS Portalı'nda özel varlık işaretlemek mümkün olmayacaktır. API ile işaretleyebilirsiniz. 

 Bu tür bir belirteç, burada bir bölümü, önceden oluşturulmuş bir varlık ile zaten işaretlenmiş, işaretlemek için önceden oluşturulmuş varlık LUIS uygulamadan kaldırabilir. Uygulamayı eğitme gerek yoktur. Ardından kendi özel varlık belirteciyle işaretleyin. Ardından önceden oluşturulmuş varlık LUIS uygulamaya geri ekleyin.

 Başka bir örnek için sınıf tercihleri listesi olarak utterance göz önünde bulundurun: `I want first year spanish, second year calculus, and fourth year english lit.` LUIS uygulaması eklendi, Prebuild sıra varsa `first`, `second`, ve `fourth` sıra sayıları ile işaretlenir. Sıralı ve sınıf yakalamak istiyorsanız, bileşik bir varlık oluşturun ve önceden oluşturulmuş sıralı ve sınıf adı için özel varlık etrafına sarın.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş bir varlık başvurusu](./luis-reference-prebuilt-entities.md)
