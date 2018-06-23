---
title: Kartlar bir konuşma öğrenen uygulamayla kullanmak için bölüm 2 - Microsoft Bilişsel hizmetler nasıl | Microsoft Docs
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
ms.openlocfilehash: 254f0953fd3e281a35857e69d9795e3decebf45d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353963"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Kart (Kısım 1 / 2) kullanma
Bu öğretici, bot doldurulabilir form kartı eklemek nasıl gösterir. Form alanlarını varlıklarının içinde nasıl hareket gösterir.

Konuşma öğrenen bot başlatıldığı dizinde bulunduğundan, "kart" adlı bir dizin yer kartı tanım dosyalarınızı bekliyor.

## <a name="requirements"></a>Gereksinimler
Bu öğretici genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Kullanıcının bir seçenek konuşmada seçmesine izin kullanıcı Arabirimi öğeleri kartlardır. 

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesinde Öğreticisi-14-kartları-2'yi tıklatın. 

### <a name="the-card"></a>Kartı

Kart tanımı şu konumdadır: C:\<installedpath\>\src\cards\shippingAddress.json.

Bu kart sevkiyat adresinin üç alan toplar: Şehir, sokak ve durumu.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Eylemler

Üç eylem oluşturduk. Aşağıda gördüğünüz gibi ilk bir kart eylemdir.

![](../media/tutorial14_actions.PNG)

Kart eylem türü nasıl oluşturulduğuna görelim:

- Adres-Sokak türünün Input.text ve kimliğini olduğu dikkat edin
- Benzer şekilde, Adres Şehir ve bir aşağı açılan ile kimliği, adres durumu yok.

Kimlikler alanları doldurulur ve gönderilen olduğunda, bu değerleri bot alacak varlık adları olanlardır gibi önemlidir.

## <a name="entities"></a>Varlıklar
Biz yukarıda gördüğümüz gibi kartı eşleşen üç varlık tanımladınız.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Eylemler

Şu iki eylem tanımladınız.

![](../media/tutorial14_actions.PNG)

- İlk burada eylem türü kartı ve Şablon açılır shippingAddress olarak seçildiği sevkiyat adresi karttır.
- İkinci geri teslimat adresi okumak için basit bir işlemdir.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Tren iletişim

Şimdi öğretme iletişim kutusundan yol.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Merhaba' girin.
2. Puan eylemini tıklatın.
3. 'Sevkiyat adresini' seçmek için tıklatın.
4. Kart doldurun ve gönderin.
    - Bu değerleri şimdi varlık belleğe taşınmış dikkat edin. Form zaten girişleri bölümlenmiş gibi hiçbir ayrıştırma gereklidir.
5. Puan Eylemler'i tıklatın.
3. Seçmek için tıklatın '... $Address aktarma ".
4. Done sınama'ı tıklatın.

![](../media/tutorial14_train_dialog.PNG)

Şimdi doldurulabilir alanlar ve bırakmalar, kartından değerleri alır ve yakalamak ve bunları bot varlıklarda toplamak öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dal oluşturma ve geri alma](./15-branching-and-undo.md)
