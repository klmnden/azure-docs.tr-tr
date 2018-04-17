---
title: Azure Media Services API erişmek için bir Azure AD uygulaması oluşturmak için PowerShell kullanın | Microsoft Docs
description: Bir Azure Active Directory (Azure AD) uygulama oluşturmak ve Azure Media Services API erişim kadar ayarlamak için PowerShell kullanmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 575e8a050a344cf3abb8adcda40b1f66fd9dcf59
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>Azure Media Services API ile kullanmak için bir Azure AD uygulaması oluşturmak için PowerShell kullanın

Azure Media Services kaynaklara erişmek için bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu oluşturmak için bir PowerShell komut dosyası kullanmayı öğrenin.  

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Bir hesabınız yoksa, başlayan bir [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için bkz: [Azure portalda bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Azure PowerShell sürümü 0.8.8 veya sonraki bir sürümü. Daha fazla bilgi için bkz: [Azure PowerShell'in nasıl kullanılacağı](https://docs.microsoft.com/powershell/azure/overview).
- Azure Resource Manager cmdlet'lerini.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>PowerShell kullanarak bir Azure AD uygulaması oluştur  

```powershell
Login-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [Rol tabanlı erişim denetimini Azure PowerShell kullanarak yönetme](../role-based-access-control/role-assignments-powershell.md)
- [Arka plan programı uygulamaları sertifikaları kullanarak el ile yapılandırma](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-portal-upload-files.md).
