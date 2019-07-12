---
title: Azure uygulama yapılandırması dinamik yapılandırma kullanarak bir .NET Core uygulaması için öğretici | Microsoft Docs
description: Bu öğreticide, .NET Core uygulamaları için yapılandırma verilerini dinamik olarak güncelleştirileceğini öğrenin
services: azure-app-configuration
documentationcenter: ''
author: abarora
manager: zhenlan
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: abarora
ms.openlocfilehash: 1649fefda5073761d616fc48c602cab84d293ed0
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799976"
---
# <a name="tutorial-use-dynamic-configuration-in-a-net-core-app"></a>Öğretici: Bir .NET Core uygulamasında dinamik yapılandırması kullan

Uygulama yapılandırma .NET Core istemci kitaplığı, yeniden başlatmak bir uygulama neden olmadan isteğe bağlı yapılandırma ayarları kümesini güncelleştirilmesini destekler. Bu örneği alarak uygulanabilir `IConfigurationRefresher` yapılandırma sağlayıcısı ve ardından arama seçeneklerden `Refresh` örneğine kodunuzdaki herhangi bir yerde.

Güncelleştirilmiş ayarları tutun ve yapılandırma deposu için çok fazla sayıda çağrı önlemek için her ayar için bir önbelleği kullanılır. Önbelleğe alınan bir ayarın değerini sona erinceye kadar bile yapılandırma deposunda değeri değiştiğinde yenileme işlemi değeri güncelleştirmez. Her istek için varsayılan süre 30 saniyedir ancak gerekirse kılınabilir.

Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmeleri nasıl uygulayacağınıza dair gösterir. Bu hızlı başlangıçlar, tanıtılan uygulama üzerine inşa edilmiştir. Devam etmeden önce [uygulama yapılandırması ile .NET Core uygulaması oluşturmak](./quickstart-dotnet-core-app.md) ilk.

Bu öğreticideki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını kullanılabilir mükemmel bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İsteğe bağlı bir uygulama yapılandırma deposuyla yapılandırmasını güncelleştirmek için uygulamanızı ayarlayın.
> * Uygulamanızın denetleyicileri en son yapılandırmayı yerleştirir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi uygulamak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Uygulama yapılandırma verileri yeniden yükleyin

Açık *Program.cs* dosyasını yenileme Yapılandırması'nda belirtin ve güncelleştirmeye `AddAzureAppConfiguration` yöntemi ve tetikleyici el ile yenileme kullanarak `Refresh` yöntemi.

```csharp
class Program
{
    private static IConfiguration _configuration = null;
    private static IConfigurationRefresher _refresher = null;

    static void Main(string[] args)
    {
        IConfigurationRefresher refresher = null;

        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(options =>
        {
            options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                    .ConfigureRefresh(refresh =>
                    {
                        refresh.Register("TestApp:Settings:Message")
                               .SetCacheExpiration(TimeSpan.FromSeconds(10));
                    });
                    
                    _refresher = options.GetRefresher();
        });

        _configuration = builder.Build();
        PrintMessage().Wait();
    }

    private static async Task PrintMessage()
    {
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");

        // Wait for the user to press Enter
        Console.ReadLine();

        await _refresher.Refresh();
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");
    }
}
```

`ConfigureRefresh` Yöntemi, bir yenileme işlemi tetiklendiğinde, uygulama yapılandırma deposu ile yapılandırma verileri güncelleştirmek için kullanılan ayarları belirtmek için kullanılır. Örneği `IConfigurationRefresher` çağrılarak alınabilir `GetRefresher` yöntemi için sağlanan seçenekleri `AddAzureAppConfiguration` yöntemi ve `Refresh` Bu örneği üzerinde yöntemi, kodunuzu herhangi bir yerindeki bir yenileme işlemi tetiklemek için kullanılabilir.
    
> [!NOTE]
> Bir yapılandırma ayarı için varsayılan önbellek sona erme süresini 30 saniye, ancak çağırarak geçersiz kılınabilir `SetCacheExpiration` seçenekleri Başlatıcı yöntemine geçirilen bağımsız değişken olarak `ConfigureRefresh` yöntemi.

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString**ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

1. Konsol uygulaması oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet build

1. Yapılandırma başarıyla tamamlandıktan sonra uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/dotnet-core-app-run.png)

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **tüm kaynakları**, hızlı başlangıç bölümünde oluşturduğunuz uygulama yapılandırma deposu örneği seçin.

1. Seçin **yapılandırması Gezgini**ve aşağıdaki anahtarlarını güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Settings:Message | Azure uygulama yapılandırması - güncelleştirilmiş verileri |

1. Komut istemi veya PowerShell penceresinde güncelleştirilmiş değeri yazdırma ve yenileme tetiklemek için Enter tuşuna basın.

    ![Hızlı Başlangıç uygulaması yenileme yerel](./media/quickstarts/dotnet-core-app-run-refresh.png)
    
    > [!NOTE]
    > Önbellek sona erme zamanı kullanarak 10 saniyeye ayarlama `SetCacheExpiration` yenileme işlemi, yapılandırma ayarı için değer yapılandırmasını belirtirken yöntemi, yalnızca en az 10 saniye sonra son yenileme geçtiyse güncelleştirilir Bu ayar için.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, eklediğiniz Azure yönetilen hizmet kimliği erişim uygulama yapılandırmasını kolaylaştırmaya ve uygulamanız için kimlik bilgileri yönetimi iyileştirmek için. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için Azure CLI örnekleri için devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
