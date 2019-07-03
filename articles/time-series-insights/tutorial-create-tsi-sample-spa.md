---
title: 'Öğretici: Bir Azure Time Series Insights tek sayfa web uygulaması oluşturma | Microsoft Docs'
description: Sorgular ve bir Azure zaman serisi görüşleri ortamından veri işleyen bir tek sayfalı web uygulaması oluşturmayı öğrenin.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/29/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: e415c681ae5a35de6e8ff76e09cfef8cc8cc98f8
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544074"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Öğretici: Azure Time Series Insights tek sayfalı web uygulaması oluşturma

Bu öğreticide, Azure Time Series Insights verilerine erişmek için kendi web tek sayfalı uygulama (SPA) oluşturma işlemi boyunca size yol gösterir.

Bu öğreticide şu konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Azure Active Directory (Azure AD) ile uygulamanızı kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme

> [!NOTE]
> * Bu öğretici için kaynak kod üzerinde sağlanan [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Time Series Insights [istemci örnek uygulaması](https://insights.timeseries.azure.com/clientsample) Bu öğreticide kullanılan tamamlanan uygulama göstermek için barındırılır.

## <a name="prerequisites"></a>Önkoşullar

* Kaydolun bir [Azure aboneliği](https://azure.microsoft.com/free/) zaten yoksa.

* Visual Studio ücretsiz bir kopyası. İndirme [2017 veya 2019 topluluk sürümlerini](https://www.visualstudio.com/downloads/) kullanmaya başlamak için.

* IIS Express, Web dağıtımı ve Azure Cloud Services araçları için çekirdek bileşenler Visual Studio. Bileşenler, Visual Studio yüklemenizi değiştirerek ekleyin.

## <a name="application-design"></a>Uygulama tasarımı

Time Series Insights örnek SPA tasarım ve Bu öğreticide kullanılan kod temelini oluşturur. Kod, zaman serisi öngörüleri JavaScript istemci kitaplığını kullanır. Time Series Insights istemci kitaplığı, iki ana API kategorisi için bir Özet sağlar:

- **Time Series Insights'ı çağırmak için sarmalayıcı yöntemleri sorgu API'leri**: JSON göre ifadeler kullanarak zaman serisi öngörüleri için veri sorgulamak için kullanabileceğiniz REST API'ler. Yöntemleri kitaplığı TsiClient.server ad alanı altında düzenlenir.

- **Oluşturma ve doldurma denetimleri grafik türleri çeşitli yöntemleri**: Bir Web sayfasındaki zaman serisi öngörüleri verilerini görselleştirmek için kullanabileceğiniz yöntemler. Yöntemleri kitaplığı TsiClient.ux ad alanı altında düzenlenir.

Bu öğreticide, örnek uygulamanın zaman serisi görüşleri ortamından veri de kullanır. Öğretici yapısını Time Series Insights örnek uygulama ve Time Series Insights istemci kitaplığını kullanma hakkında daha fazla ayrıntı için bkz [Azure zaman serisi öngörüleri JavaScript istemci kitaplığını keşfedin](tutorial-explore-js-client-lib.md).

## <a name="register-the-application-with-azure-ad"></a>Uygulamayı Azure AD’ye kaydetme

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

## <a name="build-and-publish-the-web-application"></a>Web uygulamasını derleme ve yayımlama

1. Uygulamanızın proje dosyalarını depolamak için bir dizin oluşturun. Ardından, aşağıdaki URL'lerden her birine gidin. Sağ **ham** sayfanın sağ üst köşesinde bulunan bağlantı ve ardından **Kaydet** proje dizininizde dosyaları kaydetmek için.

   - [*index.HTML*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): sayfası için JavaScript ve HTML
   - [*sampleStyles.css*]( https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): CSS stil sayfası

   > [!NOTE]
   > Tarayıcıya bağlı olarak, dosyayı kaydetmeden önce dosya uzantılarını .html veya .css değiştirmeniz gerekebilir.

1. Visual Studio'da gerekli bileşenlerin yüklü olduğunu doğrulayın. Visual Studio için IIS Express, Web dağıtımı ve Azure Cloud Services temel araçları bileşenleri yüklü olmalıdır.

    [![Visual Studio - yüklü bileşenlerin değiştirme](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    > [!NOTE]
    > Visual Studio deneyiminizi sürümü ve yapılandırma ayarlarınıza bağlı olarak gösterilen örneklerden biraz farklı olabilir.

1. Visual Studio'yu açın ve oturum açın. Web uygulaması projesi oluşturmak için **dosya** menüsünde **açık** > **Web sitesi**.

    [![Visual Studio - yeni bir çözüm oluşturma](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. İçinde **açık Web sitesi** bölmesinde, HTML ve CSS dosyaları depolandığı çalışma dizini seçin ve ardından **açık**.

   [![Visual Studio - Dosya menüsünü açın ve Web sitesi seçenekleriyle](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. Visual Studio **görünümü** menüsünde **Çözüm Gezgini**. Yeni çözümünüzü açılır. HTML ve CSS dosyaları içeren bir Web sitesi projesi (dünya simgesi) içeriyor.

   [![Visual Studio - Çözüm Gezgini'ndeki yeni çözüm](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Uygulamanızı yayımlamadan önce yapılandırma ayarlarında alter *index.html*.

   1. Yorum altında üç satırı açıklamadan çıkarın `"PROD RESOURCE LINKS"` bağımlılıkları geliştirmeden üretime geçin. Yorum altında üç satır yorum `"DEV RESOURCE LINKS"`.

      [!code-html[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Aşağıdaki örnekteki gibi bağımlılıklarınızı Açıklamalı:

      ```HTML
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css">

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="../../dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="../../dist/tsiclient.css"> -->
      ```

   1. Uygulamayı Azure AD uygulama kayıt Kimliğinizi kullanacak şekilde yapılandırmak için değiştirme `clientID` kullanılacak değeri **uygulama kimliği** , içinde kopyalanan **3. adım** olduğunda, [uygulamaya kayıtlı Azure AD'yi kullanın](#register-the-application-with-azure-ad). Oluşturulduktan sonra bir **oturum kapatma URL'si** olarak bu değeri Azure AD'de ayarlamak `postLogoutRedirectUri` değeri.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Örneğin:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   1. Değişiklikleri bitirdiğinizde kaydetmeniz *index.html*.

1. Web uygulaması, Azure aboneliğinizde bir Azure uygulama hizmeti olarak yayımlayın.  

   > [!NOTE]
   > Aşağıdaki adımlarda gösterildiği ekran görüntüleri de çeşitli seçenekler, Azure aboneliğinizde verilerle otomatik olarak doldurulur. İşlemin her bölme için tamamen yüklemek için birkaç saniye sürebilir.  

   1. Çözüm Gezgini'nde Web sitesi proje düğümüne sağ tıklayın ve ardından **Web uygulaması yayımlama**.  

      [![Visual Studio - Çözüm Gezgini Web uygulaması yayımlama seçeneğini seçin](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   1. Seçin **Başlat** uygulamanız yayımlanırken başlatmak için.

      [![Visual Studio - yayımlama profili bölmesi](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   1. Uygulamayı yayımlamak için kullanmak istediğiniz aboneliği seçin. Seçin **TsiSpaApp** proje. Sonra **Tamam**’ı seçin.

      [![Visual Studio - App Service bölmesinde yayımlama profili](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Seçin **Yayımla** web uygulamasını dağıtma işlemi.

      [![Visual Studio - Yayımla seçeneğini ve yayınlama günlük çıktısı](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Visual Studio'da başarılı Yayımla günlük görünür **çıkış** bölmesi. Dağıtım tamamlandığında, Visual Studio web uygulaması bir tarayıcı sekmesinde açılır ve oturum açma için ister. Başarılı oturum açma işleminden sonra zaman serisi görüşleri denetimlerin verilerle doldurulur.

## <a name="troubleshoot"></a>Sorun giderme  

Hata kodu/durumu | Açıklama
---------------------| -----------
*AADSTS50011: Uygulama için kayıtlı yanıt adresi yok.* | Azure AD kaydı eksik **yanıt URL'si** özelliği. Git **ayarları** > **yanıt URL'leri** , Azure AD uygulama kaydı için. Doğrulayın **yeniden yönlendirme URI'si** belirtin seçeneğine sahip **2. adım** olduğunda, [kayıtlı Azure AD kullanmak için uygulamayı](#register-the-application-with-azure-ad) mevcuttur.
*AADSTS50011: Yanıt URL'si istekte belirtilen uygulama için yapılandırılan yanıt URL'lerinden eşleşmiyor: '\<Uygulama kimliği GUID >'.* | `postLogoutRedirectUri` Belirtilen **6. adım** içinde [oluşturun ve web uygulaması yayımlamaya](#build-and-publish-the-web-application) altında belirtilen değer eşleşmelidir **ayarları**  >  **Yanıt URL'leri** , Azure AD uygulama kaydı içinde. Ayrıca değerini değiştirdiğinizden emin olun **hedef URL** kullanılacak *https* başına **5. adım** içinde [oluşturun ve web uygulaması yayımlamaya](#build-and-publish-the-web-application).
Web uygulaması yükler, ancak bir unstyled, salt metin oturum açma sayfası, beyaz arka plan bulunur. | Yolları konusunda değinildiği doğrulayın **4. adım** içinde [oluşturun ve web uygulaması yayımlamaya](#build-and-publish-the-web-application) doğrudur. Web uygulaması .css dosyalarını bulamadığında sayfa stili doğru şekilde uygulanmaz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici çalışan birkaç Azure hizmeti oluşturur. Bu öğretici serisinin tamamlanmasını planlamıyorsanız, gereksiz yinelenen maliyetler oluşmasını önlemek için tüm kaynakları silmeniz önerilir.

Azure portalında sol taraftaki menüde:

1. Seçin **kaynak grupları**ve ardından zaman serisi görüşleri ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında seçin **kaynak grubunu Sil**kaynak grubunun adını girin ve ardından **Sil**.
1. Seçin **kaynak grupları**ve ardından cihaz benzetimi çözüm Hızlandırıcı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında seçin **kaynak grubunu Sil**kaynak grubunun adını girin ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure AD'ye kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme

Bu öğreticide, Azure AD ile tümleştirir ve erişim belirteci alma oturum açmış olan kullanıcının kimliğini kullanır. Bir hizmet veya yordam uygulama kimliğini kullanarak zaman serisi öngörüleri API erişimi öğrenmek için bu makaleye bakın:

> [!div class="nextstepaction"]
> [Azure Time Series Insights API’si için kimlik doğrulaması ve yetkilendirme](time-series-insights-authentication-and-authorization.md)
