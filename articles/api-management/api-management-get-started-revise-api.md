---
title: "Azure API Management'te güvenle bölünemez değişiklik yapmak için düzeltmelerini kullanın | Microsoft Docs"
description: "API Management'te düzeltmelerini kullanarak bölünemez değişiklik yapma hakkında bilgi edinmek için Bu öğreticide adımları izleyin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 50d7ac17faebb34f1a1f9a3259aa0196950391d9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Düzeltmeleri güvenle bölünemez değişiklik yapmak için kullanın
API'nizi gitmek hazır ve geliştiriciler tarafından kullanılması başlatır, genellikle bu API ve API'nizi arayanlar kesintiye değil aynı zamanda değişiklik yaparken dikkatli gerekir. Yaptığınız değişiklikler hakkında bilmeniz geliştiriciler izin vermek yararlıdır. Azure API Management kullanarak bunu yapabilirsiniz **düzeltmelerini**. Daha fazla bilgi için bkz: [sürümleri & düzeltmelerini](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) ve [Azure API Management API sürüm oluşturma](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir düzeltme eklenmesini
> * Bölünemez değişiklik yapmak için bir düzeltme
> * Düzeltme geçerli hale ve değişiklik günlüğü girişi Ekle
> * Değişiklikleri görmek ve değişiklik günlüğü için geliştirici portalına göz atın

![Geliştirici Portalı üzerindeki değişiklik günlüğü](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Ön koşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-revision"></a>Yeni bir düzeltme eklenmesini

1. Seçin **API'leri** sayfası.
2. Seçin **konferans API** API listesinden (veya düzeltmeleri eklemek istediğiniz diğer API).
3. Tıklatın **düzeltmelerini** sayfanın üstüne yakın menüsünden sekmesi.
4. Seçin **+ düzeltme ekler**

    > [!TIP]
    > Ayrıca seçebilirsiniz **ekleme düzeltme** bağlam menüsünden (**...** ) API.
    
    ![Ekranın üstünde yakın düzeltmelerini menüsü](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Hangi onu için kullanılacağını unutmayın yardımcı olmak için yeni düzeltme için bir açıklama sağlayın.
6. Seçin **oluşturma**
7. Artık, yeni bir düzeltme oluşturulur.

    > [!NOTE]
    > Özgün API'nizi kaldığı **düzeltme 1**. Farklı bir düzeltme geçerli hale seçene kadar çağrılacak kullanıcılarınızın devam düzeltme budur.

## <a name="make-non-breaking-changes-to-your-revision"></a>Bölünemez değişiklik yapmak için bir düzeltme

1. Seçin **konferans API** API listeden.
2. Seçin **tasarım** sekmesinde ekranın üstünde yakın.
3. Dikkat **düzeltme Seçici** (doğrudan Tasarım sekmesi), geçerli düzeltmeye gösterir **düzeltme 2**.

    > [!TIP]
    > Üzerinde çalışmak istediğiniz düzeltmelerini arasında geçiş yapmak için düzeltme seçiciyi kullanın.

4. Seçin **+ ekleme işlemi**.
5. Yeni işleminizi olması için ayarlama **POST**, ad & işlem olarak görünen adını ve **test etme**
6. **Kaydet** yeni işlemi.
7. Biz şimdi bir değişiklik yapmış olduğunuz **düzeltme 2**. Kullanım **düzeltme Seçici** dönmek için sayfanın üstüne yakın **düzeltme 1**.
8. Yeni işleminizi görünmez bildirimi **düzeltme 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Düzeltme geçerli hale ve değişiklik günlüğü girişi Ekle
1. Seçin **düzeltmelerini** sayfanın üstüne yakın menüsünden sekmesi.

    ![Düzeltme ekran düzeltme menüsünde.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Bağlam menüsünü açın (**...** ) için **düzeltme 2**.
2. Seçin **geçerli hale**. Denetleme **bu API için ortak değişiklik günlüğüne sonrası**, bu değişiklik hakkında notlar sonrası istiyorsanız.
3. Seçin **bu API için ortak değişiklik günlüğüne Post**
4. Geliştiriciler, örneğin görürsünüz, değişiklik için bir açıklama sağlayın **düzeltmelerini test etme. Eklenen yeni işlem "test".**
5. **Düzeltme 2** artık geçerli değil.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Değişiklikleri görmek ve değişiklik günlüğü için geliştirici portalına göz atın
1. Azure portalında seçin **API'leri**
2. Seçin **Geliştirici Portalı** üstteki menüden.
3. Seçin **API'leri**ve ardından **konferans API**.
4. Yeni fark **test** işlemi artık kullanılabilir durumdadır.
5. Seçin **API değişiklik geçmişini** aşağıdaki API adı.
6. Değişiklik günlüğü girişi bu listede görüntülendiğine dikkat edin.

    ![Geliştirici portalı](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yeni bir düzeltme eklenmesini
> * Bölünemez değişiklik yapmak için bir düzeltme
> * Düzeltme geçerli hale ve değişiklik günlüğü girişi Ekle
> * Değişiklikleri görmek ve değişiklik günlüğü için geliştirici portalına göz atın

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [API'nizi birden fazla sürümünü yayımlama](api-management-get-started-publish-versions.md)