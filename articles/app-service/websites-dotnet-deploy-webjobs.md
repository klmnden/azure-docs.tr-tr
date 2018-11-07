---
title: Azure Visual Studio kullanarak webjob'ları dağıtma ve geliştirin
description: Geliştirme ve Azure webjobs'ın Visual Studio kullanarak Azure App Service'e dağıtma hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 09/12/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 08cbff7bc58f5925dee9b77ff195d362af4379d8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245758"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Geliştirme ve Visual Studio - Azure App Service kullanarak Web işleri dağıtma

## <a name="overview"></a>Genel Bakış

Bu konuda, bir konsol uygulama projesini bir web uygulamasında dağıtmak için Visual Studio kullanmayı açıklar [App Service](app-service-web-overview.md) olarak bir [Azure WebJob](https://go.microsoft.com/fwlink/?LinkId=390226). WebJobs kullanarak dağıtma hakkında daha fazla bilgi için [Azure portalında](https://portal.azure.com), bakın [WebJobs ile arka planda çalıştır görevleri](web-sites-create-web-jobs.md).

Visual Studio WebJobs özellikli konsol uygulama projesini dağıttığında, iki görevleri gerçekleştirir:

* Çalışma zamanı dosyalarını uygun web uygulamasını klasörüne kopyalar (*App_Data/iş/continuous* sürekli WebJobs için *App_Data/iş/triggered* zamanlanmış ve isteğe bağlı WebJobs için).
* Ayarlar [Azure Scheduler](https://docs.microsoft.com/azure/scheduler/) belirli zamanlarda çalıştırılması zamanlanan Webjob'lar için işler. (Bu sürekli WebJobs için gerekli değildir.)

WebJobs'ın etkinleştirilmiş bir proje için eklenen aşağıdaki öğeleri içerir:

* [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi.
* A [webjob yayımlama settings.json](#publishsettings) dağıtım ve Zamanlayıcı ayarlarını içeren dosya. 

![Dağıtım bir WebJob olarak etkinleştirmek için bir konsol uygulaması için eklenen gösteren diyagram](./media/websites-dotnet-deploy-webjobs/convert.png)

Bu öğeleri varolan bir konsol uygulaması projesine ekleyin veya yeni bir Web işleri özellikli konsol uygulaması projesi oluşturmak için bir şablonu kullanın. 

Bir proje tek başına bir WebJob olarak dağıtmak ve böylece web projesini dağıtma olduğunda otomatik olarak dağıtan bir web projesine bağlar. Visual Studio projeleri bağlamak için WebJobs etkin projedeki adını içeren bir [webjobs list.json](#webjobslist) web proje dosyasında.

![Bağlama Web projesine WebJob proje gösteren diyagram](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2015 kullanıyorsanız, yükleme [(Visual Studio 2015) .NET için Azure SDK'sı](https://azure.microsoft.com/downloads/).

Visual Studio 2017'yi kullanıyorsanız, yükleme [Azure geliştirme iş yükü](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---select-workloads).

## <a id="convert"></a> Mevcut bir konsol uygulaması projesi için Web işleri dağıtımı etkinleştir

İki seçeneğiniz vardır:

* [Otomatik dağıtım ile bir web projesi etkinleştirme](#convertlink).

  Mevcut bir konsol uygulama projesini, web projesini dağıtırken bir WebJob olarak otomatik olarak dağıttığı şekilde yapılandırın. İlgili web uygulama içinde çalıştırdığınız web uygulamasında, webjob'ı çalıştırmak istediğinizde bu seçeneği kullanın.

* [Bir web projesi olmadan dağıtımı etkinleştir](#convertnolink).

  Mevcut bir konsol uygulama projesini bir WebJob olarak tek başına bir web projesi bağlantı dağıtmak için yapılandırın. Bir webjob'ı bir web uygulamasında tek başına web uygulamasında çalışan hiçbir web uygulaması ile çalıştırmak istediğinizde bu seçeneği kullanın. WebJob kaynaklarınızı web uygulama kaynaklarınızı bağımsız olarak ölçeklendirme mümkün olması için bunu yapmak isteyebilirsiniz.

### <a id="convertlink"></a> Bir web projesi ile otomatik WebJobs dağıtımı etkinleştir

1. Web projesinde sağ **Çözüm Gezgini**ve ardından **Ekle** > **Azure WebJob olarak mevcut proje**.
   
    ![Azure Web işi olarak mevcut proje](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    [Azure WebJob Ekle](#configure) iletişim kutusu görüntülenir.
2. İçinde **proje adı** konsol uygulaması projesi bir WebJob olarak eklemek için aşağı açılan listesinde seçin.
   
    ![Azure WebJob Ekle iletişim kutusunda proje seçme](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Tamamlamak [Azure WebJob Ekle](#configure) iletişim ve ardından **Tamam**. 

### <a id="convertnolink"></a> Bir web projesi olmadan WebJobs dağıtımı etkinleştir
1. Konsol uygulaması projesine sağ tıklayın **Çözüm Gezgini**ve ardından **Azure WebJob olarak Yayımla...** . 
   
    ![Azure Web işi olarak Yayımla](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    [Azure WebJob Ekle](#configure) Seçili proje iletişim kutusu görünür **proje adı** kutusu.
2. Tamamlamak [Azure WebJob Ekle](#configure) iletişim kutusunu ve ardından **Tamam**.
   
   **Web'i Yayımla** Sihirbazı görünür.  Hemen Yayımla istemiyorsanız, sihirbazı kapatın. İstediğiniz zaman, girdiğiniz ayarları için kaydedilir [projeyi dağıtmak](#deploy).

## <a id="create"></a>WebJobs kullanan yeni bir proje oluşturun
WebJobs özellikli yeni bir proje oluşturmak için konsol uygulaması proje şablonu kullanın ve açıklandığı gibi WebJobs dağıtımı etkinleştir [önceki bölümde](#convert). Alternatif olarak, Web işleri yeni proje şablonu kullanabilirsiniz:

* [Bağımsız bir Web işi için WebJobs yeni proje şablonu kullanın](#createnolink)
  
    Bir proje oluşturun ve tek başına bir web projesi için hiçbir bağlantı içeren bir WebJob olarak dağıtmak için yapılandırın. Bir webjob'ı bir web uygulamasında tek başına web uygulamasında çalışan hiçbir web uygulaması ile çalıştırmak istediğinizde bu seçeneği kullanın. WebJob kaynaklarınızı web uygulama kaynaklarınızı bağımsız olarak ölçeklendirme mümkün olması için bunu yapmak isteyebilirsiniz.
* [Bir web projesine WebJob WebJobs yeni proje şablonu kullanıma bağlı](#createlink)
  
    Aynı çözüm içindeki bir web projesi dağıtıldığında otomatik olarak bir WebJob olarak dağıtmak için yapılandırılmış bir proje oluşturun. İlgili web uygulama içinde çalıştırdığınız web uygulamasında, webjob'ı çalıştırmak istediğinizde bu seçeneği kullanın.

> [!NOTE]
> WebJobs yeni proje şablonu otomatik olarak NuGet paketlerini yükler ve kodu içerir *Program.cs* için [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Web işleri SDK'yı kullanmak istemiyorsanız, kaldırmak veya değiştirmek `host.RunAndBlock` deyiminde *Program.cs*.
> 
> 

### <a id="createnolink"></a> Bağımsız bir Web işi için WebJobs yeni proje şablonu kullanın
1. Tıklayın **dosya** > **yeni proje**ve ardından **yeni proje** iletişim kutusunu tıklatıp **bulut**  >   **Azure Web işi (.NET Framework)**.
   
    ![WebJob şablon gösteren yeni proje iletişim kutusu](./media/websites-dotnet-deploy-webjobs/np.png)
2. Daha önce gösterilen yönergeleri izleyin [bağımsız bir WebJobs proje proje konsol uygulaması olun](#convertnolink).

### <a id="createlink"></a> Bir web projesine WebJob WebJobs yeni proje şablonu kullanıma bağlı
1. Web projesinde sağ **Çözüm Gezgini**ve ardından **Ekle** > **yeni Azure WebJob proje**.
   
    ![Yeni Azure WebJob proje menüsü girişi](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    [Azure WebJob Ekle](#configure) iletişim kutusu görüntülenir.
2. Tamamlamak [Azure WebJob Ekle](#configure) iletişim kutusunu ve ardından **Tamam**.

## <a id="configure"></a>Azure WebJob Ekle iletişim kutusu
**Azure WebJob Ekle** iletişim WebJob adı girin ve işinizi modu ayarı çalıştırmanıza olanak tanır. 

![Azure Web işi iletişim kutusu Ekle](./media/websites-dotnet-deploy-webjobs/aaw2.png)

Bu iletişim kutusunda alanları üzerinde alanlarına karşılık gelen **WebJob Ekle** Azure portal'ın iletişim. Daha fazla bilgi için [WebJobs ile arka planda çalıştır görevleri](web-sites-create-web-jobs.md).

> [!NOTE]
> * Komut satırı dağıtımı hakkında daha fazla bilgi için bkz: [etkinleştirme komut satırı veya Azure Web işleri sürekli teslim](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * WebJob'ı dağıtma ve Web işi ve yeniden dağıtma türünü değiştirmek istediğiniz karar verirseniz, silme gerekecektir *Web işleri yayımlama settings.json* dosya. WebJob türünü değiştirebilirsiniz, bu Visual Studio yayımlama seçeneklerini gösterme hale getirir.
> * WebJob dağıtma ve daha sonra çalışma modunda gelen sürekli sürekli olmayan veya tam tersi değiştirmek, Visual Studio, yeniden dağıtırken Azure'da yeni bir Web işi oluşturur. Diğer zamanlama ayarlarını değiştirme halde bırakın aynı çalıştırma modu veya zamanlanmış ve isteğe bağlı arasında geçiş, Visual Studio güncelleştirmeleri mevcut işi yerine yeni bir tane oluşturun.
> 
> 

## <a id="publishsettings"></a>webjob-publish-settings.json
Bir konsol uygulaması Web işleri dağıtımı için yapılandırdığınızda, Visual Studio'yu yükler [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi ve planlama bilgileri depolar bir *webjob yayımlama settings.json*  proje dosyasında *özellikleri* WebJobs proje klasörü. Bu dosyanın bir örnek aşağıda verilmiştir:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Visual Studio IntelliSense sağlar ve bu dosyayı doğrudan düzenleyebilirsiniz. Dosya şeması depolandı [ http://schemastore.org ](http://schemastore.org/schemas/json/webjob-publish-settings.json) ve orada görüntülenebilir.  

## <a id="webjobslist"></a>webjobs-list.json
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

Visual Studio IntelliSense sağlar ve bu dosyayı doğrudan düzenleyebilirsiniz. Dosya şeması depolandı [ http://schemastore.org ](http://schemastore.org/schemas/json/webjobs-list.json) ve orada görüntülenebilir.

## <a id="deploy"></a>WebJobs projesini dağıtma
Bir web projesine bağlı bir WebJobs proje web projesi ile otomatik olarak dağıtır. Web projesi dağıtımı hakkında daha fazla bilgi için bkz. **nasıl yapılır kılavuzları** > **Dağıt uygulama** sol gezinti bölmesinde.

WebJobs proje kendisi tarafından dağıtmak için projeye sağ **Çözüm Gezgini** tıklatıp **Azure WebJob olarak Yayımla...** . 

![Azure Web işi olarak Yayımla](./media/websites-dotnet-deploy-webjobs/paw.png)

Bir bağımsız Web işi, aynı için **Web'i Yayımla** web projeleri görünür, ancak daha az ayarları değiştirmek kullanılabilir olan kullanılan Sihirbazı.
