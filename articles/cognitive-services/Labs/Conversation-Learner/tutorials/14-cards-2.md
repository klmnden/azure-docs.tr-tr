---
title: Konuşma Öğrenici modeliyle kartları kullanma hakkında bölüm 2 - Microsoft Bilişsel hizmetler | Microsoft Docs
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
ms.openlocfilehash: 1c7c88742c69041594006add76f7e3c642c64dec
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170581"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Kart (Kısım 1 / 2) kullanma
Bu öğreticide, botunuzun için doldurulabilir form kart Ekle gösterilmektedir. Form alanlarını varlıklara nasıl hareket gösterilir.

Konuşma Öğrenici kart tanım dosyalarınızı olduğu bot başlatıldığı dizinde "kartları" adlı bir dizinde bulunmasını bekliyor.

## <a name="video"></a>Video

[![Öğretici 14 Önizleme](http://aka.ms/cl-tutorial-14-preview)](http://aka.ms/blis-tutorial-14)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Konuşmada bir seçenek belirlemek kullanıcının olanak tanıyan kullanıcı Arabirimi öğeleri kartlardır. 

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi modeli listesinde Öğreticisi-14-kartları-2'yi tıklatın. 

### <a name="the-card"></a>Kart

Şu konumda kart tanımıdır: C:\<installedpath\>\src\cards\shippingAddress.json.

Bu kart teslimat adresini üç alanı toplar: Şehir ve Sokak durumu.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Eylemler

Üç eylem oluşturduk. Aşağıda görebileceğiniz gibi bir kart olduğunda ilk eylem.

![](../media/tutorial14_actions.PNG)

Diyelim ki kart eylem türünü nasıl oluşturulduğuna bakın:

- Adres-Cadde türünün Input.text ve kimliğini olduğu dikkat edin.
- Benzer şekilde, Adres Şehir ve açılan listesi ile kimliği, adres durumu yoktur.

Kimlikleri alan doldurulur ve gönderilen, bu bot bu değerleri alacak varlık adları olduğundan önemlidir.

## <a name="entities"></a>Varlıklar
Kart üzerinde gördüğümüz gibi eşleşen üç varlıkları tanımladığımız.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Eylemler

İki eylem tanımladığımız.

![](../media/tutorial14_actions.PNG)

- Burada eylem türü kart, şablon olarak shippingAddress açılır listeden seçilen teslimat adresi kart davranıştır.
- İkinci teslimat adresini okunacak basit bir işlemdir.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Train iletişim

Öğretim iletişim kutusundan atalım.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. 'Teslimat adresi' seçmek için tıklayın.
4. Karttaki doldurun ve gönderin.
    - Bu değerler artık varlık belleğe taşınmış dikkat edin. Formun zaten girişleri bölümlenmiş gibi hiçbir ayrıştırma gereklidir.
5. Puan eylemleri tıklayın.
3. Seçmek için tıklatın '... $Address aktarma ".
4. Yapılan test tıklayın.

![](../media/tutorial14_train_dialog.PNG)

Artık doldurulabilir alanları ve listeler kartından değerleri almak ve yakalamak ve bunları bot varlıklarda toplamak nasıl düştüğünü gözlemledik.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dallandırma ve geri alma](./15-branching-and-undo.md)
