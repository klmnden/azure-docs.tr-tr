---
title: "Görünümleri - Azure maliyeti Kurumsal sorunlarını giderme | Microsoft Docs"
description: "Azure portalındaki kuruluş maliyet görünümlerle sahip olabileceğiniz sorunları çözmeyi öğrenin."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: eca1ac9ed51e6c2243be451a074792fbec2840d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-enterprise-cost-views"></a>Görünümleri maliyet Kurumsal sorun giderme 

Kurumsal kayıtları içinde maliyetleri görüntülemek üzere olmayan kullanıcıların kayıt içinde neden olabilecek birden çok ayarları vardır.  Kayıt doğrudan Microsoft'a satın değil, bu ayarlar kayıt yönetici tarafından veya iş ortağı tarafından yönetilir.  Bu makalede, ayarları nelerdir ve bunlar kayıt nasıl etkiler anlamanıza yardımcı olur. Bu ayarları bağımsız olarak [Azure RBAC rolleri](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure). 

> [!Note]
> Bu özellik şu anda bir özel önizlemede değil. [Burada oturum](https://forms.office.com/Pages/DesignPage.aspx#FormId=v4j5cvGGr0GRqy180BHbR0YtfU6ham9OsGsPPYdu2xdUNk1BQUwzTkUyOVc5NUpCTFcwR0pIOVFETS4u) kaydınızı Önizleme katılmak için.     

## <a name="enabling-access-to-costs"></a>Maliyetleri erişimini etkinleştirme

Bir ileti yetkisiz, gördüğünüz olan veya *"Maliyet görünümleri devre dışı bırakılır ilişkin kaydınızı."* Maliyet bilgileri bakarken? ![yetkisiz](media/billing-enterprise-mgmt-groups/unauthorized.png)

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
* Abonelikler için başvuru [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) belge rolü gerekli Yardım için.
* Yönetim grupları için RBAC erişim kullanılabilir değil ve yakında çıkıyor. Enterprise portal'ınızın yönetme erişimi atanan kişi.   
