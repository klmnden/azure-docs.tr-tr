---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler bekleyin ve bekleme olmayan eylemlerini kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle bekleyin ve bekleme olmayan Eylemler kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f349dab23b9156d3a5656e8275533ebe6a82cdf9
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45540942"
---
# <a name="wait-and-non-wait-actions"></a>Bekleyin ve eylemlerin-wait

Bu öğretici konuşma Öğrenici bekleme Eylemler ve bekleme olmayan Eylemler arasındaki farkı gösterir.

## <a name="video"></a>Video

[![Öğretici 2-Önizleme](http://aka.ms/cl-tutorial-02-preview)](http://aka.ms/blis-tutorial-02)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

- Eylem bekleyin: Sistem "wait" eylem gerçekleştirildikten sonra bu eylemleri almayı durdurmak ve kullanıcı girişini bekleme.
- Non-wait eylem: Sistem "olmayan-wait" eylem gerçekleştirildikten sonra bunu hemen başka bir eylem (kullanıcı için ilk giriş beklemenize gerek kalmadan) seçersiniz.

## <a name="steps"></a>Adımlar

### <a name="create-a-new-model"></a>Yeni model oluştur

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. WaitNonWait adı girin. Oluştur'a tıklayın.

### <a name="create-the-first-wait-action"></a>İlk bekleme Eylem oluştur

1. Eylemler ve ardından yeni bir eylem tıklayın.
2. Yanıtta 'hangi donatarak istiyorsunuz?' girin.
    - Bu bir bekleme eylemi, böylece bekleyin yanıt kutusunu işaretli bırakın.
3. Oluştur’a tıklayın.

### <a name="create-a-non-wait-action"></a>Non-Wait Eylem oluştur

1. Yeni Eylem tıklayın
2. Yanıtta 'İnekler moo söyleyin' yazın.
3. Onay kutusu yanıtı bekle kaldırma denetleyin.
4. Oluştur’a tıklayın

### <a name="create-a-second-non-wait-action"></a>İkinci bir bekleme olmayan Eylem oluştur

1. Yeni Eylem tıklayın
2. Yanıtta 'Ördek quack söyleyin' yazın.
3. Onay kutusu yanıtı bekle kaldırma denetleyin.
4. Oluştur’a tıklayın

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>Botunuzu

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
2. Türü 'hello'
3. Puan Eylemler ve 'hangi donatarak istiyorsunuz?' seçin.
4. 'İnek' girin
5. Puan Eylemler ve 'İnekler moo söyleyin' seçin.
    - Bot girişini bekleme değil ve sonraki eylemi gerçekleştirir.
2. 'Hangi donatarak istiyorsunuz?' seçin.
3. 'Duck' girin
5. Puan Eylemler ve 'Ördek quack söyleyin' seçin.

![](../media/tutorial2_dialogs.PNG)

> [!NOTE]
> Bot yanıtları bakımından bekleyin ve bekleme olmayan Eylemler dizisi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlıkları giriş](./3-introduction-to-entities.md)
