---
title: "Profil ASP.NET core Azure Linux web apps uygulama Öngörüler Profil Oluşturucu ile | Microsoft Docs"
description: "Kavram genel bakış ve onu etkinleştirme hakkında adım adım öğretici"
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 7f3a0a1a22e14f12b86474dd5b1985029074444e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profil ASP.NET Core Azure Linux Web uygulamaları ile uygulama Öngörüler Profil Oluşturucu

Bu özellik şu anda önizlemede değil

Ne kadar süre canlı web uygulamanızın her bir yöntemin kullanırken harcanan Bul [Application Insights](app-insights-overview.md). Profil Oluşturucu artık Linux uygulama hizmetleri üzerinde barındırılan ASP.NET çekirdek web uygulamaları için kullanılabilir. Bu kılavuz profil oluşturucu izlemeleri ASP.NET core Linux web uygulamaları için toplanabilir nasıl adım adım yönergeler sağlar.

Bu kılavuzu tamamladıktan sonra uygulamanızı profil oluşturucu izlemelerini aşağıdaki ekran görüntüsüne benzer toplar. Bu örnekte profil oluşturucu izleme bekliyor harcanan çoğu zaman çünkü belirli web isteği yavaş gösterir. Uygulamasını yavaşlatabilecek kodu etkin yolunda yangın simgesiyle gelmelidir. Bu örnekte gösterilir `About` yönteminde `HomeController` çağırma için web uygulamasını yavaşlatabilecek `Thread.Sleep`.

![Profil Oluşturucu izlemeleri](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="pre-requisites"></a>Ön koşullar
Tüm Windows, Linux ve Mac geliştirme ortamlarını uygulanmış aşağıdaki yönergeleri:

* Yükleme [.NET core SDK 2.1.2 veya daha yenisi](https://www.microsoft.com/net/download/windows/build)
* Kısmındaki yönergeleri izleyerek Git yükleme [Git yükleme Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="setup-project-locally"></a>Yerel olarak Kurulum projesi

1. Makinenizde bir komut istemi açın. Aşağıdaki yönergeleri tüm Windows, Linux ve Mac geliştirme ortamları için çalışır.

2. Bir ASP.NET core MVC web uygulaması oluşturma
    ```
    dotnet new mvc -n LinuxProfilerTest
    ```
3. Proje kök klasörü için komut istemindeki Dizin Değiştir

4. Profil Oluşturucu izlemeleri toplamak için Nuget paketini ekleyin.
    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```
5. Birkaç saniye içinde HomeController.cs rastgele gecikme kod satırını ekleyin

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
6. Kaydet ve yerel depo, değişiklikleri

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-azure-app-service-for-hosting-your-project"></a>Projenizi barındırmak için Azure App Service oluştur
1. Uygulama Hizmetleri Linux ortam oluşturma

    ![Linux uygulama hizmetleri oluşturma](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Dağıtım kimlik bilgileri oluşturun. Bu daha sonra uygulamanızı dağıtırken ihtiyacınız parolanızı not alın.

    ![Dağıtım kimlik bilgileri oluşturun](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Dağıtım seçeneğini seçin. Azure Portal'da yönergeleri izleyerek web uygulamasında yerel bir Git deposu ayarlama. Bir Git deposu otomatik olarak oluşturulur.

    ![Git deposu Kurulumu](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Daha fazla dağıtım seçenekleri mevcuttur [burada](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type)

## <a name="deploy-your-project"></a>Projenizi dağıtma

1. Komut İstemi'nde, proje kök klasörüne gidin. Uygulama hizmetleri üzerinde bir işaret edecek şekilde Git uzak deponuza ekleyin:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```
    * "Dağıtım kimlik bilgileri oluşturun." adımından 'username' kullanın
    * 'Uygulama hizmeti oluşturun.", adımdan 'app name' kullanın

2. Projeyi Azure'a değişiklikleri ileterek dağıtın

    ```
    git push azure master
    ```
Aşağıdakine benzer bir çıktı görürsünüz:

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
1. [Application Insights kaynağı oluşturma](./app-insights-create-new-resource.md)
2. Application Insights kaynağına ait iKey kopyalayın ve aşağıdaki ayarlar, uygulama hizmetleri

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Uygulama ayarlarını belirleme](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Uygulama ayarlarını değiştirme otomatik olarak site yeniden başlatılır. Yeni ayarları uygulandıktan sonra Profil Oluşturucu hemen 2 dakika başlatılır. Ardından, 2 dakika saatte çalışır.

3. Bazı trafiği Web sitenize oluşturur. Siteyi Yenile ```About``` için sayfa birkaç kez.

4. Olaylar için Application Insights kümelenebilir için 2-5 dakika bekleyin.

5. Azure portalında Application Insights performans bölmesine gidin. Profil Oluşturucu izlemeleri sağ alt köşedeki kullanılabilir görürsünüz.

    ![Görünüm izlemeleri](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="report-issues-to-project-github-repository"></a>Proje Github deposuna rapor sorunları
Github depomuzda sorunları veya önerileri varsa, lütfen bildirin: [Applicationınsights profil oluşturucu AspNetCore: sorunları](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues)
