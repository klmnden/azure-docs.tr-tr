---
title: Dal oluşturma ve geri alma işlemleri bir konuşma öğrenen uygulama - Microsoft Bilişsel hizmetler ile nasıl kullanılacağı | Microsoft Docs
titleSuffix: Azure
description: Dal oluşturma ve geri alma işlemleri bir konuşma öğrenen uygulama ile nasıl kullanılacağı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 724a9e47267e0bd7417130efe54c609ac7a465fb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354034"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Dal oluşturma kullanmayı ve geri alma işlemleri
Bu öğreticide, geri alma ve dallanma işlemler alacağız.

## <a name="details"></a>Ayrıntılar
- Geri Al: "kullanıcı girişi veya eylem seçeneği geri almak" developer sağlar. Arka planda "geri alma" gerçekten yeni bir iletişim kutusu oluşturur ve kadar önceki adımı yeniden oynatılır.  Varlık algılama geri çağırma ve API çağrıları iletişim kutusunda Bunun anlamı tekrar çağrılır.

- Dal: Varolan eğitmek iletişim – bu efor kaydeder gibi aynı şekilde başlayan yeni bir tren iletişim oluşturur el ile yeniden girmeden iletişim kutusu açar. Arka planda "dal" yeni bir iletişim kutusu oluşturur ve seçili adımı kadar varolan tren iletişim yeniden oynatılır.  Varlık algılama geri çağırma ve API çağrıları iletişim kutusunda Bunun anlamı tekrar çağrılır.


## <a name="requirements"></a>Gereksinimler
Bu öğretici pizza sipariş bot çalışıyor olması gerekir:

    npm run demo-pizza

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesinde TutorialDemo Pizza siparişe tıklayın. 

Pizza sipariş demo hakkında daha fazla bilgi için Pizza sipariş öğretici bakın.

## <a name="undo"></a>Geri Al

Biz iletişim kutusunun bir parçası geri alabilir ve bu adımdaki yeniden oluşturun.

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
Eğitim oturumu başlayalım. 

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Bir pizza sipariş' girin.
2. Puan eylemini tıklatın.
3. 'Ne, pizza üzerinde istiyorsunuz?' Select için tıklatın
4. 'Mushrooms ve Peynir' girin.
5. Puan Eylemler'i tıklatın.
3. 'Üzerinde pizza $Toppings sahip' seçmek için tıklatın.
6. ', Başka bir şey ister misiniz?' seçin
7. 'Mushrooms kaldırın ve peppers Ekle' girin.
    - Mushrooms ve kaldırma onay Toppings varlığı seçin. Biz geri bir eylem oluşturuyoruz.
2. Geri alma adımı'ı tıklatın.
    - En son giriş kaldırılır ve geri 'başka bir şey istersiniz?' olan unutmayın  (aşağıdaki ekran görüntüsünde)
2. 'Mushrooms kaldırın ve peppers Ekle' girin.
8. 'Üzerinde pizza $Toppings sahip' seçmek için tıklatın
    - Her iki varlığa doğru seçili olduğundan emin olun.
2. Puan eylemini tıklatın. Düzeltilmiş iletişim kutusuyla şimdi devam edebilirsiniz.
4. Öğretme Bitti'yi tıklatın.

Şimdi bir kullanıcı girişi kaldırmak için geri alma ve eylem kullanmayı gördünüz.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Dal

Örnek olarak, şimdi varolan bir tren iletişim kutusunu açın ve başka bir tren iletişim dallara ayırma tarafından oluşturun.

1. Tren iletişim kutuları, ardından 'yeni sırası' tıklayın varolan iletişim kutusunu açın. 
2. Son iletişim kutusunda 'Hayır' tıklayın (ekran görüntüsüne bakın).
3. Dal'ı tıklatın.
    - 'Hayır' kaldırılır ve bu noktaya kadar tüm iletişim yeni bir tane kopyalanır unutmayın. 
    - Bu noktadan, yukarıdaki yeniden girmeye kapatır yeni bir "dalı" keşfetmek için bu kaydeder.
1. 'Evet' seçeneğini girin.
2. Puan eylemini tıklatın.
3. 'Üzerinde pizza $Toppings sahip' seçin.
6. ', Başka bir şey ister misiniz?' seçin
7. 'Hayır' girin.
4. Öğretme Bitti'yi tıklatın.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma ve etiketleme](./16-versioning-and-tagging.md)
