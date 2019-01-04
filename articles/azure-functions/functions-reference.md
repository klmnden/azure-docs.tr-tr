---
title: Azure işlevleri geliştirmeye yönelik rehberlik | Microsoft Docs
description: Azure işlevleri kavramlarını ve tüm programlama dilleri ve bağlamaları, azure'da işlevleri geliştirmek için ihtiyacınız teknikleri öğrenin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Geliştirici Kılavuzu, azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 42635852bb5c6e7b388d4dc58b9d5bfaa6212438
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725869"
---
# <a name="azure-functions-developers-guide"></a>Azure işlevleri Geliştirici Kılavuzu
Azure işlevleri'nde belirli işlevleri birkaç temel teknik kavramlar ve bileşenler, dil veya kullandığınız bağlama bağımsız olarak paylaşın. Belirtilen dil veya bağlama için belirli ayrıntıları öğrenme moduna kullanmaya başlamadan önce bunların tümüne uygulanan bu genel bakışta aracılığıyla okuduğunuzdan emin olun.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevlerine genel bakış](functions-overview.md) ve bilginiz [Tetikleyicileri ve bağlamaları JobHost çalışma zamanı gibi Web işleri SDK'sı kavramları](https://github.com/Azure/azure-webjobs-sdk/wiki). Azure işlevleri WebJobs SDK'da dayanır. 

## <a name="function-code"></a>İşlev kodu
A *işlevi* Azure işlevleri'nde birincil kavramdır. Dilediğiniz bir dilde bir işlev için kod yazma ve kod ve yapılandırma dosyaları aynı klasöre kaydedin. Yapılandırma adlı `function.json`, JSON yapılandırma verilerini içerir. Desteklenen çeşitli diller ve her birinin bu dil için en iyi şekilde çalışması için en iyi duruma getirilmiş biraz farklı bir deneyimi vardır. 

Function.json dosyası, işlev bağlamaları ve diğer yapılandırma ayarlarını tanımlar. Çalışma zamanı izlenecek olaylar belirlemek için bu dosya ve verileri aktarmak ve veri işlevi yürütülmesini döndürmek nasıl kullanır. Function.json dosyası örneği verilmiştir.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Ayarlama `disabled` özelliğini `true` yürütülmekte olan işlevin önlemek için.

`bindings` Özelliği hem Tetikleyicileri ve bağlamaları yapılandırdığınız. Her bağlama, birkaç ortak ayarları ve belirli bir bağlama türüne özgü bazı ayarlar paylaşır. Her bağlamanın, aşağıdaki ayarları gerektirir:

| Özellik | Değerler/türleri | Yorumlar |
| --- | --- | --- |
| `type` |dize |Bağlama türü. Örneğin, `queueTrigger`. |
| `direction` |'de,' 'out' |Bağlama işlevdeki veri alma veya işlevden veri göndermek için uygun olup olmadığını gösterir. |
| `name` |dize |İşlevde bağlı veriler için kullanılan ad. C# için bu bir bağımsız değişken adıdır; JavaScript için bir anahtar/değer listesinde anahtardır. |

## <a name="function-app"></a>İşlev uygulaması
Bir işlev uygulaması, işlevlerinizin çalıştığı azure'da bir yürütme bağlamı sağlar. Bir işlev uygulaması birlikte Azure App Service tarafından yönetilen bir veya daha fazla tekil işlevler içerir. Tüm işlevlerin bir işlev uygulaması, aynı fiyatlandırma planı, sürekli dağıtım ve çalışma zamanı sürümü paylaşır. Bir işlev uygulaması düzenlemek ve topluca işlevlerinizi yönetmek için bir yol düşünün. 

> [!NOTE]
> İle başlayarak [sürüm 2.x](functions-versions.md) aynı dilde Azure işlevleri çalışma zamanı, bir işlev uygulamasında tüm işlevleri yazılması gerekir.

## <a name="runtime"></a>Çalışma Zamanı
Azure işlevleri çalışma zamanı veya komut dosyası ana bilgisayarı, olayları dinleyen, toplar ve veri gönderen ve sonuçta kodunuzun çalıştığı temel alınan ana bilgisayardır. Bu aynı konak Web işleri SDK'sı tarafından kullanılır.

Çalışma zamanı için HTTP tetikleyici istekleri işleyen bir web ana bilgisayarı yok. İki ana sahip Önden çalışma zamanının ayırmaya yardımcı web ana bilgisayar tarafından yönetilen trafiği sonlandırın.

## <a name="folder-structure"></a>klasör yapısı
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bir işlev uygulaması ile Azure işlevleri dağıtmak için bir proje ayarlama, bu klasör yapısı site kodunuzu davranabilirsiniz. Kullanmanızı öneririz [dağıtım paketini](deployment-zip-push.md) projenizi işlevi uygulamanızı Azure'a dağıtmak için. Gibi mevcut araçları da kullanabilirsiniz [sürekli tümleştirme ve dağıtım](functions-continuous-deployment.md) ve Azure DevOps.

> [!NOTE]
> Dağıtılacak emin olun, `host.json` dosya ve klasörleri doğrudan işlev `wwwroot` klasör. Dahil etmezseniz `wwwroot` dağıtımlarınızı klasöründe. Aksi takdirde, elde edersiniz `wwwroot\wwwroot` klasörleri.

## <a id="fileupdate"></a> İşlev uygulaması dosyalarını güncelleştirme
Azure portalda yerleşik işlev Düzenleyicisi, güncelleştirmenizi sağlar *function.json* dosyası ve bir işlev için kod dosyası. Karşıya yükleme veya güncelleştirme gibi diğer dosyaları *package.json* veya *project.json* veya bağımlılıkları, diğer dağıtım yöntemlerini kullanmanız.

İşlev uygulamaları App Service üzerinde bu nedenle tüm yerleşik [standart web Apps'e dağıtım seçeneklerini](../app-service/deploy-local-git.md) işlev uygulamaları için kullanılabilir. Karşıya yükleme veya güncelleştirme işlevini uygulama dosyaları için kullanabileceğiniz bazı yöntemleri aşağıda verilmiştir. 

#### <a name="use-local-tools-and-publishing"></a>Yerel araçlarını kullanın ve yayımlama
İşlev uygulamaları yazılabilir ve dahil olmak üzere çeşitli araçları kullanarak yayımlanan [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [Intellij](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md)ve [Azure işlevleri çekirdek Araçları](./functions-develop-local.md). Daha fazla bilgi için [kod ve test, Azure işlevleri yerel olarak](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="continuous-deployment"></a>Sürekli dağıtım
Konu başlığı altındaki yönergeleri [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralel yürütme
Birden çok tetikleyici olayı işlevi tek iş parçacıklı çalışma zamanı bunları işleyebileceğinden daha hızlı ortaya çıktığında, çalışma zamanı içinde birden çok kez paralel işlevi çağırabilir.  Bir işlev uygulaması kullanıyorsanız [tüketim barındırma planı](functions-scale.md#how-the-consumption-plan-works), işlev uygulamasını otomatik olarak ölçeği.  Her işlev uygulaması örneğini uygulama tüketim planı veya normal barındırma çalıştığına [App Service barındırma planında](../app-service/overview-hosting-plans.md), birden çok iş parçacığı kullanarak paralel eş zamanlı işlev çağrılarını işleyebilir.  Eş zamanlı işlev çağrılarını her işlev uygulaması örnek sayısı kullanılan tetikleyici ve bunun yanı sıra diğer işlevleri içinde işlev uygulaması tarafından kullanılan kaynakları türüne göre değişir.

## <a name="functions-runtime-versioning"></a>İşlevler çalışma zamanı sürümü oluşturma

Kullanarak işlevler çalışma zamanı sürümünü yapılandırabilirsiniz `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı. Örneğin, "~ 2" değerini işlev uygulamanızı kendi ana sürüm 2.x kullanacağını gösterir. İşlev uygulamaları, yayınlandıkça her yeni ikincil sürümüne yükseltilir. İşlev uygulamanızı'nün tam sürümünü görüntüleme dahil olmak üzere daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md).

## <a name="repositories"></a>Depolar
Azure işlevleri kodu açık kaynaktır ve GitHub depolarında depolanan:

* [Azure işlevleri çalışma zamanı](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure işlevleri portalına](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure işlevleri şablonları](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure Web işleri SDK'sı](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bağlamalar
Tüm desteklenen bağlamaları tablosu aşağıdadır.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Bağlamaları çıkacak hatalarla sorun mu yaşıyorsunuz? Gözden geçirme [Azure işlevleri bağlama hata kodları](functions-bindings-error-pages.md) belgeleri.

## <a name="reporting-issues"></a>Raporlama konuları
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure işlevleri C# Geliştirici Başvurusu](functions-reference-csharp.md)
* [Azure işlevleri F# Geliştirici Başvurusu](functions-reference-fsharp.md)
* [Azure işlevleri NodeJS Geliştirici Başvurusu](functions-reference-node.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure işlevleri: Yolculuğu](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) üzerinde Azure App Service ekibi blogu. Azure işlevleri nasıl geliştirilmiştir geçmişi.

