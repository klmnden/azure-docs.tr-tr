---
title: Konuşma Öğrenici modeliyle kartları kullanma hakkında bölüm 1 - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle kartları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 988a2433f098f41bca4796299825293efd4de44b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171151"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Kart (Kısım 1 / 2) kullanma

Bu öğreticide, ekleyebilir ve basit bir kart botunuzun içinde gösterilir.

> [!NOTE]
> Şu anda konuşma Öğrenici bot başlatıldığı dizinde mevcut "kartları" adlı bir dizinde bulunmasını kart tanım dosyalarınızı bekliyor. Bu gelecekte yapılandırılabilir bulunacağız.

## <a name="video"></a>Video

[![Öğreticisi 13 Önizleme](http://aka.ms/cl-tutorial-13-preview)](http://aka.ms/blis-tutorial-13)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Konuşmada bir seçenek belirlemek kullanıcının olanak tanıyan kullanıcı Arabirimi öğeleri kartlardır. 

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi modeli listesinde Öğreticisi-13-kartları-1'i tıklatın. 

### <a name="the-card"></a>Kart

Şu konumda kart tanımıdır: C:\<installedpath\>\src\cards\prompt.json.

Bu kartlar dizinde kart tanımlarınızı bulmak sistem bekliyor.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> Gövde türü fark `TextBlock` ve `{{question}}` yer tutucu metin alanına.
> İki gönderme düğmeler ve her biri için gönderilen metin vardır.

### <a name="actions"></a>Eylemler

Üç eylem oluşturduk. Aşağıda görebileceğiniz gibi bir kart olduğunda ilk eylem.

![](../media/tutorial13_actions.PNG)

Diyelim ki kart eylem türünü nasıl oluşturulduğuna bakın:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> Soru giriş ve düğmeleri 1 ve 2. Kart şablonu başvurular soru ve yanıtlarının girebileceğiniz olanlardır. Ayrıca, başvuru ve varlıkları ya da metin ve varlıkların bir karışımını kullanabilirsiniz.

Simgelerine kart nasıl göründüğünü gösterir.

### <a name="train-dialog"></a>Train iletişim

Öğretim iletişim kutusundan atalım.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. 'Sol veya sağ istemi Git' seçmek için tıklayın.
    - 'Left' veya 'right', 'left' veya 'doğru' sırasıyla kullanıcı yazmakla eşdeğerdir. 
4. Puan eylemleri tıklayın.
4. Seç 'left' tıklayın. Bekleme olmayan eylem budur.
6. 'Sol veya sağ istemi Git' seçmek için tıklayın.
4. 'Doğru' tıklayın.
5. Puan eylemleri tıklayın.
3. 'Right' değerine seçmek için tıklayın
6. 'Sol veya sağ istemi Git' seçmek için tıklayın.
4. Yapılan test tıklayın.

Şimdi, kartların nasıl çalıştığını öğrendiniz. Bunlar kartlar dizinde json şablonları tanımlanır. Şablonlar, bir dize veya bir varlık veya her ikisi de bir karışımını kullanarak doldurabilirsiniz kullanıcı Arabiriminde belirir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kartları bölüm 2](./14-cards-2.md)
