---
title: "Azure’da ASP.NET web uygulaması oluşturma | Microsoft Docs"
description: "Varsayılan ASP.NET web uygulamasını dağıtarak Azure App Service'te web uygulamalarını çalıştırma hakkında bilgi edinin."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 36eead97f111025940e7e11902420e18b1b97fce
ms.contentlocale: tr-tr
ms.lasthandoff: 07/19/2017

---
# <a name="create-an-aspnet-web-app-in-azure"></a>Azure’da ASP.NET web uygulaması oluşturma

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıçta, Azure Web Apps’e ilk ASP.NET web uygulamanızı dağıtma işlemi gösterilmektedir. İşlemi tamamladığınızda bir App Service planı ve dağıtılmış web uygulaması ile Azure web uygulamasından oluşan kaynak grubunuz olacaktır.

![Azure App Service’te ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)’yi aşağıdaki iş yükleri ile yükleyin:
    - **ASP.NET ve web geliştirme**
    - **Azure geliştirme**

    ![ASP.NET ve web geliştirme ile Azure geliştirme (Web ve Bulut altında)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>ASP.NET web uygulaması oluşturma

Visual Studio'da **Dosya > Yeni > Proje**’yi seçerek bir proje oluşturun. 

**Yeni Proje** iletişim kutusunda **Visual C# > Web > ASP.NET Web Uygulaması (.NET Framework)** öğesini seçin.

Uygulamayı _myFirstAzureWebApp_ olarak adlandırın ve ardından **Tamam**’ı seçin.
   
![Yeni Proje iletişim kutusu](./media/app-service-web-get-started-dotnet/new-project.png)

Azure’a herhangi bir türde ASP.NET web uygulaması dağıtabilirsiniz. Bu hızlı başlangıçta **MVC** şablonunu seçin ve kimlik doğrulamasının **Kimlik Doğrulaması Yok** olarak ayarlandığından emin olun.
      
**Tamam**’ı seçin.

![Yeni ASP.NET Projesi iletişim kutusu](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

Menüden **Hata Ayıkla > Hata Ayıklamadan Başla**’yı seçerek web uygulamasını yerel olarak çalıştırın.

![Uygulamayı yerel olarak çalıştırma](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-to-azure"></a>Azure’da Yayımlama

**Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

**Microsoft Azure App Service**’in seçili olduğundan emin olup **Yayımla**’yı seçin.

![Projeye genel bakış sayfasından yayımlama](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Bu işlem, ASP.NET web uygulamasını Azure’da çalıştırmak için gereken tüm Azure kaynaklarını oluşturmanıza yardımcı olan **App Service Oluştur** iletişim kutusunu açar.

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
| Boyut | Ücretsiz | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/), barındırma özelliklerini belirler. |

**Tamam**’ı seçin.

## <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

**Web Uygulaması Adı**’na benzersiz bir ad girin (geçerli karakterler `a-z`, `0-9` ve `-` karakterleridir) veya otomatik olarak oluşturulan adı kabul edin. Web uygulamasının URL'si `http://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, web uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

![Web uygulaması adını yapılandırma](./media/app-service-web-get-started-dotnet/web-app-name.png)

Sihirbaz tamamlandıktan sonra ASP.NET web uygulamasını Azure’da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

[Oluşturma ve yayımlama adımında](#create-and-publish-the-web-app) belirtilen web uygulaması adı `http://<app_name>.azurewebsites.net` biçiminde URL ön eki olarak kullanılır.

Tebrikler, ASP.NET web uygulamanız Azure App Service’te çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

**Çözüm Gezgini** menüsünden _Görünümler\Giriş\Index.cshtml_ dosyasını açın.

Üst kısımda `<div class="jumbotron">` HTML etiketini bulun ve tüm öğeyi aşağıdaki kodla değiştirin:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

Yayımlama sayfasında **Yayımla**'yı seçin.

Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

![Azure’da güncelleştirilmiş ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

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
> [SQL Veritabanı ile ASP.NET](app-service-web-tutorial-dotnet-sqldatabase.md)

