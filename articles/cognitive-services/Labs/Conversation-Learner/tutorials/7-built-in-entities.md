---
title: Önceden derlenmiş varlıklar bir konuşma öğrenen uygulama - Microsoft Bilişsel hizmetler eklemek nasıl | Microsoft Docs
titleSuffix: Azure
description: Önceden derlenmiş varlıklar bir konuşma öğrenen uygulamaya eklemeyi öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f014464419bfac39a9e57e679fcd28a737e9ebdb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355090"
---
# <a name="how-to-add-pre-built-entities"></a>Önceden derlenmiş varlıklar ekleme
Bu öğretici "önceden oluşturulmuş" varlıklar konuşma öğrenen uygulamanıza eklemek gösterilmektedir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Önceden derlenmiş varlıklar varlıklar, sayıları, tarihleri, para tutarlar ve diğerleri gibi yaygın türlerini kabul eder.  Özel varlıklar aksine "out-of--box" çalışır ve hiçbir eğitim gerektirmez.  Özel varlıklar, davranışlarını değiştirilemez.  Varsayılan olarak, önceden oluşturulmuş varlıkları birden çok değerli - bot'ın bellek tanımlanan her varlık örneği başka bir deyişle, toplanacaktır.

## <a name="steps"></a>Adımlar

### <a name="create-the-application"></a>Uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. BuiltInEntities adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-an-entity"></a>Bir varlık oluşturun

1. Varlıklar, yeni varlık tıklatın.
2. EntityType aşağı üzerinde tıklatın ve datetimev2 seçin.
    - Derleme öncesi varlıklar geçerli değil çünkü programlanabilir ve Negatable seçenekleri devre dışıdır.
3. Oluştur’a tıklayın.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>İki eylem oluşturma

1. Eylemler tıklatın, yeni eylem
2. Yanıtta, türü ' $luis tarihidir-datetimev2'.
3. Oluştur’a tıklayın.

![](../media/tutorial7_actions.PNG)

Sonra ikinci bir eylem oluşturun:

1. Eylemler, sonra yeni eylem ikinci bir eylem oluşturmak için tıklayın.
3. Yanıtta 'Tarihi nedir?' yazın.
4. Adayını varlıklarda 'Haluk-datetimev2' girin.
4. Oluştur’a tıklayın

![](../media/tutorial7_actions2.PNG)

Şimdi iki eylemlere sahiptir.

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. 'Hello' yazın.
3. Puan Eylemler'i tıklatın ve 'Tarihi nedir?' seçin
2. 'Bugün' girin. 
    - Bildirim bugün etiketlenir ve ikinci satırında, önceden oluşturulmuş bir varlık olduğundan görüntülenir ve düzenlenemez.
5. Puan Eylemler
    - Varlık bellek bölümünde tarih şimdi göründüğüne dikkat edin. 
    - Tarih fare, kullanılabilir HALUK tarafından sağlanan ek verileri görürsünüz ve kodda daha fazla işlenebilir. 
6. Seç ' $luis tarihidir-datetimev2'.
7. Öğretme Bitti'yi tıklatın

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alternatif girişleri](./8-alternative-inputs.md)
