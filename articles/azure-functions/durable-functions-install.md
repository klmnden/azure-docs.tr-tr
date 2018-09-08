---
title: Dayanıklı işlevler uzantısını ve örnekleri - Azure'ı yükleme
description: Dayanıklı işlevler uzantısını portal geliştirme veya Visual Studio geliştirme için Azure işlevleri için yüklemeyi öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 8c5f3114172a7d27685e7aee2972b43b9ebef4e9
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093015"
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Örnekler (Azure işlevleri) ve dayanıklı işlevler uzantısını yükleme

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı Azure işlevleri için NuGet paketi sağlanan [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Bu makalede, paket ve örnekler için aşağıdaki geliştirme ortamları kümesi nasıl yükleneceği gösterilmektedir:

* Visual Studio 2017 (C# için önerilir) 
* Visual Studio kod (JavaScript için önerilir)
* Azure portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio şu anda dayanıklı işlevler kullanan uygulamalar geliştirmek için en iyi deneyimi sağlar.  İşlevlerinizi yerel olarak çalıştırılabilir ve Azure'a yayımlanabilir. Boş bir proje veya örnek işlevler kümesi ile başlayabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* Yükleme [Visual Studio'nun en son sürümünü](https://www.visualstudio.com/downloads/) (sürüm 15.3 veya üzeri). Dahil **Azure geliştirme** iş yükü, Kurulum Seçenekleri.

### <a name="start-with-sample-functions"></a>Örnek işlevleri ile Başlat 

1. İndirme [Visual Studio için örnek uygulamayı .zip dosyasını](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Örnek Proje zaten sahip olduğu NuGet başvuru eklemeniz gerekmez.
2. Yükleme ve çalıştırma [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 veya sonraki bir sürümü. Alternatif olarak, güncelleştirme *local.appsettings.json* gerçek Azure depolama bağlantı dizeleri içeren dosya.
3. Projeyi Visual Studio 2017'de açın. 
4. Örneği çalıştırmak yönergeler için başlayan [Function zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'da yayımlanan.

### <a name="start-with-an-empty-project"></a>Boş bir proje ile Başlat
 
Örnek ile başlayan olduğu gibi aynı yönergeleri izleyin, ancak indirmek yerine aşağıdaki adımları uygulayın *.zip* dosyası:

1. Bir işlev uygulaması projesi oluşturun.
2. Aşağıdaki NuGet paketi başvurusu kullanarak arama *NuGet paketlerini Yönet* ve projeye ekleyin: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.5.0
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code, başlıca platformların tümüne - Windows, macOS ve Linux ele alındığı bir yerel geliştirme deneyimi sağlar.  İşlevlerinizi yerel olarak çalıştırın ve ayrıca Azure'da yayımlanabilmesi. Boş bir proje veya örnek işlevler kümesi ile başlayabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* Yükleme [en son sürümünü Visual Studio Code](https://code.visualstudio.com/Download) 

* "Azure işlevleri çekirdek araçları yükleyin" altında adresindeki yönergeleri [kod ve yerel olarak test Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Azure işlevleri Çapraz Platform Araçları zaten varsa, bunları kullanılabilir en son sürüme güncelleştirin.

    >[!IMPORTANT]
    >Dayanıklı işlevler javascript'teki sürüm gerektirir, Azure işlevleri çekirdek araçları 2.x.

*  Bir Windows makinede mevcut değilse, yükleyin ve çalıştırın [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 veya sonraki bir sürümü. Alternatif olarak, güncelleştirme *local.appsettings.json* gerçek Azure depolama bağlantılı dosya. 


### <a name="start-with-sample-functions"></a>Örnek işlevleri ile Başlat

#### <a name="c"></a>C#

1. Kopya [dayanıklı işlevler depo](https://github.com/Azure/azure-functions-durable-extension.git).
2. Makinenize gidin [C# kod örnekleri klasörü](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Azure işlevleri dayanıklı uzantısı aşağıdaki bir komut çalıştırarak yükleme istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.5.0
    ```
4. Aşağıdaki komutta çalıştırarak Azure işlevleri Twilio uzantısı yükleme istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0-beta5
    ```
5. Azure Storage öykünücüsü veya güncelleştirme çalıştırması *local.appsettings.json* gerçek Azure depolama bağlantı dizesi içeren dosya.
6. Visual Studio Code'da projeyi açın. 
7. Örneği çalıştırmak yönergeler için başlayan [Function zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'da yayımlanan.
8. Aşağıdaki komutu komut istemi / terminal çalıştırarak proje başlatın:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

1. Kopya [dayanıklı işlevler depo](https://github.com/Azure/azure-functions-durable-extension.git).
2. Makinenize gidin [JavaScript örnekler klasörüne](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Azure işlevleri dayanıklı uzantısı aşağıdaki bir komut çalıştırarak yükleme istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.5.0
    ```
4. Aşağıdaki komutta çalıştırarak npm paketlerini geri yükleme istemi / terminal penceresi:
    
    ```bash
    npm install
    ``` 
5. Güncelleştirme *local.appsettings.json* gerçek Azure depolama bağlantı dizesi içeren dosya.
6. Visual Studio Code'da projeyi açın. 
7. Örneği çalıştırmak yönergeler için başlayan [Function zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'da yayımlanan.
8. Aşağıdaki komutu komut istemi / terminal çalıştırarak proje başlatın:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Boş bir proje ile Başlat
 
1. Komut istemi / terminal işlev uygulamanızı barındıracak klasöre gidin.
2. Aşağıdaki komutta çalıştırarak Azure işlevleri dayanıklı uzantıyı yükleme istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.5.0
    ```
3. Aşağıdaki komutu çalıştırarak bir işlev uygulaması projesi oluşturun:

    ```bash
    func init
    ``` 
4. Azure Storage öykünücüsü veya güncelleştirme çalıştırması *local.appsettings.json* gerçek Azure depolama bağlantı dizesi içeren dosya.
5. Ardından, aşağıdaki komutu çalıştırarak yeni bir işlev oluşturun ve sihirbaz adımlarını izleyin:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Şu anda sağlam bir işlev şablonu kullanılabilir olmayan ancak desteklenen seçeneklerinden biri ile başlayın ve ardından kodu değiştirin. Örnekler için başvurmak [düzenleme istemcisi](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [düzenleme tetikleyici](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), ve [etkinlik tetikleyici](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Visual Studio Code'da proje klasörü açın ve şablon kodunu değiştirerek devam edin. 
7. Aşağıdaki komutu komut istemi / terminal çalıştırarak proje başlatın:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Azure portal

Tercih ederseniz kullanabilirsiniz [Azure portalında](https://portal.azure.com) dayanıklı işlevler geliştirme için.

   > [!NOTE]
   > JavaScript içinde dayanıklı işlevler henüz portalda kullanılabilir değildir.

### <a name="create-an-orchestrator-function"></a>Bir düzenleyici işlevi oluşturma

1. Gösterildiği gibi Portalı'nda yeni bir işlev uygulaması oluşturma [işlevleri hızlı başlangıç makalesi](functions-create-first-azure-function.md#create-a-function-app).

2. İşlev uygulaması için yapılandırma [2.0 çalışma zamanı sürümünü kullanan](set-runtime-version.md).

   Dayanıklı işlevler uzantısını 1.X çalışma zamanı hem 2.0 çalışma zamanı üzerinde çalışır, ancak Azure portalında şablonları yalnızca 2.0 çalışma zamanı hedeflendiğinde kullanılabilir.

3. Seçerek yeni bir işlev oluşturma **"kendi özel işlevinizi oluşturun."** .

4. Değişiklik **dil** için **C#**, **senaryo** için **dayanıklı işlevler** seçip **dayanıklı işlevler Http Başlatıcısı -C#** şablonu.

5. Altında **uzantılar yüklü değil**, tıklayın **yükleme** NuGet.org adresinden uzantısı indirilemedi. 

6. Yükleme tamamlandıktan sonra bir düzenleme istemci işlevinin – oluşturma işlemine devam etmek **"HttpStart"** seçerek oluşturulan **dayanıklı işlevler Http Başlatıcısı - C#** şablonu.

7. Şimdi bir düzenleme işlevi oluşturun **"HelloSequence"** gelen **dayanıklı işlevler Düzenleyicisi - C#** şablonu.

8. Ve son işlevi çağrılacak **"Hello"** gelen **dayanıklı işlevler etkinliği - C#** şablonu.

9. Git **"HttpStart"** işlev ve URL'sini kopyalayın.

10. Dayanıklı işlevi çağırmak için Postman veya cURL kullanın. Test etmeden önce URL'yi değiştirin **{functionName}** orchestrator işlevi adıyla - **HelloSequence**.  Hiçbir veri gereklidir, POST edimi kullanmanız yeterlidir. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Ardından, arama **"statusQueryGetUri"** uç nokta ve dayanıklı işlevinin geçerli durumunu görebilirsiniz

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Arama devam **"statusQueryGetUri"** durum olana kadar uç nokta **"Tamamlandı"** 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Tebrikler! Dayanıklı ilk işlevinizi, Azure Portalı'nda hazır ve çalışır durumda!

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Zincirleme örneği işlevi çalıştırın](durable-functions-sequence.md)
