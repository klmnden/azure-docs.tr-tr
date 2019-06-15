---
title: Azure işlevleri ve Azure SignalR hizmeti ile gerçek zamanlı uygulamalar oluşturma
description: Azure SignalR hizmeti kullanarak sunucusuz uygulamalar genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: c06203cfd7537dd4c9bc35c75a3f21d12bf64d26
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61401888"
---
# <a name="build-real-time-apps-with-azure-functions-and-azure-signalr-service"></a>Azure işlevleri ve Azure SignalR hizmeti ile gerçek zamanlı uygulamalar oluşturma

Azure SignalR Service ve Azure İşlevlerinin ikisi de altyapıyı yönetmek yerine uygulama geliştirmeye odaklanmanıza imkan tanıyan tam yönetimli, yüksek düzeyde ölçeklenebilir hizmetler olduğundan [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda gerçek zamanlı iletişim sağlamak için iki hizmetin birlikte kullanılması yaygındır.

## <a name="integrate-real-time-communications-with-azure-services"></a>Gerçek zamanlı iletişimleri Azure hizmetleriyle tümleştirme

Azure İşlevleri, bulutta olaylar gerçekleştiğinde tetiklen bir kodu JavaScript, C# ve Java dahil olmak üzere [çeşitli dillerde](../azure-functions/supported-languages.md) yazmanıza olanak tanır. Bu olayların örnekleri arasında şunlar yer alır:

* HTTP ve web kancası istekleri
* Periyodik zamanlayıcılar
* Azure hizmetlerindeki olaylar, örneğin:
    - Event Grid
    - Event Hubs
    - Service Bus
    - Cosmos DB değişiklik akışı
    - Depolama - bloblar ve kuyruklar
    - Salesforce ve SQL Server gibi Logic Apps bağlayıcıları

BU olayları Azure SignalR Service ile tümleştirmek üzere Azure İşlevlerini kullanarak olaylar gerçekleştiğinde binlerce istemciye bildirim gönderme özelliğine sahip olursunuz.

Azure Functions ve SignalR Service ile uygulayabileceğiniz gerçek zamanlı sunucusuz mesajlaşmaya ilişkin bazı yaygın senaryolar arasında şunlar yer alır:

* IoT cihaz telemetrisini gerçek zamanlı bir pano veya haritada görselleştirme
* Belgeler Cosmos DB’de güncelleştirildiğinde uygulamadaki verileri güncelleştirme
* Salesforce’da yeni siparişler oluşturulduğunda uygulama içi bildirimler gönderme

## <a name="signalr-service-bindings-for-azure-functions"></a>Azure İşlevleri için SignalR Service bağlamaları

Azure İşlevleri için SignalR Service bağlamaları Azure İşlevi uygulamasının SignalR Service’e bağlı istemcilerde iletiler yayımlamasına olanak tanır. İstemciler .NET, JavaScript ve Java’da kullanıma sunulan bir SignalR istemci SDK’sını kullanarak hizmete bağlanabilir, daha fazla dil desteği yakında gelecektir.

### <a name="an-example-scenario"></a>Örnek Senaryo

SignalR Service bağlamalarının kullanma örneklerinden birisi Cosmos DB değişiklik akışında yeni olaylar belirdiğinde gerçek zamanlı iletiler göndermek üzere Azure Cosmos DB ve SignalR Service ile tümleştirmek için Azure İşlevlerini kullanmaktır.

![Cosmos DB, Azure İşlevleri, SignalR Hizmeti](media/signalr-concept-azure-functions/signalr-cosmosdb-functions.png)

1. Cosmos DB koleksiyonunda bir değişiklik yapıldı
2. Değişiklik olayı Cosmos DB değişiklik akışına yayıldı
3. Azure İşlevleri, Cosmos DB tetikleyicisini kullanan değişiklik olayı tarafından tetiklendi
4. SignalR Service çıktı bağlaması SignalR Service’te bir ileti yayımlar
5. SignalR Service iletiyi tüm bağlı istemcilerde yayımlar

### <a name="authentication-and-users"></a>Kimlik doğrulama ve kullanıcılar

SignalR Service iletilerin tüm istemcilere veya tek bir kullanıcıya ait olanlar gibi belirli bir istemci alt kümesine yayınlanmasına olanak tanır. Azure İşlevleri için SignalR Service bağlamaları; Azure Active Directory, Facebook ve Twitter gibi sağlayıcılarda kullanıcıların kimliğini doğrulamak için App Service Kimlik Doğrulama ile birleştirilebilir. Ardından iletileri doğrudan bu kimliği doğrulanmış kullanıcılara gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, geniş yelpazedeki gerçek zamanlı mesajlaşma senaryolarını etkinleştirmek üzere Azure İşlevlerini SignalR Service ile birlikte kullanmaya yönelik bir genel bakış edinirsiniz.

Azure işlevleri ve SignalR hizmeti birlikte kullanma hakkında tam Ayrıntılar için aşağıdaki kılavuzu okuyun.

* [Azure işlevleri geliştirme ve SignalR hizmeti ile yapılandırma](signalr-concept-serverless-development-config.md)

Daha fazla bilgi edinmek için bu hızlı başlangıçlardan birini izleyin.

* [Azure SignalR Service Sunucusuz Hızlı Başlangıcı - C#](signalr-quickstart-azure-functions-csharp.md)
* [Azure SignalR Service Sunucusuz Hızlı Başlangıcı - JavaScript](signalr-quickstart-azure-functions-javascript.md)