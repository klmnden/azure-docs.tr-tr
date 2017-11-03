---
title: "Azure İşlevlerine Genel Bakış | Microsoft Belgeleri"
description: "Uyumsuz iş yüklerini dakikalar içinde iyileştirmek için Azure İşlevlerinin nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 41f26a4b03a6431aaad21bda6336b8840d2d923f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="an-introduction-to-azure-functions"></a>Azure İşlevleri'ne giriş  
Azure İşlevleri, küçük kod parçalarını veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Tüm uygulama veya bunu çalıştıracak altyapı hakkında endişelenmeden elinizdeki sorun için ihtiyacınız olan kodu yazabilirsiniz. Geliştirme işlevlerini daha da verimli hale getirebilir ve C#, F #, Node.js, Java veya PHP gibi tercih ettiğiniz geliştirme dilini kullanabilirsiniz. Yalnızca kodunuzun çalıştığı zaman için ödeme yapın ve ihtiyaca göre ölçekleme konusunda Azure'a güvenin. Azure işlevleri geliştirmenize olanak tanır [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) Microsoft Azure üzerinde uygulamalar.

Bu konu başlığında, Azure İşlevlerine üst düzey bir genel bakış sağlanmıştır. Hemen ve işlevleriyle başlamak istiyorsanız, başlayın [ilk Azure işlevinizi oluşturma](functions-create-first-azure-function.md). İşlevler hakkında daha teknik bilgi arıyorsanız bkz. [geliştirici başvurusu](functions-reference.md).

## <a name="features"></a>Özellikler
İşlevlerinin önemli özelliklerinden bazıları şunlardır:

* **Dil Seçimi** - C#, F #, Node.js, Java, PHP, batch, bash veya herhangi bir yürütülebilir dosya kullanarak işlevleri yazın.
* **Kullandıkça ödeme fiyatlandırma modeli** - Yalnızca kodunuzu çalıştırmaya harcanan zaman için ödeme yapın. [Fiyatlandırma bölümünde](#pricing) Tüketim barındırma planı seçeneğine bakın.  
* **Kendi bağımlılıklarınızı getirin** - İşlevler NuGet ve NPM'yi desteklediğinden, sık kullandığınız kitaplıklarınızı kullanabilirsiniz.  
* **Tümleşik güvenlik** - Azure Active Directory, Facebook, Google, Twitter ve Microsoft Hesabı gibi OAuth sağlayıcılarıyla HTTP tetiklemeli işlevleri koruyun.  
* **Basitleştirilmiş tümleştirme** - Azure hizmetlerini ve yazılım olarak hizmet (SaaS) tekliflerini kolayca kullanın. Bazı örnekler için [tümleştirmeler bölümüne](#integrations) bakın.  
* **Esnek geliştirme** - işlevlerinizi doğrudan portalda kodlayın veya sürekli tümleştirme kurup kodunuzu ve aracılığıyla dağıtabilirsiniz [GitHub](../app-service/scripts/app-service-cli-continuous-deployment-github.md), [Visual Studio Team Services](../app-service/scripts/app-service-cli-continuous-deployment-vsts.md)ve diğer [desteklenen geliştirme araçları](../app-service/app-service-deploy-local-git.md).  
* **Açık kaynak** - İşlevler çalışma zamanı açık kaynaklıdır ve [GitHub'da kullanılabilir](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>İşlevler ile ne yapabilirim?
İşlevleri; verileri işleme-in-interneti ile (IOT) çalışma ve basit API'ler ve mikro hizmetler oluşturma sistemleri tümleştirme için harika bir çözümdür. Görüntü veya sıra işleme, dosya bakımı veya bir zamanlayıcıyla çalıştırmak istediğiniz herhangi bir görev için İşlevleri kullanabilirsiniz. 

İşlevler, aşağıdakiler dahil olmak üzere önemli senaryolara giriş için şablonlar sağlar:

* **HTTPTrigger** - Bir HTTP isteği kullanarak kodunuzun yürütülmesini tetikler. Bir örnek için bkz: [ilk işlevinizi oluşturma](functions-create-first-azure-function.md).
* **TimerTrigger** - Önceden tanımlanmış bir zamanlamaya göre temizleme veya diğer toplu işlem görevlerini yürütür. Bir örnek için bkz: [zamanlayıcısı tarafından tetiklenen bir işlev oluşturun](functions-create-scheduled-function.md).
* **GitHub web kancası** - GitHub depolarınızda gerçekleşen olaylara yanıt verir. Bir örnek için bkz: [GitHub Web kancası tarafından tetiklenen bir işlev oluşturun](functions-create-github-webhook-triggered-function.md).
* **Genel web kancası** - Web kancalarını destekleyen herhangi bir hizmetten web kancası HTTP isteklerini işler. Bir örnek için bkz: [genel bir Web kancası tarafından tetiklenen bir işlev oluşturun](functions-create-generic-webhook-triggered-function.md).
* **CosmosDBTrigger** -işlem Azure Cosmos DB belgeleri eklendiğinde veya bir NoSQL veritabanı koleksiyonda güncelleştirildi. Bir örnek için bkz: [Azure Cosmos DB tarafından tetiklenen bir işlev oluşturun](functions-create-cosmos-db-triggered-function.md).
* **BlobTrigger** - Azure Storage bloblarını kapsayıcılara eklendiklerinde eşler. Görüntüyü yeniden boyutlandırmak için bu işlevi kullanabilirsiniz. Daha fazla bilgi için bkz: [Blob Depolama bağlamaları](functions-bindings-storage-blob.md).
* **QueueTrigger** - Bir Azure Storage kuyruğuna geldiklerinde iletilere yanıt verir. Bir örnek için bkz: [Azure kuyruk Depolama tarafından tetiklenen bir işlev oluşturun](functions-create-storage-queue-triggered-function.md).
* **EventHubTrigger** - Bir Azure Event Hub'ına teslim edilen olaylara yanıt verir. Uygulama izleme, kullanıcı deneyiminde veya iş akışı işlemede ve Nesnelerin İnterneti (IoT) senaryolarında özellikle kullanışlıdır. Daha fazla bilgi için bkz: [olay hub'ları bağlamaları](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger** - İleti kuyruklarını dinleyerek kodunuzu diğer Azure hizmetlerine veya şirket içi hizmetlere bağlar. Daha fazla bilgi için bkz: [Service Bus bağlamaları](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger** - Konu başlıklarına abone olarak kodunuzu diğer Azure hizmetlerine veya şirket içi hizmetlere bağlar. Daha fazla bilgi için bkz: [Service Bus bağlamaları](functions-bindings-service-bus.md).

Azure İşlevleri, kodunuzu yürütmeye başlama yolu olan *tetikleyicileri* ve giriş ve çıkış verilerini kodlamayı basitleştirme yolu olan *bağlamaları* destekler. Azure İşlevlerinin sağladığı tetikleyicilerin ve bağlamaların ayrıntılı bir açıklaması için bkz. [Azure İşlevleri tetikleyicileri ve bağlamaları geliştirici başvurusu](functions-triggers-bindings.md)

## <a name="integrations"></a>Tümleştirmeler
Azure İşlevleri, çeşitli Azure ve üçüncü taraf hizmetleri ile tümleştirilebilir. Bu hizmetler, işlevinizi tetikleyip yürütmeyi başlatabilir ya da kodunuz için giriş ve çıkış görevi görebilir. Aşağıdaki hizmet tümleştirmeleri Azure işlevleri tarafından desteklenir:

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Event Grid
* Azure Mobile Apps (tablolar)
* Azure Notification Hubs
* Azure Service Bus (kuyruklar ve konu başlıkları)
* Azure Storage (blob, kuyruklar ve tablolar) 
* GitHub (web kancaları)
* Şirket içi (Service Bus kullanarak)
* Twilio (SMS mesajları)

## <a name="pricing"></a>İşlevlerin maliyeti ne kadardır?
Azure işlevleri planları fiyatlandırma iki tür vardır. Gereksinimlerinize en uygun olanı seçin: 

* **Tüketim planı** - İşleviniz çalıştığında Azure tüm gerekli bilgi işlem kaynaklarını sağlar. Kaynak yönetimi hakkında endişelenmenize gerek yoktur ve yalnızca kodunuzun çalıştığı süre için ödeme yaparsınız. 
* **App Service planı** - İşlevlerinizi web, mobil ve API uygulamalarınız gibi çalıştırın. Diğer uygulamalarınız için App Service'i zaten kullanıyor olmanız halinde işlevlerinizi hiçbir ek ücret ödemeden aynı planda çalıştırabilirsiniz. 

Barındırma planları hakkında daha fazla bilgi için bkz: [Azure planı karşılaştırma barındırma işlevleri](functions-scale.md). Fiyatlandırma ayrıntılarının tümü [İşlevler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/) bulunur.

## <a name="next-steps"></a>Sonraki Adımlar
* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)  
  Hemen başlayın ve Azure İşlevleri hızlı başlangıcını kullanarak ilk işlevinizi oluşturun. 
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Azure İşlevleri çalışma zamanı hakkında daha teknik bilgiler ve işlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bir başvuru sağlar.
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 
* [Azure Uygulama Hizmeti hakkında daha fazla bilgi edinin](../app-service/app-service-web-overview.md)  
  Azure işlevleri yararlanır Azure App Service çekirdek işlevlerin dağıtımlar, ortam değişkenleri ve tanılama gibi. 

