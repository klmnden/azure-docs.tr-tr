---
title: Hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntüleme
description: Hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntülemeye ilişkin adım adım yönergeler için.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: daveba
ms.openlocfilehash: 0cf1915e4013451dbb09f2c4af3df2bad6166475
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438958"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntüleyin

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, hizmet sorumlusu, PowerShell kullanarak bir yönetilen kimlik görüntüleme hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md).
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/).
- Etkinleştirme [bir sanal makinede sistem tarafından atanan kimlik](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulama](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).
- PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). 
- PowerShell'i yerel ortamda çalıştırıyorsanız şunları da yapmanız gerekir: 
    - Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` komutunu çalıştırın.
    - [PowerShellGet'in en son sürümünü](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget) yükleyin.
    - `Install-Module -Name PowerShellGet -AllowPrerelease` komutunu çalıştırarak `PowerShellGet` modülünün yayın öncesi sürümünü alın (`AzureRM.ManagedServiceIdentity` modülünü yüklemek için bu komutu çalıştırdıktan sonra geçerli PowerShell oturumundan `Exit` ile çıkmanız gerekebilir).
    - Çalıştırma `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` bir ön sürümünü yüklemek için `AzureRM.ManagedServiceIdentity` modülü kullanıcı tarafından atanan gerçekleştirmek için bu makaledeki kimlik işlemleri yönetilen.

## <a name="view-the-service-principal"></a>Hizmet sorumlusu görüntüleyin

Bu aşağıdaki komutu etkin sistem tarafından atanan kimlikle bir VM veya uygulama hizmet sorumlusu görüntüleme işlemini göstermektedir. Değiştirin `<VM or application name>` kendi değerlerinizle.

```PowerShell
Get-AzureRmADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

PowerShell kullanarak Azure AD hizmet sorumlusu görüntüleme ile ilgili daha fazla bilgi için bkz: [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal).


