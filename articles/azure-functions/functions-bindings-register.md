---
title: Azure işlevleri bağlama uzantılarını kaydetme
description: Ortamınıza bağlı olarak bir Azure işlevleri bağlama uzantısını kaydetmek öğrenin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 5969c3e0d270b45347f8132b2d655ba2e56cb2c0
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625888"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure işlevleri bağlama uzantılarını kaydetme

Azure işlevleri sürüm 2.x [bağlamaları](./functions-triggers-bindings.md) ayrı işlevler çalışma zamanı paketi olarak kullanılabilir. .NET işlevleri aracılığıyla NuGet paketlerinin bağlamaları erişirken, uzantı paketleri tüm bağlamaları bir yapılandırma ayarı ile diğer işlevler erişim sağlar.

Uzantıları bağlamayla ilgili aşağıdakileri göz önünde bulundurun:

- Bağlama uzantıları işlevleri açıkça kayıtlı olmayan Aşağıdakiler haricinde 1.x [oluşturma bir C# Visual Studio kullanarak bir sınıf kitaplığı](#local-csharp).

- HTTP ve Zamanlayıcı Tetikleyicileri, varsayılan olarak desteklenir ve bir uzantı gerekmez.

Aşağıdaki tabloda, ne zaman ve nasıl bağlamaları kaydetme gösterir.

| Geliştirme ortamı |Kayıt<br/> işlevlerde 1.x  |Kayıt<br/> işlevlerde 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Otomatik|Otomatik|
|.NET olmayan dil ya da yerel Azure Core araçlarını geliştirme|Otomatik|[Azure işlevleri çekirdek araçları ve uzantı paketleri kullanın](#extension-bundles)|
|C#Visual Studio kullanarak bir sınıf kitaplığı|[NuGet araçları kullanın](#vs)|[NuGet araçları kullanın](#vs)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullanma](#vs-code)|

## <a name="extension-bundles"></a>Yerel geliştirme için uzantı paketleri

Uzantı paketleri, uyumlu bir işlev uygulaması projenizi uzantılarını bağlama işlevler kümesi eklemenize olanak sağlayan sürüm 2.x çalışma zamanı için bir yerel geliştirme teknolojisidir. Azure'a dağıtırken bu uzantı paketleri dağıtım paketinde dahil edilir. Paketleri bir ayar aracılığıyla kullanılabilir olan Microsoft tarafından yayımlanan tüm bağlamaları yapar *host.json* dosya. Bir paket içinde tanımlanan uzantı paketleri, paketleri arasındaki çakışmaları önleme yardımcı olan birbiriyle uyumlu değildir. Ne zaman geliştirme yerel olarak en son sürümünü kullandığınızdan emin olun [Azure işlevleri çekirdek Araçları](functions-run-local.md#v2).

Uzantı paketleri, Azure işlevleri çekirdek araçları veya Visual Studio Code kullanarak tüm yerel geliştirme için kullanın.

Uzantı paketleri kullanmazsanız, .NET Core yükleyin, herhangi bir bağlama uzantısı yüklemeden önce yerel bilgisayarınızda 2.x SDK. Paketler, yerel geliştirme için bu gereksinimi kaldırır. 

Uzantı paketleri kullanmak için güncelleştirme *host.json* eklemek için şu girdiyi dosyaya `extensionBundle`:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

Aşağıdaki özellikler kullanılabilir `extensionBundle`:

| Özellik | Açıklama |
| -------- | ----------- |
| **`id`** | Microsoft Azure işlevleri uzantı paketleri ad alanı. |
| **`version`** | Yüklemek için Paket sürümü. İşlevler çalışma zamanı, her zaman aralığı veya sürüm aralığı tarafından tanımlanan en fazla izin verilen sürüm seçer. Yukarıdaki sürüm değeri kadar 1.0.0 ancak 2.0.0 hariç tüm paket sürümlerini sağlar. Daha fazla bilgi için [sürüm aralıklarını belirtmek için aralığı gösterimi](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). |

Paket sürümleri artırma paket değişikliği paketler. Genellikle bir değişiklik işlevler çalışma zamanı ana sürümü ile örtüşür bir ana sürüm paketleri paketteki artırılacak ana sürüm değişiklikleri ortaya çıkar.  

Geçerli varsayılan paket yüklü uzantıları kümesini bu listelenmiş [extensions.json dosya](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json).

<a name="local-csharp"></a>

## <a name="vs"></a> C\# Visual Studio'yla sınıf kitaplığı

İçinde **Visual Studio**, Paket Yöneticisi Konsolu'nu kullanarak paketleri yükleyebilirsiniz [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aşağıdaki örnekte gösterildiği gibi komut:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<TARGET_VERSION>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

Kullanırsanız `Install-Package` kullanın gerekmez bağlama başvurmak için [uzantı paketleri](#extension-bundles). Bu yaklaşım, Visual Studio'da yerleşik olarak sınıf kitaplıkları için özeldir.

## <a name="vs-code"></a> C# sınıf kitaplığı Visual Studio Code ile

> [!NOTE]
> Kullanmanızı öneririz [uzantı paketleri](#extension-bundles) uyumlu bir uzantı paketleri bağlama kümesi otomatik olarak yüklemeniz işlevleri sağlamak için.

İçinde **Visual Studio Code**, paketleri yüklemek bir C# sınıf kitaplığı projesi kullanarak komut istemi [dotnet paketini ekleyin](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) .NET Core CLI komutu. Aşağıdaki örnek, bir bağlama nasıl eklediğiniz gösterir:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

.NET Core CLI, yalnızca Azure işlevleri 2.x geliştirme için kullanılabilir.

Değiştirin `<BINDING_TYPE_NAME>` başvuru makalesinde, istenen bağlama için sağlanan paket adı. İstenen bağlama başvuru makalesinde bulabilirsiniz [desteklenen bağlamaları listesi](./functions-triggers-bindings.md#supported-bindings).

Değiştirin `<TARGET_VERSION>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure işlevi tetikleyici ve bağlama örneği](./functions-bindings-example.md)
