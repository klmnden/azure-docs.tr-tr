---
title: Azure işlevleri çalışma zamanı sürümleri genel bakış
description: Azure İşlevleri, birden fazla çalışma zamanı sürümünü destekler. Bunları sizin için doğru olanı seçme arasındaki farkları öğrenin.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: glenga
ms.openlocfilehash: 6988fb547b07f81891efea3caad8bf34f4c8a476
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036323"
---
# <a name="azure-functions-runtime-versions-overview"></a>Azure işlevleri çalışma zamanı sürümleri genel bakış

 Azure işlevleri çalışma zamanı ana iki sürümü vardır: 1.x ve 2.x'i. Hem de üretim senaryolarında desteklenir ancak burada yeni özellik çalışmalarına ve geliştirmeler yapılmıştır geçerli 2.x sürümüdür.  İkisi arasındaki farklılıklar aşağıdaki ayrıntıları nasıl, her sürüm oluşturabilir ve 2.x'i 1.x ' yükseltme.

> [!NOTE]
> Bu makalede, Azure işlevleri bulut hizmetine başvurur. Azure işlevleri şirket içi çalıştırmanıza olanak sağlayan bir önizleme ürünü hakkında daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanına genel bakış](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Platformlar arası geliştirme

Sürüm 2.x çalışma zamanı, macOS ve Linux gibi .NET Core tarafından desteklenen tüm platformlarda çalışmasını sağlayan .NET Core 2 üzerinde çalışır. .NET Core üzerinde çalışan platformlar arası geliştirme ve barındırma senaryolarında sağlar.

Buna karşılık olarak sürüm 1.x çalışma zamanı yalnızca geliştirme ve Azure portalında veya Windows bilgisayarlarda barındırma destekler.

## <a name="languages"></a>Languages

Sürüm 2.x çalışma zamanı, yeni bir dil genişletilebilirlik modeli kullanır. Sürüm 2.x, bir işlev uygulamasında tüm işlevleri aynı dilde paylaşım gerekir. Bir işlev uygulaması işlevlerinde dili uygulama oluşturulurken seçilir.

Azure işlevleri 1.x Deneysel dillerden bunlar 2.x'i desteklenmez. Bu nedenle yeni modeli kullanmak için güncelleştirilmez. Aşağıdaki tablo, hangi programlama dilleri içinde her çalışma zamanı sürümü şu anda desteklenen belirtir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için bkz. [Desteklenen diller](supported-languages.md).

## <a name="creating-1x-apps"></a>Sürümünde çalışmasını 1.x

Varsayılan olarak, Azure portalında oluşturulan işlev uygulamaları için sürüm ayarlanır 2.x. Mümkün olduğunda, yeni özellik Yatırımlar burada yapılan bu çalışma zamanı sürümünü kullanmanız gerekir. Gerek duyarsanız, bir işlev uygulaması sürüm 1.x çalışma zamanı üzerinde çalıştırabilirsiniz. İşlev uygulamanızı oluşturduktan sonra ancak tüm işlevleri eklemeden önce yalnızca çalışma zamanı sürümünü değiştirebilirsiniz. 1.x çalışma zamanı sürümüne sabitleme öğrenmek için bkz. [görüntüleyin ve güncelleştirme geçerli çalışma zamanı sürümünü](set-runtime-version.md#view-and-update-the-current-runtime-version).

## <a name="migrating-from-1x-to-2x"></a>2.x için 1.x geçiş

Bunun yerine sürümünü kullanmak için sürüm 1.x çalışma zamanı kullanmak için yazılmış var olan bir uygulamayı geçirmek seçebilirsiniz 2.x. Yapmanız gereken değişiklikleri çoğu .NET Framework 4.7 ile .NET Core 2 arasındaki C# API değişiklikleri gibi dil çalışma zamanı değişiklikleri ilgilidir. Ayrıca kodunuzun emin olmanız gerekir ve kitaplıkları seçtiğiniz dil çalışma zamanı ile uyumludur. Son olarak, herhangi bir değişiklik tetikleyici, bağlamalar ve aşağıda vurgulanan özellikleri not etmeyi unutmayın. En iyi geçiş sonuçlar için sürüm için yeni bir işlev uygulaması oluşturmanız gerekir, mevcut sürümü 1.x işlevi yeni uygulamasına kod 2.x ve bağlantı noktası.  

### <a name="changes-in-triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar değişiklikleri

Sürüm 2.x belirli Tetikleyicileri ve bağlamaları işlevler uygulamanız tarafından kullanılan uzantıları yüklemek gerektirir. Bir uzantı gerektirmeyen bu HTTP ve Zamanlayıcı Tetikleyicileri için tek özel durum.  Daha fazla bilgi için [kaydedin ve uzantılarını bağlama yükleme](./functions-bindings-register.md).

Ayrıca bazı değişiklikleri de olmuştur `function.json` veya işlevin sürümleri arasında öznitelikleri. Örneğin, olay hub'ı `path` özelliği, artık `eventHubName`. Bkz: [varolan bağlama tablosu](#bindings) her bağlama için belgelere bağlantılar için.

### <a name="changes-in-features-and-functionality"></a>Özellikler ve işlevsellik değişiklikler

Ayrıca kaldırılmış olan bazı özellikleri güncelleştirilmiş veya yeni bir sürümde değiştirildi. Bu bölümde sürümünde gördüğünüz değişiklikleri ayrıntıları 2.x sürümü kullandıktan sonra 1.x.

Sürüm 2.x, aşağıdaki değişiklikler yapıldı:

* HTTP uç noktalarını çağırma anahtarlar her zaman şifreli Azure Blob Depolama alanında depolanır. Sürümünde 1.x, anahtarları depolanmış Azure dosya depolama, varsayılan olabilir. Bir uygulama sürümüne yükseltirken 1.x sürümüne 2.x, dosya depolama alanında olan mevcut gizli dizileri sıfırlanır.

* Sürüm 2.x çalışma zamanı, Web kancası sağlayıcıları için yerleşik destek içermez. Performansı artırmak için bu değişiklik yapılmıştır. HTTP Tetikleyicileri uç noktaları için Web kancaları kullanmaya devam edebilirsiniz.

* Ana bilgisayar yapılandırma dosyası (host.json) boş olabilir veya dize sahip `"version": "2.0"`.

* İzleme, Web işleri Panosu'nu kullanılan portalında geliştirmek için [ `AzureWebJobsDashboard` ](functions-app-settings.md#azurewebjobsdashboard) ayarı, Azure kullanan uygulamanızla, değiştirilir [ `APPINSIGHTS_INSTRUMENTATIONKEY` ](functions-app-settings.md#appinsights_instrumentationkey) ayarı. Daha fazla bilgi için [İzleyici Azure işlevleri](functions-monitoring.md).

* Bir işlev uygulamasında tüm işlevleri aynı dilde paylaşmanız gerekir. Bir işlev uygulaması oluşturduğunuzda, uygulama için çalışma zamanı yığını seçmeniz gerekir. Çalışma zamanı yığını tarafından belirtilen [ `FUNCTIONS_WORKER_RUNTIME` ](functions-app-settings.md#functions_worker_runtime) uygulama ayarlarındaki değer. Bu gereksinim, Ayak izi ve başlangıç süresini kısaltmak için eklendi. Yerel olarak geliştirirken, bu ayarda de dahil etmelisiniz [local.settings.json dosyasında](functions-run-local.md#local-settings-file).

* App Service planı işlevleri için varsayılan zaman aşımını 30 dakika ile değiştirilir. El ile zaman aşımı sınırsız kullanarak değiştirebileceğiniz [functionTimeout](functions-host-json.md#functiontimeout) host.json ayarlama.

* Örnek başına 100 eş zamanlı isteklerin varsayılan tüketim planı işlevleri için varsayılan olarak HTTP eşzamanlılık kısıtlamalar uygulanır. Bu konuda değiştirebilirsiniz [ `maxConcurrentRequests` ](functions-host-json.md#http) host.json dosyasında ayarı.

* Nedeniyle [.NET core sınırlamaları](https://github.com/Azure/azure-functions-host/issues/3414), desteği F# betik (.fsx) işlevleri kaldırıldı. Derlenmiş F# işlevleri (.fs) hala destekleniyor.

* Olay Kılavuzu tetikleyicisi Web kancaları URL biçimi değiştirildi `https://{app}/runtime/webhooks/{triggerName}`.

### <a name="migrating-a-locally-developed-application"></a>Yerel olarak geliştirilmiş bir uygulamayı geçirme

İşlev uygulaması projeleri yerel olarak sürüm 1.x çalışma zamanı kullanılarak geliştirilen mevcut olabilir. Sürümüne yükseltmek için 2.x sürümüne karşı yerel işlev uygulaması projesi oluşturmak, varolan kod yeni bir uygulamaya 2.x ve bağlantı noktası. El ile güncelleştirmeniz mevcut proje ve kod, "yerinde" Yükseltme bir tür. Ancak, diğer iyileştirmeler sürümü arasında vardır 1.x ve sürümü hala yapmanız gerekebilir 2.x. Örneğin, C# hata ayıklama nesnesi değiştirildi `TraceWriter` için `ILogger`. Yeni bir sürüm 2.x projesi oluşturarak, güncelleştirilmiş işlevler en son sürüm 2.x şablonlar temelinde ile başlayın.

#### <a name="visual-studio-runtime-versions"></a>Visual Studio çalışma zamanı sürümleri

Visual Studio'da bir proje oluşturduğunuzda, çalışma zamanı sürümünü seçin. Visual Studio için Azure işlevleri araçları iki ana çalışma zamanı sürümünü destekler. Hata ayıklama ve yayımlama proje ayarlarına göre doğru sürümü kullanılır. Sürüm ayarlarını tanımlanan `.csproj` dosyasında aşağıdaki özellikleri:

##### <a name="version-1x"></a>Sürüm 1.x

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

##### <a name="version-2x"></a>Sürüm 2.x

```xml
<TargetFramework>netcoreapp2.2</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Hata ayıklama veya projenizi yayımlamak, doğru çalışma zamanı sürümü kullanılır.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code ve Azure işlevleri temel araçları

[Azure işlevleri temel araçları](functions-run-local.md) komut satırı geliştirme için ve ayrıca tarafından kullanılan [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) Visual Studio Code için. Sürüm karşı geliştirmek için 2.x, yükleme sürüm 2.x çekirdek araçlar. Sürüm 1.x geliştirme sürümünü gerektirir 1.x temel araçlar. Daha fazla bilgi için [Azure Functions Core Tools'u yükleme](functions-run-local.md#install-the-azure-functions-core-tools).

Visual Studio Code geliştirme için Ayrıca kullanıcı ayarını güncelleştirmeniz gerekebilir `azureFunctions.projectRuntime` araçlarının yüklü sürümüyle eşleşecek şekilde.  Bu ayar, ayrıca şablonları ve işlev uygulaması oluşturma sırasında kullanılan dilleri güncelleştirir.

### <a name="changing-version-of-apps-in-azure"></a>Azure'da uygulama sürümünü değiştirme

Azure'da yayımlanan uygulamalar tarafından kullanılan işlevler çalışma zamanı sürümü tarafından dikte [ `FUNCTIONS_EXTENSION_VERSION` ](functions-app-settings.md#functions_extension_version) uygulama ayarı. Değerini `~2` sürüm 2.x çalışma zamanını hedefleyen ve `~1` sürüm 1.x çalışma zamanını hedefleyen. Rasgele diğer uygulama ayarı değişikliklerini ve kod değişikliklerini işlevlerinizi büyük olasılıkla gerekli olduğu için bu ayarı değiştirmeyin. İşlev uygulamanızın farklı çalışma zamanı sürümüne geçirmek için önerilen yöntem hakkında bilgi edinmek için bkz. [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md).

## <a name="bindings"></a>Bağlamalar

Yeni bir sürüm 2.x çalışma zamanı kullanan [bağlama genişletilebilirlik modeli](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) , aşağıdaki avantajları sunar:

* Üçüncü taraf bağlama uzantıları için destek.

* Çalışma zamanı bağlamaları ve ayırma. Bu değişiklik, bağlama uzantıları tutulan ve bağımsız olarak yayımlanan olmasını sağlar. Örneğin, temel alınan bir SDK'ın daha yeni bir sürümüne bağımlı bir uzantı sürümünü yükseltmek için iyileştirilmiş olabilir.

* Bağlamaları kullanımda olduğu bilinen ve çalışma zamanı tarafından yüklenen bir açık yürütme ortamı.

HTTP ve Zamanlayıcı Tetikleyicileri hariç tüm bağlamaları gerekir açıkça işlevi uygulaması projesine eklemek veya Portalı'nda kayıtlı. Daha fazla bilgi için [kaydetme bağlama uzantıları](./functions-bindings-expressions-patterns.md).

Aşağıdaki tablo, hangi bağlamaları her çalışma zamanı sürümünde desteklenen gösterir.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
* [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md)
* [Sürüm notları](https://github.com/Azure/azure-functions-host/releases)
