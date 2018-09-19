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
ms.openlocfilehash: d97766b0a8c0df3b414d78f563406530f67c313b
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125383"
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
Bir işlev uygulaması birlikte Azure App Service tarafından yönetilen bir veya daha fazla tekil işlevler içerir. Tüm işlevlerin bir işlev uygulaması, aynı fiyatlandırma planı, sürekli dağıtım ve çalışma zamanı sürümü paylaşır. Birden çok dilde yazılmış işlevleri tüm işlev uygulamasının paylaşabilirsiniz. Bir işlev uygulaması düzenlemek ve topluca işlevlerinizi yönetmek için bir yol düşünün. 

## <a name="runtime-script-host-and-web-host"></a>Çalışma zamanı (komut dosyası ana bilgisayarı ve web ana bilgisayarı)
Çalışma zamanı veya komut dosyası ana bilgisayarı, olayları dinleyen, toplar ve veri gönderen ve sonuçta kodunuzun çalıştığı temel Web işleri SDK'sı ana bilgisayardır. 

HTTP Tetikleyicileri kolaylaştırmak için de mevcuttur, komut dosyası ana bilgisayarı üretim senaryolarında önünde sit için tasarlanmış bir web ana bilgisayarı. İki ana sahip web ana bilgisayar tarafından yönetilen trafiği ön betik konaktan ayırmaya yardımcı sonlandırın.

## <a name="folder-structure"></a>klasör yapısı
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bir işlev uygulaması ile Azure işlevleri dağıtmak için bir proje ayarlama, bu klasör yapısı site kodunuzu davranabilirsiniz. Kullanmanızı öneririz [dağıtım paketini](deployment-zip-push.md) projenizi işlevi uygulamanızı Azure'a dağıtmak için. Gibi mevcut araçları da kullanabilirsiniz [sürekli tümleştirme ve dağıtım](functions-continuous-deployment.md) ve Azure DevOps.

> [!NOTE]
> Dağıtılacak emin olun, `host.json` dosya ve klasörleri doğrudan işlev `wwwroot` klasör. Dahil etmezseniz `wwwroot` dağıtımlarınızı klasöründe. Aksi takdirde, elde edersiniz `wwwroot\wwwroot` klasörleri.

## <a id="fileupdate"></a> İşlev uygulaması dosyalarını güncelleştirme
Azure portalda yerleşik işlev Düzenleyicisi, güncelleştirmenizi sağlar *function.json* dosyası ve bir işlev için kod dosyası. Karşıya yükleme veya güncelleştirme gibi diğer dosyaları *package.json* veya *project.json* veya bağımlılıkları, diğer dağıtım yöntemlerini kullanmanız.

İşlev uygulamaları App Service üzerinde bu nedenle tüm yerleşik [standart web Apps'e dağıtım seçeneklerini](../app-service/app-service-deploy-local-git.md) işlev uygulamaları için kullanılabilir. Karşıya yükleme veya güncelleştirme işlevini uygulama dosyaları için kullanabileceğiniz bazı yöntemleri aşağıda verilmiştir. 

#### <a name="to-use-app-service-editor"></a>App Service Düzenleyicisi kullanmak için
1. Azure işlevleri portalında **Platform özellikleri**.
2. İçinde **geliştirme araçları** bölümünde **App Service Düzenleyicisi**.   
   App Service Düzenleyicisi yüklendikten sonra göreceğiniz *host.json* dosya ve işlev klasörlerinin *wwwroot*. 
5. Bunları, düzenlemek veya sürükleyip dosyaları karşıya yüklemek için geliştirme makinenizden dosyalarını açın.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>İşlevi uygulamanın SCM (Kudu) uç noktası kullanılacak
1. Gidin: `https://<function_app_name>.scm.azurewebsites.net`.
2. Tıklayın **konsol hata ayıklama > CMD**.
3. Gidin `D:\home\site\wwwroot\` güncelleştirilecek *host.json* veya `D:\home\site\wwwroot\<function_name>` işlevin dosyalarını güncelleştirmek için.
4. Sürükle ve bırak dosya kılavuzuna uygun klasöre karşıya yüklemek istediğiniz bir dosya. Bir dosya yere bırakabilirsiniz dosya kılavuzda iki alan vardır. İçin *.zip* dosyaları, etiketle bir kutusu görünür "sürükleyin buraya yükleyin ve sıkıştırmasını açın." Diğer dosya türlerinde, dosya kılavuzunda, ancak "sıkıştırmasını" kutusunu dışında bırakın.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="to-use-continuous-deployment"></a>Sürekli dağıtımı kullanmak için
Konu başlığı altındaki yönergeleri [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralel yürütme
Birden çok tetikleyici olayı işlevi tek iş parçacıklı çalışma zamanı bunları işleyebileceğinden daha hızlı ortaya çıktığında, çalışma zamanı içinde birden çok kez paralel işlevi çağırabilir.  Bir işlev uygulaması kullanıyorsanız [tüketim barındırma planı](functions-scale.md#how-the-consumption-plan-works), işlev uygulamasını otomatik olarak ölçeği.  Her işlev uygulaması örneğini uygulama tüketim planı veya normal barındırma çalıştığına [App Service barındırma planında](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), birden çok iş parçacığı kullanarak paralel eş zamanlı işlev çağrılarını işleyebilir.  Eş zamanlı işlev çağrılarını her işlev uygulaması örnek sayısı kullanılan tetikleyici ve bunun yanı sıra diğer işlevleri içinde işlev uygulaması tarafından kullanılan kaynakları türüne göre değişir.

## <a name="functions-runtime-versioning"></a>İşlevler çalışma zamanı sürümü oluşturma

Kullanarak işlevler çalışma zamanı sürümünü yapılandırabilirsiniz `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı. Örneğin, "~ 1" değeri işlev uygulamanızı kendi ana sürüm 1 kullanacağını belirtir. İşlev uygulamaları, yayınlandıkça her yeni ikincil sürümüne yükseltilir. İşlev uygulamanızı'nün tam sürümünü görüntüleme dahil olmak üzere daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md).

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
* [Azure işlevleri F # Geliştirici Başvurusu](functions-reference-fsharp.md)
* [Azure işlevleri NodeJS Geliştirici Başvurusu](functions-reference-node.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure işlevleri: Yolculuğu](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) üzerinde Azure App Service ekibi blogu. Azure işlevleri nasıl geliştirilmiştir geçmişi.

