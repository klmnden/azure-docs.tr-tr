---
title: Geliştirme ve Visual Studio - Azure kullanarak Web işleri dağıtma
description: Geliştirmek ve Azure Web işleri Visual Studio kullanarak Azure App Service'e dağıtma hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: tdykstra
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: babe190c0865f5be4aeecb40ca48b52673c6920e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Geliştirme ve Visual Studio - Azure uygulama hizmeti kullanarak Web işleri dağıtma

## <a name="overview"></a>Genel Bakış

Bu konuda, bir web uygulamasında bir konsol uygulama projesi dağıtmak için Visual Studio kullanımı açıklanmaktadır [uygulama hizmeti](app-service-web-overview.md) olarak bir [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226). Web işleri kullanarak dağıtma hakkında bilgi için [Azure portal](https://portal.azure.com), bkz: [Web işleri ile Çalıştır arka plan görevleri](web-sites-create-web-jobs.md).

Visual Studio Web işleri etkin konsol uygulama projesi dağıtırken, iki görevleri gerçekleştirir:

* Web uygulamasında uygun klasöre çalışma zamanı dosyalarını kopyalar (*App_Data/işleri/sürekli* sürekli Webjob'lar için *App_Data/işleri/tetiklenen* zamanlanmış ve isteğe bağlı Webjob için).
* Ayarlayan [Azure Scheduler](https://docs.microsoft.com/azure/scheduler/) belirli zamanlarda çalışmak üzere zamanlanmış Web işleri için işler. (Bu sürekli Webjob'lar için gerekli değildir.)

WebJobs kullanan bir proje için eklenen aşağıdaki öğeleri içerir:

* [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi.
* A [webjob yayımlama settings.json](#publishsettings) dağıtım ve Zamanlayıcı ayarlarını içeren dosya. 

![Bir Web işi olarak dağıtımını etkinleştirmek için bir konsol uygulaması için eklenen gösteren diyagram](./media/websites-dotnet-deploy-webjobs/convert.png)

Bu öğeleri varolan bir konsol uygulaması projesine ekleyin veya yeni bir Web işleri etkin konsol uygulama projesi oluşturmak için bir şablon kullanın. 

Bir Web işi tek başına bir proje dağıtma veya böylece web projesini dağıtma olduğunda otomatik olarak dağıtan bir web projesi Bağla. Visual Studio projeleri bağlamak için Web işleri etkin projesinde adını içeren bir [webjobs list.json](#webjobslist) web projesi dosyasında.

![Web projesine bağlama Web işi projesinin gösteren diyagram](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2015 kullanıyorsanız, yükleme [(Visual Studio 2015) .NET için Azure SDK](https://azure.microsoft.com/downloads/).

Visual Studio 2017 kullanıyorsanız, yükleme [Azure geliştirme iş yükü](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---select-workloads).

## <a id="convert"></a> Mevcut bir konsol uygulama projesi için Web işleri dağıtımı etkinleştir

İki seçeneğiniz vardır:

* [Bir web projesi ile otomatik dağıtımı etkinleştirmek](#convertlink).

  Bir web projesi dağıttığınızda, otomatik olarak bir Web işi olarak dağıtan mevcut bir konsol uygulama projesini yapılandırın. İlgili web uygulamasını çalıştıran web uygulamasında, WebJob çalıştırmak istediğinizde bu seçeneği kullanın.

* [Bir web projesi dağıtımıdır etkinleştirmek](#convertnolink).

  Bir Web işi tek başına bir web projesi bağlantısı dağıtmak için mevcut bir konsol uygulama projesi yapılandırın. Bir Web işi, bir web uygulamasında kendisi tarafından web uygulamasında çalışan hiçbir web uygulaması ile çalıştırmak istediğinizde bu seçeneği kullanın. Web uygulaması kaynaklarınıza bağımsız olarak, Web işi kaynaklarınızı ölçeklemek için bunun isteyebilirsiniz.

### <a id="convertlink"></a> Bir web projesi ile otomatik WebJobs dağıtımı etkinleştir

1. Web projesine sağ tıklayın **Çözüm Gezgini**ve ardından **Ekle** > **Azure Web işi olarak mevcut proje**.
   
    ![Azure Web işi olarak mevcut proje](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    [Azure Web işine Ekle](#configure) iletişim kutusu görüntülenir.
2. İçinde **proje adı** konsol uygulaması proje bir Web işi eklemek için aşağı açılan listesinde seçin.
   
    ![Azure Web işine Ekle iletişim kutusunda proje seçme](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Tamamlamak [Azure Web işine Ekle](#configure) iletişim ve ardından **Tamam**. 

### <a id="convertnolink"></a> Bir web projesi olmadan Web işleri dağıtımı etkinleştir
1. Konsol uygulaması projesine sağ tıklayın **Çözüm Gezgini**ve ardından **Azure Web işi olarak Yayımla...** . 
   
    ![Azure Web işi Yayımla](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    [Azure Web işine Ekle](#configure) iletişim kutusu görüntülenirse, seçili proje ile **proje adı** kutusu.
2. Tamamlamak [Azure Web işine Ekle](#configure) iletişim kutusunu ve ardından **Tamam**.
   
   **Web'i Yayımla** Sihirbazı görünür.  Hemen yayımlamak istemiyorsanız sihirbazı kapatın. Aşağıdakileri yapmak istediğinizde, girdiğiniz ayarları için kaydedilir [projesini dağıtma](#deploy).

## <a id="create"></a>Yeni bir Web işleri etkin projesi oluşturma
Web işleri etkin yeni bir proje oluşturmak için konsol uygulaması proje şablonunu kullanın ve Web işleri dağıtım açıklandığı gibi etkinleştirin [önceki bölümde](#convert). Alternatif olarak, Web işleri yeni proje şablonunu kullanarak şunları yapabilirsiniz:

* [Bağımsız bir Web işi için Web işleri yeni proje şablonu kullanın](#createnolink)
  
    Proje oluşturma ve tek başına bir web projesi bağlantısı ile bir Web işi olarak dağıtmak için yapılandırın. Bir Web işi, bir web uygulamasında kendisi tarafından web uygulamasında çalışan hiçbir web uygulaması ile çalıştırmak istediğinizde bu seçeneği kullanın. Web uygulaması kaynaklarınıza bağımsız olarak, Web işi kaynaklarınızı ölçeklemek için bunun isteyebilirsiniz.
* [Bir web projesi bağlı bir Web işi için Web işleri yeni proje şablonu kullanın](#createlink)
  
    Bir web projesi aynı çözümde dağıtıldığında bir Web işi olarak otomatik olarak dağıtmak için yapılandırılmış bir proje oluşturun. İlgili web uygulamasını çalıştıran web uygulamasında, WebJob çalıştırmak istediğinizde bu seçeneği kullanın.

> [!NOTE]
> Web işleri yeni proje şablonu otomatik olarak NuGet paketlerini yükler ve kod içerir *Program.cs* için [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). WebJobs SDK'yı kullanmak istemiyorsanız, kaldırma veya değiştirme `host.RunAndBlock` deyiminde *Program.cs*.
> 
> 

### <a id="createnolink"></a> Bağımsız bir Web işi için Web işleri yeni proje şablonu kullanın
1. Tıklatın **dosya** > **yeni proje**ve ardından **yeni proje** iletişim kutusunda **bulut**  >   **Azure Web işi (.NET Framework)**.
   
    ![Web işi şablon gösteren yeni proje iletişim kutusu](./media/websites-dotnet-deploy-webjobs/np.png)
2. Önceki için gösterilen yönergeleri izleyin [bağımsız bir Web işleri proje proje konsol uygulaması olun](#convertnolink).

### <a id="createlink"></a> Bir web projesi bağlı bir Web işi için Web işleri yeni proje şablonu kullanın
1. Web projesine sağ tıklayın **Çözüm Gezgini**ve ardından **Ekle** > **yeni Azure Web işi projesi**.
   
    ![Yeni Azure Web işi projesinin menü girişi](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    [Azure Web işine Ekle](#configure) iletişim kutusu görüntülenir.
2. Tamamlamak [Azure Web işine Ekle](#configure) iletişim kutusunu ve ardından **Tamam**.

## <a id="configure"></a>Azure Web işine Ekle iletişim kutusu
**Azure Web işine Ekle** iletişim Web işi adı girin ve modu ayarı, Web işi için çalıştırmanıza olanak sağlar. 

![Azure Web işi iletişim ekleyin](./media/websites-dotnet-deploy-webjobs/aaw2.png)

Bu iletişim kutusunu alanları üzerindeki alanlarına karşılık gelen **WebJob Ekle** Azure portalının iletişim. Daha fazla bilgi için bkz: [Web işleri ile Çalıştır arka plan görevleri](web-sites-create-web-jobs.md).

> [!NOTE]
> * Komut satırı dağıtımı hakkında daha fazla bilgi için bkz: [etkinleştirme komut satırı veya Azure Web işleri sürekli teslim](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Bir Web işi dağıtın ve ardından Web işi ve yeniden dağıtın türünü değiştirmek istediğiniz karar verirseniz, silmeniz gerekir *Web işleri yayımlama settings.json* dosya. Web işi türünü değiştirebilmeniz için bu Visual Studio yayımlama seçeneklerini gösterme hale getirir.
> * Bir Web işi dağıtırsanız ve daha sonra çalışma modunda gelen sürekli sürekli olmayan veya tersi değiştirmek Visual Studio, yeniden dağıtırken Azure'da yeni bir Web işi oluşturur. Diğer zamanlama ayarlarını değiştirmek, ancak bırakın aynı çalıştırma modu veya zamanlanmış ve isteğe bağlı arasında geçiş yapma, Visual Studio güncelleştirmeleri varolan işi yerine yeni bir tane oluşturun.
> 
> 

## <a id="publishsettings"></a>webjob-publish-settings.json
Bir konsol uygulaması Web işleri dağıtımı için yapılandırdığınızda, Visual Studio yükler [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi ve planlama bilgilerini de depolar bir *webjob yayımlama settings.json*  proje dosyasında *özellikleri* WebJobs projesinin klasör. Bu dosyayı bir örneği burada verilmiştir:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Visual Studio IntelliSense sağlar ve bu dosyayı doğrudan düzenleyebilirsiniz. Dosya şeması konumunda depolanan [ http://schemastore.org ](http://schemastore.org/schemas/json/webjob-publish-settings.json) ve orada görüntülenebilir.  

## <a id="webjobslist"></a>webjobs-list.json
WebJobs kullanan bir proje bir web projesi bağladığınızda, Visual Studio Web işleri projesinde adını depolar bir *webjobs list.json* web projesinin dosyasında *özellikleri* klasör. Liste, aşağıdaki örnekte gösterildiği gibi birden çok Web işleri projeleri içerebilir:

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

Visual Studio IntelliSense sağlar ve bu dosyayı doğrudan düzenleyebilirsiniz. Dosya şeması konumunda depolanan [ http://schemastore.org ](http://schemastore.org/schemas/json/webjobs-list.json) ve orada görüntülenebilir.

## <a id="deploy"></a>Web işleri projesini dağıtma
Bir web projesi bağlı bir Web işleri proje ile web projesi otomatik olarak dağıtır. Web projesi dağıtımı hakkında daha fazla bilgi için bkz: **nasıl yapılır kılavuzları** > **dağıtma uygulama** sol gezinti bölmesinde.

Tek başına bir Web işleri projeyi dağıtmak için projeye sağ **Çözüm Gezgini** tıklatıp **Azure Web işi olarak Yayımla...** . 

![Azure Web işi Yayımla](./media/websites-dotnet-deploy-webjobs/paw.png)

Bir bağımsız Web işi için aynı **Web'i Yayımla** web projeleri görünür, ancak daha az ayarları değiştirmek kullanılabilir olan kullanılan Sihirbazı.
