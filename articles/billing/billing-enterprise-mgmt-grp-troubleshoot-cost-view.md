---
title: Görünümleri - Azure maliyeti Kurumsal sorunlarını giderme | Microsoft Docs
description: Azure portalındaki kuruluş maliyet görünümlerle sahip olabileceğiniz sorunları çözmeyi öğrenin.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2017
ms.author: rithorn
ms.openlocfilehash: 193e4691d2e10dd1c5ad16c26ad25534e3156745
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-enterprise-cost-views"></a>Görünümleri maliyet Kurumsal sorun giderme 

Kurumsal kayıtları içinde maliyetleri görüntülemek üzere olmayan kullanıcıların kayıt içinde neden olabilecek birden çok ayarları vardır.  Kayıt doğrudan Microsoft'a satın değil, bu ayarlar kayıt yönetici tarafından veya iş ortağı tarafından yönetilir.  Bu makalede, ayarları nelerdir ve bunlar kayıt nasıl etkiler anlamanıza yardımcı olur. Bu ayarları bağımsız olarak [Azure RBAC rolleri](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). 


## <a name="enabling-access-to-costs"></a>Maliyetleri erişimini etkinleştirme

Misiniz bir ileti yetkisiz, görme veya *"Maliyet görünümleri devre dışı bırakılır ilişkin kaydınızı."* Maliyet bilgileri bakarken? ![yetkisiz](media/billing-enterprise-mgmt-groups/unauthorized.png)

Aşağıdaki nedenlerden biri olabilir:

1. Bir kurumsal iş ortağı üzerinden Azure satın aldığınız ve iş ortağı henüz fiyatlandırma yayımlanan kurmadı. Fiyatlandırma serbest bırakmak için ayarı içinde güncelleştirmek için iş ortağınıza başvurun [Enterprise portal](https://ea.azure.com).
2. Alternatif olarak, EA doğrudan bir müşteri değilseniz, birkaç olanakları vardır:
    * Bir hesap sahibi olduğunuz ve kayıt yöneticiniz "ayarı AO görünüm ücretleri" devre dışı bıraktı.  
    * Bir bölüm yöneticisiyseniz ve kayıt yöneticiniz "ayarını DA görünüm ücretleri" devre dışı bıraktı.
    * Kayıt erişim almak için yöneticinize başvurun. Kayıt yönetim ziyaret edebilirsiniz [Enterprise portal](https://ea.azure.com/manage/enrollment) ve burada görüldüğü gibi ayarını güncelleştirin:

![Enterprise Portal ayarları](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Varlık kullanılamıyor? 
"Bu varlık kullanılamıyor" hata iletisi alıyorsanız, bir abonelik veya yönetim grubu, sonra erişmeye sahip olmadığında bu öğeyi görüntülemek için doğru rol.  

![varlık olmayan bulundu](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Erişim verilecek abonelik veya yönetim gruplarının Yönet başvurun.  
* Abonelikler için başvuru [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) belge rolü gerekli Yardım için.
