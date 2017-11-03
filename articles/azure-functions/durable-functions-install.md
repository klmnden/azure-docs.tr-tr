---
title: "Dayanıklı işlevleri uzantısı ve örnekler - Azure yükleyin"
description: "Dayanıklı işlevleri uzantısı için Azure işlevleri, portal geliştirme veya Visual Studio geliştirme için nasıl yükleneceğini öğrenin."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: debadde78d937bcd4ec1df665aacfd1887fbcd02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Dayanıklı işlevleri uzantısı ve örnekleri (Azure işlevleri) yükleyin

[Dayanıklı işlevleri](durable-functions-overview.md) uzantısı Azure işlevleri için NuGet paketi sağlanan [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Bu makalede, paket ve örnekler için aşağıdaki geliştirme ortamlarını kümesi nasıl yükleneceği gösterilmektedir:

* Visual Studio 2017 (önerilir) 
* Azure portalına

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio şu anda sağlam işlevleri kullanan uygulamalar geliştirmek için en iyi deneyimi sağlar.  İşlevlerinizi yerel olarak çalıştırın ve Azure'a de yayımlanabilir. Boş bir proje veya bir örnek işlevler kümesi ile başlayabilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

* Yükleme [en son sürümünü Visual Studio](https://www.visualstudio.com/downloads/) (sürüm 15.3 veya daha büyük). Azure Araçları, kurulum seçenekleri içerir.

### <a name="start-with-sample-functions"></a>Örnek işlevleriyle Başlat

1. Karşıdan [Visual Studio için örnek uygulama .zip dosyasını](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Örnek Proje zaten sahip olduğu NuGet başvuru eklemeniz gerekmez.
2. Yükleme ve çalıştırma [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 veya sonraki bir sürümü. Alternatif olarak, güncelleştirme *local.appsettings.json* gerçek Azure Storage bağlantı dizelerini dosyasıyla.
3. Proje içinde Visual Studio 2017 açın. 
4. Örneği çalıştırmak yönergeler için başlayın [işlev zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'a yayımlanmalıdır.

### <a name="start-with-an-empty-project"></a>Boş bir proje ile Başlat
 
Örnek ile başlatma için olduğu gibi aynı yönergeleri izleyin, ancak indirmek yerine aşağıdaki adımları uygulayın *.zip* dosyası:

1. Bir işlev uygulaması projesi oluşturun.
2. Aşağıdaki NuGet paketi başvuru ekleyin, *.csproj* dosyası:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```

## <a name="azure-portal"></a>Azure portalına

İsterseniz, dayanıklı işlevleri geliştirme için Azure portalını kullanabilirsiniz.

### <a name="create-an-orchestrator-function"></a>Bir orchestrator işlevi oluşturma

1. En yeni bir işlev uygulaması oluşturmak [functions.azure.com](https://functions.azure.com/signin).
2. İşlev uygulaması yapılandırma [2.0 çalışma zamanı sürümü kullanmak](functions-versions.md).
3. Yeni bir işlev oluşturun ve seçin **dayanıklı işlevleri Orchestrator:-C#** şablonu.
4. Altında **yüklü uzantıları**, tıklatın **yüklemek** uzantısı NuGet.org karşıdan yüklemek için.

### <a name="copy-sample-code-to-the-function-app"></a>Örnek kod için işlev uygulaması kopyalayın

1. Karşıdan [DFSampleApp.zip](https://github.com/Azure/azure-functions-durable-extension/raw/master/docfx/files/DFSampleApp.zip) dosya.
2. Örnek dosyanın sıkıştırmasını `D:\home\site\wwwroot` Kudu veya FTP kullanarak işlevi uygulama.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek zincirleme işlevi çalıştırma](durable-functions-sequence.md)
