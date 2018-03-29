---
title: Azure’da ASP.NET Core web uygulaması oluşturma | Microsoft Docs
description: Varsayılan ASP.NET web uygulamasını dağıtarak Azure App Service'te web uygulamalarını çalıştırma hakkında bilgi edinin.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: 698f23507da0707a4612f8d33fe7e2995429f361
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-aspnet-core-web-app-in-azure"></a>Azure’da ASP.NET Core web uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux_ üzerinde App Service'e dağıtım yapmak için bkz. [Linux üzerinde App Service'te .NET Core web uygulaması oluşturma](./containers/quickstart-dotnetcore.md).
>

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıçta, Azure Web Apps’e ilk ASP.NET Core web uygulamanızı dağıtma işlemi gösterilmektedir. İşlemi tamamladığınızda bir App Service planı ve dağıtılmış web uygulaması ile Azure web uygulamasından oluşan kaynak grubunuz olacaktır.

> [!NOTE]
> ASP.NET Framework Web Uygulaması derleme ve dağıtma adımlarını arıyorsanız, ilgili makaleyi [burada](app-service-web-get-started-dotnet-framework.md) bulabilirsiniz. 
>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017</a>’yi aşağıdaki iş yükleri ile yükleyin:
    - **ASP.NET ve web geliştirme**
    - **Azure geliştirme**

    ![ASP.NET ve web geliştirme ile Azure geliştirme (Web ve Bulut altında)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Visual Studio'da **Dosya > Yeni > Proje**’yi seçerek bir proje oluşturun. 

**Yeni Proje** iletişim kutusunda **Visual C# > Web > ASP.NET Core Web Uygulaması** öğesini seçin.

Uygulamayı _myFirstAzureWebApp_ olarak adlandırın ve ardından **Tamam**’ı seçin.
   
![Yeni Proje iletişim kutusu](./media/app-service-web-get-started-dotnet/new-project.png)

Azure’a herhangi bir türde ASP.NET Core web uygulaması dağıtabilirsiniz. Bu hızlı başlangıçta **Web Uygulaması** şablonunu seçin ve kimlik doğrulamasının **Kimlik Doğrulaması Yok** olarak ayarlandığından emin olun.
      
**Tamam**’ı seçin.

![Yeni ASP.NET Projesi iletişim kutusu](./media/app-service-web-get-started-dotnet/razor-pages-aspnet-dialog.png)

ASP.NET Core projesi oluşturulduktan sonra, başlamanıza yardımcı olacak çeşitli kaynak bağlantıları sağlayan ASP.NET Core karşılama sayfası gösterilir. 

![Karşılama Sayfası](./media/app-service-web-get-started-dotnet/aspnet-core-welcome-page.png)

Menüden **Hata Ayıkla > Hata Ayıklamadan Başla**’yı seçerek web uygulamasını yerel olarak çalıştırın.

![Uygulamayı yerel olarak çalıştırma](./media/app-service-web-get-started-dotnet/razor-web-app-running-locally.png)

## <a name="publish-to-azure"></a>Azure’da Yayımlama

**Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-get-started-dotnet/right-click-publish.png)

**Microsoft Azure App Service**’in seçili olduğundan emin olup **Yayımla**’yı seçin.

![Projeye genel bakış sayfasından yayımlama](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Bu işlem, ASP.NET Core web uygulamasını Azure’da çalıştırmak için gereken tüm Azure kaynaklarını oluşturmanıza yardımcı olan **App Service Oluştur** iletişim kutusunu açar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

**App Service Oluştur** iletişim kutusunda **Hesap ekle**’yi seçin ve Azure aboneliğinizde oturum açın. Oturumunuz zaten açıksa, açılan menüden istediğiniz aboneliği içeren hesabı seçin.

> [!NOTE]
> Zaten oturum açtıysanız **Oluştur** öğesini henüz seçmeyin.
>
>
   
![Azure'da oturum açma](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesini seçin.

Kaynak grubunuzu **myResourceGroup** olarak adlandırıp **Tamam**’ı seçin.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

**App Service Planı**’nın yanındaki **Yeni**’yi seçin. 

**App Service Planını Yapılandır** iletişim kutusunda, ekran görüntüsünü izleyerek tablodaki ayarları kullanın.

![App Service planı oluşturma](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Ayar | Önerilen Değer | Açıklama |
|-|-|-|
|App Service Planı| myAppServicePlan | App Service planının adı. |
| Konum | Batı Avrupa | Web uygulamasının barındırıldığı veri merkezi. |
| Boyut | Ücretsiz | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |

**Tamam**’ı seçin.

## <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

**Web Uygulaması Adı**’na benzersiz bir ad girin (geçerli karakterler `a-z`, `0-9` ve `-` karakterleridir) veya otomatik olarak oluşturulan adı kabul edin. Web uygulamasının URL'si `http://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, web uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

![Web uygulaması adını yapılandırma](./media/app-service-web-get-started-dotnet/web-app-name.png)

Sihirbaz tamamlandıktan sonra ASP.NET Core web uygulamasını Azure’da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

[Oluşturma ve yayımlama adımında](#create-and-publish-the-web-app) belirtilen web uygulaması adı `http://<app_name>.azurewebsites.net` biçiminde URL ön eki olarak kullanılır.

Tebrikler, ASP.NET Core web uygulamanız Azure App Service’te çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

**Çözüm Gezgini** menüsünden _Pages/Index.cshtml_ dosyasını açın.

Üst kısımda `<div id="myCarousel" class="carousel slide" data-ride="carousel" data-interval="6000">` HTML etiketini bulun ve tüm öğeyi aşağıdaki kodla değiştirin:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

Yayımlama sayfasında **Yayımla**'yı seçin.

Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

![Azure’da güncelleştirilmiş ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/web-app-running-live-updated.png)

## <a name="manage-the-azure-web-app"></a>Azure web uygulamasını yönetme

Web uygulamasını yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri**'ni ve ardından Azure web uygulamanızın adını seçin.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-dotnet/access-portal.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. 

![Azure portalında App Service dikey penceresi](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL Veritabanı ile ASP.NET Core](app-service-web-tutorial-dotnetcore-sqldb.md)
