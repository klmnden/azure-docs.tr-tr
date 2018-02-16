---
title: "Azure işlevleri geliştirmek için Kılavuzu | Microsoft Docs"
description: "Azure işlevleri kavramlarını ve işlevlerinin Azure, tüm programlama dilleri ve bağlamaları geliştirmek için gereken teknikleri öğrenin."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: "Geliştirici Kılavuzu, azure işlevleri, İşlevler, olay işleme, Web kancalarını, dinamik işlem, sunucusuz mimarisi"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/12/2017
ms.author: tdykstra
ms.openlocfilehash: 461557b415ec816860acb5308e7aeba34468f4ae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-functions-developers-guide"></a>Azure işlevleri Geliştirici Kılavuzu
Azure işlevleri, belirli işlevleri birkaç temel teknik kavramlar ve bileşenler, dil veya kullandığınız bağlama bağımsız olarak paylaşın. Belirtilen dil ya da bağlama belirli Ayrıntılar öğrenme moduna geçmek önce bunların tümüne uygulanır Bu genel bakışta aracılığıyla okuduğunuzdan emin olun.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevlerine genel bakış](functions-overview.md) ve bilginiz [WebJobs SDK'sı kavramları Tetikleyicileri ve bağlamaları JobHost çalışma zamanı gibi](https://github.com/Azure/azure-webjobs-sdk/wiki). Azure işlevleri WebJobs SDK dayanır. 

## <a name="function-code"></a>İşlev kodu
A *işlevi* Azure işlevlerinde birincil kavramdır. Kod bir işlev için tercih ettiğiniz dilde yazmak ve kod ve yapılandırma dosyaları aynı klasöre kaydedin. Yapılandırma adlı `function.json`, JSON yapılandırma verilerini içerir. Çeşitli diller desteklenir ve her birinin bu dil için en iyi şekilde çalışması için en iyi duruma getirilmiş biraz farklı bir deneyimle vardır. 

Function.json dosyası, işlev bağlamaları ve diğer yapılandırma ayarlarını tanımlar. Çalışma zamanı izlenecek olaylar belirlemek için bu dosyaya ve verilerini geçirin ve işlev yürütülmesini veri dönmek nasıl kullanır. Function.json dosyası örneği verilmiştir.

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

Ayarlama `disabled` özelliğine `true` işlevi çalıştırılmasını engellemek için.

`bindings` Özelliktir burada Tetikleyicileri ve bağlamaları yapılandırın. Her bağlama birkaç genel ayarları ve belirli bir bağlama türüne özgü bazı ayarlar paylaşır. Her bağlama aşağıdaki ayarları gerektirir:

| Özellik | Değerleri/türleri | Yorumlar |
| --- | --- | --- |
| `type` |string |Bağlama türü. Örneğin, `queueTrigger`. |
| `direction` |'in', 'out' |Bağlama işlevdeki veri alma veya işlevinden veri göndermek için olup olmadığını gösterir. |
| `name` |string |Bağlı veri işlevinde için kullanılan ad. C# ' ta bir bağımsız değişken adı budur; JavaScript için bir anahtar/değer listesinde anahtardır. |

## <a name="function-app"></a>İşlev uygulaması
Bir işlev uygulaması birlikte Azure App Service tarafından yönetilen bir veya daha fazla tekil işlevler oluşur. Bir işlev uygulaması işlevlerde tümünün aynı fiyatlandırma planı, sürekli dağıtımı ve çalışma zamanı sürümü paylaşır. Çeşitli dillerde yazılmış işlevleri tüm aynı işlev uygulaması paylaşabilirsiniz. Bir işlev uygulaması düzenlemek ve topluca işlevlerinizi yönetmek için bir yol olarak düşünün. 

## <a name="runtime-script-host-and-web-host"></a>Çalışma zamanı (komut dosyası ana bilgisayarı ve web ana bilgisayarı)
Çalışma zamanı ya da komut dosyası ana bilgisayarı, olaylarını dinler, toplar ve verileri gönderir ve sonuçta kodunuzun çalıştığı temel Web işleri SDK'si ana bilgisayardır. 

HTTP Tetikleyicileri kolaylaştırmak için de bulunmaktadır komut dosyası ana bilgisayarı üretim senaryolarında önünde sit için tasarlanmış bir web ana bilgisayarı. İki ana sahip komut dosyası ana bilgisayarı Önden ayırmaya yardımcı web ana bilgisayar tarafından yönetilen trafiği sonlandırın.

## <a name="folder-structure"></a>Klasör yapısı
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Azure App Service'te bir işlev uygulaması için işlevleri dağıtmak için bir proje ayarlama, bu klasör yapısı, site kodu olarak davranabilirsiniz. Sürekli tümleştirme ve dağıtım gibi mevcut araçlar kullanabilir veya özel dağıtım yapmak için komut dosyaları zaman paket yükleme dağıtmak veya transpilation kod.

> [!NOTE]
> Dağıtmak emin olun, `host.json` dosya ve klasörleri doğrudan işlev `wwwroot` klasör. İçermeyen `wwwroot` dağıtımlarınızı klasöründe. Aksi takdirde, şunun `wwwroot\wwwroot` klasörler. 
> 
> 

## <a id="fileupdate"></a>İşlev uygulama dosyaları güncelleştirme
Azure portalda yerleşik işlevi Düzenleyicisi güncelleştirmenizi sağlayan *function.json* dosyası ve bir işlev için kod dosyası. Karşıya yükleme veya güncelleştirme gibi diğer dosyaları *package.json* veya *project.json* veya bağımlılıkları, zorunda diğer dağıtım yöntemleri kullanabilirsiniz.

İşlev uygulamalarının, uygulama hizmeti, bu nedenle tüm yerleşiktir [standart web uygulamaları için dağıtım seçenekleri](../app-service/app-service-deploy-local-git.md) de işlevi uygulamaları için kullanılabilir. Karşıya yükleme veya işlevi uygulama dosyalarını güncelleştirmek için kullanabileceğiniz bazı yöntemler şunlardır. 

#### <a name="to-use-app-service-editor"></a>Uygulama hizmeti Düzenleyicisi'ni kullanmak için
1. Azure işlevleri Portalı'nda tıklatın **Platform özellikleri**.
2. İçinde **geliştirme araçları** 'yi tıklatın **App Service Düzenleyicisi**.   
   Uygulama hizmeti Düzenleyicisi yüklendikten sonra göreceğiniz *host.json* altındaki dosya ve işlev klasörleri *wwwroot*. 
5. Bunları, düzenlemek veya sürükleyip dosyaları karşıya yüklemek için geliştirme makinenizden dosyalarını açın.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>İşlev uygulamanın SCM (Kudu) uç noktası kullanmak için
1. Gidin: `https://<function_app_name>.scm.azurewebsites.net`.
2. Tıklatın **Debug konsol > CMD**.
3. Gidin `D:\home\site\wwwroot\` güncelleştirmek için *host.json* veya `D:\home\site\wwwroot\<function_name>` bir işlevin dosyaları güncelleştirmek için.
4. Sürükle ve bırak dosya kılavuzundaki uygun klasöre yüklemek istediğiniz bir dosya. Dosya burada bırakamazsınız dosya kılavuzda iki alan vardır. İçin *.zip* dosyaları, etiketle bir kutu görünür "sürükleyin burada karşıya yükleyin ve sıkıştırmasını açın." Diğer dosya türlerinde, dosya kılavuzunda ancak "sıkıştırmasını" kutusunu dışında bırakın.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="to-use-continuous-deployment"></a>Sürekli dağıtım kullanmak için
Konudaki yönergeleri izleyerek [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralel yürütme
Birden çok tetikleyici olaylar tek iş parçacıklı işlevi çalışma zamanı bunları işleyebileceğinden daha hızlı gerçekleştiğinde, çalışma zamanı işlevinde birden çok kez paralel çağırabilir.  Bir işlev uygulaması kullanıyorsanız [barındırma planı tüketim](functions-scale.md#how-the-consumption-plan-works), işlev uygulaması otomatik olarak ölçeğini.  Her bir işlev uygulaması örneği olup olmadığını barındırma planı veya bir normal tüketimi'de uygulama'yı çalıştıran [App Service barındırma planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), eşzamanlı işlev çağrılarını birden çok iş parçacığı kullanma paralel işlem.  En fazla eşzamanlı işlev çağrılarını her işlevi app örneğindeki türü, işlev uygulaması içinde diğer işlevleri tarafından kullanılan kaynakları yanı sıra kullanılan tetikleyici göre değişir.

## <a name="functions-runtime-versioning"></a>İşlevler çalışma zamanı sürüm oluşturma

Çalışma zamanı işlevleri kullanarak sürümünü yapılandırabilirsiniz `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı. Örneğin, "~ 1" değeri, işlev uygulaması kendi ana sürüm 1 kullanacağını gösterir. İşlev uygulamalarının en yeni her ikincil sürüme yükseltilir. İşlev uygulamanızı'nün tam sürümünü görüntülemek nasıl dahil olmak üzere daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı sürümlerini hedefleyen nasıl](set-runtime-version.md).

## <a name="repositories"></a>Depolar
Azure işlevleri için kod açık bir kaynaktır ve GitHub depolarının depolanır:

* [Azure işlevleri çalışma zamanı](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure işlevleri portalına](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure işlevleri şablonları](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure Web işleri SDK'si](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure Web işleri SDK'si uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bağlamalar
Aşağıda, tüm desteklenen bağlamaları tablosu verilmiştir.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Bağlantılardan gelen hatalarla sorunları sorun mu yaşıyorsunuz? Gözden geçirme [Azure işlevleri bağlama hata kodları](functions-bindings-error-pages.md) belgeleri.

## <a name="reporting-issues"></a>Raporlama konuları
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure işlevleri C# Geliştirici Başvurusu](functions-reference-csharp.md)
* [Azure işlevleri F # Geliştirici Başvurusu](functions-reference-fsharp.md)
* [Azure işlevleri NodeJS Geliştirici Başvurusu](functions-reference-node.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure işlevleri: Gezisine](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) Azure App Service ekip blogunda. Azure işlevlerinin nasıl geliştirilmiştir geçmişi.

