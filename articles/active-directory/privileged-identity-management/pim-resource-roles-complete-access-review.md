---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için erişim gözden geçirmesi tamamlama | Microsoft Docs
description: Azure kaynakları için erişim gözden geçirmesi tamamlama açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e21d0240469a9c775e610c97f98c073b8f83ce8e
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442214"
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için erişim değerlendirmesi tamamlama
Ayrıcalıklı rol yöneticileri, ayrıcalıklı erişim sonra gözden geçirebileceğiniz bir [güvenlik incelemesi çalışmaya](pim-resource-roles-start-access-review.md). Azure kaynakları için Privileged Identity Management (PIM) otomatik olarak kullanıcıların erişimini gözden geçirmek için kullanıcıların ister bir e-posta gönderir. Bir kullanıcı bir e-posta almazsa, bunları yönergeleri gönderebilirsiniz [güvenlik incelemesi gerçekleştirme](pim-resource-roles-perform-access-review.md).

Güvenlik incelemesi süresi bittikten sonra veya tüm kullanıcılar, kendi kendini gözden bitirdikten sonra gözden geçirme yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-security-reviews"></a>Güvenlik incelemeleri yönetme
1. [Azure Portal](https://portal.azure.com/) gidin. Panoda, ardından **Azure kaynaklarını** uygulama.

2. Kaynağınızı seçin.

3. Seçin **erişim gözden geçirmeleriyle** Pano bölümü.
![Erişim gözden geçirmeleri](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Yönetmek istediğiniz erişim gözden geçirmesi seçin.

Erişim gözden geçirmesi ayrıntıları dikey penceresinde, gözden geçirme yönetmek için birkaç seçenek vardır. Seçenekleri aşağıdaki gibidir:

![Bir gözden geçirme yönetimi seçenekleri](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Durdur
Tüm erişim gözden geçirmeleri bir bitiş tarihi vardır, ancak kullanabileceğiniz **Durdur** düğmesini erken tamamlayın. Bu süreye göre gözden geçirmelerini tamamlamadınız tüm kullanıcılar gözden durdurduktan sonra bitirmek mümkün olmayacaktır. Durdurulmuş sonra bir gözden geçirme yeniden başlatılamıyor.

### <a name="reset"></a>Sıfırla
Üzerinde yapılan tüm kararları kaldırmak için erişim gözden geçirmesi sıfırlayabilirsiniz. Erişim gözden geçirmesi sıfırladık sonra tüm kullanıcılar olarak işaretlenmiş yeniden gözden geçirilmeyen. 

### <a name="apply"></a>Uygula
Erişim gözden geçirmesi tamamlandığında, kullanın **Uygula** gözden geçirme sonucunu uygulamak için düğme. İncelemede kullanıcı erişimi reddedildiyse, bu adım, rol ataması kaldırır.  

### <a name="delete"></a>Sil
İncelemede daha ilgileniyor olmayan değilse silebilirsiniz. **Sil** düğmesini gözden PIM uygulamadan kaldırır.

## <a name="results"></a>Sonuçlar
Üzerinde **sonuçları** sekmesinde görüntüleyin ve sonuçlarını gözden geçirme listesini indirin. 
![Sonuçları sekmesi](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Gözden Geçirenler
Görüntüleyebilir ve mevcut erişim gözden geçirmeniz için gözden geçirenleri ekleyin. Geçirmeyi tamamlamak için gözden geçirenler hatırlatın.
![Gözden geçirenler ekleme](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



