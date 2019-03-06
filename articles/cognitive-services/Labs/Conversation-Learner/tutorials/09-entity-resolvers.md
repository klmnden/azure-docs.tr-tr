---
title: Microsoft Bilişsel hizmetler - konuşma Öğrenici modelinde varlık Çözümleyicileri | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici varlık Çözümleyicileri kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ebe555bfd7b34efd87d400d786049964665c76e6
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57451068"
---
# <a name="entity-resolvers"></a>Varlık çözümleyiciler

Bu öğreticide, varlık Çözümleyicileri konuşma Öğrenici içinde kullanmak gösterilir

## <a name="video"></a>Video

[![Varlık Çözümleyicileri öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

- Çözümleyici türü özel varlıklar isteğe bağlı bir özelliktir.
- Varlık Çözümleyicileri özel varlığınız için ek ayrıntılar ve netlik sağlamak için LUIS önceden eğitilmiş varlık tanıyıcılar işlem gücünü kullanır.

## <a name="steps"></a>Adımlar

### <a name="create-a-new-model"></a>Yeni Model Oluştur

1. Web kullanıcı Arabiriminde "Yeni modeli" düğmesine tıklayın.
2. "Name" alanında "Varlık Çözümleyicileri" yazın, isabet girin ya da "Oluştur" düğmesine tıklayın.

### <a name="create-a-pair-of-entities"></a>Varlıkların bir çifti oluşturma

1. Sol paneldeki "Varlık"'a tıklayın ve ardından "Yeni varlık" düğmesine tıklayın.
2. "Varlık adı" alanında "ayrılma" yazın.
3. "DatetimeV2" "Çözümleyici türü" açılan listesinde seçin.
4. "Oluştur" düğmesine tıklayın.
5. Bilgiler açılan okuduktan sonra "Tamam" düğmesini tıklatın.
6. Aynı adımları izleyerek da çözümleyici "datetimeV2" türünde "return" adlı başka bir varlık oluşturun.

### <a name="create-a-pair-of-actions"></a>Bir eylem çifti oluşturma

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt" alan türü, ", üzerinde $departure bırakarak ve üzerinde $return döndüren".
    - Önemli - "çıkmaya ihtiyacınız $ içinde [entityName]" yazarak girin ya da açılan varlık tıklayın, aksi takdirde konuşma Öğrenici bunu bir varlık yerine metin olarak dikkate alacaktır.
    - "Varlık gerekli" alanı ayrıca bu varlıkları alır ve kaldırılamaz dikkat edin. Bu, her ikisi de mevcut varlıkları gerekli kadar bu eylemin kullanılabilir hale gelmeden engeller.
3. "Oluştur" düğmesine tıklayın.
4. İkinci bir eylemi yeniden oluşturmak için "Yeni Eylem" düğmesine tıklayın.
5. İçinde "Botun yanıt", "Olduğunda, seyahat planlıyor musunuz?" türü, alan.
6. "Varlıkları eleyerek" alan türü, "ayrılma" ve türü Ayrıca, "return".
    - Bu, bizim Bot bu varlıkların herhangi bir değer içeriyorsa, bu eylem olmayacağına söyleyin.
7. "Oluştur" düğmesine tıklayın.


### <a name="training"></a>Eğitim

1. İzleme "eğitim: [Status]" "tamamlanması için" tamamlanmasını bekleyin ve sayfayı parçası sol üst.
    - Bu çok uzun sürerse "Yenile" bağlantısına tıklayabilirsiniz.
    - Biz modeli eğitmek, bizim varlık Çözümleyicileri çalışmak üzere durum Eğitim "Completed" gereklidir.
2. Sol paneldeki "Train iletişim kutuları"'a tıklayın ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
3. İlk kullanıcı utterance, "bana bir kitap" türü. 
4. "Puan Eylemler" düğmesine tıklayın.
5. "Olduğunda, seyahat planlıyor musunuz?" yanıt'ı seçin.
6. Bir kullanıcı olarak, "yarın bırakarak ve sonraki hafta Pazar döndüren" yanıt.
    - Kullanıcı "tarih önceden eğitilmiş" iki varlıktaki kapatma konuşma Öğrenici nasıl algıladı dikkat edin.
7. "Varlık algılama" panelinde metni "yarın" seçin ve "ayrılma" etiketleyin
8. Ayrıca "Sunday sonraki hafta" metin "return" etiket
9. "Puan Eylemler" düğmesine tıklayın.
    - "Bellek" bölmesinde kalkış ve dönüş tarihler nasıl içerdiğine dikkat edin.
    - Her birinin gelin ve "yarın" ve "Sunday" ın aksine gerçek takvim tarihi açıkça yakalama tarih nesneleri nasıl varlıklardır gözlemleyin.
10. ", Bırakarak üzerinde..." seçin Bot yanıtı.
11. "Kaydet" düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Diğer girişler](./10-alternative-inputs.md)
