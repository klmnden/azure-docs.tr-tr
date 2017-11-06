---
title: "API'nizi Azure API Management'te güncelleştirmek için düzeltmelerini kullanın | Microsoft Docs"
description: "API Management'te düzeltmelerini kullanarak bölünemez değişiklik yapma hakkında bilgi edinmek için Bu öğreticide adımları izleyin."
services: api-management
documentationcenter: 
author: mattfarm
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2017
ms.author: apimpm
ms.openlocfilehash: 0d67166a16ae4d640080ad83e7625e594b0dc246
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="make-non-breaking-changes-safely-using-revisions"></a>Güvenli bir şekilde düzeltmelerini kullanarak bölünemez değişiklik yapma
Bu öğreticide, API için güvenli bir şekilde değişiklik yaparak, geliştiricilerin değişikliği iletişim açıklar.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için bir API Management hizmeti oluşturabilir ve varolan bir API (konferans API yerine) alter gerekir.

## <a name="about-revisions"></a>Düzeltmeler hakkında
API'nizi gitmek hazır ve geliştiriciler tarafından kullanılması başlatır, genellikle bu API, API arayanlar kesintiye değil olarak şekilde için - değişiklik yaparken dikkatli gerekir. Yaptığınız değişiklikler hakkında bilmeniz geliştiriciler izin vermek yararlıdır. Azure API Management kullanarak bunu yapabilirsiniz **düzeltmelerini**.

## <a name="walkthrough"></a>Kılavuz
Bu kılavuzda Biz yeni bir düzeltme eklenmesini, bir işlem ekleyin ve sonra bu değişiklik yapıldığında değişiklik günlük girişi oluşturma - geçerli olun.

## <a name="add-a-new-revision"></a>Yeni bir düzeltme eklenmesini
1. Gözat **API'leri** API Management hizmetiniz Azure portalında sayfasında.
2. Seçin **konferans API** API listeden seçip **düzeltmelerini** sayfanın üstüne yakın menüsünden sekmesi.
3. Seçin **+ düzeltme ekler**

    > [!TIP]
    > Ayrıca seçebilirsiniz **ekleme düzeltme** bağlam menüsünden (**...** ) API üzerinde.
![Ekranın üstünde yakın düzeltmelerini menüsü](media/api-management-getstarted-revise-api/TopMenu.PNG)

4. Hangi onu için kullanılacağını unutmayın yardımcı olmak için yeni düzeltme için bir açıklama sağlayın.
5. Seçin **oluşturma**
6. Artık, yeni bir düzeltme oluşturulur.

    > [!NOTE]
    > İçinde özgün API kalır **düzeltme 1**. Kullanıcılarınız farklı bir düzeltme geçerli hale seçene kadar çağrılacak devam edecek düzeltme budur.

## <a name="make-non-breaking-changes-to-your-revision"></a>Bölünemez değişiklik yapmak için bir düzeltme
1. Seçin **tasarım** sekmesinde ekranın üstünde yakın.
2. Dikkat **düzeltme Seçici** (doğrudan Tasarım sekmesi), geçerli düzeltmeye gösterir **düzeltme 2**.

    > [!TIP]
    > Üzerinde çalışmak istediğiniz düzeltmelerini arasında geçiş yapmak için düzeltme seçiciyi kullanın.

3. Seçin **+ ekleme işlemi**.
4. Yeni işleminizi olması için ayarlama **POST**, ad & işlem olarak görünen adını ve **geri bildirim**
5. **Kaydet** yeni işlemi.
6. Biz şimdi bir değişiklik yapmış olduğunuz **düzeltme 2**. Kullanım **düzeltme Seçici** dönmek için sayfanın üstüne yakın **düzeltme 1**.
7. Yeni işleminizi görünmez bildirimi **düzeltme 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Düzeltme geçerli hale ve değişiklik günlüğü girişi Ekle
1. Seçin **düzeltmelerini** sayfanın üstüne yakın menüsünden sekmesi.
![Düzeltme ekran düzeltme menüsünde.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
2. Bağlam menüsünü açın (**...** ) için **düzeltme 2**.
3. Seçin **geçerli hale**.
![Düzeltme geçerli hale ve değişiklik günlüğü için gönderme](media/api-management-getstarted-revise-api/MakeCurrent.PNG)
4. Seçin **bu API için ortak değişiklik günlüğüne Post**
5. Geliştiriciler, örneğin görürsünüz, değişiklik için bir açıklama sağlayın **"Yeni geri bildirim işlemi eklendi."**
6. **Düzeltme 2** artık geçerli değil.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Değişiklikleri görmek ve değişiklik günlüğü için geliştirici portalına göz atın
1. Seçin **Geliştirici Portalı** üstteki menüden.
2. Seçin **API'leri**ve ardından **konferans API**.
3. Yeni fark **geri bildirim** işlemi artık kullanılabilir durumdadır.
4. Seçin **API değişiklik geçmişini** aşağıdaki API adı.
5. Değişiklik günlüğü girişi bu listede görüntülendiğine dikkat edin.
![Geliştirici Portalı üzerindeki değişiklik günlüğü](media/api-management-getstarted-revise-api/ChangeLogDevPortal.PNG)

## <a name="next-steps"></a>Sonraki adımlar
[Azure API Management ile API sürümlerini yayımlama](#api-management-getstarted-publish-versions.md)