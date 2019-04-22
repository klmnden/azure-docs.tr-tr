---
title: Belirteç süresini değiştirme için özel olarak geliştirilmiş bir uygulamada varsayılan olarak | Microsoft Docs
description: Azure AD'de geliştirmekte olduğunuz uygulama için belirteç ömrü ilkeleri güncelleştirme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: celested
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04abdedf5ac19be3d5a43e7502cbc97f8f5fee43
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59360300"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada için belirteç ömrü varsayılanlarını değiştirme

Bu makalede, bir belirteç ömrü ilkesi ayarlamak için Azure AD PowerShell kullanmayı gösterir. Azure AD Premium, uygulama geliştiricileri ve Kiracı yöneticileri gizli olmayan istemciler için verilen belirteçlere ömrünü yapılandırmak için sağlar. Belirteç ömrü ilkeleri, Kiracı genelinde bir temele veya erişilen kaynaklara ayarlanır.

1. Bir belirteç ömrü ilkesi ayarlamak için indirmeniz gerekir [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureADPreview).
1. Çalıştırma **Connect-AzureAD-onaylayın** komutu.

    En yüksek yaş tek Faktörlü yenileme belirtecini ayarlayan bir örnek ilke aşağıda verilmiştir. İlke Oluştur: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Azure AD'de yapılandırılabilir belirteç ömrünü](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) kuruluşunuzdaki tüm uygulamalar, çok kiracılı uygulama veya belirli bir hizmet için belirteç ömrünü ayarlama dahil olmak üzere Azure AD tarafından verilen belirteç ömrünü yapılandırma hakkında bilgi edinmek için Kuruluşunuzdaki sorumlu. 
* [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

