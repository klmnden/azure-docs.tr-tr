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
ms.date: 02/22/2017
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: d35996b16d615a198b9a6039386f6b295172f388
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60615731"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Kurumsal Maliyet görünümleri sorunlarını giderme

Kurumsal kayıtları içinde maliyetleri görmek, kullanıcılar kayıt içinde neden olabilecek birkaç ayar vardır.  Bu ayarlar kayıt Yöneticisi tarafından yönetilir. Ya da kayıt doğrudan Microsoft aldıysanız, bu değil ayarları iş ortağı tarafından yönetiliyor.  Bu makalede ayarlar nelerdir ve bunların kaydı nasıl etkilediği anlamanıza yardımcı olur. Bu ayarlar, Azure rol tabanlı erişim denetimi (RBAC) rollerini bağımsızdır.

## <a name="enabling-access-to-costs"></a>Maliyetleri erişimini etkinleştirme

Misiniz, yetkisiz bir iletiyi görmekten veya *"Maliyet görünümleri devre dışı bırakıldı'na kaydınızı."* Maliyet bilgileri ararken?
![Abonelik için geçerli Maliyet alanında "yetkisiz" gösteren ekran görüntüsü.](media/billing-enterprise-mgmt-groups/unauthorized.png)

Aşağıdakilerden biri olabilir:

1. Azure bir kurumsal iş ortağı aracılığıyla satın aldığım ve iş ortağı henüz bir fiyatlandırmaya sürüm kaydetmedi. Fiyatlandırma içinde ayarını güncelleştirmek için iş ortağınızla iletişime [kurumsal portal](https://ea.azure.com).
2. Bir doğrudan EA müşterisiyseniz, birkaç olasılık vardır:
    * Bir hesap sahibi ve devre dışı kayıt yöneticinize işiniz **AO ücretleri görüntüle** ayarı.  
    * Bir departman Yöneticisi ve devre dışı kayıt yöneticinize işiniz **DA ücretleri görüntüle** ayarı.
    * Kayıt erişim almak için yöneticinize başvurun. Kayıt yöneticisi ayarlarını güncelleştirebilirsiniz [kurumsal portal](https://ea.azure.com/manage/enrollment).

      ![Ücretleri görüntüle Kurumsal Portal ayarları gösteren ekran görüntüsü.](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>Varlık kullanılamıyor

"Bu varlık kullanılamıyor" hata iletisi alırsanız, ne zaman bir abonelik veya yönetim grubu, ardından erişmeye değilsiniz bu öğeyi görüntülemek için doğru rol.  

!["Varlık kullanılamıyor" iletisini gösteren ekran görüntüsü.](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Azure, abonelik veya yönetim grubu yöneticisi erişim isteğinde bulunun. Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).
