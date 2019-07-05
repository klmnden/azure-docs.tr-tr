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
ms.date: 02/25/2019
ms.author: cshoe
ms.openlocfilehash: 88ffd6ec24ed19dd3b1e57277884c8759cdac1f9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480327"
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
|C#Visual Studio 2019 kullanarak sınıf kitaplığı|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2019)|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2019)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullanma](#c-class-library-with-visual-studio-code)|

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

## <a name="c-class-library-with-visual-studio-2019"></a>C\# ile Visual Studio 2019 sınıf kitaplığı

İçinde **Visual Studio 2019**, Paket Yöneticisi Konsolu'nu kullanarak paketleri yükleyebilirsiniz [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aşağıdaki örnekte gösterildiği gibi komut:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<TARGET_VERSION>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="c-class-library-with-visual-studio-code"></a>C# sınıf kitaplığı Visual Studio Code ile

> [!NOTE]
> Kullanmanızı öneririz [uzantı paketleri](#extension-bundles) uyumlu bir uzantı paketleri bağlama kümesi otomatik olarak yüklemeniz işlevleri sağlamak için.

İçinde **Visual Studio Code**, paketleri yüklemek bir C# sınıf kitaplığı projesi kullanarak komut istemi [dotnet paketini ekleyin](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) aşağıdaki örnekte gösterildiği gibi .NET Core CLI, komut:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <TARGET_VERSION>
```

.NET Core CLI, yalnızca Azure işlevleri 2.x geliştirme için kullanılabilir.

Kullanmak için belirli bir bağlama için paket adını bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<TARGET_VERSION>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure işlevi tetikleyici ve bağlama örneği](./functions-bindings-example.md)

