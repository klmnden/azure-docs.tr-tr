---
title: Uygulama kimlik doğrulama - Azure SQL veritabanı değerlerini alma | Microsoft Docs
description: Koddan SQL veritabanına erişmek için bir hizmet sorumlusu oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: f9b66a430e1ca5b24853a5ff31ce13393b12b0c5
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063804"
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Bir uygulama kodundan SQL veritabanına erişmek için kimlik doğrulaması için gerekli değerleri alma
Oluşturma ve SQL veritabanı koddan yönetmek için Azure kaynaklarınızın nerede oluşturdunuz Azure Active Directory (AAD) etki alanında aboneliği uygulamanızı kaydetmeniz gerekir.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Bir hizmet sorumlusu kaynaklarına erişmek için dosyasından bir uygulama oluşturma
En son ihtiyacınız [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) yüklü ve çalışır. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

Aşağıdaki PowerShell betiği Active Directory (AD) uygulamasını ve C# uygulamamızda kimlik doğrulamak için gereken hizmet sorumlusunu oluşturur. Betik önceki C# örneği için gereken değerleri çıkarır. Ayrıntılı bilgi için bkz. [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Connect-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Ayrıca bkz.
* [C# ile bir SQL veritabanı oluşturma](sql-database-get-started-csharp.md)
* [Azure Active Directory kimlik doğrulamasını kullanarak SQL veritabanına bağlanma](sql-database-aad-authentication.md)

