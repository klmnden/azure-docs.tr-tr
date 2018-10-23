---
title: Önceden oluşturulmuş varlıklar için Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık destek LUIS uygulamanızı kültüre göre değişir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: 2f7c724b14efd569a5993f9a9319c9004874bc43
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647604"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Ortak veri türleri tanımak için önceden oluşturulmuş varlıklar

LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. 

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

1. Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **varlıkları** sol tarafındaki. 

1. Üzerinde **varlıkları** sayfasında **önceden oluşturulmuş varlıklarla yönetme**.

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


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş bir varlık başvurusu](./luis-reference-prebuilt-entities.md)
