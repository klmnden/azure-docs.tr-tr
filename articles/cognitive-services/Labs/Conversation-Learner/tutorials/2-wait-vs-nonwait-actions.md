---
title: Bekleme ve bekleme olmayan eylemleri bir konuşma öğrenen uygulaması - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Bekleme ve bekleme olmayan eylemleri bir konuşma öğrenen uygulaması ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: bb2cbd55b6c8be51213095926bb592169613d1fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353945"
---
# <a name="wait-and-non-wait-actions"></a>Bekleyin ve Eylemler bekleyin

Bu öğretici konuşma öğrenen bekleme Eylemler ve bekleme olmayan eylemleri arasındaki farkı gösterir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

- Eylem bekleyin: Sistem "bekleme" eylem gerçekleştirildikten sonra bu işlemleri yapmayı durduracak ve kullanıcı girişi için bekleyin.
- Olmayan bekleme eylem: Sistem "bekleme olmayan" eylem gerçekleştirildikten sonra hemen başka bir eylem (önce kullanıcı inpu beklemeden) seçeceksiniz.

## <a name="steps"></a>Adımlar

### <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

1. Yeni uygulama Web kullanıcı Arabiriminde tıklatın
2. WaitNonWait adı girin. Ardından, Oluştur'u tıklatın.

### <a name="create-the-first-wait-action"></a>İlk bekleme eylem oluşturma

1. Eylemler, yeni bir eylem tıklatın.
2. Yanıtta 'hangi hayvan, istiyor musunuz?' girin.
    - Bu bir bekleme eylem, bu nedenle yanıt kutusunu işaretli bekle bırakın.
3. Bitti'yi tıklatın.

### <a name="create-a-non-wait-action"></a>Olmayan bekleme eylem oluşturun

1. Yeni Eylem tıklatın
2. Yanıtta 'İnekler moo söyleyin' yazın.
3. Kaydını onay yanıtı onay kutusu bekleyin.
4. Oluştur’a tıklayın

### <a name="create-a-second-non-wait-action"></a>İkinci bir olmayan bekleme eylem oluşturma

1. Yeni Eylem tıklatın
2. Yanıtta 'Ducks quack söyleyin' yazın.
3. Kaydını onay yanıtı onay kutusu bekleyin.
4. Oluştur’a tıklayın

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>Bot eğitme

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
2. Türü 'hello'
3. Puan Eylemler'i tıklatın ve 'hangi hayvan, istiyor musunuz?' seçin.
4. 'İnek' girin
5. Puan Eylemler'i tıklatın ve 'İnekler moo söyleyin' seçin.
    - Bot için giriş beklemeleri gerekir ve bir sonraki eylem sürecek unutmayın.
2. 'Hangi hayvan, istiyor musunuz?' seçin.
3. 'Duck' girin
5. Puan Eylemler'i tıklatın ve 'Ducks quack söyleyin' seçin.

![](../media/tutorial2_dialogs.PNG)

Bekleme ve bekleme olmayan eylemleri göre bot yanıtları dizisini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlıkları giriş](./3-introduction-to-entities.md)
