---
title: Hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntüleme
description: Hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntülemeye ilişkin adım adım yönergeler için.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: priyamo
ms.openlocfilehash: bb0462820cc94c06054ada12f0c764d4cfb190f7
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900162"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntüleyin

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntüleme hakkında bilgi edinin.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/).
- Etkinleştirme [bir sanal makinede sistem tarafından atanan kimlik](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulama](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).
- En son sürümünü yükleyin [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="view-the-service-principal"></a>Hizmet sorumlusu görüntüleyin

Bu aşağıdaki komutu etkin sistem tarafından atanan kimlikle bir VM veya uygulama hizmet sorumlusu görüntüleme işlemini göstermektedir. Değiştirin `<VM or application name>` kendi değerlerinizle.

```PowerShell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

PowerShell kullanarak Azure AD hizmet sorumlusu görüntüleme ile ilgili daha fazla bilgi için bkz: [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).


