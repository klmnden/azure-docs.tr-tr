---
title: Dal oluşturma ve geri alma işlemleri bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler ile nasıl kullanılacağını | Microsoft Docs
titleSuffix: Azure
description: Dal oluşturma ve geri alma işlemleri bir konuşma Öğrenici modeliyle nasıl kullanılacağını öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 05140693026e21a73b756ed0ea7bc9936bef067e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173307"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Dallanma kullanmayı ve geri alma işlemleri
Bu öğreticide, geri alma ve dal oluşturma işlemleri alacağız.


## <a name="details"></a>Ayrıntılar
- Geri Al: "bir kullanıcı girişi veya eylem seçim geri almak" Geliştirici sağlar. Planda, "geri" gerçekten yeni bir iletişim kutusu oluşturur ve önceki adımdan kadar yeniden oynatılır.  Varlık algılama geri çağırma ve API çağrıları iletişim kutusunda Bunun anlamı tekrar çağrılır.

- Dal: mevcut bir eğitim iletişim – bu efor kaydeder gibi aynı şekilde başlayan yeni train iletişim kutusu oluşturur el ile yeniden girildiğinde iletişim kutusu açar. Planda, "dal" yeni bir iletişim kutusu oluşturur ve mevcut eğitme iletişim seçili adımın kadar yeniden oynatılır.  Varlık algılama geri çağırma ve API çağrıları iletişim kutusunda Bunun anlamı tekrar çağrılır.


## <a name="requirements"></a>Gereksinimler
Bu öğreticide, pizza sipariş bot çalışıyor olması gerekir:

    npm run demo-pizza

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi modeli listesinde TutorialDemo Pizza siparişine tıklayın. 

Pizza sipariş tanıtım hakkında daha fazla bilgi için Pizza sipariş öğretici bakın.

## <a name="undo"></a>Geri Al

Biz iletişim kutusunun bir parçası geri alabilir ve bu adımda yeniden oluşturun.

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
Eğitim oturumu başlayalım. 

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Bir pizza sipariş' girin.
2. Puan eylemini tıklatın.
3. İçin seçin 'ne, pizza üzerinde ister misiniz?' tıklayın
4. 'Mushrooms ve peynirlerine ayırıyor' girin.
5. Puan eylemleri tıklayın.
3. 'Üzerinde pizza $Toppings sahip' seçmek için tıklayın.
6. 'Başka bir şey ister misiniz?' seçin
7. 'Mushrooms kaldırın ve ekmeye Ekle' girin.
    - Mushrooms ve işaretini kaldırın Toppings varlığı seçin. Geri alacak bir eylem oluşturuyoruz.
2. Geri alma adımı tıklayın.
    - En son giriş kaldırılır ve geri 'başka bir şey istersiniz?' duyuyoruz  (aşağıdaki ekran)
2. 'Mushrooms kaldırın ve ekmeye Ekle' girin.
8. 'Üzerinde pizza $Toppings sahip' seçmek için tıklayın
    - Her iki varlıkları doğru seçildiğinden emin olun.
2. Puan eylemini tıklatın. Düzeltilmiş iletişim kutusuyla artık devam edebilirsiniz.
4. Öğretim Bitti'ye tıklayın.

Artık bir kullanıcı girişi kaldırmak için geri alma ve eylemi kullanmayı öğrendiniz.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Dal

Örneğin, şimdi bir mevcut eğitme iletişim kutusunu açın ve başka bir eğitimi iletişim dallara ayırma tarafından oluşturun.

1. Train iletişim kutuları ve ardından 'Yeni Sipariş' tıklayın mevcut iletişim kutusunu açın. 
2. Son iletişim kutusunda 'Hayır' tıklayın (aşağıdaki ekran görüntüsüne bakın).
3. Dal'ı tıklatın.
    - 'Hayır' da kaldırılır ve tüm iletişim bu noktaya kadar yeni bir tane kopyalanır. 
    - Bu noktadan itibaren önceki yeniden girmeye kapatır yeni bir "dal" keşfetmek için bu kaydeder.
1. 'Evet' girin.
2. Puan eylemini tıklatın.
3. 'Üzerinde pizza $Toppings sahip' seçin.
6. 'Başka bir şey ister misiniz?' seçin
7. 'Hayır' girin.
4. Öğretim Bitti'ye tıklayın.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma ve etiketleme](./16-versioning-and-tagging.md)
