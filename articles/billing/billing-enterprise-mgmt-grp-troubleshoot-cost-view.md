---
title: Azure Kurumsal Maliyet görünümleri sorunlarını giderme | Microsoft Docs
description: Azure portalında Kurumsal Maliyet görünümlerle sahip olabileceğiniz tüm sorunları çözmeyi öğrenin.
author: rthorn17
manager: adpick
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2017
ms.author: cwatson
ms.openlocfilehash: be74714667c2fed57a797a972ddcd7949cae5933
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274679"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Kurumsal Maliyet görünümleri sorunlarını giderme 

Kurumsal kayıtları içinde maliyetleri görüntülemek üzere olmayan kullanıcıların kayıt içinde neden olabilecek birden çok ayar vardır.  Kayıt doğrudan Microsoft'tan satın aldıysanız değil, bu ayarlar, kayıt Yöneticisi tarafından veya iş ortağı tarafından yönetilir.  Bu makalede ayarlar nelerdir ve bunların kaydı nasıl etkilediği anlamanıza yardımcı olur. Bu ayarlar bağımsızdır [Azure RBAC rolleri](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). 


## <a name="enabling-access-to-costs"></a>Maliyetleri erişimini etkinleştirme

Misiniz, yetkisiz bir iletiyi görmekten veya *"Maliyet görünümleri devre dışı bırakıldı'na kaydınızı."* Maliyet bilgileri ararken? ![yetkisiz](media/billing-enterprise-mgmt-groups/unauthorized.png)

Bunun nedeni aşağıdakilerden biri olabilir:

1. Azure bir kurumsal iş ortağı aracılığıyla satın aldığınız ve iş ortağı henüz bir fiyatlandırmaya serbest edilmemiş. Fiyatlandırma yayımlamayı içinde ayarını güncelleştirmek için ortağınızla iletişim kurun [kurumsal portal](https://ea.azure.com).
2. Alternatif olarak, bir EA Direct müşterisiyseniz, birkaç olasılık vardır:
    * Bir hesap sahibi olduğunuz ve kayıt yöneticiniz "ayarını AO ücretleri görüntüle" devre dışı bıraktı.  
    * Bir bölüm yöneticisiyseniz ve kayıt yöneticiniz "ayarını DA ücretleri görüntüle" devre dışı bıraktı.
    * Kayıt erişim almak için yöneticinize başvurun. Kayıt Yöneticisi ziyaret edebilirsiniz [kurumsal portal](https://ea.azure.com/manage/enrollment) ve burada görüldüğü gibi ayarını güncelleştirin:

![Kurumsal Portal ayarları](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Varlık kullanılamıyor mu? 
"Bu varlık kullanılamıyor" hata iletisi alıyorsanız, ne zaman bir abonelik veya yönetim grubu, ardından erişmeye gerekmez bu öğeyi görüntülemek için doğru rol.  

![Varlık bulunamadı](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Yönetici erişim verilmesi için abonelik veya yönetim gruplarının başvurun.  
* Abonelikler için başvuru [Azure rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) belge Yardım rolü gereklidir.
