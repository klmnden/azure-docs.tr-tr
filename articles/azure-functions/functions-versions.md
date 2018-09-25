---
title: Azure işlevleri çalışma zamanı sürümleri genel bakış
description: Azure işlevleri çalışma zamanı birden çok sürümünü destekler. Bunları sizin için doğru olanı seçme arasındaki farkları öğrenin.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: glenga
ms.openlocfilehash: a601ea42549abad84d6cab5c429cf94147776436
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978633"
---
# <a name="azure-functions-runtime-versions-overview"></a>Azure işlevleri çalışma zamanı sürümleri genel bakış

 Azure işlevleri çalışma zamanı ana iki sürümü vardır: 1.x ve 2.x'i. Hem de üretim senaryolarında desteklenir ancak burada yeni özellik çalışmalarına ve geliştirmeler yapılmıştır geçerli 2.x sürümüdür.  İkisi arasındaki farklılıklar aşağıdaki ayrıntıları nasıl, her sürüm oluşturabilir ve 2.x'i 1.x ' yükseltme.

> [!NOTE] 
> Bu makalede, Azure işlevleri bulut hizmetine başvurur. Azure işlevleri şirket içi çalıştırmanıza olanak sağlayan bir önizleme ürünü hakkında daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanına genel bakış](functions-runtime-overview.md).

## <a name="creating-1x-apps"></a>1.x uygulamaları oluşturma

Bu en güncel sürümü ve yeni özellik Yatırımlar burada yaptığınız gibi Azure portalında oluşturduğunuz yeni uygulamalar için 2.x varsayılan olarak ayarlanır.  Ancak yine de, aşağıdakileri yaparak v1.x uygulamalar oluşturabilirsiniz.

1. Azure portalında bir Azure işlevi oluşturma
1. Oluşturulan uygulamayı açın ve boş iken açık **işlev ayarları**
1. ~ 2'den sürüm değiştirin ~ 1.  *İşlevler uygulamanız varsa bu iki durumlu devre dışı bırakılacak*.
1. Kaydet'e tıklayın ve uygulamayı yeniden başlatın.  Tüm şablonları artık oluşturun ve 1.x içinde çalıştırın.

## <a name="cross-platform-development"></a>Platformlar arası geliştirme

Çalışma zamanının 1.x destekler geliştirme ve yalnızca portalı veya Windows üzerinde barındırma. .NET Core 2, macOS ve Linux gibi .NET Core tarafından desteklenen tüm platformlarda çalıştırabilirsiniz anlamına çalışma zamanı 2.x çalışır. Bu platformlar arası geliştirme ve barındırma senaryolarında sağlar.

## <a name="languages"></a>Diller

Çalışma zamanı 2.x yeni bir dil genişletilebilirlik modeli kullanır. Ayrıca, araç kullanımı ve performansı artırmak için her uygulamayı 2.x yalnızca tek bir dilde işlevlerde sınırlıdır. Şu anda desteklenen dillerde 2.x C#, F #, JavaScript ve Java ' dir. Azure işlevleri 1.x Deneysel dillerden 2.x'i desteklenmez bu nedenle yeni modeli kullanmak için güncelleştirilmedi. Aşağıdaki tablo, her çalışma zamanı sürümünde desteklenen programlama dilleri belirtir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için [desteklenen diller](supported-languages.md).

## <a name="migrating-from-1x-to-2x"></a>2.x için 1.x geçiş

1.x 2.x için yazılmış mevcut bir uygulamaya taşımak isteyebilirsiniz.  Çoğu sürümler arasında taşıma gereken önemli noktalar (örneğin .NET Framework 4.7 ' .NET Core 2'ye taşıma #c) yukarıda listelenen dil çalışma zamanı değişiklikleri ilgilidir.  Kodunuzun emin olmanız gerekir ve kitaplıkları kullanılan dil çalışma zamanlarını ile uyumludur.  Ayrıca, herhangi bir değişiklik tetikleyici, bağlamalar ve aşağıda vurgulanan özellikleri not etmeyi unutmayın.

### <a name="changes-in-triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar değişiklikleri

Birçok tetikleyici ve bağlama özellikleri ve yapılandırmalar sürümleri arasında aynı kalsa da 2.x'i herhangi tetikleyicisi veya bağlaması yüklemek ihtiyacınız olacak uygulama. Bunun tek istisnası, HTTP ve Zamanlayıcı Tetikleyicileri ' dir.  Bkz: [kaydedin ve uzantılarını bağlama yükleme](./functions-triggers-bindings.md#register-binding-extensions).  Ayrıca olabilir bir not değişiklikler `function.json` veya sürüm arasındaki işlev özniteliklerini (örneğin, CosmosDB `connection` özelliği, artık `ConnectionStringSetting`).  Başvuru [varolan bağlama tablosu](#bindings) her bağlama için belgelere bağlantılar için.

### <a name="changes-in-features-available"></a>Kullanılabilir özellikler değişiklikleri

Diller ve bağlamalar değişikliklerin yanı sıra kaldırılmış, güncelleştirilmiş veya sürümleri arasında değiştirildi bazı özellikler mevcuttur.  1.x kullandıktan sonra 2.x ile başlangıç yapmak için ana konuları bazıları aşağıda verilmiştir.  İçinde'nın v2.x aşağıdaki değişiklikler yapılmıştır:

* Bir işlevi çağırmak için anahtarlar her zaman şifreli blob depolama alanında depolanır. 1.x varsayılan olarak, dosya depolama alanında olan ve yuvaları gibi özellikleri etkinleştirmek, blob taşınabilir.  Dosya depolama alanında 2.x ve gizli anahtarları için bir 1.x uygulama yükseltme şu anda olması durumunda bunlar sıfırlanır.
* Performansı artırmak için "Web kancası" türündeki Tetikleyiciler kaldırılır ve "HTTP" Tetikleyiciler ile değiştirilir.
* Ana Bilgisayar Yapılandırması (`host.json`) ya da boş olamaz veya içeremez olmalıdır `version` , `2.0` özelliklerden biri için.
* İzleme ve observability, Web işleri Panosu'nu geliştirmek için (`AzureWebJobsDashboard`) ile değiştirilir [Azure Application Insights](functions-monitoring.md) (`APPINSIGHTS_INSTRUMENTATIONKEY`)
* Uygulama ayarları (`local.settings.json`) özelliği için bir değer gerektirir `FUNCTIONS_WORKER_RUNTIME` uygulama diline eşler `dotnet | node | java | python`.
    * Ayak izi ve başlangıç süresini iyileştirmek için tek bir dil için uygulamalar sınırlıdır. Aynı çözüm için farklı dillerde işlevleri sağlamak için birden fazla uygulama yayımlayabilirsiniz.
* App service planı işlevleri için varsayılan zaman aşımını 30 dakikadır.  Bunu hala el ile sınırsız ayarlanabilir.
* [Sınırlamaları nedeniyle .NET core](https://github.com/Azure/azure-functions-host/issues/3414), `.fsx` F # işlevleri kaldırılmış için komut dosyaları. Derlenmiş F # işlevleri hala desteklenmektedir.
* Web kancası tabanlı tetikleyiciler (örneğin, Event Grid) biçimi değiştirildi `https://{app}/runtime/webhooks/{triggerName}`

### <a name="upgrading-a-locally-developed-application"></a>Yerel olarak geliştirilmiş bir uygulamayı yükseltme

V1.x uygulamanızı yerel olarak geliştirilmiştir, uygulama veya proje v2 ile uyumlu hale getirmek için değişiklik yapabilirsiniz.  Yeni uygulama ve bağlantı noktası, kodunuzu yeni App oluşturmak için önerilir.  Mevcut bir uygulamaya bir yerinde gerçekleştirmek için yapılması değişiklikler varken yükseltme, v1 ve v2, büyük olasılıkla eski kod yararlanarak değil arasındaki diğer iyileştirmeler vardır (örneğin, C# değişiklik gelen `TraceWriter` için `ILogger`) .  

Önerilen yolun başlangıç v2 şablonları birinden ve yeni bir proje veya uygulama kodu Taşı.

#### <a name="visual-studio-runtime-versions"></a>Visual Studio çalışma zamanı sürümleri

Visual Studio'da bir proje oluşturduğunuzda, çalışma zamanı sürümünü seçin.  Visual Studio, her iki ana sürümler için BITS var ve doğru bir proje için dinamik olarak kullanabilir.  Bu ayarlar türetilmiştir `.csproj` dosya.  1.x uygulamalar için proje özelliklerine sahiptir.

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

Proje Özellikleri v2'de

```xml
<TargetFramework>netstandard2.0</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Tıklayarak hata ayıklama veya yayımlama doğru sürüme ilişkin proje ayarlarını doğru şekilde ayarlayın.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code ve Azure işlevleri temel araçları

Yerel diğer araçları, Azure işlevleri çekirdek araçları kullanır.  Bu araçlar makineye yüklenir ve genellikle yalnızca bir sürümü geliştirme makinenizde aynı anda yüklenir.  Bkz: [yönergeleri araçlara belirli sürümlerini yüklemek nasıl](./functions-run-local.md).

VS Code için de kullanıcı ayarını güncelleştirmek ihtiyacınız `azureFunctions.projectRuntime` araçlarının yüklü sürümüyle eşleşecek şekilde.  Bu ayrıca şablonlar ve yeni uygulamalar oluşturma sırasında ortaya dilleri güncelleştirir.

### <a name="changing-version-of-apps-in-azure"></a>Azure'da uygulama sürümünü değiştirme

Yayımlanan uygulama sürümleri, uygulama ayarı aracılığıyla ayarlanır `FUNCTIONS_RUNTIME_VERSION`.  Bu ayar `~2` v2 uygulamaları için ve `~1` v1 uygulamaları için.  Ayrıca bu işlevlerin kodunu değiştirmeden yayımladığınız mevcut işlevleri olan bir uygulama çalışma zamanı sürümünü değiştirmek için kesinlikle önerilmez.  Yeni bir işlev uygulaması oluşturma ve uygun sürüme ayarlamak, değişiklikleri test etmek ve ardından devre dışı bırakın veya önceki uygulamayı silmek için önerilen yoldur bakın.

## <a name="bindings"></a>Bağlamalar 

Çalışma zamanı 2.x kullanan yeni bir [bağlama genişletilebilirlik modeli](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) , aşağıdaki avantajları sunar:

* Üçüncü taraf bağlama uzantıları için destek.
* Çalışma zamanı bağlamaları ve ayırma. Bu bağlama uzantıları tutulan ve bağımsız olarak yayımlanan olmasını sağlar. Örneğin, temel alınan bir SDK'ın daha yeni bir sürümüne bağımlı bir uzantı sürümünü yükseltmek için iyileştirilmiş olabilir.
* Bağlamaları kullanımda olduğu bilinen ve çalışma zamanı tarafından yüklenen bir açık yürütme ortamı.

Tüm yerleşik Azure işlevleri bağlamaları bu modeli benimseyen ve artık varsayılan olarak, Zamanlayıcı tetikleyicisi ve HTTP tetikleyicisi dışında yer alır. Bu uzantıları, Visual Studio gibi Araçlar üzerinden veya portal aracılığıyla işlevleri oluşturduğunuzda otomatik olarak yüklenir.

Aşağıdaki tabloda, hangi bağlamaları her çalışma zamanı sürümünde desteklendiğini gösterir.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
* [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md)
* [Sürüm notları](https://github.com/Azure/azure-functions-host/releases)
