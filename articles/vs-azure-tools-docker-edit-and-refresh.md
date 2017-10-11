---
title: "Yerel bir Docker kapsayıcısı uygulamalarında hata ayıklama | Microsoft Docs"
description: "Yerel bir Docker kapsayıcısı içinde çalışan bir uygulamayı değiştirmek öğrenin, düzenleme ve yenileme aracılığıyla kapsayıcı yenileyin ve hata ayıklama kesme noktaları ayarlayın"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: fcd58736d8915a61683a416fb9bf3892ba7b7bd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Yerel Docker kapsayıcısındaki uygulamalar için hata ayıklama
## <a name="overview"></a>Genel Bakış
Docker için Visual Studio Araçları, geliştirmek ve uygulamanızda yerel olarak Linux Docker kapsayıcısı doğrulamak için tutarlı bir yol sağlar.
Kapsayıcı bir kod değişikliği yaptığınız her zaman yeniden başlatmanız gerekmez.
Bu makalede "Düzenle ve Yenile" özelliği bir yerel Docker kapsayıcısı bir ASP.NET çekirdek Web uygulaması başlatın, gerekli değişiklikleri yapın ve bu değişiklikleri görmek için tarayıcıyı yenilemek için nasıl kullanılacağı gösterilmektedir.
Bu makalede ayrıca hata ayıklama kesme noktalarını ayarlama gösterilmektedir.

> [!NOTE]
> Windows kapsayıcı desteği gelecekteki bir sürümde geliyor
>
>

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki araçları yüklü olması gerekir.

* [Visual Studio en son sürümü](https://www.visualstudio.com/downloads/)
* [Microsoft ASP.NET Core 1.0 SDK'sı](https://go.microsoft.com/fwlink/?LinkID=809122)

Yerel olarak Docker kapsayıcıları çalıştırmak için bir yerel docker istemci gerekir.
Kullanabilirsiniz [Docker araç](https://www.docker.com/products/docker-toolbox)devre dışı bırakılması Hyper-V gerektirir veya kullanabilirsiniz [Windows için Docker](https://www.docker.com/get-docker), Hyper-V kullanır ve Windows 10 gerektirir.

Docker araç kullanıyorsanız, gerekecektir [Docker istemciyi Yapılandırma](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Web uygulaması oluşturma
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker desteği ekleme
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. Kod ve yenileme Düzenle
Hızlı bir şekilde değişiklikleri yinelemek için bir kapsayıcı içindeki uygulamanızı başlatın ve değişiklik yapmak IIS Express ile olduğu gibi bunları görüntüleme devam edebilirsiniz.

1. Çözüm yapılandırma kümesi `Debug` ve basın  **&lt;CTRL + F5 >** docker görüntünüzü oluşturmak ve yerel olarak çalıştırmak için.

    Kapsayıcı görüntü oluşturulduğundan ve Docker kapsayıcısı içinde çalışan sonra Visual Studio, varsayılan tarayıcıda Web uygulamasını başlatır.
    Microsoft Edge tarayıcı kullanıyorsanız veya aksi halde hatalar varsa, bkz: [sorun giderme](vs-azure-tools-docker-troubleshooting-docker-errors.md) bölümü.
2. Bizim değişiklik yapmak için burada yapacağız olan hakkında sayfasına gidin.
3. Visual Studio'ya dönmek ve açık `Views\Home\About.cshtml`.
4. Aşağıdaki HTML içeriğini dosyanın sonuna ekleyin ve değişiklikleri kaydedin.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. .NET derleme tamamlandığında ve bu satırları görmek için çıkış penceresine görüntüleme, tarayıcınızın geri geçin ve hakkında sayfayı yenileyin.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. Yaptığınız değişiklikleri uygulandı!

## <a name="4-debug-with-breakpoints"></a>4. Kesme noktaları ile hata ayıklama
Genellikle, değişiklikleri daha ayrıntılı denetleme, Visual Studio hata ayıklama özelliklerini yararlanarak gerekir.

1. Visual Studio'ya geri dönün ve açın`Controllers\HomeController.cs`
2. About() yöntemi içeriğini aşağıdakiyle değiştirin:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. Sol tarafında bir kesme noktası belirleyerek `string message`... satır.
4. İsabet  **&lt;F5 >** hata ayıklama başlatılamıyor.
5. Kesme noktası isabet hakkında sayfasına gidin.
6. Kesme noktası görüntülemek için Visual Studio geçin ve ileti değerini inceleyin.

   ![][2]

## <a name="summary"></a>Özet
İle [Docker için Visual Studio 2015 Araçları](https://aka.ms/DockerToolsForVS), Docker kapsayıcısı içinde geliştirmenin üretim daha iyi ile yerel olarak çalışan verimliliği elde edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[Visual Studio Docker geliştirme sorunlarını giderme](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Visual Studio, Windows ve Azure ile Docker hakkında daha fazla bilgi
* [Visual Studio için docker Araçları](http://aka.ms/dockertoolsforvs) -.NET Core kodunuzda bir kapsayıcı geliştirme
* [Visual Studio Team Services için docker Araçları](http://aka.ms/dockertoolsforvsts) - yapı ve docker kapsayıcıları dağıtın
* [Visual Studio Code için docker Araçları](http://aka.ms/dockertoolsforvscode) -daha fazla e2e senaryo gelen docker dosyalarını düzenlemek için dil Hizmetleri
* [Windows kapsayıcı bilgileri](http://aka.ms/containers)-Windows Server ve Nano Server bilgileri
* [Azure kapsayıcı hizmeti](https://azure.microsoft.com/services/container-service/) - [Azure kapsayıcı hizmeti içerik](http://aka.ms/AzureContainerService)
* Docker ile çalışmaya ilişkin daha fazla örnek için bkz: [Docker ile çalışma](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) gelen [tanıtımında](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). HealthClinic.biz tanıtımından daha fazla hızlı başlangıç ipuçları için bkz. [Azure Geliştirici Araçları Hızlı Başlangıç İpuçları](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Çeşitli Docker araçları
[Bazı harika docker Araçlar (Steve Lasker'ın blogu)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>İyi makaleler
[Mikro giriş NGINX gelen](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Sunular
* [Steve Lasker: VS Las Vegas 2016 - Docker e2e Canlı](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [ASP.NET Core giriş yapı 2016 - Burada, adresindeki Demo @](https://channel9.msdn.com/Events/Build/2016/B810)
* [Kapsayıcılardaki Channel 9 .NET uygulamaları geliştirme](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
