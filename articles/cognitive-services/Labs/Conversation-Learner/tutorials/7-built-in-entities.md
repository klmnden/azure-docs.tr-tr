---
title: Bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler için önceden derlenmiş varlıklar ekleme | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modele önceden derlenmiş varlıklar ekleme konusunda bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: cdd9ad16096c85db21829840b2bfd7acaced5942
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683517"
---
# <a name="how-to-add-pre-built-entities"></a>Önceden derlenmiş varlıklar ekleme
Bu öğreticide, "önceden oluşturulmuş" varlıkları konuşma Öğrenici modelinize eklemek gösterilir.

## <a name="video"></a>Video

[![Öğretici 7 Preview](https://aka.ms/cl-tutorial-07-preview)](https://aka.ms/blis-tutorial-07)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Önceden derlenmiş varlıklar, sayılar, tarihler, parasal tutarların ve diğerleri gibi varlıkları genel türleri tanır.  Özel varlıklar farklı olarak "out-of--box" çalışma ve tüm eğitim gerektirmez.  Özel varlıklar davranışları değiştirilemez.  Varsayılan olarak, önceden derlenmiş varlıklar birden çok değerli - tanımlanan her varlık örneği botun bellek diğer bir deyişle, accumulate.

## <a name="steps"></a>Adımlar

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. BuiltInEntities adı girin. Oluştur'a tıklayın.

### <a name="create-an-entity"></a>Bir varlık oluşturma

1. Varlıklar ve ardından yeni bir varlık tıklayın.
2. EntityType açılır listesini tıklatın ve datetimev2 seçin.
    - Programlanabilir ve Negatable seçenekleri devre dışıdır ve önceden derlenmiş varlıklar için geçerli değildir.
3. Oluştur’a tıklayın.

![](../media/tutorial7_entities_a.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın.
1. Yanıt olarak, türü ' $builtin tarihtir-datetimev2'.
1. Gerekli varlıklarda girin ' $builtin-datetimev2'.
1. Oluştur’a tıklayın.

![](../media/tutorial7_actions_a.PNG)

Ardından ikinci bir eylem oluşturun:

1. Eylemler ve ardından yeni bir eylem ikinci bir eylem oluşturmak için tıklayın.
1. Yanıtta 'tarih nedir?' yazın.
1. Eleyerek varlıklarda girin ' $builtin-datetimev2'.
1. Oluştur’a tıklayın.

![](../media/tutorial7_actions2_a.PNG)

Artık iki eylem var.

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'tarih nedir?' seçin
2. 'Bugün' girin. 
    - Bildirim bugün etiketlenmiş ve ikinci satırında, önceden oluşturulmuş bir varlık olduğundan gösterilir ve düzenlenemez.
5. Puan eylemleri tıklayın.
    - Artık tarih varlığı bellek bölümünde göründüğüne dikkat edin. 
    - Tarih üzerinde fare, kullanılabilir LUIS tarafından sağlanan ek verileri görürsünüz ve kodda daha yönetilebilir. 
6. Seç ' $builtin tarihtir-datetimev2'.
7. Öğretim Bitti'ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Diğer girişler](./8-alternative-inputs.md)
