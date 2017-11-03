---
title: "Özel bağlayıcılar Web API'lerden - Azure mantıksal uygulamaları oluşturma | Microsoft Docs"
description: "Web API'özel bağlayıcılar derleme"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 22bf335718721d29933557142b436e75778f8c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-custom-connectors-from-web-apis"></a>Web API'özel bağlayıcılar oluşturma

Azure mantıksal uygulamaları, Microsoft Flow veya Microsoft PowerApps kullanabileceğiniz özel bir bağlayıcı oluşturmak için önce konak Azure Web Apps ile Azure Active Directory ile kimlik doğrulaması ve Logic Apps, akış, sahip bir bağlayıcı olarak kaydeden bir Web API oluşturun veya PowerApps. Bu öğretici, bir ASP.NET Web API uygulamasını oluşturarak bu görevleri gerçekleştirmek nasıl gösterir. Yapı, bağlayıcı'nın tetikleyiciler ve Eylemler, kodunu Göster desenleri görmek için [mantığı uygulama akışlarından çağırabilirsiniz özel API oluşturma](../logic-apps/logic-apps-create-api-app.md).

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2013 veya üzeri](https://www.visualstudio.com/vs/). Bu öğreticide Visual Studio 2015 kullanır.

* Web API için kod. Yok, Bu öğretici deneyin: [ASP.NET Web API 2 (C#) ile çalışmaya başlama](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).

* Azure aboneliği. Bir aboneliğiniz yoksa ile başlayabilirsiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Aksi takdirde kaydolun bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/).

## <a name="create-and-deploy-an-aspnet-web-app-to-azure"></a>Oluşturun ve Azure'a bir ASP.NET web uygulaması dağıtma

Bu öğreticide, bir Visual C# ASP.NET web uygulaması oluşturun. 

1. Visual Studio'yu açın ve ardından **dosya** > **yeni proje**.

   1. Genişletme **yüklü**gidin **şablonları** > **Visual C#** > **Web**seçip **ASP .NET web uygulaması**.

   2. Bir proje adını, konumunu ve uygulamanız için çözüm adı sağlayın, sonra seçin **Tamam**.

   Örneğin:

   ![Visual C# ASP.NET web uygulaması oluşturma](./media/custom-connector-build-web-api-app-tutorial/visual-studio-new-project-aspnet-web-app.png)

2. İçinde **yeni ASP.NET Web uygulaması** kutusunda **Web API** şablonu. Seçili değilse seçin **bulutta Barındır**. Seçin **değiştirmek kimlik doğrulama**.

   !["Bulutta barındırmayı", "Web API'sini" şablonu seçin, "Kimlik doğrulamayı Değiştir"](./media/custom-connector-build-web-api-app-tutorial/visual-studio-web-api-template.png)

3. Seçin **doğrulaması yok**ve seçin **Tamam**. Daha sonra kimlik doğrulamasını ayarlayabilirsiniz.

   !["Kimlik doğrulaması" seçeneğini belirleyin](./media/custom-connector-build-web-api-app-tutorial/visual-studio-change-authentication.png)

4. Zaman **yeni ASP.NET Web uygulaması** kutusunda görüntülenir, seçin **Tamam**. 

5. İçinde **App Service Oluştur** kutusunda tabloda tanımlanan barındırma ayarlarını gözden geçirin, değişiklikleri yapın ve seçin **oluşturma**. 

   Bir [uygulama hizmeti planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) uygulamaları Azure aboneliğinizde barındırmak için kullanılan fiziksel kaynakları koleksiyonunu temsil eder. Hakkında bilgi edinin [uygulama hizmeti](../app-service/app-service-value-prop-what-is.md).

   ![Uygulama hizmeti oluşturma](./media/custom-connector-build-web-api-app-tutorial/visual-studio-create-app-service.png)

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | Azure iş veya Okul hesabı veya kişisel Microsoft hesabınızı | *kullanıcı hesabı Sihirbazı* | Kullanıcı hesabınızı seçin. | 
   | **Web uygulaması adı** | *Özel-web-API-app-name* veya varsayılan adı | Uygulamanızın URL'de örneğin kullanılan Web API uygulamanız için bir ad girin: http://*web API uygulamanızın adı*. | 
   | **Abonelik** | *Azure abonelik adı* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak Grubu** | *Azure kaynak grubu adı* | Varolan bir Azure kaynak grubu seçin veya henüz yapmadıysanız, bir kaynak grubu oluşturun. <p>**Not**: bir Azure kaynak grubu, Azure aboneliğinizin Azure kaynaklarında düzenler. | 
   | **Uygulama hizmeti planı** | *Uygulama hizmeti planı adı* | Var olan bir uygulama hizmeti planı seçin veya henüz yapmadıysanız, bir plan oluşturun. | 
   |||| 

   Bir uygulama hizmeti planı oluşturursanız, bu ayarları belirtin:

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Konum** | *Dağıtım bölgesi* | Uygulamanızı dağıtmak için bölge seçin. | 
   | **Boyut** | *Uygulama hizmeti planı boyutu* | Maliyet ve hizmet planınız için bilgi işlem kaynak kapasitesini belirler, planı boyutunu seçin. | 
   |||| 

   Uygulamanız tarafından gereken diğer kaynakları ayarlamak için tercih **diğer Azure hizmetlerini keşfedin**.

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Kaynak Türü** | *Azure kaynak türü* | Seçin ve uygulamanız tarafından gerekli ek kaynaklarını ayarlayın. | 
   |||| 

6. Visual Studio, projenizin dağıtıldıktan sonra uygulamanız için kod oluşturur.

## <a name="create-an-openapi-swagger-file-that-describes-your-web-api"></a>Web API'sini açıklayan bir OpenAPI (Swagger) dosyası oluşturun

Logic Apps için Web API uygulamasına bağlanmak için gereken bir [OpenAPI (önceki adıyla Swagger) dosya](http://swagger.io/) , API işlemlerini açıklar. API ile için kendi OpenAPI tanımı yazabilirsiniz [Swagger çevrimiçi düzenleyicisini](http://editor.swagger.io/), ancak bu öğreticide adlı bir açık kaynak aracı kullanır [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md).

1. Henüz yapmadıysanız, Visual Studio projenizde Swashbuckle Nuget paketini yükleyin.

   1. Visual Studio'da, **Araçları** > **NuGet Paket Yöneticisi** > 
    **Paket Yöneticisi Konsolu**.

   2. İçinde **Paket Yöneticisi Konsolu**, var. zaten değilseniz, uygulamanızın proje dizinine gidin (çalıştırmak `Set-Location "project-path"`), ve bu PowerShell komutunu çalıştırın: 
   
      `Install-Package Swashbuckle`

      Örneğin:

      ![Paket Yöneticisi konsolu, Swashbuckle yükleyin](./media/custom-connector-build-web-api-app-tutorial/visual-studio-package-manager-install-swashbuckle.png)

   > [!TIP]
   > Swashbuckle yükledikten sonra uygulamanızı çalıştırırsanız Swashbuckle bu URL'de bir OpenAPI dosyası oluşturur: 
   >
   > http://*{your-web-api-app-root-URL}*/swagger/docs/v1
   > 
   > Swashbuckle de bu URL'de bir kullanıcı arabirimi oluşturur: 
   > 
   > http://*{your-web-api-app-root-URL}*  /swagger

3. Hazır olduğunuzda, Azure'a Web API uygulamanızı yayımlayın. Visual Studio'dan yayımlamak için Çözüm Gezgini'nde web projenize sağ tıklayın, seçin **Yayımla...** ve yönergeleri izleyin.

   > [!IMPORTANT]
   > Yinelenen işlem kimlikleri olun bir OpenAPI belgesi geçersiz. Örnek C# şablonu, şablon kullandıysanız *bu işlem kimliği iki kez yineler*:`Values_Get` 
   > 
   > Bu sorunu gidermek için bir örneğine değiştirmek `Value_Get` ve yeniden yayımlayın.

4. Bu konuma göz atarak OpenAPI belge alın: 

   http://*{your-web-api-app-root-URL}*/swagger/docs/v1

   Ayrıca indirebilirsiniz bir [örnek OpenAPI belge](https://pwrappssamples.blob.core.windows.net/samples/webAPI.json) bu öğreticiden. 
   Hangi başlayarak açıklamaları kaldırdığınızdan emin olun "/ /", belge kullanmadan önce.

5. İçeriği bir JSON dosyası olarak kaydedin. Tarayıcınıza alarak, metnin bir boş metin dosyasında kopyalayıp gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik doğrulaması özel bağlayıcılar için ayarlama](../logic-apps/custom-connector-azure-active-directory-authentication.md)











