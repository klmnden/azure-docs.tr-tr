---
title: Yerel bir Docker kapsayıcısı uygulamalarında hata ayıklama | Microsoft Docs
description: Yerel bir Docker kapsayıcısı içinde çalışan bir uygulamayı değiştirmek öğrenin, kapsayıcı düzenleme ve yenileme aracılığıyla yenileyin ve kesme noktaları hata ayıklama ayarlayın
services: azure-container-service
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: 01741ba25ac3a6ab187a08636b7e26ee58dbee90
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054757"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Yerel Docker kapsayıcısındaki uygulamalar için hata ayıklama
## <a name="overview"></a>Genel Bakış
Visual Studio 2017 Linux Docker kapsayıcısı içinde geliştirin ve uygulamanızı yerel olarak doğrulamak için tutarlı bir yol sağlar.
Kapsayıcı bir kod değişikliği yaptığınızda her zaman yeniden başlatmanız gerekmez.
Bu makalede, yerel bir Docker kapsayıcısında ASP.NET Core Web uygulaması başlatın, gerekli değişiklikleri yapın ve ardından bu değişiklikleri görmek için tarayıcıyı yenilemek için "Düzenle ve Yenile" özelliğini kullanmayı göstermektedir.
Bu makalede ayrıca hata ayıklama için kesme noktaları ayarlama işlemini gösterir.

> [!NOTE]
> Windows kapsayıcı desteği gelecekteki bir sürümde kullanıma sunulacaktır
>
>

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki araçlar yüklü olması gerekir.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)
* [Microsoft ASP.NET Core 1.0 SDK'sı](https://go.microsoft.com/fwlink/?LinkID=809122)

Yerel olarak Docker kapsayıcıları çalıştırmak için bir yerel docker istemcisi gerekir.
Kullanabileceğiniz [Docker araç kutusu](https://www.docker.com/products/docker-toolbox), Hyper-V devre dışı bırakılmasını gerektiren veya kullanabilirsiniz [için Docker Windows](https://www.docker.com/get-docker), Hyper-V kullanır ve Windows 10 gerektirir.

Docker araç kutusu kullanıyorsanız gerekecektir [Docker istemciyi Yapılandırma](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Web uygulaması oluşturma
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker desteği ekleme
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. Kodunuz ve yenileme Düzenle
Değişikliklerini hızlıca yinelemek için bir kapsayıcı içindeki uygulamanızı başlatın ve değişiklik yapmak, IIS Express ile olduğu gibi bunları görüntüleme devam edebilirsiniz.

1. Çözüm yapılandırması ayarlanmış `Debug` basın  **&lt;CTRL + F5 tuşlarına basarak >** , docker görüntüsünü derleyin ve yerel olarak çalıştırın.

    Kapsayıcı görüntüsü oluşturuldu ve bir Docker kapsayıcısı içinde çalışırken sonra Visual Studio Web uygulamasını varsayılan tarayıcınızda başlatır.
    Aksi takdirde hatalar var veya Microsoft Edge tarayıcı kullanıyorsanız, bkz. [sorun giderme](vs-azure-tools-docker-troubleshooting-docker-errors.md) bölümü.
2. Bizim değişiklik yapmak için burada ekleyeceğiz olan hakkında sayfasına gidin.
3. Visual Studio'ya geri dönün ve açık `Views\Home\About.cshtml`.
4. Aşağıdaki HTML içeriği dosyanın sonuna ekleyin ve değişiklikleri kaydedin.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Çıkış penceresi, .NET derleme tamamlandığında ve şu satırları görmeniz görüntüleme, tarayıcınızın geri geçiş ve hakkında sayfayı yenileyin.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. Yaptığınız değişiklikleri uygulanmış!

## <a name="4-debug-with-breakpoints"></a>4. Kesme noktaları ile hata ayıklama
Genellikle, değişiklikleri daha fazla inceleme, Visual Studio hata ayıklama özelliklerinin yararlanarak gerekir.

1. Visual Studio'ya geri dönün ve açın `Controllers\HomeController.cs`
2. About() yönteminin içeriklerini aşağıdakiyle değiştirin:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. Sol tarafında bir kesme noktası ayarlamak `string message`... satır.
4. İsabet  **&lt;F5 >** hata ayıklama başlatılamıyor.
5. Kesme noktasına isabet hakkında sayfasına gidin.
6. Kesme noktası görüntülemek için Visual Studio'ya geçiş yapın ve ileti değerini denetleyin.

   ![][2]

## <a name="summary"></a>Özet
Visual Studio 2017'de Docker desteği sayesinde, yerel olarak geliştirme konusunda bir Docker kapsayıcısı içinde üretim gerçekçilik çalışma verimliliği elde edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[Visual Studio Docker geliştirme sorunlarını giderme](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Visual Studio, Windows ve Azure ile Docker hakkında daha fazla bilgi
* [Visual Studio Team Services için docker Araçları](http://aka.ms/dockertoolsforvsts) - derleme ve docker kapsayıcıları dağıtma
* [Visual Studio Code için docker Araçları](http://aka.ms/dockertoolsforvscode) -yakında daha fazla e2e senaryoları ile docker dosyalarını düzenlemek için dil Hizmetleri
* [Windows kapsayıcı bilgileri](http://aka.ms/containers)-Windows Server ve Nano sunucu bilgileri
* [Azure Container Service'i](https://azure.microsoft.com/services/container-service/) - [Azure kapsayıcı hizmeti içeriği](http://aka.ms/AzureContainerService)
* Docker ile çalışmaya ilişkin daha fazla örnek için bkz: [Docker ile çalışma](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) gelen [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [tanıtım](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). HealthClinic.biz tanıtımından daha fazla hızlı başlangıç ipuçları için bkz. [Azure Geliştirici Araçları Hızlı Başlangıç İpuçları](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Çeşitli Docker araçları
[Bazı harika docker Araçları (Steve Lasker'ın blogu)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>İyi makaleler
[Mikro hizmetler nginx'ten giriş](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Sunular
* [Steve Lasker: VS, Las Vegas 2016 - Docker e2e Canlı](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Build 2016 - Burada, en tanıtım @ ASP.NET Core'a giriş](https://channel9.msdn.com/Events/Build/2016/B810)
* [Kapsayıcılar, Channel 9, .NET uygulamaları geliştirme](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
