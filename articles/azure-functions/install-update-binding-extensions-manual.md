---
title: El ile yüklemek veya Azure işlevleri bağlama uzantıları güncelleştirme
description: Yükleme veya dağıtılmış bir işlev uygulamaları için Azure işlevleri bağlama uzantıları güncelleştirme öğrenin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, bağlama uzantıları, NuGet, güncelleştirmeleri
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/26/2018
ms.author: glenga
ms.openlocfilehash: cda977ba59070c3ddaac05784277d6c0b5109f0f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035769"
---
# <a name="manually-install-or-update-azure-functions-binding-extensions-from-the-portal"></a>El ile yüklemek veya portaldan Azure işlevleri bağlama uzantıları güncelleştirme

Azure işlevleri sürüm 2.x çalışma zamanı, tetikleyiciler ve bağlamalar için kod uygulamak için bağlama uzantıları kullanır. Bağlama uzantıları NuGet paketleri içinde sağlanır. Bir uzantıyı kaydetmek için aslında bir paketini yükleyin. İşlevler geliştirme, bağlama uzantıları yükleme yolu geliştirme ortamınıza bağlıdır. Daha fazla bilgi için [kaydetme bağlama uzantıları](./functions-bindings-register.md) Tetikleyicileri ve bağlamaları makalesi.

Bazen el ile yüklemek veya Azure portalında bağlama uzantılarınızı güncelleştirmek gerekir. Örneğin, kayıtlı bir bağlama yeni bir sürüme güncelleştirmeniz gerekebilir. Ayrıca de yüklenemez, desteklenen bir bağlama kaydetmeniz gerekebilir **tümleştir** portalında sekmesi.

## <a name="install-a-binding-extension"></a>Bir bağlama uzantısını yükle

El ile yüklemek veya portalından Uzantıları'nı güncelleştirmek için aşağıdaki adımları kullanın.

1. İçinde [Azure portalında](https://portal.azure.com), işlev uygulamanızı bulun ve seçin. Seçin **genel bakış** sekmenize **Durdur**.  Böylece değişiklik yapılabilmesi için işlev uygulaması durduruluyor dosyaları kilidini açar.

1. Seçin **Platform özellikleri** sekmesi altında **geliştirme araçları** seçin **Gelişmiş araçlar (Kudu)**. Kudu uç noktası (`https://<APP_NAME>.scm.azurewebsites.net/`) yeni bir pencerede açılır.

1. Kudu penceresinde **hata ayıklama konsoluna** > **CMD**.  

1. Komut penceresinde gidin `D:\home\site\wwwroot` ve Sil simgesini seçin `bin` klasörü silinemedi. Seçin **Tamam** silme işlemini onaylamak için.

1. Düzenleme simgesini seçin `extensions.csproj` işlev uygulamasına bağlama uzantıları tanımlayan dosya. Proje dosyası çevrimiçi düzenleyicisinde açılır.

1. Gereken eklemeleri ve güncelleştirmeleri **PackageReference** öğeler **ItemGroup**, ardından **Kaydet**. Geçerli desteklenen paket sürümleri listesini bulunabilir [paketleri yapmam gerekir mi?](https://github.com/Azure/azure-functions-host/wiki/Updating-your-function-app-extensions#what-nuget-packages-do-i-need) wiki makalesi. Üç tüm Azure depolama bağlamaları Microsoft.Azure.WebJobs.Extensions.Storage paketi gerektirir.

1. Gelen `wwwroot` başvurulan derlemelerde yeniden oluşturmak için aşağıdaki komutu çalıştırın, klasör `bin` klasör.

    ```cmd
    dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget
    ```

1. Geri **genel bakış** sekmesini Portalı'nda, **Başlat** işlev uygulamasını yeniden başlatmak için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
