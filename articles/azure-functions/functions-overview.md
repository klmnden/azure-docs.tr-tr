---
title: "Azure İşlevlerine Genel Bakış | Microsoft Belgeleri"
description: "Uyumsuz iş yüklerini dakikalar içinde iyileştirmek için Azure İşlevlerinin nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/20/2016
ms.author: cfowler;mahender;glenga
translationtype: Human Translation
ms.sourcegitcommit: 30cc3b8749d5b36b89b242e2691003cc6f67f7d2
ms.openlocfilehash: 00359057d702c556cd8beb91cf17ccf41c96f601


---
# <a name="azure-functions-overview"></a>Azure İşlevlerine Genel Bakış
Azure İşlevleri, küçük kod parçalarını veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Tüm uygulama veya bunu çalıştıracak altyapı hakkında endişelenmeden elinizdeki sorun için ihtiyacınız olan kodu yazabilirsiniz. İşlevler geliştirme sürecinizi daha da verimli hale getirebilir ve tercih ettiğiniz geliştirme dilini (C#, F#, Node.js, Python veya PHP gibi) kullanabilirsiniz. Yalnızca kodunuzun çalıştığı zaman için ödeme yapın ve ihtiyaca göre ölçekleme konusunda Azure'a güvenin.

Bu konu başlığında, Azure İşlevlerine üst düzey bir genel bakış sağlanmıştır. Azure İşlevlerini kullanmaya hemen başlamak isterseniz [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md) ile başlayın. İşlevler hakkında daha teknik bilgi arıyorsanız bkz. [geliştirici başvurusu](functions-reference.md).

## <a name="features"></a>Özellikler
Azure İşlevlerinin önemli özelliklerinden bazıları şunlardır:

* **Dil seçimi** - C#, F#, Node.js, Python, PHP, batch, bash veya herhangi bir yürütülebilir dosya kullanarak işlevleri yazın.
* **Kullandıkça ödeme fiyatlandırma modeli** - Yalnızca kodunuzu çalıştırmaya harcanan zaman için ödeme yapın. [Fiyatlandırma bölümünde](#pricing) Tüketim barındırma planı seçeneğine bakın.  
* **Kendi bağımlılıklarınızı getirin** - İşlevler NuGet ve NPM'yi desteklediğinden, sık kullandığınız kitaplıklarınızı kullanabilirsiniz.  
* **Tümleşik güvenlik** - Azure Active Directory, Facebook, Google, Twitter ve Microsoft Hesabı gibi OAuth sağlayıcılarıyla HTTP tetiklemeli işlevleri koruyun.  
* **Basitleştirilmiş tümleştirme** - Azure hizmetlerini ve yazılım olarak hizmet (SaaS) tekliflerini kolayca kullanın. Bazı örnekler için [tümleştirmeler bölümüne](#integrations) bakın.  
* **Esnek geliştirme** - İşlevlerinizi doğrudan portalda kodlayın veya sürekli tümleştirme kurup kodunuzu GitHub, Visual Studio Team Services ve diğer [desteklenen geliştirme araçları](../app-service-web/web-sites-deploy.md#deploy-using-an-ide) aracılığıyla dağıtın.  
* **Açık kaynak** - İşlevler çalışma zamanı açık kaynaklıdır ve [GitHub'da kullanılabilir](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>İşlevler ile ne yapabilirim?
Azure İşlevleri; verileri işleme, sistemleri tümleştirme, nesnelerin İnterneti (IoT) ile çalışma ve basit API'ler ve mikro hizmetler oluşturma için harika bir çözümdür. Görüntü veya sıra işleme, dosya bakımı, arka plan iş parçacığında çalıştırmak istediğiniz uzun süre çalışan görevler veya bir zamanlayıcıyla çalıştırmak istediğiniz herhangi bir görev için İşlevleri dikkate alın. 

İşlevler, aşağıdakiler dahil olmak üzere önemli senaryolara giriş için şablonlar sağlar:

* **BlobTrigger** - Azure Storage bloblarını kapsayıcılara eklendiklerinde eşler. Görüntüyü yeniden boyutlandırmak için bu işlevi kullanabilirsiniz.
* **EventHubTrigger** - Bir Azure Event Hub'ına teslim edilen olaylara yanıt verir. Uygulama izleme, kullanıcı deneyiminde veya iş akışı işlemede ve Nesnelerin İnterneti (IoT) senaryolarında özellikle kullanışlıdır.
* **Genel web kancası** - Web kancalarını destekleyen herhangi bir hizmetten web kancası HTTP isteklerini işler.
* **GitHub web kancası** - GitHub depolarınızda gerçekleşen olaylara yanıt verir. Örnek olarak bkz. [Bir web kancası veya API işlevi oluşturma](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - Bir HTTP isteği kullanarak kodunuzun yürütülmesini tetikler.
* **QueueTrigger** - Bir Azure Storage kuyruğuna geldiklerinde iletilere yanıt verir. Bir örnek için bkz. [Bir Azure hizmetine bağlanan bir Azure İşlevi oluşturma](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - İleti kuyruklarını dinleyerek kodunuzu diğer Azure hizmetlerine veya şirket içi hizmetlere bağlar. 
* **ServiceBusTopicTrigger** - Konu başlıklarına abone olarak kodunuzu diğer Azure hizmetlerine veya şirket içi hizmetlere bağlar. 
* **TimerTrigger** - Önceden tanımlanmış bir zamanlamaya göre temizleme veya diğer toplu işlem görevlerini yürütür. Örnek olarak bkz. [Olay işleme işlevi oluşturma](functions-create-an-event-processing-function.md).

Azure İşlevleri, kodunuzu yürütmeye başlama yolu olan *tetikleyicileri* ve giriş ve çıkış verilerini kodlamayı basitleştirme yolu olan *bağlamaları* destekler. Azure İşlevlerinin sağladığı tetikleyicilerin ve bağlamaların ayrıntılı bir açıklaması için bkz. [Azure İşlevleri tetikleyicileri ve bağlamaları geliştirici başvurusu](functions-triggers-bindings.md)

## <a name="a-nameintegrationsaintegrations"></a><a name="integrations"></a>Tümleştirmeler
Azure İşlevleri, çeşitli Azure ve üçüncü taraf hizmetleri ile tümleştirilebilir. Bu hizmetler, işlevinizi tetikleyip yürütmeyi başlatabilir ya da kodunuz için giriş ve çıkış görevi görebilir. Aşağıdaki hizmet tümleştirmeleri Azure İşlevleri tarafından desteklenir. 

* Azure DocumentDB
* Azure Event Hubs 
* Azure Mobile Apps (tablolar)
* Azure Notification Hubs
* Azure Service Bus (kuyruklar ve konu başlıkları)
* Azure Storage (blob, kuyruklar ve tablolar) 
* GitHub (web kancaları)
* Şirket içi (Service Bus kullanarak)

## <a name="a-namepricingahow-much-does-functions-cost"></a><a name="pricing"></a>İşlevlerin maliyeti ne kadardır?
Azure İşlevlerinin iki tür fiyatlandırma planı bulunur; ihtiyaçlarınızı en iyi şekilde karşılayanı seçin: 

* **Tüketim planı** - İşleviniz çalıştığında Azure tüm gerekli bilgi işlem kaynaklarını sağlar. Kaynak yönetimi hakkında endişelenmenize gerek yoktur ve yalnızca kodunuzun çalıştığı süre için ödeme yaparsınız. 
* **App Service planı** - İşlevlerinizi web, mobil ve API uygulamalarınız gibi çalıştırın. Diğer uygulamalarınız için App Service'i zaten kullanıyor olmanız halinde işlevlerinizi hiçbir ek ücret ödemeden aynı planda çalıştırabilirsiniz. 

Fiyatlandırma ayrıntılarının tümü [İşlevler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/) bulunur. İşlevlerinizi ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure İşlevlerini ölçeklendirme](functions-scale.md)

## <a name="next-steps"></a>Sonraki Adımlar
* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)  
  Hemen başlayın ve Azure İşlevleri hızlı başlangıcını kullanarak ilk işlevinizi oluşturun. 
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Azure İşlevleri çalışma zamanı hakkında daha teknik bilgiler ve işlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bir başvuru sağlar.
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 
* [Azure Uygulama Hizmeti hakkında daha fazla bilgi edinin](../app-service/app-service-value-prop-what-is.md)  
  Azure İşlevleri; dağıtımlar, ortam değişkenleri ve tanılama gibi temel işlevler için Azure App Service platformunu kullanır. 




<!--HONumber=Dec16_HO1-->


