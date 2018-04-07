---
title: Azure kaynakları - Azure kaynakları için tam erişim gözden geçirme için Privileged Identity Management | Microsoft Docs
description: Azure kaynakları için erişim gözden geçirme tamamlanması açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5ce02c2d27ec3de87fe44e9c904f19409600e5c5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---finish-access-review"></a>Privileged Identity Management - Kaynak rolü - son erişim gözden geçirme
Ayrıcalıklı rol Yöneticiler gözden geçirebileceğiniz ayrıcalıklı erişim kez bir [güvenlik incelemesi başlatıldıysa](pim-resource-roles-start-access-review.md). Azure kaynakları için ayrıcalıklı Kimlik Yönetimi (PIM) otomatik olarak erişimleri gözden geçirmek için kullanıcıların isteyen bir e-posta gönderir. Bir kullanıcı bir e-posta almadığını varsa, bunları yönergeleri gönderebilirsiniz [güvenlik incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

Güvenlik değerlendirme süresi sona erer veya tüm kullanıcılar kendi kendini gözden tamamladıktan sonra gözden geçirme yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-security-reviews"></a>Güvenlik değerlendirmeleri yönetme
1. Git [Azure portal](https://portal.azure.com/) seçip **Azure kaynaklarını** Panonuzda uygulama.
2. Kaynağınızı seçin.
3. Seçin **erişim incelemeler** bölümü.
![](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)
4. Yönetmek istediğiniz erişim gözden geçirme seçin.

Erişim gözden geçirme 's ayrıntı dikey penceresinde bu gözden geçirme yönetmek için birkaç seçenek vardır.
![](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur
Bitiş tarihi tüm erişim değerlendirmeleri sahip, ancak kullanabileceğiniz **durdurmak** erken bitirme düğmesi. Tüm kullanıcılar bu zamana kadar gözden yapmadıysanız, gözden geçirme durdurduktan sonra için değiştiremezler. Durdurulmuş sonra bir gözden geçirme yeniden başlatılamıyor.

### <a name="reset"></a>Sıfırla
Üzerinde yapılan tüm kararlar kaldırmak için bir erişim gözden geçirme sıfırlayabilirsiniz. Bir erişim gözden geçirme sıfırladınız sonra tüm kullanıcılar olarak işaretlenmiş yeniden gözden geçirilmeyen. 

### <a name="apply"></a>Uygula
Bir erişim gözden geçirme tamamlandıktan sonra da bitiş tarihi ulaşıldı veya el ile durduruldu çünkü **Uygula** düğmesi gözden sonucunu uygular. Bir kullanıcının erişimi incelemede reddedildiyse, kendi rol atamasını kaldıracak adım budur.  

### <a name="delete"></a>Sil
Başka incelemede değil ilgileniyorsanız, dosyayı silin. **Silmek** düğmesini gözden geçirme PIM uygulamasını kaldırır.

## <a name="results"></a>Sonuçlar
Görüntüleyin ve gözden geçirme sonuçlarınızı sonuçları sekmesinde bir listesini indirir. ![](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden geçirenler
Görüntüleyin ve var olan erişim incelemeniz gözden geçirenleri ekleme. Gözden geçirenler geçirmeyi tamamlamak için şu aralıklarla uyar.
![](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



