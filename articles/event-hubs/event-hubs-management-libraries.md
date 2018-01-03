---
title: "Azure Event Hubs yönetim kitaplıklarını | Microsoft Docs"
description: "Olay hub'ları ad alanları ve varlıkları .NET yönetme"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 2ae2f8f2006507284338fb4fa62e4942476cf2bc
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="event-hubs-management-libraries"></a>Olay hub'ları yönetim kitaplıkları

Olay hub'ları yönetim kitaplıklarını dinamik olarak Event Hubs ad alanları ve varlıkları sağlayabilirsiniz. Program aracılığıyla sağlamak için hangi varlıkları belirleyebilmesi bu dinamik yapısı karmaşık dağıtımlar ve mesajlaşma senaryoları sağlar. Bu kitaplıklar, .NET için şu anda kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevi

* Namespace oluşturma, güncelleştirme, silme
* Olay hub'ları oluşturma, güncelleştirme, silme
* Tüketici grubu oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Önkoşullar

Olay hub'ları yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory'ye (AAD) kimliğini doğrulaması gerekir. AAD Azure kaynaklarınızı erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması gerektirir. Bir hizmet sorumlusu oluşturma hakkında daha fazla bilgi için Bu makalelerden birine bakın:  

* [Active Directory Uygulama ve kaynaklarına erişebilir hizmet sorumlusu oluşturmak için Azure portalını kullanma](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Bu öğreticiler sağladığını bir `AppId` (istemci kimliği) `TenantId`, ve `ClientSecret` (kimlik doğrulama anahtarı), her biri yönetim kitaplıklarını tarafından kimlik doğrulaması için kullanılır. Bilmeniz gereken **sahibi** çalıştırmak istediğiniz kaynak grubu için izinleri.

## <a name="programming-pattern"></a>Programlama modeli

Ortak bir protokolle herhangi bir olay hub'ları kaynak yönetmenize olanak deseni izler:

1. AAD kullanan bir belirteç elde `Microsoft.IdentityModel.Clients.ActiveDirectory` kitaplığı.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Oluşturma `EventHubManagementClient` nesnesi.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Ayarlama `CreateOrUpdate` belirtilen değerlerinizi parametreleri.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Çağrı yürütün.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [.NET Yönetim örnek](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub başvurusu](/dotnet/api/Microsoft.Azure.Management.EventHub) 
