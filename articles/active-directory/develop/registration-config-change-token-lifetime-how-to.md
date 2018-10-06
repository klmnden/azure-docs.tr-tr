---
title: Belirteç süresini değiştirme için özel olarak geliştirilmiş bir uygulamada varsayılan olarak | Microsoft Docs
description: Azure AD'de geliştirmekte olduğunuz uygulama için belirteç ömrü ilkeleri güncelleştirme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 4a730c340ea4d1e1137a7449c6d1005ea59917bf
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814017"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada için belirteç ömrü varsayılanlarını değiştirme

Azure AD Premium, uygulama geliştiricileri ve Kiracı yöneticileri gizli olmayan istemciler için verilen belirteçlere ömrünü yapılandırmak için sağlar. Belirteç ömrü ilkeleri, Kiracı genelinde bir temele veya erişilen kaynaklara ayarlanır.

1. Bir belirteç ömrü ilkesi ayarlamak için indirmeniz gerekir [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureADPreview).
1. Çalıştırma **Connect-AzureAD-onaylayın** komutu.

    En yüksek yaş tek Faktörlü yenileme belirtecini ayarlayan bir örnek ilke aşağıda verilmiştir. İlke Oluştur: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Azure AD'de yapılandırılabilir belirteç ömrünü](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) kuruluşunuzdaki tüm uygulamalar, çok kiracılı uygulama veya belirli bir hizmet için belirteç ömrünü ayarlama dahil olmak üzere Azure AD tarafından verilen belirteç ömrünü yapılandırma hakkında bilgi edinmek için Kuruluşunuzdaki sorumlu. 
* [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

