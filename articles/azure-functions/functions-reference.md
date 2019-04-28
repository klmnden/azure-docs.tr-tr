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
ms.openlocfilehash: 5b2b7f3cd6bfa219b794edc63d6bf8b2784b713c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62120747"
---
# <a name="azure-functions-developers-guide"></a>Azure işlevleri Geliştirici Kılavuzu
Azure işlevleri'nde belirli işlevleri birkaç temel teknik kavramlar ve bileşenler, dil veya kullandığınız bağlama bağımsız olarak paylaşın. Belirtilen dil veya bağlama için belirli ayrıntıları öğrenme moduna kullanmaya başlamadan önce bunların tümüne uygulanan bu genel bakışta aracılığıyla okuduğunuzdan emin olun.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevlerine genel bakış](functions-overview.md).

## <a name="function-code"></a>İşlev kodu
A *işlevi* Azure işlevleri'nde birincil kavramdır. Bir işlev, iki önemli parçaları - çeşitli dillerde yazılmış kodunuzu ve bazı yapılandırma function.json dosyası içerir. Derlenmiş diller için bu yapılandırma dosyası ek açıklamalar, kod içinde otomatik olarak oluşturulur. Komut dosyası dilleri için yapılandırma dosyasını kendiniz sağlamalısınız.

Function.json dosyası, işlevin tetikleyici, bağlamalar ve diğer yapılandırma ayarlarını tanımlar. Her işlev bir ve yalnızca bir tetikleyicisine sahiptir. Çalışma zamanı izlenecek olaylar belirlemek için bu yapılandırma dosyası ve verileri aktarmak ve bir işlevin yürütülmesini dönüş verileri nasıl kullanır. Function.json dosyası örneği verilmiştir.

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

`bindings` Özelliği hem Tetikleyicileri ve bağlamaları yapılandırdığınız. Her bağlama, birkaç ortak ayarları ve belirli bir bağlama türüne özgü bazı ayarlar paylaşır. Her bağlamanın, aşağıdaki ayarları gerektirir:

| Özellik | Değerler/türleri | Yorumlar |
| --- | --- | --- |
| `type` |string |Bağlama türü. Örneğin, `queueTrigger`. |
| `direction` |'de,' 'out' |Bağlama işlevdeki veri alma veya işlevden veri göndermek için uygun olup olmadığını gösterir. |
| `name` |string |İşlevde bağlı veriler için kullanılan ad. C# için bu bir bağımsız değişken adıdır; JavaScript için bir anahtar/değer listesinde anahtardır. |

## <a name="function-app"></a>İşlev uygulaması
Bir işlev uygulaması, işlevlerinizin çalıştığı azure'da bir yürütme bağlamı sağlar. Bir işlev uygulaması yönetilen, dağıtılan ve birlikte ölçeği genişletilmiş bir veya daha fazla bireysel işlevleri oluşur. Tüm işlevlerin bir işlev uygulaması, aynı fiyatlandırma planı, sürekli dağıtım ve çalışma zamanı sürümü paylaşır. Bir işlev uygulaması düzenlemek ve topluca işlevlerinizi yönetmek için bir yol düşünün. 

> [!NOTE]
> Bir işlev uygulamasında tüm işlevleri aynı dilde yazılmış olabilir. İçinde [önceki sürümler](functions-versions.md) Azure işlevleri çalışma zamanı, bu gerekli değildi.

## <a name="folder-structure"></a>klasör yapısı
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Yukarıdaki varsayılan (ve önerilen) bir işlev uygulaması için klasör yapısı. Bir işlevin kodunu dosya konumunu değiştirmek isterseniz değiştirme `scriptFile` bölümünü _function.json_ dosya. Ayrıca kullanmanızı öneririz [dağıtım paketini](deployment-zip-push.md) projenizi işlevi uygulamanızı Azure'a dağıtmak için. Gibi mevcut araçları da kullanabilirsiniz [sürekli tümleştirme ve dağıtım](functions-continuous-deployment.md) ve Azure DevOps.

> [!NOTE]
> Bir paket el ile dağıtma, dağıtılacak emin olun, _host.json_ dosya ve klasörleri doğrudan işlev `wwwroot` klasör. Dahil etmezseniz `wwwroot` dağıtımlarınızı klasöründe. Aksi takdirde, elde edersiniz `wwwroot\wwwroot` klasörleri.

#### <a name="use-local-tools-and-publishing"></a>Yerel araçlarını kullanın ve yayımlama
İşlev uygulamaları yazılabilir ve araçlar da dahil olmak üzere, çeşitli kullanılarak yayımlanan [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [Intellij](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md)ve [Azure işlevleri çekirdek Araçları](./functions-develop-local.md). Daha fazla bilgi için [kod ve test, Azure işlevleri yerel olarak](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

## <a id="fileupdate"></a> Azure portalında işlevler düzenleme
Azure portalda yerleşik işlevler Düzenleyicisi, kodunuzu güncelleştirmenizi sağlar ve *function.json* doğrudan satır içi dosya. VS Code gibi bir yerel geliştirme aracını kullanmak için en iyi - Bu, yalnızca küçük değişiklikler veya kavram kanıtları için önerilir.

## <a name="parallel-execution"></a>Paralel yürütme
Birden çok tetikleyici olayı işlevi tek iş parçacıklı çalışma zamanı bunları işleyebileceğinden daha hızlı ortaya çıktığında, çalışma zamanı içinde birden çok kez paralel işlevi çağırabilir.  Bir işlev uygulaması kullanıyorsanız [tüketim barındırma planı](functions-scale.md#how-the-consumption-and-premium-plans-work), işlev uygulamasını otomatik olarak ölçeği.  Her işlev uygulaması örneğini uygulama tüketim planı veya normal barındırma çalıştığına [App Service barındırma planında](../app-service/overview-hosting-plans.md), birden çok iş parçacığı kullanarak paralel eş zamanlı işlev çağrılarını işleyebilir.  Eş zamanlı işlev çağrılarını her işlev uygulaması örnek sayısı kullanılan tetikleyici ve bunun yanı sıra diğer işlevleri içinde işlev uygulaması tarafından kullanılan kaynakları türüne göre değişir.

## <a name="functions-runtime-versioning"></a>İşlevler çalışma zamanı sürümü oluşturma

Kullanarak işlevler çalışma zamanı sürümünü yapılandırabilirsiniz `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı. Örneğin, "~ 2" değerini işlev uygulamanızı kendi ana sürüm 2.x kullanacağını gösterir. İşlev uygulamaları, yayınlandıkça her yeni ikincil sürümüne yükseltilir. İşlev uygulamanızı'nün tam sürümünü görüntüleme dahil olmak üzere daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md).

## <a name="repositories"></a>Depolar
Azure işlevleri kodu açık kaynaktır ve GitHub depolarında depolanan:

* [Azure İşlevleri](https://github.com/Azure/Azure-Functions)
* [Azure işlevleri konak](https://github.com/Azure/azure-functions-host/)
* [Azure işlevleri portalına](https://github.com/azure/azure-functions-ux)
* [Azure işlevleri şablonları](https://github.com/azure/azure-functions-templates)
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

* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](./functions-develop-local.md)
* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure işlevleri C# Geliştirici Başvurusu](functions-reference-csharp.md)
* [Azure işlevleri NodeJS Geliştirici Başvurusu](functions-reference-node.md)
