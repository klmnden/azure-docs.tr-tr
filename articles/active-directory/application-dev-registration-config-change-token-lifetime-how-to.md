---
title: Belirteç süresini değiştirme için özel olarak geliştirilmiş bir uygulamada varsayılan olarak | Microsoft Docs
description: Azure AD'de geliştirmekte olduğunuz uygulama için belirteç ömrü ilkeleri güncelleştirme
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0ecb1f55309901abd2c623d2c3d23ef717fb176b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365111"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada için belirteç ömrü varsayılanlarını değiştirme

Azure AD Premium, uygulama geliştiricileri ve Kiracı yöneticileri gizli olmayan istemciler için verilen belirteçlere ömrünü yapılandırmak için sağlar. Belirteç ömrü ilkeleri, Kiracı genelinde bir temele veya erişilen kaynaklara ayarlanır.

 * Bir belirteç ömrü ilkesi ayarlamak için indirmeniz gerekir [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureADPreview).

 * Çalıştırma **Connect-AzureAD-onaylayın** komutu.

 * En yüksek yaş tek Faktörlü yenileme belirtecini ayarlayan bir örnek ilke aşağıda verilmiştir. İlke Oluştur: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Kullanıma alma [belirteç ömrü yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) belgenin diğer özel oluşturmayı öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
[Belirteç ömrü yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

