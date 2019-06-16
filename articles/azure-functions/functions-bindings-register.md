---
title: Azure işlevleri bağlama uzantılarını kaydetme
description: Ortamınıza bağlı olarak bir Azure işlevleri bağlama uzantısını kaydetmek öğrenin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/25/2019
ms.author: cshoe
ms.openlocfilehash: 53eb5fc9389d913ecacec3729a06e47a1c2bf56b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65864540"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure işlevleri bağlama uzantılarını kaydetme

Azure işlevleri sürüm 2.x [bağlamaları](./functions-triggers-bindings.md) ayrı işlevler çalışma zamanı paketi olarak kullanılabilir. .NET işlevleri aracılığıyla NuGet paketlerinin bağlamaları erişirken, uzantı paketleri tüm bağlamaları bir yapılandırma ayarı ile diğer işlevler erişim sağlar.

Uzantıları bağlamayla ilgili aşağıdakileri göz önünde bulundurun:

- Bağlama uzantıları işlevleri açıkça kayıtlı olmayan Aşağıdakiler haricinde 1.x [oluşturma bir C# sınıf kitaplığı kullanarak Visual Studio 2019](#local-csharp).

- HTTP ve Zamanlayıcı Tetikleyicileri, varsayılan olarak desteklenir ve bir uzantı gerekmez.

Aşağıdaki tabloda, ne zaman ve nasıl bağlamaları kaydetme gösterir.

| Geliştirme ortamı |Kayıt<br/> işlevlerde 1.x  |Kayıt<br/> işlevlerde 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Otomatik|Otomatik|
|.NET olmayan dil ya da yerel Azure Core araçlarını geliştirme|Otomatik|[Azure işlevleri çekirdek araçları ve uzantı paketleri kullanın](#local-development-with-azure-functions-core-tools-and-extension-bundles)|
|C#Visual Studio 2019 kullanarak sınıf kitaplığı|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2019)|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2019)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullanma](#c-class-library-with-visual-studio-code)|

## <a name="local-development-with-azure-functions-core-tools-and-extension-bundles"></a>Azure işlevleri çekirdek araçları ve uzantı paketleri ile yerel geliştirme

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2019"></a>C#Visual Studio 2019 ile sınıf kitaplığı

İçinde **Visual Studio 2019**, Paket Yöneticisi Konsolu'nu kullanarak paketleri yükleyebilirsiniz [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aşağıdaki örnekte gösterildiği gibi komut:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<TARGET_VERSION>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="c-class-library-with-visual-studio-code"></a>C# sınıf kitaplığı Visual Studio Code ile

İçinde **Visual Studio Code**, komut istemi kullanarak paketleri yükleyebilirsiniz [dotnet paketini ekleyin](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) aşağıdaki örnekte gösterildiği gibi .NET Core CLI, komut:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <TARGET_VERSION>
```

.NET Core CLI, yalnızca Azure işlevleri 2.x geliştirme için kullanılabilir.

Kullanmak için belirli bir bağlama için paket adını bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<TARGET_VERSION>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure işlevi tetikleyici ve bağlama örneği](./functions-bindings-example.md)

