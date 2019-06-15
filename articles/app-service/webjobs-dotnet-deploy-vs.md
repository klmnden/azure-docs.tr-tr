---
title: Azure Visual Studio kullanarak webjob'ları dağıtma ve geliştirin
description: Geliştirme ve Azure webjobs'ın Visual Studio kullanarak Azure App Service'e dağıtma hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 02/18/2019
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 9f4d3ff6fa02369c0e4a01949cc686b842a63a12
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808474"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Geliştirme ve Visual Studio - Azure App Service kullanarak Web işleri dağıtma

Bu makalede, Visual Studio konsol uygulama projesini bir web uygulamasında dağıtmak için kullanmayı açıklar [App Service](overview.md) olarak bir [Azure WebJob](https://go.microsoft.com/fwlink/?LinkId=390226). WebJobs kullanarak dağıtma hakkında daha fazla bilgi için [Azure portalında](https://portal.azure.com), bakın [WebJobs ile arka planda çalıştır görevleri](webjobs-create.md).

Tek bir web uygulaması için birden çok Web işleri yayımlayabilirsiniz. Bir web uygulamasında her bir Web işi benzersiz bir ad olduğundan emin olun.

Sürüm, 3.x [Azure WebJobs SDK](webjobs-sdk-how-to.md) .NET Core uygulamaları ya da .NET Framework olarak çalışırken sürüm 2.x destekler yalnızca .NET Framework uygulamalarını çalıştırmaya WebJobs geliştirmenizi sağlar. WebJobs proje dağıtma şeklinizi olanları .NET Framework ve .NET Core projeleri için farklıdır.

## <a name="webjobs-as-net-core-console-apps"></a>WebJobs olarak .NET Core konsol uygulamaları

Sürüm kullanırken 3.x WebJobs için oluşturma ve .NET Core konsol uygulamaları olarak Web işleri yayımlama. Oluşturmak ve bir Web işi olarak .NET Core konsol uygulamasını azure'da yayımlamak adım adım yönergeler için bkz: [olay odaklı arka planda işleme için Azure Web işleri SDK'sı ile çalışmaya başlama](webjobs-sdk-get-started.md).

> [!NOTE]
> .NET core Web işleri, web projeleriyle bağlanamaz. WebJob'ınıza bir web uygulaması ile dağıtmanız gerekiyorsa, şunları yapmalısınız [bir .NET Framework konsol uygulaması olarak, WebJob oluşturma](#webjobs-as-net-framework-console-apps).  

### <a name="deploy-to-azure-app-service"></a>Azure App Service'e dağıtma

.NET Core WebJob, App Service'te Visual Studio'dan yayımlama, ASP.NET Core uygulaması yayımlama olarak aynı araçları kullanır.

[!INCLUDE [webjobs-publish-net-core](../../includes/webjobs-publish-net-core.md)] 

### <a name="webjob-types"></a>WebJob türü

Varsayılan olarak, bir .NET Core konsol projesi sadece tetiklendiğinde çalışır veya isteğe bağlı WebJob yayımladı. Projeye da güncelleştirebilirsiniz [bir zamanlamaya göre çalıştırma](#scheduled-execution) veya sürekli olarak çalıştırın.

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

#### <a name="scheduled-execution"></a>Zamanlanan yürütme

Azure, bir .NET Core konsol uygulaması yayımladığınızda, yeni bir *settings.job* dosya projeye eklendi. WebJob'ınıza bir yürütme zamanlamasını ayarlamak için bu dosyayı kullanın. Daha fazla bilgi için [Tetiklenmiş bir Web işi zamanlaması](#scheduling-a-triggered-webjob).

#### <a name="continuous-execution"></a>Sürekli yürütme

Visual Studio, webjob'ı sürekli olarak her zaman açık Azure'da etkin olduğunda çalıştırılacak değiştirmek için kullanabilirsiniz.

1. Bunu zaten bunu yapmadıysanız [projeyi Azure'da yayımlamanın](#deploy-to-azure-app-service).

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. İçinde **Yayımla** sekmesini, **ayarları**. 

1. İçinde **profil ayarları** iletişim kutusunda seçin **sürekli** için **WebJob türü**ve **Kaydet**.

    ![Bir Web işi için yayımlama Ayarları iletişim kutusu](./media/webjobs-dotnet-deploy-vs/publish-settings.png)

1. Seçin **Yayımla** WebJob güncelleştirilmiş ayarlarla yeniden yayımlamak için.

## <a name="webjobs-as-net-framework-console-apps"></a>WebJobs olarak .NET Framework konsol uygulamaları  

Visual Studio .NET Framework konsol uygulamasını WebJobs etkin proje dağıttığında, iki görevleri gerçekleştirir:

* Çalışma zamanı dosyalarını uygun web uygulamasını klasörüne kopyalar (*App_Data/iş/continuous* sürekli WebJobs için ve *App_Data/iş/triggered* zamanlanmış veya isteğe bağlı WebJobs için).
* Ayarlar [Azure Scheduler](https://docs.microsoft.com/azure/scheduler/) belirli zamanlarda çalıştırılması zamanlanan Webjob'lar için işler. (Bu sürekli WebJobs için gerekli değildir.)

WebJobs'ın etkinleştirilmiş bir proje için eklenen aşağıdaki öğeleri içerir:

* [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi.
* A [webjob yayımlama settings.json](#publishsettings) dağıtım ve Zamanlayıcı ayarlarını içeren dosya. 

![Dağıtım bir WebJob olarak etkinleştirmek için bir konsol uygulaması için eklenen gösteren diyagram](./media/webjobs-dotnet-deploy-vs/convert.png)

Bu öğeleri varolan bir konsol uygulaması projesine ekleyin veya yeni bir Web işleri özellikli konsol uygulaması projesi oluşturmak için bir şablonu kullanın. 

Bir proje tek başına bir WebJob olarak dağıtmak ve böylece web projesini dağıtma olduğunda otomatik olarak dağıtan bir web projesine bağlar. Visual Studio projeleri bağlamak için WebJobs etkin projedeki adını içeren bir [webjobs list.json](#webjobslist) web proje dosyasında.

![Bağlama Web projesine WebJob proje gösteren diyagram](./media/webjobs-dotnet-deploy-vs/link.png)

### <a name="prerequisites"></a>Önkoşullar

Visual Studio 2015 kullanıyorsanız, yükleme [(Visual Studio 2015) .NET için Azure SDK'sı](https://azure.microsoft.com/downloads/).

Visual Studio 2019 kullanıyorsanız, yükleme [Azure geliştirme iş yükü](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---choose-workloads).

### <a id="convert"></a> Mevcut bir konsol uygulaması projesi için Web işleri dağıtımı etkinleştir

İki seçeneğiniz vardır:

* [Otomatik dağıtım ile bir web projesi etkinleştirme](#convertlink).

  Mevcut bir konsol uygulama projesini, web projesini dağıtırken bir WebJob olarak otomatik olarak dağıttığı şekilde yapılandırın. İlgili web uygulama içinde çalıştırdığınız web uygulamasında, webjob'ı çalıştırmak istediğinizde bu seçeneği kullanın.

* [Bir web projesi olmadan dağıtımı etkinleştir](#convertnolink).

  Mevcut bir konsol uygulama projesini bir WebJob olarak tek başına bir web projesi bağlantı dağıtmak için yapılandırın. Bir webjob'ı bir web uygulamasında tek başına web uygulamasında çalışan hiçbir web uygulaması ile çalıştırmak istediğinizde bu seçeneği kullanın. WebJob kaynaklarınızı web uygulama kaynaklarınızı bağımsız olarak ölçeklendirme mümkün olması için bunu yapmak isteyebilirsiniz.

#### <a id="convertlink"></a> Bir web projesi ile otomatik WebJobs dağıtımı etkinleştir

1. Web projesinde sağ **Çözüm Gezgini**ve ardından **Ekle** > **Azure WebJob olarak mevcut proje**.
   
    ![Azure Web işi olarak mevcut proje](./media/webjobs-dotnet-deploy-vs/eawj.png)
   
    [Azure WebJob Ekle](#configure) iletişim kutusu görüntülenir.
2. İçinde **proje adı** konsol uygulaması projesi bir WebJob olarak eklemek için aşağı açılan listesinde seçin.
   
    ![Azure WebJob Ekle iletişim kutusunda proje seçme](./media/webjobs-dotnet-deploy-vs/aaw1.png)
3. Tamamlamak [Azure WebJob Ekle](#configure) iletişim ve ardından **Tamam**. 

#### <a id="convertnolink"></a> Bir web projesi olmadan WebJobs dağıtımı etkinleştir
1. Konsol uygulaması projesine sağ tıklayın **Çözüm Gezgini**ve ardından **Azure WebJob olarak Yayımla...** . 
   
    ![Azure Web işi olarak Yayımla](./media/webjobs-dotnet-deploy-vs/paw.png)
   
    [Azure WebJob Ekle](#configure) Seçili proje iletişim kutusu görünür **proje adı** kutusu.
2. Tamamlamak [Azure WebJob Ekle](#configure) iletişim kutusunu ve ardından **Tamam**.
   
   **Web'i Yayımla** Sihirbazı görünür.  Hemen Yayımla istemiyorsanız, sihirbazı kapatın. İstediğiniz zaman, girdiğiniz ayarları için kaydedilir [projeyi dağıtmak](#deploy).

### <a id="create"></a>WebJobs kullanan yeni bir proje oluşturun
WebJobs özellikli yeni bir proje oluşturmak için konsol uygulaması proje şablonu kullanın ve açıklandığı gibi WebJobs dağıtımı etkinleştir [önceki bölümde](#convert). Alternatif olarak, Web işleri yeni proje şablonu kullanabilirsiniz:

* [Bağımsız bir Web işi için WebJobs yeni proje şablonu kullanın](#createnolink)
  
    Bir proje oluşturun ve tek başına bir web projesi için hiçbir bağlantı içeren bir WebJob olarak dağıtmak için yapılandırın. Bir webjob'ı bir web uygulamasında tek başına web uygulamasında çalışan hiçbir web uygulaması ile çalıştırmak istediğinizde bu seçeneği kullanın. WebJob kaynaklarınızı web uygulama kaynaklarınızı bağımsız olarak ölçeklendirme mümkün olması için bunu yapmak isteyebilirsiniz.
* [Bir web projesine WebJob WebJobs yeni proje şablonu kullanıma bağlı](#createlink)
  
    Aynı çözüm içindeki bir web projesi dağıtıldığında otomatik olarak bir WebJob olarak dağıtmak için yapılandırılmış bir proje oluşturun. İlgili web uygulama içinde çalıştırdığınız web uygulamasında, webjob'ı çalıştırmak istediğinizde bu seçeneği kullanın.

> [!NOTE]
> WebJobs yeni proje şablonu otomatik olarak NuGet paketlerini yükler ve kodu içerir *Program.cs* için [WebJobs SDK](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Web işleri SDK'yı kullanmak istemiyorsanız, kaldırmak veya değiştirmek `host.RunAndBlock` deyiminde *Program.cs*.
> 
> 

#### <a id="createnolink"></a> Bağımsız bir Web işi için WebJobs yeni proje şablonu kullanın
1. Tıklayın **dosya** > **yeni proje**ve ardından **yeni proje** iletişim kutusunu tıklatıp **bulut**  >   **Azure Web işi (.NET Framework)** .
   
    ![WebJob şablon gösteren yeni proje iletişim kutusu](./media/webjobs-dotnet-deploy-vs/np.png)
2. Daha önce gösterilen yönergeleri izleyin [bağımsız bir WebJobs proje proje konsol uygulaması olun](#convertnolink).

#### <a id="createlink"></a> Bir web projesine WebJob WebJobs yeni proje şablonu kullanıma bağlı
1. Web projesinde sağ **Çözüm Gezgini**ve ardından **Ekle** > **yeni Azure WebJob proje**.
   
    ![Yeni Azure WebJob proje menüsü girişi](./media/webjobs-dotnet-deploy-vs/nawj.png)
   
    [Azure WebJob Ekle](#configure) iletişim kutusu görüntülenir.
2. Tamamlamak [Azure WebJob Ekle](#configure) iletişim kutusunu ve ardından **Tamam**.

### <a id="configure"></a>Azure WebJob Ekle iletişim kutusu
**Azure WebJob Ekle** iletişim WebJob adı girin ve işinizi modu ayarı çalıştırmanıza olanak tanır. 

![Azure Web işi iletişim kutusu Ekle](./media/webjobs-dotnet-deploy-vs/aaw2.png)

Bu iletişim kutusunda alanları üzerinde alanlarına karşılık gelen **WebJob Ekle** Azure portal'ın iletişim. Daha fazla bilgi için [WebJobs ile arka planda çalıştır görevleri](webjobs-create.md).

> [!NOTE]
> * Komut satırı dağıtımı hakkında daha fazla bilgi için bkz: [etkinleştirme komut satırı veya Azure Web işleri sürekli teslim](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * WebJob'ı dağıtma ve Web işi ve yeniden dağıtma türünü değiştirmek istediğiniz karar verirseniz, silme gerekecektir *Web işleri yayımlama settings.json* dosya. WebJob türünü değiştirebilirsiniz, bu Visual Studio yayımlama seçeneklerini gösterme hale getirir.
> * WebJob dağıtma ve daha sonra çalışma modunda gelen sürekli sürekli olmayan veya tam tersi değiştirmek, Visual Studio, yeniden dağıtırken Azure'da yeni bir Web işi oluşturur. Diğer zamanlama ayarlarını değiştirme halde bırakın aynı çalıştırma modu veya zamanlanmış ve isteğe bağlı arasında geçiş, Visual Studio güncelleştirmeleri mevcut işi yerine yeni bir tane oluşturun.
> 
> 

### <a id="publishsettings"></a>webjob-publish-settings.json
Bir konsol uygulaması Web işleri dağıtımı için yapılandırdığınızda, Visual Studio'yu yükler [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi ve planlama bilgileri depolar bir *webjob yayımlama settings.json*  proje dosyasında *özellikleri* WebJobs proje klasörü. Bu dosyanın bir örnek aşağıda verilmiştir:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Visual Studio IntelliSense sağlar ve bu dosyayı doğrudan düzenleyebilirsiniz. Dosya şeması depolandı [ https://schemastore.org ](https://schemastore.org/schemas/json/webjob-publish-settings.json) ve orada görüntülenebilir.  

### <a id="webjobslist"></a>webjobs-list.json
WebJobs'ın etkinleştirilmiş bir proje için bir web projesi bağladığınızda, Visual Studio WebJobs projenin adını depolar. bir *webjobs list.json* web proje dosyasında *özellikleri* klasör. Liste, aşağıdaki örnekte gösterildiği gibi birden çok Web işleri proje içerebilir:

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

Visual Studio IntelliSense sağlar ve bu dosyayı doğrudan düzenleyebilirsiniz. Dosya şeması depolandı [ https://schemastore.org ](https://schemastore.org/schemas/json/webjobs-list.json) ve orada görüntülenebilir.

### <a id="deploy"></a>WebJobs projesini dağıtma
Bir web projesine bağlı bir WebJobs proje web projesi ile otomatik olarak dağıtır. Web projesi dağıtımı hakkında daha fazla bilgi için bkz. **nasıl yapılır kılavuzları** > **Dağıt uygulama** sol gezinti bölmesinde.

WebJobs proje kendisi tarafından dağıtmak için projeye sağ **Çözüm Gezgini** tıklatıp **Azure WebJob olarak Yayımla...** . 

![Azure Web işi olarak Yayımla](./media/webjobs-dotnet-deploy-vs/paw.png)

Bir bağımsız Web işi, aynı için **Web'i Yayımla** web projeleri görünür, ancak daha az ayarları değiştirmek kullanılabilir olan kullanılan Sihirbazı.

## <a name="scheduling-a-triggered-webjob"></a>Tetiklenmiş bir Web işi zamanlama

WebJobs kullanan bir *settings.job* bir webjob'ı ne zaman çalıştırıldığını olmadığının dosya. WebJob'ınıza bir yürütme zamanlamasını ayarlamak için bu dosyayı kullanın. Aşağıdaki örnek, her saat 17: 00 için 9'da çalışır:

```json
{
    "schedule": "0 0 9-17 * * *"
}
```

Bu dosya Web işleri klasörün kökünde, yan gibi WebJob'ın betik boyunca `wwwroot\app_data\jobs\triggered\{job name}` veya `wwwroot\app_data\jobs\continuous\{job name}`. Visual Studio'dan bir Web işi dağıttığınızda işaretlemek, `settings.job` dosya özellikleri olarak **yeniyse Kopyala**. 

Olduğunda, [Azure portalından bir WebJob oluşturma](webjobs-create.md), settings.job dosyası oluşturulur.

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

### <a name="cron-expressions"></a>Sıralanmış iş ifadeleri

Web işleri, Azure işlevleri'nde Zamanlayıcı tetikleyicisi olarak zamanlama için aynı sıralanmış iş ifadeleri kullanır. CRON desteği hakkında daha fazla bilgi edinmek için [Zamanlayıcı tetikleyici başvurusu makalesinde](../azure-functions/functions-bindings-timer.md#cron-expressions).

### <a name="settingjob-reference"></a>Setting.job başvurusu

Aşağıdaki ayarlar, Web işleri tarafından desteklenir:

| **Ayar** | **Tür**  | **Açıklama** |
| ----------- | --------- | --------------- |
| `is_in_place` | Tümü | İşin ilk temp klasörüne kopyalanmasını olmadan yerinde çalışmasını sağlar. Daha fazla bilgi için bkz. [WebJobs çalışma dizini](https://github.com/projectkudu/kudu/wiki/WebJobs#webjob-working-directory). |
| `is_singleton` | Sürekli | Yalnızca Web işleri, ölçeği, tek bir örneği çalıştırın. Daha fazla bilgi için bkz. [sürekli bir işe singleton ayarlamak](https://github.com/projectkudu/kudu/wiki/WebJobs-API#set-a-continuous-job-as-singleton). |
| `schedule` | Tetiklenen | WebJob CRON tabanlı bir zamanlamaya göre çalıştırın. Daha fazla bilgi için bkz. [Zamanlayıcı tetikleyici başvurusu makalesinde](../azure-functions/functions-bindings-timer.md#cron-expressions). |
| `stopping_wait_time`| Tümü | Kapatma davranışının denetimi sağlar. Daha fazla bilgi için bkz. [kapatılmasını](https://github.com/projectkudu/kudu/wiki/WebJobs#graceful-shutdown). |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web işleri SDK'sı hakkında daha fazla bilgi edinin](webjobs-sdk-how-to.md)
