---
title: Application Insights Profiler ile ASP.NET Core Azure Linux web uygulamalarının profilini | Microsoft Docs
description: Kavramsal genel bakış ve Application Insights Profiler ' ı kullanma hakkında adım adım öğretici.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: db2972019e81ab413857d49ffa65eade3ff061ec
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051195"
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Application Insights Profiler ile ASP.NET Core Azure Linux web app'ler profil

Bu özellik şu anda önizleme sürümündedir.

Ne kadar zaman canlı web uygulamanızın her bir yöntemin kullanırken harcandığını Bul [Application Insights](../../application-insights/app-insights-overview.md). Application Insights Profiler, artık Linux'ta Azure App Service üzerinde barındırılan ASP.NET Core web uygulamaları için kullanılabilir. Bu kılavuz, Profiler izlemeleri ASP.NET Core Linux web apps için toplanabilir nasıl adım adım yönergeler sağlar.

Bu kılavuzu tamamladıktan sonra uygulamanızı Profiler izlemeleri izlemeler görüntüde gösterildiği gibi toplayabilirsiniz. Bu örnekte, Profiler izlemesi, belirli bir web isteği harcanan süre bekleme nedeniyle yavaş olduğunu gösterir. *Etkin yolu* uygulama yavaşlatıyor kodda bir Ateş simgesiyle işaretlenir. **Hakkında** yönteminde **HomeController** bölümü yöntemini çağırmak için web uygulaması yavaşlatmasını **net_thread_sleep** işlevi.

![Profiler izlemeleri](./media/profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki yönergeler, tüm Windows, Linux ve Mac geliştirme ortamları için geçerlidir:

* Yükleme [.NET Core SDK'sı 2.1.2'yi veya üzeri](https://dotnet.microsoft.com/download/archives).
* Yönergeleri izleyerek Git yükleme [Git'i yükledikten Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## <a name="set-up-the-project-locally"></a>Yerel olarak projesi kurun

1. Makinenizde bir komut istemi penceresi açın. Aşağıdaki yönergeler için tüm Windows, Linux ve Mac geliştirme ortamlarında çalışın.

2. Bir ASP.NET Core MVC web uygulaması oluşturun:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

3. Çalışma dizini, proje kök klasörüne değiştirin.

4. Profiler izlemeleri toplamak için NuGet paketini ekleyin:

    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

5. Bir kod satırını ekleyin **HomeController.cs** birkaç saniye rastgele gecikme bölümü:

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```

6. Kaydet ve yaptığınız değişiklikleri yerel depoya kaydedin:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>Projenizi barındırmak için Linux web uygulaması oluşturma

1. Linux üzerinde App Service kullanarak web uygulama ortamı oluşturun:

    ![Linux web uygulaması oluşturma](./media/profiler-aspnetcore-linux/create-linux-appservice.png)

2. Dağıtım kimlik bilgilerini oluşturun:

    > [!NOTE]
    > Web uygulamanızı dağıtırken daha sonra kullanmak üzere parolanızı kaydedin.

    ![Dağıtım kimlik bilgileri oluşturma](./media/profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Dağıtım seçenekleri seçin. Azure portalında yönergeleri izleyerek web uygulamasına yerel Git deposunda ayarlayın. Git deposunu otomatik olarak oluşturulur.

    ![Git deposu ayarlama](./media/profiler-aspnetcore-linux/setup-git-repo.png)

Daha fazla dağıtım seçeneği için bkz: [bu makalede](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Projenizi dağıtın

1. Komut İstemi penceresinde, projenizin kök klasörüne göz atın. App Service'te depoyu işaret etmek için bir Git uzak deposu ekleyin:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Kullanım **kullanıcıadı** dağıtım kimlik bilgilerini oluşturmak için kullanılır.
    * Kullanım **uygulama adı** Linux üzerinde App Service kullanarak web uygulamasını oluşturmak için kullanılan.

2. Projeyi Azure'a değişiklikleri ileterek dağıtın:

    ```
    git push azure master
    ```

Aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Web uygulamalarınızı izlemek için Application Insights ekleme

1. [Bir Application Insights kaynağı oluşturma](./../../application-insights/app-insights-create-new-resource.md).

2. Kopyalama **iKey** Application Insights kaynağının değeri ve web uygulamalarınızda aşağıdaki ayarlar:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Uygulama ayarlarını yapılandırma](./media/profiler-aspnetcore-linux/set-appsettings.png)

    Uygulama ayarları değiştiğinde, siteyi otomatik olarak yeniden başlatır. Yeni ayarları uyguladıktan sonra Profiler hemen için iki dakikada bir çalışır. Profiler sonra iki dakika başı çalıştırır.

3. Bazı Web trafiği oluşturur. Site yenileyerek trafik oluşturabilir **hakkında** sayfa birkaç kez.

4. Olaylara iki ila beş dakika bekleyin Application ınsights'a toplama.

5. Uygulama anlayışları'na göz atın **performans** bölmesinde Azure portalında. Profiler izlemeleri bölmenin sağ alt köşede görüntüleyebilirsiniz.

    ![Profiler izlemeleri görüntüleme](./media/profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="the-enable-action-in-the-profiler-configuration-pane-doesnt-work"></a>Profiler yapılandırma bölmesinde etkinleştir eylemi çalışmıyor

> [!NOTE]
> Linux üzerinde App Service'ı kullanarak uygulamanızı barındırın, Profiler içinde yeniden etkinleştirmeniz gerekmez **performans** Application Insights portalında bölmesinde. NuGet paketini projenize eklemek ve Application ınsights'ı **iKey** Profiler'ı etkinleştirmek için web app ayarlarınız içindeki değer.

Etkinleştirme iş akışını izleyin [için Application Insights Profiler Windows](./profiler.md) seçip **etkinleştirmek** içinde **Profiler yapılandırma** bölmesinde, bir hata alırsınız. Linux ortamında Profiler Aracısı Windows sürümünü yüklemek etkinleştir eylemine çalışır.

Bu sorun için bir çözüm üzerinde çalışıyoruz.

![Profiler performans bölmesinde yeniden etkinleştirmeyi deneyin yok](./media/profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Sonraki adımlar
Azure App Service tarafından barındırılan özel kapsayıcılar kullanırsanız, içindeki yönergeleri izleyin [ kapsayıcılı bir ASP.NET Core uygulaması Service Profiler'ı etkinleştirme](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) Application Insights Profiler ' ı etkinleştirmek için.

Herhangi bir probleminiz ya da Application Insights GitHub deposunu önerileri bildirin: [Applicationınsights-Profiler-AspNetCore: Sorunları](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
