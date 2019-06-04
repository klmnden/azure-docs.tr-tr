---
title: Konuşma Öğrenici modelinde - Azure Bilişsel hizmetler varlık Çözümleyicileri | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici varlık Çözümleyicileri kullanmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 51f74f504f0ad70c8c7f73be8ee6a05add685824
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475746"
---
# <a name="entity-resolvers"></a>Varlık çözümleyiciler

Bu öğreticide, varlık Çözümleyicileri konuşma Öğrenici içinde kullanmak gösterilir

## <a name="video"></a>Video

[![Varlık Çözümleyicileri öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

- Çözümleyici türü özel varlıklar isteğe bağlı bir özelliktir.
- Varlık Çözümleyicileri özel varlığınız için ek ayrıntılar ve netlik sağlamak için LUIS önceden eğitilmiş varlık tanıyıcılar işlem gücünü kullanır.

## <a name="steps"></a>Adımlar

Giriş sayfasında Web kullanıcı arabiriminde başlatın.

### <a name="create-the-model"></a>Model oluşturma

1. Seçin **yeni modeli**.
2. Girin **varlık Çözümleyicileri** için **adı**.
3. **Oluştur**’u seçin.

### <a name="create-a-pair-of-entities"></a>Varlıkların bir çifti oluşturma

1. Seçin **varlıkları** sol bölmesinde, ardından **yeni varlık**.
2. Girin **kalkış** için **varlık adı**.
3. Seçin **datetimeV2** için **çözümleyici türünün**.
4. **Oluştur**’u seçin. Bilgilendirme açılan seçerek Kapat **Tamam**.
5. Adlı ikinci bir varlık oluşturmak için 1-4 arasındaki adımları yineleyin **dönüş** ile **datetimeV2** çözümleyici türü.

![](../media/T09_entities.png)

### <a name="create-a-pair-of-actions"></a>Bir eylem çifti oluşturma

1. Seçin **eylemleri** sol bölmesinde, ardından **yeni eylem**.
2. Girin **üzerinde $departure bırakarak ve üzerinde $return döndüren** için **Botun yanıt...** .
    - ÖNEMLİ - [entityName] $ içinde çıkmaya ihtiyacınız yazarak girin veya varlık aksi konuşma Öğrenici açılan tıklayarak bunu bir varlık yerine metin olarak dikkate alacaktır.
    - Dikkat **gerekli varlıkları** alan da elde edersiniz bu varlıkları ve kaldırılamaz. Bu, her ikisi de mevcut varlıkları gerekli kadar bu eylemin kullanılabilir hale gelmeden engeller.
3. **Oluştur**’u seçin.
4. Seçin **yeni eylem** ikinci bir eylem oluşturmak için.
5. Girin **ne zaman, planlıyorsanız seyahat?** için **Botun yanıt...** .
6. Girin **kalkış** ve **dönüş** için **eleyerek varlıkları**. Bu, bizim Bot bu varlıkların herhangi bir değer içeriyorsa, bu eylem olmayacağına söyleyin.
7. **Oluştur**’u seçin.

![](../media/T09_actions.png)

### <a name="training"></a>Eğitim

1. İzleme **eğitim: [Status]** için sol alt köşedeki **tamamlandı**.
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

![](../media/T09_training.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Enum varlıklar](./tutorial-enum-set-entity.md)
