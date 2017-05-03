---
title: "Beş dakika içinde Azure’da ilk ASP.NET web uygulamanızı oluşturma | Microsoft Docs"
description: "Örnek bir ASP.NET uygulaması dağıtarak, App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin."
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
ms.date: 03/27/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: e0bfa7620feeb1bad33dd2fe4b32cb237d3ce158
ms.openlocfilehash: 24e9f1d7bdf4401d009ba04fb62351b6abda6079
ms.lasthandoff: 04/21/2017


---
# <a name="create-your-first-aspnet-web-app-in-azure-in-five-minutes"></a>Beş dakika içinde Azure’da ilk ASP.NET web uygulamanızı oluşturma

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Bu Hızlı Başlangıç, ilk ASP.NET web uygulamanızı birkaç dakika içinde [Azure Uygulama Hizmeti](../app-service/app-service-value-prop-what-is.md)’ne dağıtmanıza yardımcı olur. İşiniz bittiğinde, bulutta çalışır hale gelecek basit bir uygulamanız olur.

![Azure App Service’te ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide Visual Studio 2017’yi kullanarak bir ASP.NET web uygulaması oluşturma ve Azure’a dağıtma işlemi gösterilmektedir. Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>ASP.NET web uygulaması oluşturma

Visual Studio’da `Ctrl`+`Shift`+`N` ile yeni bir proje oluşturun.

**Yeni Proje** iletişim kutusunda **Visual C# > Web > ASP.NET Web Uygulaması (.NET Framework)** seçeneğine tıklayın.

Uygulamayı **myFirstAzureWebApp** olarak adlandırın ve ardından **Tamam**’a tıklayın.
   
![Yeni Proje iletişim kutusu](./media/app-service-web-get-started-dotnet/new-project.png)

Azure’a herhangi bir türde ASP.NET web uygulaması dağıtabilirsiniz. Bu öğreticide **MVC** şablonunu seçin ve kimlik doğrulamasının **Kimlik Doğrulaması Yok** olarak ayarlandığından emin olun.
      
**Tamam**’a tıklayın.

![Yeni ASP.NET Projesi iletişim kutusu](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

## <a name="publish-to-azure"></a>Azure’da Yayımlama

**Çözüm Gezgini**’nde **myFirstAzureWebApp** projenize sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

**Microsoft Azure App Service**’in seçili olduğundan emin olup **Yayımla**’ya tıklayın.

![Projeye genel bakış sayfasından yayımlama](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Bu işlem, ASP.NET web uygulamanızı Azure’da çalışmanız için gereken tüm Azure kaynaklarını oluşturmanıza yardımcı olan **App Service Oluştur** iletişim kutusunu açar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

**App Service Oluştur** iletişim kutusunda **Hesap ekle**’ye tıklayın ve ardından Azure aboneliğinizde oturum açın. Bir Microsoft hesabında zaten oturum açtıysanız hesabın Azure aboneliğinizi barındırdığından emin olun. Oturum açtığınız Microsoft hesabında Azure aboneliğiniz yoksa, doğru hesabı eklemek için tıklayın.
   
![Azure'da oturum açma](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Oturum açtıktan sonra bu iletişim kutusunda Azure web uygulamanız için gereken tüm kaynakları oluşturmaya hazır olursunuz.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

İlk olarak bir _kaynak grubu_ gereklidir. 

> [!NOTE] 
> Kaynak grubu; web uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.
>
>

**Kaynak Grubu**’nun yanındaki **Yeni** öğesine tıklayın.

Kaynak grubunuzu **myResourceGroup** olarak adlandırıp **Tamam**’a tıklayın.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

Azure web uygulamanız için bir _App Service planı_ da gereklidir. 

> [!NOTE]
> App Service planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynak bileşimini temsil eder. Bir App Service planına atanan tüm uygulamalar plan tarafından tanımlanan kaynakları paylaşarak birden çok uygulamayı barındırırken maliyetten tasarruf etmenize imkan sağlar. 
>
> App Service planları şunları tanımlar:
>
> - Bölge (Kuzey Avrupa, Doğu ABD, Güneydoğu Asya)
> - Örnek Boyutu (Küçük, Orta, Büyük)
> - Ölçek Sayısı (bir, iki, üç örnek, vb.) 
> - SKU (Ücretsiz, Paylaşılan, Temel, Standart, Premium)
>
>

**App Service Planı**’nın yanındaki **Yeni** öğesine tıklayın. 

**App Service Planını Yapılandır** iletişim kutusunda yeni App Service planını aşağıdaki ayarlarla yapılandırın:

- **App Service Planı**: **myAppServicePlan** yazın. 
- **Konum**: **Batı Avrupa** veya istediğiniz başka bir bölgeyi seçin.
- **Boyut**: **Ücretsiz** veya istediğiniz başka bir [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/app-service/) seçin.

**Tamam** düğmesine tıklayın.

![Yeni App Service planı oluşturma](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

## <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

Bundan sonra yapmanız gereken tek şey, web uygulamanızı adlandırmaktır. **Web Uygulaması Adı** alanına benzersiz bir uygulama adı girin. Bu ad, uygulamanızın (`<app_name>.azurewebsites.net`) varsayılan DNS adında kullanılır, bu nedenle Azure’daki tüm uygulamalarda benzersiz olmalıdır. Daha sonra, uygulamanızı kullanıcılarınızın kullanımına sunmadan önce uygulamanıza özel bir etki alanı adı eşleyebilirsiniz.

Ayrıca, zaten benzersiz olan, otomatik oluşturulmuş adı kabul edebilirsiniz.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’a tıklayın.

![Web uygulaması adını yapılandırma](./media/app-service-web-get-started-dotnet/web-app-name.png)

Sihirbaz Azure kaynaklarını oluşturmayı tamamladıktan sonra ASP.NET web uygulamanızı Azure’da ilk kez otomatik olarak yayımlar ve ardından yayımlanmış Azure web uygulamasını varsayılan tarayıcınızda başlatır.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

URL daha önce belirttiğiniz web uygulaması adını `http://<app_name>.azurewebsites.net` biçimiyle kullanır. 

Tebrikler, ilk ASP.NET web uygulamanız Uygulama Hizmeti’nde çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Güncelleştirmek ve Azure’a yeniden dağıtmak çok kolaydır. Ana sayfada bir güncelleştirme yapalım.

**Çözüm Gezgini** menüsünden **Görünümler\Giriş\Index.cshtml** dosyasını açın.

Üst kısımda `<div class="jumbotron">` HTML etiketini bulun ve tüm etiketi aşağıdaki kodla değiştirin:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projenize sağ tıklayıp **Yayımla**’yı seçin.

Yayımlama sayfasında **Yayımla**'ya tıklayın.

Visual Studio tamamlandığında güncelleştirilmiş Azure web uygulamasını tarayıcınızda başlatır.

![Azure’da güncelleştirilmiş ASP.NET web uygulaması](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-your-new-azure-web-app"></a>Yeni Azure web uygulamanızı yönetme

Azure portalına giderek yeni oluşturduğunuz web uygulamasına göz atın. 

Bunu yapmak için [https://portal.azure.com](https://portal.azure.com) sayfasında oturum açın.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-dotnet/access-portal.png)

Web uygulamanızın _dikey penceresini_ açtınız (yatay yönde açılan portal sayfası). 

Varsayılan olarak, web uygulamanızın dikey penceresinde **Genel Bakış** sayfası gösterilir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Dikey pencerenin sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir. 

![Azure portalında App Service dikey penceresi](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Dikey penceredeki bu sekmelerde web uygulamanıza ekleyebileceğiniz çok sayıda harika özellik gösterilir. Aşağıdaki listede yalnızca birkaç olasılık sunulmaktadır:

- Özel bir DNS adı eşleme
- Özel bir SSL sertifikası bağlama
- Sürekli dağıtımı yapılandırma
- Ölçeği artırma ve genişletme
- Kullanıcı kimlik doğrulaması ekleme

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlk Azure web uygulamanızı silmek için **Genel Bakış** sayfasında **Sil**’e tıklayabilirsiniz. Ancak, bu hızlı başlangıç bölümünde oluşturduğunuz her şeyi silmenin daha iyi bir yolu vardır. Web uygulamanızın **Genel Bakış** sayfasında, kaynak grubuna tıklayarak dikey penceresini açın. 

![App Service dikey penceresinden kaynak grubuna erişim](./media/app-service-web-get-started-dotnet/access-resource-group.png)

Kaynak grubu dikey penceresinde, Visual Studio’nun sizin için oluşturduğu App Service planını ve App Service uygulamasını görebilirsiniz. 

Dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın. 

<!--![Delete resource group in Azure portal](./media/app-service-web-get-started-dotnet/delete-resource-group.png)-->

Onay dikey penceresinde **myResourceGroup** kaynak grubu adını metin kutusuna yazarak onaylayın ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş [Web apps PowerShell betiklerini](app-service-powershell-samples.md) keşfedin.

