---
title: Azure Media Services API'sine erişmek için bir Azure AD uygulaması oluşturmak için PowerShell kullanma | Microsoft Docs
description: Azure Active Directory (Azure AD) uygulama oluşturma ve Azure Media Services API'sine erişim kadar ayarlamak için PowerShell kullanmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: bec7fdbbbedb44cb4e74206c05ecec18400a4dbb
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55989023"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>Azure Media Services API'sine ile kullanmak için bir Azure AD uygulaması oluşturmak için PowerShell kullanma

Bir PowerShell Betiği, Azure Media Services kaynaklarına erişmek için bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu oluşturmak için kullanmayı öğrenin.  

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Bir hesabınız yoksa, başlayan bir [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için [Azure portalında bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Azure PowerShell sürümü 0.8.8 veya sonraki bir sürümü. Daha fazla bilgi için [Azure PowerShell'in nasıl kullanılacağına](https://docs.microsoft.com/powershell/azure/overview).
- Azure Resource Manager cmdlet'leri.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>PowerShell kullanarak bir Azure AD uygulamanızı oluşturma  

```powershell
Connect-AzureRmAccount
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

- [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Rol tabanlı erişim denetimini Azure PowerShell kullanarak yönetme](../../role-based-access-control/role-assignments-powershell.md)
- [Sertifikaları kullanarak arka plan programları el ile yapılandırma](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-portal-upload-files.md).
