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
ms.date: 09/11/2018
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c0700ff40063bfb7709b583f849eed179648306
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203582"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada için belirteç ömrü varsayılanlarını değiştirme

Azure AD Premium, uygulama geliştiricileri ve Kiracı yöneticileri gizli olmayan istemciler için verilen belirteçlere ömrünü yapılandırmak için sağlar. Belirteç ömrü ilkeleri, Kiracı genelinde bir temele veya erişilen kaynaklara ayarlanır.

1. Bir belirteç ömrü ilkesi ayarlamak için indirmeniz gerekir [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureADPreview).
1. Çalıştırma **Connect-AzureAD-onaylayın** komutu.

    En yüksek yaş tek Faktörlü yenileme belirtecini ayarlayan bir örnek ilke aşağıda verilmiştir. İlke Oluştur: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Azure AD'de yapılandırılabilir belirteç ömrünü](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) kuruluşunuzdaki tüm uygulamalar, çok kiracılı uygulama veya belirli bir hizmet için belirteç ömrünü ayarlama dahil olmak üzere Azure AD tarafından verilen belirteç ömrünü yapılandırma hakkında bilgi edinmek için Kuruluşunuzdaki sorumlu. 
* [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

