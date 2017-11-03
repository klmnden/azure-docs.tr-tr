---
title: "Azure’da ilk Java web uygulamanızı oluşturma"
description: "Basit bir Java uygulaması dağıtarak App Service'te web uygulamalarının nasıl çalıştırılacağını öğrenin."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc, devcenter
ms.openlocfilehash: ac8ef479be5a93b2c4baa76279c8d3e53389409a
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a>Azure’da ilk Java web uygulamanızı oluşturma

Azure [Web Apps](app-service-web-overview.md) düzeyde ölçeklenebilir, otomatik olarak düzeltme eki uygulama web barındırma hizmeti sağlar. Bu hızlı başlangıçta, [Java EE Geliştiricileri için Eclipse IDE](http://www.eclipse.org/) kullanarak App Service’e nasıl Java web uygulaması dağıtılacağı gösterilmektedir.

!["Merhaba Azure!" örnek web uygulaması](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunları yükleyin:

* Ücretsiz [Java EE Geliştiricileri için Eclipse IDE](http://www.eclipse.org/downloads/). Bu hızlı başlangıçta Eclipse Neon kullanılır.
* [Eclipse için Azure Araç Takımı](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Eclipse’te dinamik web projesi oluşturma

Eclipse’te **Dosya** > **Yeni** > **Dinamik Web Projesi**’ni seçin.

**Yeni Dinamik Web Projesi** iletişim kutusunda, projeyi **MyFirstJavaOnAzureWebApp** olarak adlandırın ve **Son**’u seçin.
   
![Yeni Dinamik Web Projesi iletişim kutusu](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>JSP sayfası ekleme

Proje Gezgini görüntülenmiyorsa, gezgini geri yükleyin.

![Eclipse için Java EE çalışma alanı](./media/app-service-web-get-started-java/pe.png)

Proje Gezgini'nde **MyFirstJavaOnAzureWebApp** projesini genişletin.
**WebContent**’e sağ tıklayın ve **Yeni** > **JSP Dosyası**’nı seçin.

![Proje Gezgini'nde yeni JSP dosyasına yönelik menü](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

**Yeni JSP Dosyası** iletişim kutusunda:

* Dosyayı **index.jsp** olarak adlandırın.
* **Son**’u seçin.

  ![Yeni JSP Dosyası iletişim kutusu](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

index.jsp dosyasında, `<body></body>` öğesini aşağıdaki işaretlemeyle değiştirin:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Değişiklikleri kaydedin.

## <a name="publish-the-web-app-to-azure"></a>Web uygulamasını Azure’da yayımlama

Proje Gezgini’nde projeye sağ tıklayın ve **Azure** > **Azure Web App olarak yayımla**’yı seçin.

![Azure Web App Olarak Yayımla bağlam menüsü](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

**Azure Oturum Açma** iletişim kutusunda **Etkileşimli** seçeneğini değiştirmeyin ve **Oturum aç**’ı seçin.

Oturum açma yönergelerini izleyin.

### <a name="deploy-web-app-dialog-box"></a>Web Uygulaması Dağıtma iletişim kutusu

Azure hesabınızda oturum açtıktan sonra, **Web Uygulaması Dağıtma** iletişim kutusu açılır.

**Oluştur**’u seçin.

![Web Uygulaması Dağıtma iletişim kutusu](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>App Service Oluşturma iletişim kutusu

**App Service Oluşturma** iletişim kutusu varsayılan değerlerle açılır. Aşağıdaki görüntüde gösterilen **170602185241** sayısı, sizin iletişim kutunuzda farklıdır.

![App Service Oluşturma iletişim kutusu](./media/app-service-web-get-started-java/cas1.png)

**App Service Oluşturma** iletişim kutusunda:

* Web uygulaması için oluşturulan adı değiştirmeyin. Bu ad Azure genelinde benzersiz olmalıdır. Ad, web uygulamasının URL adresinin bir parçasıdır. Örneğin, web uygulamasının adı **MyJavaWebApp** ise URL şu şekildedir: *myjavawebapp.azurewebsites.net*.
* Varsayılan web kapsayıcısını değiştirmeyin.
* Bir Azure aboneliği seçin.
* **App Service planı** sekmesinde:

  * **Yeni oluştur**: App Service planının adı olan varsayılan değeri değiştirmeyin.
  * **Konum**: **Batı Avrupa**’yı veya size yakın olan bir konumu seçin.
  * **Fiyatlandırma katmanı**: Ücretsiz seçeneğini belirleyin. Özellikler için bkz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

   ![App Service Oluşturma iletişim kutusu](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Kaynak grubu sekmesi

**Kaynak grubu** sekmesini seçin. Kaynak grubu için oluşturulmuş varsayılan değeri değiştirmeyin.

![Kaynak grubu sekmesi](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

**Oluştur**’u seçin.

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Azure Araç Takımı web uygulamasını oluşturur ve bir ilerleme durumu iletişim kutusu görüntüler.

![App Service Oluşturma İlerleme Durumu iletişim kutusu](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Web Uygulaması Dağıtma iletişim kutusu

**Web Uygulaması Dağıtma** iletişim kutusunda **Köke dağıt**’ı seçin. *wingtiptoys.azurewebsites.net* adresinde bir uygulama hizmetiniz varsa ve köke dağıtmazsanız, **MyFirstJavaOnAzureWebApp** adlı web uygulaması *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp* adresine dağıtılır.

![Web Uygulaması Dağıtma iletişim kutusu](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

İletişim kutusunda Azure, JDK ve web kapsayıcısı seçimleri gösterilir.

Web uygulamasını Azure’da yayımlamak için **Dağıt**’ı seçin.

Yayımlama işlemi tamamlandığında, **Azure Etkinlik Günlüğü** iletişim kutusundaki **Yayımlandı** bağlantısını seçin.

![Azure Etkinlik Günlüğü iletişim kutusu](./media/app-service-web-get-started-java/aal.png)

Tebrikler! Web uygulamanızı başarılı bir şekilde Azure’da dağıttınız. 

!["Merhaba Azure!" örnek web uygulaması](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a>Web uygulamasını güncelleştirme

Örnek JSP kodunu farklı bir ileti olarak değiştirin.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Değişiklikleri kaydedin.

Proje Gezgini’nde projeye sağ tıklayın ve **Azure** > **Azure Web App olarak yayımla**’yı seçin.

**Web Uygulaması Dağıtma** iletişim kutusu açılır ve daha önce oluşturduğunuz uygulama hizmetini gösterir. 

> [!NOTE]
> Her yayımlama işleminde **Köke dağıt**’ı seçin.
>

Web uygulamasını seçip **Dağıt**’ı seçin. Bunu yaptığınızda değişiklikler yayımlanır.

**Yayımlanıyor** bağlantısı göründüğünde, web uygulamasına gitmek ve değişiklikleri görmek için bu bağlantıyı seçin.

## <a name="manage-the-web-app"></a>Web uygulamasını yönetme

Oluşturduğunuz web uygulamasını görmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Soldaki menüden **Kaynak Grupları**'nı seçin.

![Portalda kaynak gruplarına gitme](media/app-service-web-get-started-java/rg.png)

Kaynak grubunu seçin. Sayfada bu hızlı başlangıçta oluşturduğunuz kaynaklar gösterilir.

![myResourceGroup kaynak grubu](media/app-service-web-get-started-java/rg2.png)

Web uygulamasını (önceki resimde **webapp-170602193915**) seçin.

**Genel Bakış** sayfası açılır. Bu sayfada uygulamanın nasıl çalıştığına ilişkin bir görünüm sağlanır. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırmaları gösterir. 

![Azure portalında App Service sayfası](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel etki alanı eşleme](app-service-web-tutorial-custom-domain.md)
