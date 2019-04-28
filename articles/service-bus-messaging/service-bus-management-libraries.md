---
title: Azure Service Bus yönetim kitaplıkları | Microsoft Docs
description: Service Bus ad alanı ve mesajlaşma varlıkları net'ten yönetin.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/05/2018
ms.author: aschhab
ms.openlocfilehash: d6c2cd813e96b40fc9f95785a1fd28e324a0437b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61315887"
---
# <a name="service-bus-management-libraries"></a>Service Bus yönetim kitaplıkları

Azure Service Bus yönetim kitaplıkları, Service Bus ad alanlarını ve varlıkları dinamik olarak sağlayabilirsiniz. Bu karmaşık dağıtımları ve mesajlaşma senaryoları etkinleştirir ve hangi varlıkları sağlamak için programlı olarak belirlemek mümkün kılar. Bu kitaplıklar, şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevi

* Namespace oluşturma, güncelleştirme, silme
* Sıra oluşturma, güncelleştirme, silme
* Konu oluşturma, güncelleştirme, silme
* Abonelik oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Önkoşullar

Service Bus yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (Azure AD) hizmeti ile kimlik doğrulaması gerekir. Azure AD, Azure kaynaklarınıza erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması gerektirir. Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için şu makalelerden birine bakın:  

* [Active Directory uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için Azure portalını kullanma](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Bu öğreticiler sunmak bir `AppId` (istemci kimliği) `TenantId`, ve `ClientSecret` (kimlik doğrulama anahtarı), tüm yönetim kitaplıkları ile kimlik doğrulama için kullanılır. Olmalıdır **sahibi** üzerinde istediğiniz çalıştırmak kaynak grubu için izinleri.

## <a name="programming-pattern"></a>Programlama düzeni

Tüm Service Bus kaynakları yönetmek için ortak bir protokolle yapıdadır:

1. Azure AD kullanarak bir belirteç elde **Microsoft.IdentityModel.Clients.activedirectory** kitaplığı:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. Oluşturma `ServiceBusManagementClient` nesnesi:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Ayarlama `CreateOrUpdate` belirttiğiniz değerlerin parametreleri:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Çağrı yürütün:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [.NET Yönetim örneği](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API Başvurusu](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
