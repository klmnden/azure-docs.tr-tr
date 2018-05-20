---
title: Azure yönetim Azure Active Directory'de koşullu erişim ile yönetme
description: Azure yönetim erişimi yönetmek için koşullu erişim Azure AD'de kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: dcf701c15ee20b7d9aeea65f3d06b794006a5202
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Azure Yönetim için koşullu erişim ile yönetme

Koşullu erişim Azure Active Directory'de (Azure AD) uygulamaları, belirttiğiniz belirli koşullara göre bulut erişimi denetler. Erişime izin vermek için izin verme veya engelleme İlkesi gereksinimleri desteklemediğini karşılandığından göre erişimi koşullu erişim ilkeleri oluşturun. 

Genellikle, bulut uygulamalarınıza erişimi denetlemek için koşullu erişim kullanın. İlkeleri, belirli koşullar (örneğin, oturum açma risk, konum veya aygıt) göre Azure yönetim erişimi denetlemek için ve çok faktörlü kimlik doğrulaması gibi gereksinimleri zorlamak için de ayarlayabilirsiniz.

Azure Yönetim için bir ilke oluşturmak için seçtiğiniz **Microsoft Azure Management** altında **bulut uygulamaları** ilkenin uygulanacağı uygulama seçerken.

![Azure yönetimi için koşullu erişim](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Oluşturduğunuz ilke Klasik Azure portalı, Azure portal, Azure Resource Manager sağlayıcısı, Klasik dahil olmak üzere tüm Azure yönetim uç noktaları için hizmet yönetim API'leri ve Azure PowerShell uygulanır.

> [!CAUTION]
> Koşullu erişim anladığınızdan emin olmak için Azure yönetim erişimi yönetmek üzere bir ilke ayarlamadan önce çalışır. Portal kendi erişimini engelleme koşulları oluşturmayın emin olun.

Ayarlama ve koşullu erişim kullanma konusunda daha fazla bilgi için bkz: [Azure Active Directory'de koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md).