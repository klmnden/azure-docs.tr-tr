---
title: "Belirteç ömrü değiştirmek nasıl özel geliştirilmiş bir uygulama için varsayılan olarak | Microsoft Docs"
description: "Belirteç ömrü ilkelerini üzerinde Azure AD geliştirdiğiniz uygulamanız için güncelleştirme"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8067ecf3e274f65abe2c82f20dd2f4469344f3b6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel geliştirilmiş bir uygulama için belirteç ömrü varsayılanları değiştirme

Uygulama geliştiriciler ve Kiracı yöneticileri gizli olmayan istemciler için yayınlanan belirteçleri ömrü yapılandırmak için Azure AD Premium sağlar. Belirteç ömrü ilkeleri Kiracı genelinde veya erişilen kaynaklar üzerinde ayarlanır.

 * Belirteç ömrü ilkesi ayarlamak için indirme gerekir [Azure AD PowerShell Modülü](https://www.powershellgallery.com/packages/AzureADPreview).

 * Çalıştırma **Connect-Azuread'i-Onayla** komutu.

 * Maksimum yaş tek Faktörlü yenileme belirteci ayarlar bir örnek İlkesi aşağıdadır. İlke Oluştur:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Checkout [yapılandırma belirteç ömrü](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) belge diğer özel oluşturmayı öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
[Belirteç ömrü yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

