---
title: Azure İşlevlerine Genel Bakış | Microsoft Belgeleri
description: Uyumsuz iş yüklerini dakikalar içinde iyileştirmek için Azure İşlevlerinin nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: mattchenderson
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: b8d57a2bbaa53a0291dc9c05ab234c3238322a71
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61020290"
---
# <a name="an-introduction-to-azure-functions"></a>Azure İşlevleri'ne giriş  
Azure İşlevleri, küçük kod parçalarını veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Tüm uygulama veya bunu çalıştıracak altyapı hakkında endişelenmeden elinizdeki sorun için ihtiyacınız olan kodu yazabilirsiniz. İşlevler geliştirme sürecinizi daha da verimli hale getirebilir ve tercih ettiğiniz geliştirme dilini (C#, F#, Node.js, Java veya PHP gibi) kullanabilirsiniz. Yalnızca kodunuzun çalıştığı zaman için ödeme yapın ve ihtiyaca göre ölçekleme konusunda Azure'a güvenin. Azure İşlevleri, Microsoft Azure'da [sunucusuz](https://azure.microsoft.com/solutions/serverless/) uygulamalar geliştirmenizi sağlar.

Bu konu başlığında, Azure İşlevlerine üst düzey bir genel bakış sağlanmıştır. İşlevleri kullanmaya hemen başlamak isterseniz [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md) ile başlayın. İşlevler hakkında daha teknik bilgi arıyorsanız bkz. [geliştirici başvurusu](functions-reference.md).

## <a name="features"></a>Özellikler
İşlevlerin önemli özelliklerinden bazıları şunlardır:

* **Dil seçimi** - C#, F# veya Javascript dillerinden istediğinizi kullanarak işlevler yazın. Diğer seçenekler için bkz. [Desteklenen diller](supported-languages.md).
* **Kullandıkça ödeme fiyatlandırma modeli** - Yalnızca kodunuzu çalıştırmaya harcanan zaman için ödeme yapın. [Fiyatlandırma bölümünde](#pricing) Tüketim barındırma planı seçeneğine bakın.  
* **Kendi bağımlılıklarınızı getirin** - İşlevler NuGet ve NPM'yi desteklediğinden, sık kullandığınız kitaplıklarınızı kullanabilirsiniz.  
* **Tümleşik güvenlik** - Azure Active Directory, Facebook, Google, Twitter ve Microsoft Hesabı gibi OAuth sağlayıcılarıyla HTTP tetiklemeli işlevleri koruyun.  
* **Basitleştirilmiş tümleştirme** - Azure hizmetlerini ve yazılım olarak hizmet (SaaS) tekliflerini kolayca kullanın. Bazı örnekler için [tümleştirmeler bölümüne](#integrations) bakın.  
* **Esnek geliştirme** - İşlevlerinizi doğrudan portalda kodlayın veya sürekli tümleştirme kurup kodunuzu [GitHub](../app-service/scripts/cli-continuous-deployment-github.md), [Azure DevOps Services](../app-service/scripts/cli-continuous-deployment-vsts.md) ve diğer [desteklenen geliştirme araçları](../app-service/deploy-local-git.md) aracılığıyla dağıtın.  
* **Açık kaynak** - İşlevler çalışma zamanı açık kaynaklıdır ve [GitHub'da kullanılabilir](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>İşlevler ile ne yapabilirim?
İşlevler; verileri işleme, sistemleri tümleştirme, nesnelerin İnterneti (IoT) ile çalışma ve basit API'ler ve mikro hizmetler oluşturma için harika bir çözümdür. Görüntü veya sıra işleme, dosya bakımı veya bir zamanlayıcıyla çalıştırmak istediğiniz herhangi bir görev için İşlevleri kullanabilirsiniz. 

İşlevler, aşağıdakiler dahil olmak üzere önemli senaryolara giriş için şablonlar sağlar:

* **HTTPTrigger** - Bir HTTP isteği kullanarak kodunuzun yürütülmesini tetikler. Bir örnek için, bkz. [İlk işlevinizi oluşturma](functions-create-first-azure-function.md).
* **TimerTrigger** - Önceden tanımlanmış bir zamanlamaya göre temizleme veya diğer toplu işlem görevlerini yürütür. Bir örnek için, bkz. [Bir zamanlayıcı tarafından tetiklenen bir işlev oluşturma](functions-create-scheduled-function.md).
* **CosmosDBTrigger** - Azure Cosmos DB belgelerini bir NoSQL veritabanına eklendiğinde veya güncelleştirildiğinde işleyin. Daha fazla bilgi için bkz. [Azure Cosmos DB'de bağlamaları](functions-bindings-cosmosdb-v2.md).
* **BlobTrigger** - Azure Storage bloblarını kapsayıcılara eklendiklerinde eşler. Görüntüyü yeniden boyutlandırmak için bu işlevi kullanabilirsiniz. Daha fazla bilgi için bkz. [Blob depolama bağlamaları](functions-bindings-storage-blob.md).
* **QueueTrigger** - Bir Azure Storage kuyruğuna geldiklerinde iletilere yanıt verir. Daha fazla bilgi için bkz. [Azure Kuyruk Depolama bağlamaları](functions-bindings-storage-queue.md).
* **EventGridTrigger** - Azure Event Grid’deki bir aboneliğe teslim edilen olaylara yanıt verir. Filtreleme içeren olayları almak için abonelik tabanlı bir model destekler. Olay tabanlı mimariler oluşturmak için iyi bir çözümdür. Örnek için, bkz. [Yüklenen görüntülerin yeniden boyutlandırılmasını Event Grid kullanarak otomatikleştirme](../event-grid/resize-images-on-storage-blob-upload-event.md).
* **EventHubTrigger** - Bir Azure Event Hub'ına teslim edilen olaylara yanıt verir. Uygulama izleme, kullanıcı deneyiminde veya iş akışı işlemede ve nesnelerin İnternet'e (IOT) senaryolarında yararlıdır. Daha fazla bilgi için bkz. [Event Hubs bağlamaları](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger** - İleti kuyruklarını dinleyerek kodunuzu diğer Azure hizmetlerine veya şirket içi hizmetlere bağlar. Daha fazla bilgi için, bkz. [Service Bus bağlamaları](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger** - Konu başlıklarına abone olarak kodunuzu diğer Azure hizmetlerine veya şirket içi hizmetlere bağlar. Daha fazla bilgi için, bkz. [Service Bus bağlamaları](functions-bindings-service-bus.md).

Azure İşlevleri, kodunuzu yürütmeye başlama yolu olan *tetikleyicileri* ve giriş ve çıkış verilerini kodlamayı basitleştirme yolu olan *bağlamaları* destekler. Azure İşlevlerinin sağladığı tetikleyicilerin ve bağlamaların ayrıntılı bir açıklaması için bkz. [Azure İşlevleri tetikleyicileri ve bağlamaları geliştirici başvurusu](functions-triggers-bindings.md)

## <a name="integrations"></a>Tümleştirmeler
Azure İşlevleri, çeşitli Azure ve üçüncü taraf hizmetleri ile tümleştirilebilir. Bu hizmetler, işlevinizi tetikleyip yürütmeyi başlatabilir ya da kodunuz için giriş ve çıkış görevi görebilir. Aşağıdaki hizmet tümleştirmeleri Azure İşlevleri tarafından desteklenir:

* Azure Cosmos DB
* Azure Event Hubs
* Azure Event Grid
* Azure Notification Hubs
* Azure Service Bus (kuyruklar ve konu başlıkları)
* Azure Storage (blob, kuyruklar ve tablolar)
* Şirket içi (Service Bus kullanarak)
* Twilio (SMS mesajları)

## <a name="pricing"></a>İşlevlerin maliyeti ne kadardır?
Azure İşlevleri iki fiyatlandırma planı türüne sahiptir. Gereksinimlerinize en uygun olanı seçin: 

* **Tüketim planı** - İşleviniz çalıştığında Azure tüm gerekli bilgi işlem kaynaklarını sağlar. Kaynak yönetimi hakkında endişelenmenize gerek yoktur ve yalnızca kodunuzun çalıştığı süre için ödeme yaparsınız. 
* **App Service planı** - İşlevlerinizi web uygulamalarınız gibi çalıştırın. Diğer uygulamalarınız için App Service'i zaten kullanıyor olmanız halinde işlevlerinizi hiçbir ek ücret ödemeden aynı planda çalıştırabilirsiniz. 

Barındırma planları hakkında daha fazla bilgi için bkz. [Azure İşlevleri barındırma planı karşılaştırma](functions-scale.md). Fiyatlandırma ayrıntılarının tümü [İşlevler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/) bulunur.

## <a name="next-steps"></a>Sonraki Adımlar
* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)  
  Hemen başlayın ve Azure İşlevleri hızlı başlangıcını kullanarak ilk işlevinizi oluşturun. 
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Azure İşlevleri çalışma zamanı hakkında daha teknik bilgiler ve işlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bir başvuru sağlar.
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 
* [Azure Uygulama Hizmeti hakkında daha fazla bilgi edinin](../app-service/overview.md)  
  Azure İşlevleri; dağıtımlar, ortam değişkenleri ve tanılama gibi temel işlevler için Azure App Service’i kullanır. 

