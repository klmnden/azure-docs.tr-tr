---
title: Kartlar bir konuşma öğrenen uygulamayla kullanmak için bölüm 1 - Microsoft Bilişsel hizmetler nasıl | Microsoft Docs
titleSuffix: Azure
description: Bir konuşma öğrenen uygulamayla kartları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e90ccd42b21eea6139c402937be7e20513d73c84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353981"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Kart (Kısım 1 / 2) kullanma

Bu öğretici ekleyin ve basit bir kart, bot nasıl kullanılacağını gösterir.

Konuşma öğrenen bot başlatıldığı dizinde bulunduğundan "kartları" adlı bir dizin yer kartı tanım dosyalarınızı beklediğini unutmayın.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Kullanıcının bir seçenek konuşmada seçmesine izin kullanıcı Arabirimi öğeleri kartlardır. 

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesinde Öğreticisi-13-kartları-1'i tıklatın. 

### <a name="the-card"></a>Kartı

Kart tanımı şu konumdadır: C:\<installedpath\>\src\cards\prompt.json.

Bu kartlar dizinde kartı tanımlarınızı bulmak sistem bekliyor.

![](../media/tutorial13_prompt.PNG)

- TextBlock ve sorunun şablonu unutmayın.
- İki gönderme düğmeler ve her biri için gönderilen metni vardır.

### <a name="actions"></a>Eylemler

Üç eylem oluşturduk. Aşağıda gördüğünüz gibi ilk bir kart eylemdir.

![](../media/tutorial13_actions.PNG)

Kart eylem türü nasıl oluşturulduğuna görelim:

![](../media/tutorial13_cardaction.PNG)

Soru giriş ve düğmeleri 1 ve 2 unutmayın. Soru ve yanıtlarının gireceğiniz kartı şablon başvurularında izinlerdir. Ayrıca, başvuru ve varlıklar ya da metin ve varlıkları bileşimi kullanın.

Göz simgesini kartı nasıl göründüğünü gösterir.

### <a name="train-dialog"></a>Tren iletişim

Şimdi öğretme iletişim kutusundan yol.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. 'Sola veya sağa komut istemi Git' seçmek için tıklatın.
    - 'Left' veya 'right' ı tıklatarak, kullanıcı 'left' veya 'sağ' sırasıyla yazmaya eşdeğerdir. 
4. Puan Eylemler'i tıklatın.
4. Seç 'left' tıklayın. Bu bir bekleme olmayan eylemdir.
6. 'Sola veya sağa komut istemi Git' seçmek için tıklatın.
4. 'Sağ' tıklayın.
5. Puan Eylemler'i tıklatın.
3. 'Right' seçmek için tıklatın
6. 'Sola veya sağa komut istemi Git' seçmek için tıklatın.
4. Done sınama'ı tıklatın.

Şimdi, kartları nasıl gördünüz. Bunlar kartları dizinde json şablonları olarak tanımlanır. Şablonlar, bir dize veya bir varlık veya her ikisinin bir karışımı kullanarak doldurabilir kullanıcı Arabiriminde belirir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kartlar bölüm 2](./14-cards-2.md)
