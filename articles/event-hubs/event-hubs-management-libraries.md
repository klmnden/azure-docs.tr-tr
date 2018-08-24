---
title: Azure Event Hubs yönetim kitaplıkları | Microsoft Docs
description: Event Hubs ad alanlarını ve varlıkları,. NET'ten yönetme
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/13/2018
ms.author: shvija
ms.openlocfilehash: 79cddcac4d469753bc39107e6db2d8ce901111d1
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746425"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs yönetim kitaplıkları

Event Hubs ad alanlarını ve varlıkları dinamik olarak sağlamak için Azure Event Hubs yönetim kitaplıkları'nı kullanabilirsiniz. Bu dinamik yapı karmaşık dağıtımları ve mesajlaşma senaryolarına etkinleştirir, hangi varlıkları sağlamak için programlı olarak belirleyebilirsiniz. Bu kitaplıklar, şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevi

* Namespace oluşturma, güncelleştirme, silme
* Event hubs'ı oluşturma, güncelleştirme, silme
* Tüketici grubu oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Önkoşullar

Event Hubs yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (AAD) ile kimlik doğrulaması gerekir. AAD, Azure kaynaklarınıza erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması gerektirir. Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için şu makalelerden birine bakın:  

* [Active Directory uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için Azure portalını kullanma](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Bu öğreticiler sunmak bir `AppId` (istemci kimliği) `TenantId`, ve `ClientSecret` (kimlik doğrulama anahtarı), tüm yönetim kitaplıkları ile kimlik doğrulama için kullanılır. Olmalıdır **sahibi** üzerinde çalıştırmak istediğiniz kaynak grubu için izinleri.

## <a name="programming-pattern"></a>Programlama düzeni

Tüm Event Hubs kaynağı yönetmek için ortak bir protokolle yapıdadır:

1. AAD kullanarak belirteç edinme `Microsoft.IdentityModel.Clients.ActiveDirectory` kitaplığı.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Oluşturma `EventHubManagementClient` nesne.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Ayarlama `CreateOrUpdate` , belirtilen değerleri için parametreleri.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Çağrısını yürütün.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [.NET Yönetim örneği](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub başvurusu](/dotnet/api/Microsoft.Azure.Management.EventHub) 
