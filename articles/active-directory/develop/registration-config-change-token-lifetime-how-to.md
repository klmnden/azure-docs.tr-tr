---
title: Belirteç süresini değiştirme için özel olarak geliştirilmiş bir uygulamada varsayılan olarak | Microsoft Docs
description: Azure AD'de geliştirmekte olduğunuz uygulama için belirteç ömrü ilkeleri güncelleştirme
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: ryanwi
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: c3d11d282a2405d37614bfac41dd3f7ad49353d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545520"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada için belirteç ömrü varsayılanlarını değiştirme

Bu makalede, bir belirteç ömrü ilkesi ayarlamak için Azure AD PowerShell kullanmayı gösterir. Azure AD Premium, uygulama geliştiricileri ve Kiracı yöneticileri gizli olmayan istemciler için verilen belirteçlere ömrünü yapılandırmak için sağlar. Belirteç ömrü ilkeleri, Kiracı genelinde bir temele veya erişilen kaynaklara ayarlanır.

1. Bir belirteç ömrü ilkesi ayarlamak için indirmeniz gerekir [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureADPreview).
1. Çalıştırma **Connect-AzureAD-onaylayın** komutu.

    En yüksek yaş tek Faktörlü yenileme belirtecini ayarlayan bir örnek ilke aşağıda verilmiştir. İlke Oluştur: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Azure AD'de yapılandırılabilir belirteç ömrünü](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) kuruluşunuzdaki tüm uygulamalar, çok kiracılı uygulama veya belirli bir hizmet için belirteç ömrünü ayarlama dahil olmak üzere Azure AD tarafından verilen belirteç ömrünü yapılandırma hakkında bilgi edinmek için Kuruluşunuzdaki sorumlu. 
* [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

