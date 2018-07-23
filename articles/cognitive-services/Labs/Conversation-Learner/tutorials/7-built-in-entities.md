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
ms.openlocfilehash: 84d73add5586aaaf130253a8122a4152e39bcbe9
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171376"
---
# <a name="how-to-add-pre-built-entities"></a>Önceden derlenmiş varlıklar ekleme
Bu öğreticide, "önceden oluşturulmuş" varlıkları konuşma Öğrenici modelinize eklemek gösterilir.

## <a name="video"></a>Video

[![Öğretici 7 Preview](http://aka.ms/cl-tutorial-07-preview)](http://aka.ms/blis-tutorial-07)

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
    - Programlanabilir ve Negatable seçenekleri devre dışıdır, bu varlıkları derleme öncesi geçerli değildir.
3. Oluştur’a tıklayın.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler ve ardından yeni bir eylem tıklayın
2. Yanıt olarak, türü ' $luis tarihtir-datetimev2'.
3. Oluştur’a tıklayın.

![](../media/tutorial7_actions.PNG)

Ardından ikinci bir eylem oluşturun:

1. Eylemler ve ardından yeni bir eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta 'tarih nedir?' yazın.
4. 'Datetimev2 luıs' nitelemesini varlıklarda girin.
4. Oluştur’a tıklayın

![](../media/tutorial7_actions2.PNG)

Artık iki eylem var.

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. 'Hello' yazın.
3. Puan Eylemler ve 'tarih nedir?' seçin
2. 'Bugün' girin. 
    - Bildirim bugün etiketlenmiş ve ikinci satırında, önceden oluşturulmuş bir varlık olduğundan gösterilir ve düzenlenemez.
5. Puan Eylemler
    - Artık tarih varlığı bellek bölümünde göründüğüne dikkat edin. 
    - Tarih üzerinde fare, kullanılabilir LUIS tarafından sağlanan ek verileri görürsünüz ve kodda daha yönetilebilir. 
6. Seç ' $luis tarihtir-datetimev2'.
7. Öğretim bitti seçeneğine tıklayın

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Diğer girişler](./8-alternative-inputs.md)
