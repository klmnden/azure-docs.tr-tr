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
ms.date: 06/05/2019
ms.author: aschhab
ms.openlocfilehash: 3836eb87516eed546ae6bb69f53bf64e5df00906
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693202"
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

## <a name="complete-code-to-create-a-queue"></a>Bir kuyruk oluşturmak için tam kod
Service Bus kuyruğuna oluşturmak için tam kod aşağıdaki gibidir: 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.core.windows.net/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

> [!IMPORTANT]
> Tam bir örnek için bkz. [.NET Yönetim örneği github'daki]((https://github.com/Azure-Samples/service-bus-dotnet-management/)). 

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft.Azure.Management.ServiceBus API Başvurusu](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
