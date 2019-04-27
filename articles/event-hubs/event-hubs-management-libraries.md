---
title: Yönetim kitaplıkları - Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs ad alanlarını ve varlıkları,. NET'ten yönetmek için kullanabileceğiniz Kitaplığı hakkında bilgiler sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 431fe04461f422274697d1e91c4b56e914ce2d4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60746667"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs yönetim kitaplıkları

Event Hubs ad alanlarını ve varlıkları dinamik olarak sağlamak için Azure Event Hubs yönetim kitaplıkları'nı kullanabilirsiniz. Bu dinamik yapı karmaşık dağıtımları ve mesajlaşma senaryolarına etkinleştirir, hangi varlıkları sağlamak için programlı olarak belirleyebilirsiniz. Bu kitaplıklar, şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevi

* Namespace oluşturma, güncelleştirme, silme
* Event hubs'ı oluşturma, güncelleştirme, silme
* Tüketici grubu oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Önkoşullar

Event Hubs yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (AAD) ile kimlik doğrulaması gerekir. AAD, Azure kaynaklarınıza erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması gerektirir. Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için şu makalelerden birine bakın:  

* [Active Directory uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için Azure portalını kullanma](../active-directory/develop/howto-create-service-principal-portal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
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
