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
origin.date: 02/18/2019
ms.date: 04/26/2019
ms.author: v-junlch
ms.openlocfilehash: 5534086d5754691f650370e465fa2c63210e0dc7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61437863"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure işlevleri bağlama uzantılarını kaydetme

Azure işlevleri HTTP ve Zamanlayıcı hazır destekler. Diğer hizmetler ile çalışmak için yüklemeniz gerekir ya da *kaydetme* bir [bağlama](./functions-triggers-bindings.md) uzantısı. Bağlama uzantıları, Azure Core araçları veya NuGet paketi sağlanır. 

Aşağıdaki tabloda, ne zaman ve nasıl bağlamaları kaydetme gösterir.

| Geliştirme ortamı |Kayıt<br/> işlevlerde 1.x  |Kayıt<br/> işlevlerde 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Automatic|[Otomatik İstemi ile](#azure-portal-development)|
|.NET olmayan dil ya da yerel Azure Core araçlarını geliştirme|Automatic|[Çekirdek araçları CLI komutlarını kullanın](#local-development-azure-functions-core-tools)|
|Visual Studio 2017'yi kullanarak C# sınıf kitaplığı|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2017)|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2017)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullanma](#c-class-library-with-visual-studio-code)|

Aşağıdaki bağlama türleri, açık kaydı otomatik olarak tüm sürümleri ve ortamları kayıtlı olduğundan gerektirmeyen özel durumlar şunlardır: HTTP ve Zamanlayıcı.

> [!IMPORTANT]
> Bu içerik, bu makalenin geri kalanında yalnızca geçerlidir 2.x çalışır. Bağlama uzantıları işlevleri açıkça kayıtlı olmayan Aşağıdakiler haricinde 1.x [oluşturma bir C# Visual Studio 2017'yi kullanarak bir sınıf kitaplığı](#local-csharp).

## <a name="azure-portal-development"></a>Azure portal geliştirme

Bir işlev oluşturma veya bağlama eklemek, tetikleyicisi veya bağlaması uzantısı kaydı gerektirdiğinde, sizden istenir. Tıklayarak istemine yanıt **yükleme** uzantısını kaydetmek için. Yükleme, bir tüketim planında 10 dakikaya kadar sürebilir. 

Yalnızca bir kez bir verilen işlev uygulaması için her bir uzantı yüklemeniz gerekir. Portalda veya güncelleştirmek için kullanılabilir olmayan desteklenen bağlamaları için yüklenmiş bir uzantı ayrıca [el ile yüklemek veya Azure Portalı'ndan uzantılarını bağlama işlevleri güncelleştirme](install-update-binding-extensions-manual.md).  

## <a name="local-development-azure-functions-core-tools"></a>Yerel geliştirme Azure işlevleri çekirdek araçları

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2017"></a>C# sınıf kitaplığı ile Visual Studio 2017

İçinde **Visual Studio 2017**, Paket Yöneticisi Konsolu'nu kullanarak paketleri yükleyebilirsiniz [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aşağıdaki örnekte gösterildiği gibi komut:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <target_version>
```

Kullanmak için belirli bir bağlama için paket adını bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<target_version>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="c-class-library-with-visual-studio-code"></a>C# sınıf kitaplığı Visual Studio Code ile

İçinde **Visual Studio Code**, komut istemi kullanarak paketleri yükleyebilirsiniz [dotnet paketini ekleyin](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) aşağıdaki örnekte gösterildiği gibi .NET Core CLI, komut:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <target_version>
```

.NET Core CLI, yalnızca Azure işlevleri 2.x geliştirme için kullanılabilir.

Kullanmak için belirli bir bağlama için paket adını bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<target_version>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure işlevi tetikleyici ve bağlama örneği](./functions-bindings-example.md)


