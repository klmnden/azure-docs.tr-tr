---
title: 'Öğretici: Bir Azure Time Series Insights tek sayfa web uygulaması oluşturma | Microsoft Docs'
description: TSI ortamındaki verileri sorgulayan ve işleyen tek sayfalı bir web uygulamasını nasıl oluşturacağınızı öğrenin.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 18f5c14a9427f4d7e34a802b2bcc0612a51a804a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573244"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Öğretici: Azure Time Series Insights tek sayfalı web uygulaması oluşturma

Bu öğretici, Time Series Insights verilerine erişmek için kendi tek sayfa web uygulaması oluşturma işlemi boyunca size yol gösterir. Özellikle, hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure Active Directory’ye (AD) kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme

> [!NOTE]
> * Bu öğretici için kaynak kod üzerinde sağlanan [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Time Series Insights [istemci örnek uygulaması](https://insights.timeseries.azure.com/clientsample) Bu öğreticide kullanılan tamamlanan uygulama göstermek için barındırılır.

## <a name="prerequisites"></a>Önkoşullar

* Aboneliğiniz yoksa [ücretsiz Azure aboneliğine](https://azure.microsoft.com/free/) kaydolun.

* Visual Studio ücretsiz bir kopyasını da gerekir. İndirme [2017 veya 2019 topluluk sürümü](https://www.visualstudio.com/downloads/) kullanmaya başlamak için.

* Ayrıca gerekir **IIS Express**, **Web dağıtımı**, ve **Azure Cloud Services temel araçları** bileşenleri Visual Studio için. Visual Studio yüklemenizi değiştirerek bu ekleyin.

## <a name="application-design-overview"></a>Uygulama tasarımına genel bakış

Time Series Insights örnek tek sayfalı uygulama, tasarım ve Bu öğreticide kullanılan kod için temel sağlar. Kodda TSI İstemcisi JavaScript kitaplığı kullanılır. TSI İstemcisi kitaplığı iki ana API kategorisi için bir özet sunar:

- **TSI sorgu API'leri çağırmak için sarmalayıcı yöntemleri**: Sorgu olanak tanıyan TSI verileri JSON göre ifadeler kullanarak REST API'ler. Metotlar, kitaplığın `TsiClient.server` ad alanı altında düzenlenmiştir.

- **Oluşturma ve doldurma denetimleri grafik türleri çeşitli yöntemleri**: Bir web sayfasında TSI verileri görselleştirmek için kullanılan yöntemleri. Metotlar, kitaplığın `TsiClient.ux` ad alanı altında düzenlenmiştir.

Bu öğreticide örnek uygulamanın TSI ortamındaki veriler de kullanılacaktır. TSI örnek uygulamasının yapısı ve TSI İstemci kitaplığının kullanımı hakkında ayrıntılı bilgi için [Azure Time Series Insights JavaScript istemci kitaplığını keşfetme](tutorial-explore-js-client-lib.md) öğreticisine bakın.

## <a name="register-the-application-with-azure-ad"></a>Uygulamayı Azure AD’ye kaydetme

Uygulamayı derlemeden önce Azure AD’ye kaydetmeniz gerekir. Kayıt işlemi sırasında uygulama için kimlik yapılandırması gerçekleştirilerek çoklu oturum açma için OAuth desteğini kullanması sağlanır. OAuth için SPA’larda "örtük" yetkilendirme kullanılması ve bunu uygulama bildiriminde güncelleştirmeniz gerekir. Uygulama bildirimi, uygulama kimliği yapılandırmasının JSON gösterimidir.

1. Azure abonelik hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.  
1. Seçin **Azure Active Directory** sol bölmesinde, ardından kaynak **uygulama kayıtları**, ardından **+ yeni uygulama kaydı**.

   [![Azure portalında Azure AD uygulama kaydı](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png#lightbox)

1. Üzerinde **Oluştur** sayfasında, gerekli parametreleri doldurun.

   Parametre|Açıklama
   ---|---
   **Ad** | Anlamlı bir kayıt adı girin.  
   **Uygulama türü** | SPA web uygulaması oluşturduğunuz için "Web uygulaması/API" olarak bırakın.
   **Oturum Açma URL'si** | Uygulamanın giriş/oturum açma sayfasının URL’sini girin. Uygulamayı (daha sonra) Azure App Service'te barındırılacak olduğundan, içinde bir URL kullanmanız gerekir "https:\//azurewebsites.net" etki alanı. Bu örnekte ad, kayıt adına dayalıdır.

   Girişleri tamamladığınızda yeni uygulama kaydını oluşturmak için **Oluştur**’a tıklayın.

   [![Azure portalında Azure AD uygulama kaydı - oluşturma](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png#lightbox)

1. Kaynak uygulamaları, diğer uygulamalar tarafından kullanılan ve Azure AD’ye kaydedilecek REST API’lerini sağlar. API’ler "kapsamları" kullanarak istemci uygulamalarına ayrıntılı/güvenli erişim sağlar. Uygulamanız "Azure Time Series Insights" API’sini kullanacağından çalışma zamanında iznin isteneceği/verileceği API’yi ve kapsamı belirtmeniz gerekir. Seçin **ayarları**, ardından **gerekli izinler**, ardından **+ Ekle**.

   [![Azure portalında Azure AD izinleri ekleme](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png#lightbox)

1. **API erişimi ekle** sayfasında **1 Bir API seçin**’e tıklayarak TSI API’sini belirtin. **Bir API seçin** sayfasında arama alanına "azure time" yazın. Ardından sonuçlar listesinde "Azure zaman serisi görüşleri" API'ı seçin ve tıklayın **seçin**.

   [![Azure portalında Azure AD izinleri - API ekleme](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png#lightbox)

1. Şimdi bir API kapsamı belirtin. **API erişimi ekle** sayfasında **2 İzinleri seçin**’e tıklayın. **Erişimi Etkinleştir** sayfasında "Access Azure Time Series Insights hizmeti" kapsamını seçin. Tıklayın **seçin**, size döndürülür **API erişimi Ekle** erişebilen sayfasında **Bitti**.

   [![Azure portalında Azure AD izinleri - kapsamı Ekle](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png#lightbox)

1. **Gerekli izinler** sayfasına döndüğünüzde "Azure Time Series Insights" API’sinin listelenmiş olduğunu görürsünüz. Ayrıca tüm kullanıcılar için, uygulamanın API’ye ve kapsama erişmesi amacıyla ön onay izni vermeniz gerekir. Tıklayın **izinleri verin** düğmesini üstünde ve **Evet**.

   [![Azure portalında Azure AD izinleri - onayı gerekli](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png#lightbox)

1. Önceden belirtildiği üzere uygulama bildirimini de güncelleştirmeniz gerekir. İçerik haritasındaki uygulama adına tıklayarak **Kayıtlı uygulama** sayfasına dönün. Seçin **bildirim**, değiştirme `oauth2AllowImplicitFlow` özelliğini `true`, ardından **Kaydet**.

   [![Azure portalında Azure AD'ye güncelleştirme bildirimi](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png#lightbox)

1. Son olarak içerik haritasına tıklayarak **Kayıtlı uygulama** sayfasına dönün ve uygulamanızın **Giriş sayfası** URL’si ile **Uygulama kimliği** özelliklerini kopyalayın. Bu özellikler sonraki bir adımda kullanacaksınız.

   [![Azure portalında Azure AD özellikleri](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png#lightbox)

## <a name="build-and-publish-the-web-application"></a>Web uygulamasını derleme ve yayımlama

1. Uygulamanızın proje dosyalarını depolamak için bir dizin oluşturun. Ardından aşağıdaki URL'lerden her birine göz atın, sayfa ve "proje dizininiz Kaydet" üst sağ alanı "Ham" bağlantıya sağ tıklayın.

   > [!NOTE]
   > Tarayıcıya bağlı olarak dosyayı kaydetmeden önce dosya uzantısını düzeltmeniz (HTML veya CSS olarak) değiştirmeniz gerekebilir.

   - [**index.HTML**](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): HTML ve JavaScript için sayfa.
   - [**sampleStyles.css**]( https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): CSS stil sayfası.

1. Visual Studio için gerekli bileşenlerin yüklü olduğunu doğrulayın.

    [![VS - yüklü bileşenlerin değiştirme](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    * İhtiyacınız olacak **IIS Express**, **Web dağıtımı**, ve **Azure Cloud Services temel araçları** bileşenleri Visual Studio için.

    > [!NOTE]
    > Visual Studio deneyiminizi sürümü ve yapılandırma ayarlarına bağlı olarak gösterilen örneklerden biraz farklılık gösterebilir.

1. Web uygulaması projesini oluşturmak için Visual Studio uygulamasını çalıştırın ve oturum açın. **Dosya** menüsünde **Aç**, **Web Sitesi** seçeneğini belirtin.

    [![VS - yeni çözüm oluşturma](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. Üzerinde **açık Web sitesi** iletişim kutusunda HTML ve CSS dosyalarını depoladığınız çalışma dizini seçin, ardından tıklayın **açık**.

   [![VS - dosya açık web sitesi](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. Visual Studio **Görünüm** menüsünden **Çözüm Gezgini**’ni açın. HTML ve CSS dosyaları içeren bir web sitesi projesi (dünya simgesi) içeren yeni çözümünüzü görmeniz gerekir.

   [![VS - Çözüm Gezgini yeni çözüm](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Uygulamanızı yayımlamadan önce yapılandırma ayarlarını değiştirmeniz gerekecektir **index.html**.

   a. Yorum altında üç satır uncommenting tarafından bağımlılıkları geliştirmeden üretime geçiş `"PROD RESOURCE LINKS"`. Yorum altında üç satır yorum `"DEV RESOURCE LINKS"`.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Bağımlılıklarınızı buna uygun olarak geçersiz kılınan:

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

   b. Ardından, Azure Active Directory uygulama kayıt kimliğinizi kullanmak üzere uygulamayı yapılandırır Değişiklik `clientID` ve `postLogoutRedirectUri` kullanacak alanlar **uygulama kimliği** ve **giriş sayfası URL'si** , içinde kopyalanan **9. adım** , [kaydetme Azure AD ile uygulama](#register-the-application-with-azure-ad).

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Örneğin:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   c. Kaydet **index.html** bittiğinde bu değişiklikleri yapma.

1. Şimdi, Azure aboneliğinizle bir Azure App Service web uygulamasına Yayımla:  

   > [!NOTE]
   > Aşağıdaki iletişim kutularındaki yer alan birçok alan Azure aboneliğinizden alınan verilerle doldurulmuştur. Bu nedenle devam etmeden önce iletişim kutularının tamamen yüklenmesi için birkaç saniye beklemeniz gerekebilir.  

   a. ' Nde web sitesi proje düğümüne sağ tıklayın **Çözüm Gezgini**seçip **Web uygulaması yayımlama**.  

      [![VS - Çözüm Gezgini'nde web uygulaması yayımlama](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   b. Seçin **Başlat** uygulamanız yayımlanırken başlatmak için.

      [![VS - yayımlama profili](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   c. Uygulamayı yayımlamak için kullanmak istediğiniz aboneliği seçin. Seçin **TsiSpaApp** proje. Ardından, **Tamam**:

      [![VS - profile - app Service'e yayımlama](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   d. Tıklayın **Yayımla** web uygulamasını dağıtma işlemi.

      [![VS - Web uygulamasını yayımla - yayımlama günlüğü çıktısı](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   e. Visual Studio **Çıkış** penceresinde yayımlamanın başarılı olduğunu belirten bir günlük girişi olması gerekir. Dağıtım gerçekleştirildikten sonra Visual Studio web uygulamasını tarayıcı sekmesinde açar ve oturum açmanızı ister. Başarılı oturum açma işleminden sonra verilerle doldurulmuş TSI denetimleri görürsünüz.

## <a name="troubleshooting"></a>Sorun giderme  

Hata kodu/durumu | Açıklama
---------------------| -----------
*AADSTS50011: Uygulama için kayıtlı yanıt adresi yok.* | Azure AD kaydı eksik **yanıt URL'si** özelliği. Azure AD uygulama kaydınızın **Ayarlar** / **Yanıt URL’leri** sayfasına gidin. Doğrulayın **oturum açma** URL'si belirtilen **3. adım** , [uygulamayı Azure AD'ye kaydetme](#register-the-application-with-azure-ad) mevcuttur.
*AADSTS50011: Yanıt URL'si istekte belirtilen uygulama için yapılandırılan yanıt URL'lerinden eşleşmiyor: '\<Uygulama kimliği GUID >'.* | [Web uygulamasını derleme ve yayımlama](#build-and-publish-the-web-application) bölümünün 4.b adımında belirtilen `postLogoutRedirectUri` ile Azure AD uygulama kaydınızın **Ayarlar** / **Yanıt URL’leri** özelliğinde belirtilen değerin eşleşmesi gerekir. Ayrıca değiştirdiğinizden emin olun **hedef URL** kullanılacak `https`, başına **5. adım** , [oluşturun ve web uygulaması yayımlamaya](#build-and-publish-the-web-application).
Web uygulaması yükleniyor ancak stil içermeyen, beyaz arka plana sahip salt metin oturum açma sayfası var. | Yolları konusunda değinildiği doğrulayın **4. adım** , [oluşturun ve web uygulaması yayımlamaya](#build-and-publish-the-web-application) doğrudur. Web uygulaması .css dosyalarını bulamadığında sayfa stili doğru şekilde uygulanmaz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici çalışan birkaç Azure hizmeti oluşturur. Bu öğretici serisini tamamlamayı planlamıyorsanız, gereksiz maliyetlerin doğmasını önlemek için tüm kaynakları silmeniz önerilir.

Azure portalında soldaki menüden:

1. Seçin **kaynak grupları**, ardından TSI ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın.
1. Seçin **kaynak grupları**, ardından cihaz benzetimi çözüm Hızlandırıcı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure Active Directory’ye (AD) kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme

Bu öğretici, erişim belirteci alma amacıyla oturum açmış olan kullanıcının kimliğini kullanarak Azure AD ile tümleşir. TSI API’sine bir hizmet/daemon uygulamasının kimliğini kullanarak erişmeyi öğrenmek için bkz.

> [!div class="nextstepaction"]
> [Azure Time Series Insights API’si için kimlik doğrulaması ve yetkilendirme](time-series-insights-authentication-and-authorization.md)