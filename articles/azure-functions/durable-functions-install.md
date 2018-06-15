---
title: Dayanıklı işlevleri uzantısı ve örnekler - Azure yükleyin
description: Dayanıklı işlevleri uzantısı için Azure işlevleri, portal geliştirme veya Visual Studio geliştirme için nasıl yükleneceğini öğrenin.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 4dd4bbb9c382b772f8f60b259844e7e471ec73e3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33762362"
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Dayanıklı işlevleri uzantısı ve örnekleri (Azure işlevleri) yükleyin

[Dayanıklı işlevleri](durable-functions-overview.md) uzantısı Azure işlevleri için NuGet paketi sağlanan [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Bu makalede, paket ve örnekler için aşağıdaki geliştirme ortamlarını kümesi nasıl yükleneceği gösterilmektedir:

* Visual Studio 2017 (C# için önerilir) 
* Visual Studio Code (JavaScript için önerilir)
* Azure portalına

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio şu anda sağlam işlevleri kullanan uygulamalar geliştirmek için en iyi deneyimi sağlar.  İşlevlerinizi yerel olarak çalıştırın ve Azure'a de yayımlanabilir. Boş bir proje veya bir örnek işlevler kümesi ile başlayabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* Yükleme [en son sürümünü Visual Studio](https://www.visualstudio.com/downloads/) (sürüm 15.3 veya daha büyük). Dahil **Azure geliştirme** Kurulum seçeneklerinizi iş yükü.

### <a name="start-with-sample-functions"></a>Örnek işlevleriyle Başlat 

1. Karşıdan [Visual Studio için örnek uygulama .zip dosyasını](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Örnek Proje zaten sahip olduğu NuGet başvuru eklemeniz gerekmez.
2. Yükleme ve çalıştırma [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 veya sonraki bir sürümü. Alternatif olarak, güncelleştirme *local.appsettings.json* gerçek Azure Storage bağlantı dizelerini dosyasıyla.
3. Proje içinde Visual Studio 2017 açın. 
4. Örneği çalıştırmak yönergeler için başlayın [işlev zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'a yayımlanmalıdır.

### <a name="start-with-an-empty-project"></a>Boş bir proje ile Başlat
 
Örnek ile başlatma için olduğu gibi aynı yönergeleri izleyin, ancak indirmek yerine aşağıdaki adımları uygulayın *.zip* dosyası:

1. Bir işlev uygulaması projesi oluşturun.
2. Aşağıdaki NuGet paketi başvurusu kullanmak için arama *NuGet paketlerini Yönet* ve bunu projeye ekleyin: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.4.0 (denetleyin *dahil et* için Bu paket için arama yapın)
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code tüm önemli platformlar - Windows, macOS ve Linux kapsayan bir yerel geliştirme deneyimi sağlar.  İşlevlerinizi yerel olarak çalıştırılabilir ve Azure'a da yayınlanacak. Boş bir proje veya bir örnek işlevler kümesi ile başlayabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* Yükleme [en son sürümünü Visual Studio Code](https://code.visualstudio.com/Download) 

* "Azure işlevleri çekirdek araçlarını yükleme" altında adresindeki yönergeleri [koduna ve test yerel olarak Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Azure işlevleri Çapraz Platform Araçları zaten varsa, bunları kullanılabilir en son sürüme güncelleştirin.

    >[!IMPORTANT]
    >JavaScript dayanıklı işlevlerde sürümünü gerektirir Azure işlevleri çekirdek araçların 2.x.

*  Bir Windows makinesinde varsa, yüklemek ve çalıştırmak [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 veya sonraki bir sürümü. Alternatif olarak, güncelleştirme *local.appsettings.json* gerçek Azure depolama bağlantısı ile dosya. 


### <a name="start-with-sample-functions"></a>Örnek işlevleriyle Başlat

#### <a name="c"></a>C#

1. Kopya [dayanıklı işlevleri depo](https://github.com/Azure/azure-functions-durable-extension.git).
2. Makinenize gidin [C# kod örnekleri klasörü](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Bir komut aşağıdakini çalıştırarak Azure işlevleri dayanıklı uzantısını yükleyin komut istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.4.0
    ```
4. Bir komut aşağıdakini çalıştırarak Azure işlevleri Twilio uzantısını yükleyin komut istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0-beta5
    ```
5. Azure Storage öykünücüsü veya güncelleştirme çalışması *local.appsettings.json* gerçek Azure depolama bağlantı dizesini içeren dosya.
6. Visual Studio kodda projeyi açın. 
7. Örneği çalıştırmak yönergeler için başlayın [işlev zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'a yayımlanmalıdır.
8. Proje komutta komut istemi / terminal aşağıdaki komutu çalıştırarak işe başlayın:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

1. Kopya [dayanıklı işlevleri depo](https://github.com/Azure/azure-functions-durable-extension.git).
2. Makinenize gidin [JavaScript örnekler klasörü](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Bir komut aşağıdakini çalıştırarak Azure işlevleri dayanıklı uzantısını yükleyin komut istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.4.0
    ```
4. Bir komut aşağıdakini çalıştırarak npm paketler geri komut istemi / terminal penceresi:
    
    ```bash
    npm install
    ``` 
5. Güncelleştirme *local.appsettings.json* gerçek Azure depolama bağlantı dizesini içeren dosya.
6. Visual Studio kodda projeyi açın. 
7. Örneği çalıştırmak yönergeler için başlayın [işlev zincirleme - Hello dizisi örnek](durable-functions-sequence.md). Örnek, yerel olarak çalıştırmak veya Azure'a yayımlanmalıdır.
8. Proje komutta komut istemi / terminal aşağıdaki komutu çalıştırarak işe başlayın:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Boş bir proje ile Başlat
 
1. Komut istemi / terminal işlevi uygulamanızı barındıracak klasöre gidin.
2. Bir komut aşağıdakini çalıştırarak Azure işlevleri dayanıklı uzantısını yükleyin komut istemi / terminal penceresi:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.4.0
    ```
3. Aşağıdaki komutu çalıştırarak bir işlev uygulaması projesi oluşturun:

    ```bash
    func init
    ``` 
4. Azure Storage öykünücüsü veya güncelleştirme çalışması *local.appsettings.json* gerçek Azure depolama bağlantı dizesini içeren dosya.
5. Ardından, aşağıdaki komutu çalıştırarak yeni bir işlev oluşturun ve sihirbazın adımlarını izleyin:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Şu anda sağlam işlevi şablon yok ancak desteklenen seçeneklerden birini başlatın ve sonra kodu değiştirin. Başvuru için örnekleri kullanın [Orchestration istemci](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [Orchestration tetikleyici](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), ve [etkinlik tetikleyici](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Visual Studio kodda proje klasörünü açın ve şablon kodu değiştirerek devam edin. 
7. Proje komutta komut istemi / terminal aşağıdaki komutu çalıştırarak işe başlayın:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Azure portalına

İsterseniz, dayanıklı işlevleri geliştirme için Azure portalını kullanabilirsiniz.

   > [!NOTE]
   > JavaScript dayanıklı işlevlerde henüz portalda kullanılabilir değildir.

### <a name="create-an-orchestrator-function"></a>Bir orchestrator işlevi oluşturma

1. En yeni bir işlev uygulaması oluşturmak [functions.azure.com](https://functions.azure.com/signin).

2. İşlev uygulaması yapılandırma [2.0 çalışma zamanı sürümü kullanmak](set-runtime-version.md).

   Dayanıklı işlevleri uzantısı 1.X çalışma zamanı hem 2.0 çalışma zamanı çalışır, ancak Azure Portal şablonları yalnızca 2.0 çalışma zamanı hedeflendiğinde kullanılabilir.

3. Seçerek yeni bir işlev oluşturun **"özel işlevinizi oluşturun."** .

4. Değişiklik **dil** için **C#**, **senaryo** için **dayanıklı işlevleri** seçip **dayanıklı işlevleri Http Başlatıcı -C#** şablonu.

5. Altında **yüklü uzantıları**, tıklatın **yüklemek** uzantısı NuGet.org karşıdan yüklemek için. 

6. Yükleme tamamlandıktan sonra orchestration istemci işlevi – oluşturma işlemine devam edin **"HttpStart"** seçerek oluşturulmuş **dayanıklı işlevleri Http Starter - C#** şablonu.

7. Şimdi, bir düzenleme işlevi oluşturmak **"HelloSequence"** gelen **dayanıklı işlevleri Orchestrator:-C#** şablonu.

8. Ve son işlev çağrılmaz **"Merhaba"** gelen **dayanıklı işlevleri etkinliği:-C#** şablonu.

9. Git **"HttpStart"** işlev ve URL'sini kopyalayın.

10. Postman veya cURL dayanıklı işlevi çağırmak için kullanın. Test etmeden önce URL ile değiştirin **{functionName}** orchestrator işlevi adla - **HelloSequence**.  Hiçbir veri gerekli değildir, yalnızca POST fiil kullanın. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Ardından, çağıran **"statusQueryGetUri"** endpoint ve dayanıklı işlevinin geçerli durumunu görebilirsiniz

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Arama devam **"statusQueryGetUri"** durumu alıncaya kadar endpoint **"Tamamlandı"** 

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

Tebrikler! İlk dayanıklı işlevinizi Azure Portalı'nda da çalışır durumda!

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek zincirleme işlevi çalıştırma](durable-functions-sequence.md)
