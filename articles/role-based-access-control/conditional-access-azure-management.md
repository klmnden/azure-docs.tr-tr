---
title: Azure yönetim erişimi Azure Active Directory'de koşullu erişim ile yönetme
description: Azure yönetim erişimi yönetmek için koşullu erişim Azure AD'de kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2019
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 4ee58835a5055f5382cefa3a049424ffe97a01c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122955"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Koşullu erişim ile Azure yönetim erişimi yönetme

Azure Active Directory'de (Azure AD) koşullu erişim, bulut uygulamaları, belirttiğiniz belirli koşullara göre erişimi denetler. Erişime izin vermek için izin verme veya engelleme olup olmadığını ilkesinde gereksinimlerin karşılanıp karşılanmadığından dayalı olarak erişimi koşullu erişim ilkeleri oluşturun. 

Genellikle, bulut uygulamalarınıza erişimi denetlemek için koşullu erişim kullanın. İlkeleri, Azure Yönetim (örneğin, oturum açma riski, konum veya cihaz) belirli koşullara göre erişimi denetlemek için ve çok faktörlü kimlik doğrulaması gibi gereksinimleri uygulamak için de ayarlayabilirsiniz.

Azure yönetimi için bir ilke oluşturmak için seçtiğiniz **Microsoft Azure Management** altında **bulut uygulamaları** ilkenin uygulanacağı uygulamaya seçerken.

![Azure yönetimi için koşullu erişim](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Azure portalı, Azure Resource Manager sağlayıcısı, Klasik Hizmet Yönetim API'leri, Azure PowerShell ve Visual Studio abonelikleri Yönetici portalında dahil olmak üzere tüm Azure yönetim uç noktalar için oluşturduğunuz ilke uygulanır. İlke, Azure Resource Manager API çağrılarının Azure PowerShell için geçerli olduğunu unutmayın. Geçerli olmayan [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), Microsoft Graph çağırır.

> [!CAUTION]
> Azure yönetim erişimi yönetmek için bir ilke ayarlamadan önce koşullu erişim nasıl çalışır anladığınızdan emin olun. Kendi portal erişimini engelleyebilecek koşullar oluşturmayın emin olun.

Ayarlanmış ve koşullu erişim kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory'de koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md).