---
title: Azure hizmet veri yolu yönetim kitaplıklarını | Microsoft Docs
description: Hizmet veri yolu ad alanları ve .NET varlıklardan Mesajlaşma yönetin.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/05/2018
ms.author: sethm
ms.openlocfilehash: 7946958bec8b2f444155b5a9701f1f7401fe4f3c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29120905"
---
# <a name="service-bus-management-libraries"></a>Hizmet veri yolu yönetim kitaplıkları

Azure hizmet veri yolu yönetim kitaplıklarını dinamik olarak hizmet veri yolu ad alanları ve varlıkları sağlayabilirsiniz. Bu karmaşık dağıtımlar ve mesajlaşma senaryolarına olanak sağlar ve program aracılığıyla sağlamak için hangi varlıkları belirlemek mümkün hale getirir. Bu kitaplıklar, .NET için şu anda kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevi

* Namespace oluşturma, güncelleştirme, silme
* Kuyruk oluşturma, güncelleştirme, silme
* Konu oluşturma, güncelleştirme, silme
* Abonelik oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Önkoşullar

Hizmet veri yolu yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (Azure AD) hizmeti ile kimlik doğrulaması gerekir. Azure AD, Azure kaynaklarınızı erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması gerektirir. Bir hizmet sorumlusu oluşturma hakkında daha fazla bilgi için Bu makalelerden birine bakın:  

* [Active Directory Uygulama ve kaynaklarına erişebilir hizmet sorumlusu oluşturmak için Azure portalını kullanma](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Bu öğreticiler sağladığını bir `AppId` (istemci kimliği) `TenantId`, ve `ClientSecret` (kimlik doğrulama anahtarı), her biri yönetim kitaplıklarını tarafından kimlik doğrulaması için kullanılır. Bilmeniz gereken **sahibi** üzerinde istediğiniz çalıştırmak kaynak grubu için izinleri.

## <a name="programming-pattern"></a>Programlama modeli

Ortak bir protokolle herhangi bir Service Bus kaynak yönetmenize olanak deseni izler:

1. Azure AD kullanarak bir belirteç elde **Microsoft.IdentityModel.Clients.activedirectory tarafından** kitaplığı:
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
3. Ayarlama `CreateOrUpdate` belirtilen değerlerinizi parametreleri:

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

* [.NET Yönetim örnek](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API reference](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
