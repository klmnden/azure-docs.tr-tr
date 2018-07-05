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
ms.date: 09/22/2017
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 083cb4eb84746f4a61b51f3573a0bf66110fe1ee
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435057"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Koşullu erişim ile Azure yönetim erişimi yönetme

Azure Active Directory'de (Azure AD) koşullu erişim, bulut uygulamaları, belirttiğiniz belirli koşullara göre erişimi denetler. Erişime izin vermek için izin verme veya engelleme olup olmadığını ilkesinde gereksinimlerin karşılanıp karşılanmadığından dayalı olarak erişimi koşullu erişim ilkeleri oluşturun. 

Genellikle, bulut uygulamalarınıza erişimi denetlemek için koşullu erişim kullanın. İlkeleri, Azure Yönetim (örneğin, oturum açma riski, konum veya cihaz) belirli koşullara göre erişimi denetlemek için ve çok faktörlü kimlik doğrulaması gibi gereksinimleri uygulamak için de ayarlayabilirsiniz.

Azure yönetimi için bir ilke oluşturmak için seçtiğiniz **Microsoft Azure Management** altında **bulut uygulamaları** ilkenin uygulanacağı uygulamaya seçerken.

![Azure yönetimi için koşullu erişim](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Oluşturduğunuz ilke, hizmet yönetim API'leri ve Azure PowerShell Klasik Azure portalı, Azure portalı, Azure Resource Manager sağlayıcısı, Klasik dahil olmak üzere tüm Azure yönetim uç uygulanır.

> [!CAUTION]
> Koşullu erişim anladığınızdan emin olun Azure yönetim erişimi yönetmek için bir ilke ayarlamadan önce çalışır. Kendi portal erişimini engelleyebilecek koşullar oluşturmayın emin olun.

Ayarlanmış ve koşullu erişim kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory'de koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md).