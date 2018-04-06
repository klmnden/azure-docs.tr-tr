---
title: Profil ASP.NET Core Azure Linux web uygulamalarını uygulama Öngörüler Profil Oluşturucu ile | Microsoft Docs
description: Kavramsal genel bakış ve uygulama Öngörüler profil oluşturucu kullanma hakkında adım adım öğretici.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 5596c4efeba14e9d2bfdadd7ce92bb6b2c9fcbf0
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Uygulama Öngörüler Profil Oluşturucu ile profil ASP.NET Core Azure Linux web uygulamaları

Bu özellik şu anda önizleme sürümündedir.

Ne kadar süre canlı web uygulamanızın her bir yöntemin kullanırken harcanan Bul [Application Insights](app-insights-overview.md). Uygulama Öngörüler profil oluşturucu artık Linux Azure App Service üzerinde barındırılan ASP.NET Core web uygulamaları için kullanılabilir. Bu kılavuz, profil oluşturucu izlemeleri ASP.NET Core Linux web uygulamaları için toplanabilir nasıl adım adım yönergeler sağlar.

Bu kılavuzu tamamladıktan sonra uygulamanızı profil oluşturucu izlemeleri görüntüde gösterilen izlemeleri gibi toplayabilirsiniz. Bu örnekte profil oluşturucu izleme belirli web isteği nedeniyle harcanan süre bekleme yavaş gösterir. *Etkin yolunuzda* uygulama yavaşlamasının kodda bir yangın simgesiyle işaretlenir. **Hakkında** yönteminde **HomeController** bölüm yöntemi çağırmak için web uygulaması yavaşlamasının **Thread.Sleep** işlevi.

![Profil Oluşturucu izlemeleri](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Önkoşullar
Tüm Windows, Linux ve Mac geliştirme ortamlar için aşağıdaki yönergeleri uygulayın:

* Yükleme [.NET Core SDK 2.1.2 ya da daha sonra](https://www.microsoft.com/net/download/windows/build).
* Kısmındaki yönergeleri izleyerek Git yükleyin [Git yükleme Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## <a name="set-up-the-project-locally"></a>Projeyi yerel olarak ayarlayın

1. Makinenizde bir komut istemi penceresi açın. Aşağıdaki yönergeler için tüm Windows, Linux ve Mac geliştirme ortamlarında çalışır.

2. Bir ASP.NET Core MVC web uygulaması oluşturun:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

3. Proje için kök klasör için çalışma dizini değiştirin.

4. Profil Oluşturucu izlemeleri toplamak için NuGet paketini ekleyin:

    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

5. Kod satırını ekleyin **HomeController.cs** birkaç saniye rastgele gecikme için bölümü:

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

6. Kaydet ve yerel depo, değişiklikleri uygulayın:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>Projenizi barındırmak için Linux web uygulaması oluşturma

1. Web uygulama ortamı Linux'ta uygulama hizmeti kullanarak oluşturun:

    ![Linux web uygulaması oluşturma](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Dağıtım kimlik bilgileri oluşturun:

    > [!NOTE]
    > Parolanızı daha sonra web uygulamanızın dağıtırken kullanmak üzere kaydedin.

    ![Dağıtım kimlik bilgileri oluşturun](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Dağıtım seçenekleri seçin. Azure Portal'da yönergeleri izleyerek yerel bir Git deposu web uygulamasında ayarlayın. Bir Git deposu otomatik olarak oluşturulur.

    ![Git deposu ayarlama](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Daha fazla dağıtım seçenekleri için bkz [bu makalede](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Projenizi dağıtma

1. Komut İstemi penceresinde, projeniz için kök klasöre göz atın. Uygulama hizmeti havuzda işaret etmek için bir Git uzak deponuza ekleyin:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Kullanım **kullanıcıadı** dağıtım kimlik bilgileri oluşturmak için kullanılır.
    * Kullanım **uygulama adı** Linux'ta uygulama hizmetini kullanarak web uygulaması oluşturmak için kullanılır.

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

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Web uygulamalarınızı izlemek için Application Insights Ekle

1. [Application Insights kaynağı oluşturma](./app-insights-create-new-resource.md).

2. Kopya **iKey** Application Insights kaynağı değerini ve web uygulamalarınızı aşağıdaki ayarları ayarlayın:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Uygulama ayarlarını yapılandırın](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Uygulama ayarları değiştirildiğinde, sitenin otomatik olarak yeniden başlatılır. Yeni ayarları uyguladıktan sonra Profil Oluşturucu iki dakika hemen çalıştırır. Profil Oluşturucu sonra iki dakika saatte çalışır.

3. Bazı trafiği Web sitenize oluşturur. Site yenileyerek trafiği oluşturabilir **hakkında** sayfa birkaç kez.

4. Olaylara iki için beş dakika bekleyin Application Insights toplama.

5. Application ınsights'a **performans** Azure portalında bölmesi. Profil Oluşturucu izlemeleri bölmesinin sağ alt köşede görüntüleyebilirsiniz.

    ![Profil Oluşturucu izlemeleri görüntüleme](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="the-enable-action-in-the-profiler-configuration-pane-doesnt-work"></a>Profil Oluşturucu yapılandırma bölmesinde etkinleştir eylemi çalışmıyor

> [!NOTE]
> Uygulama hizmeti Linux'ta kullanarak uygulamanızı barındırıyorsanız, Profil Oluşturucusu'nda yeniden etkinleştirmeniz gerekmez **performans** Application Insights portalındaki bölmesinde. NuGet paketini projenize eklemek ve Application ınsights **iKey** profil oluşturucu etkinleştirmek için web uygulaması ayarları değeri.

Etkinleştirme iş akışını izlerseniz [için Application Insights profil oluşturucu Windows](./app-insights-profiler.md) seçip **etkinleştirmek** içinde **yapılandırma profil oluşturucu** bölmesinde, bir hata alıyorsunuz. Linux ortamda Profil Oluşturucu aracı Windows sürümünü yüklemek etkinleştir eylemine çalışır.

Bu sorun için çözüm üzerinde çalışıyoruz.

![Profil Oluşturucu performans bölmesinde yeniden etkinleştirmeyi deneyin yok](./media/app-insights-profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Sonraki adımlar
Azure App Service tarafından barındırılan özel kapsayıcılar kullanırsanız, içindeki yönergeleri izleyin [ kapsayıcılı ASP.NET Core uygulama için hizmet profil oluşturucu etkinleştirmek](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) uygulama Öngörüler profil oluşturucu etkinleştirmek için.

Herhangi bir probleminiz ya da uygulama Öngörüler GitHub deposuna önerileri Rapor: [Applicationınsights profil oluşturucu AspNetCore: sorunları](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
