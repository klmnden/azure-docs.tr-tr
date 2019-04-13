---
title: 'Öğretici: Bir Azure Time Series Insights tek sayfa web uygulaması oluşturma | Microsoft Docs'
description: TSI ortamındaki verileri sorgulayan ve işleyen tek sayfalı bir web uygulamasını nasıl oluşturacağınızı öğrenin.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/14/2018
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: fe8b6113646589e30ff839c8bd47968138d98b03
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521447"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Öğretici: Azure Time Series Insights tek sayfalı web uygulaması oluşturma

Bu öğretici, kendi TSI verilerinize erişmek için [Time Series Insights (TSI) örnek uygulamasına](https://insights.timeseries.azure.com/clientsample) göre modellenen tek sayfalı bir web uygulaması (SPA) oluşturma işlemi boyunca size kılavuzluk yapacaktır. Bu öğreticide şu konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure Active Directory’ye (AD) kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme 

## <a name="prerequisites"></a>Önkoşullar

Aboneliğiniz yoksa [ücretsiz Azure aboneliğine](https://azure.microsoft.com/free/) kaydolun. 

Önceden yapmadıysanız Visual Studio’yu da yüklemeniz gerekir. Bu öğretici için [ücretsiz Community sürümünü veya ücretsiz deneme sürümünü indirebilir/yükleyebilirsiniz](https://www.visualstudio.com/downloads/).

## <a name="application-design-overview"></a>Uygulama tasarımına genel bakış

Yukarıda belirtildiği gibi TSI örnek uygulaması bu öğreticide kullanılan tasarım ve kod için temel oluşturur. Kodda TSI İstemcisi JavaScript kitaplığı kullanılır. TSI İstemcisi kitaplığı iki ana API kategorisi için bir özet sunar:

- **TSI sorgu API'leri çağırmak için sarmalayıcı yöntemleri**: Sorgu olanak tanıyan TSI verileri JSON göre ifadeler kullanarak REST API'ler. Metotlar, kitaplığın `TsiClient.server` ad alanı altında düzenlenmiştir.
- **Oluşturma ve doldurma denetimleri grafik türleri çeşitli yöntemleri**: Bir web sayfasında TSI verileri görselleştirmek için kullanılan yöntemleri. Metotlar, kitaplığın `TsiClient.ux` ad alanı altında düzenlenmiştir.

Bu öğreticide örnek uygulamanın TSI ortamındaki veriler de kullanılacaktır. TSI örnek uygulamasının yapısı ve TSI İstemci kitaplığının kullanımı hakkında ayrıntılı bilgi için [Azure Time Series Insights JavaScript istemci kitaplığını keşfetme](tutorial-explore-js-client-lib.md) öğreticisine bakın.

## <a name="register-the-application-with-azure-ad"></a>Uygulamayı Azure AD’ye kaydetme 

Uygulamayı derlemeden önce Azure AD’ye kaydetmeniz gerekir. Kayıt işlemi sırasında uygulama için kimlik yapılandırması gerçekleştirilerek çoklu oturum açma için OAuth desteğini kullanması sağlanır. OAuth için SPA’larda "örtük" yetkilendirme kullanılması ve bunu uygulama bildiriminde güncelleştirmeniz gerekir. Uygulama bildirimi, uygulama kimliği yapılandırmasının JSON gösterimidir. 

1. Azure abonelik hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.  
1. Sol taraftaki bölmeden **Azure Active Directory** kaynağını ve ardından **Uygulama kayıtları**, **+ Yeni uygulama kaydı**’nı seçin:  
   
   ![Azure portalı Azure AD uygulaması kaydı](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)

1. **Oluştur** sayfasında gerekli parametreleri girin:
   
   Parametre|Açıklama
   ---|---
   **Ad** | Anlamlı bir kayıt adı girin.  
   **Uygulama türü** | SPA web uygulaması oluşturduğunuz için "Web uygulaması/API" olarak bırakın.
   **Oturum Açma URL'si** | Uygulamanın giriş/oturum açma sayfasının URL’sini girin. Uygulamayı (daha sonra) Azure App Service'te barındırılacak olduğundan, içinde bir URL kullanmanız gerekir "https:\//azurewebsites.net" etki alanı. Bu örnekte ad, kayıt adına dayalıdır.

   Girişleri tamamladığınızda yeni uygulama kaydını oluşturmak için **Oluştur**’a tıklayın.

   ![Azure portalı Azure AD uygulaması kaydı - oluşturma](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)

1. Kaynak uygulamaları, diğer uygulamalar tarafından kullanılan ve Azure AD’ye kaydedilecek REST API’lerini sağlar. API’ler "kapsamları" kullanarak istemci uygulamalarına ayrıntılı/güvenli erişim sağlar. Uygulamanız "Azure Time Series Insights" API’sini kullanacağından çalışma zamanında iznin isteneceği/verileceği API’yi ve kapsamı belirtmeniz gerekir. **Ayarlar**’ı ve ardından **Gerekli izinler**’i ve **+ Ekle**’yi seçin:

   ![Azure portalı Azure AD izin ekleme](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)

1. **API erişimi ekle** sayfasında **1 Bir API seçin**’e tıklayarak TSI API’sini belirtin. **Bir API seçin** sayfasında arama alanına "azure time" yazın. Sonuç listesinden "Azure Time Series Insights" API’sini seçip **Seçin**’e tıklayın: 

   ![Azure portalı Azure AD izinleri - API](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)

1. Şimdi bir API kapsamı belirtin. **API erişimi ekle** sayfasında **2 İzinleri seçin**’e tıklayın. **Erişimi Etkinleştir** sayfasında "Access Azure Time Series Insights hizmeti" kapsamını seçin. **Seçin**’e tıklayın ve açılan **API erişimi ekle** sayfasında **Bitti**’ye tıklayın:

   ![Azure portalı Azure AD izinleri - kapsam](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)

1. **Gerekli izinler** sayfasına döndüğünüzde "Azure Time Series Insights" API’sinin listelenmiş olduğunu görürsünüz. Ayrıca tüm kullanıcılar için, uygulamanın API’ye ve kapsama erişmesi amacıyla ön onay izni vermeniz gerekir. En üstteki **İzin ver** düğmesine tıklayıp **Evet**’i seçin:

   ![Azure portalı Azure AD gerekli izinler - onay](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)

1. Önceden belirtildiği üzere uygulama bildirimini de güncelleştirmeniz gerekir. İçerik haritasındaki uygulama adına tıklayarak **Kayıtlı uygulama** sayfasına dönün. **Bildirim**’i seçin, `oauth2AllowImplicitFlow` özelliğini `true` olarak değiştirin ve **Kaydet**’e tıklayın:

   ![Azure portalı Azure AD bildirim güncelleştirme](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)

1. Son olarak içerik haritasına tıklayarak **Kayıtlı uygulama** sayfasına dönün ve uygulamanızın **Giriş sayfası** URL’si ile **Uygulama kimliği** özelliklerini kopyalayın. Bu özellikleri ilerleyen adımlarda kullanacaksınız:

   ![Azure portalı Azure AD özellikleri](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)

## <a name="build-and-publish-the-web-application"></a>Web uygulamasını derleme ve yayımlama

1. Uygulamanızın proje dosyalarını depolamak için bir dizin oluşturun. Ardından aşağıdaki URL’lere tek tek göz atıp sayfanın sağ üst bölümündeki "Ham" bağlantısına sağ tıklayın ve "Farklı kaydet" komutuyla proje dizininize kaydedin:

   > [!NOTE]
   > Tarayıcıya bağlı olarak dosyayı kaydetmeden önce dosya uzantısını düzeltmeniz (HTML veya CSS olarak) değiştirmeniz gerekebilir.

   - **index.html** Sayfa için HTML ve JavaScript https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html
   - **sampleStyles.css:** CSS stil sayfası: https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css
    
1. Web uygulaması projesini oluşturmak için Visual Studio uygulamasını çalıştırın ve oturum açın. **Dosya** menüsünde **Aç**, **Web Sitesi** seçeneğini belirtin. **Web Sitesi Aç** iletişim kutusunda HTML ve CSS dosyalarını kaydettiğiniz çalışma dizinini seçin ve **Aç**’a tıklayın:

   ![VS - Dosya, aç, web sitesi](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)

1. Visual Studio **Görünüm** menüsünden **Çözüm Gezgini**’ni açın. HTML ve CSS dosyalarını içeren bir web sitesi projesine (dünya simgesi) sahip olan yeni çözümünüzün görünmesi gerekir:

   ![VS - Çözüm gezgini, yeni çözüm](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)

1. Uygulamayı yayımlayabilmeniz için **index.html** içindeki JavaScript kodu bölümlerini güncelleştirmeniz gerekir: 

   a. İlk olarak `<head>` öğesi içindeki JavaScript ve stil sayfası referansı yollarını değiştirin. Visual Studio çözümünüzdeki **index.html** dosyasını açın ve aşağıdaki JavaScript kodu satırlarını bulun. "PROD RESOURCE LINKS" altındaki üç satırın açıklamasını kaldırın ve "DEV RESOURCE LINKS" altındaki üç satırı açıklama satırı yapın:
   
      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Değiştirilen kodunuz aşağıdaki örneğe benzemelidir:

      ```javascript
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="sampleStyles.css"></link>
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css"></link>

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="pages/samples/sampleStyles.css"></link>
      <script src="dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="dist/tsiclient.css"></link> -->
      ```

   b. Ardından yeni Azure AD uygulama kaydınızı kullanmak için erişim belirteci mantığını değiştirin. `clientID` ve `postLogoutRedirectUri` değişkenlerini [Uygulamayı Azure AD’ye kaydetme](#register-the-application-with-azure-ad) bölümünün 9. adımında kopyaladığınız Uygulama Kimliği ve Giriş Sayfası URL’si değerlerini kullanacak şekilde değiştirin:

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Değiştirilen kodunuz aşağıdaki örneğe benzemelidir:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ``` 

   c. Düzenlemeyi tamamladığınızda **index.html** dosyasını kaydedin.

1. Şimdi web uygulamasını Azure aboneliğinizde Azure App Service olarak yayımlayın:  

   > [!NOTE]
   > Aşağıdaki iletişim kutularındaki yer alan birçok alan Azure aboneliğinizden alınan verilerle doldurulmuştur. Bu nedenle devam etmeden önce iletişim kutularının tamamen yüklenmesi için birkaç saniye beklemeniz gerekebilir.  

   a. **Çözüm Gezgini**’nde web sitesi proje düğümüne sağ tıklayın ve **Web Uygulamasını Yayımla**’yı seçin:  

      ![VS - Çözüm gezgini, web uygulamasını yayımla](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)

   b. Yayımlama hedefi oluşturmak için **Microsoft Azure App Service**’i seçin:  

      ![VS - Yayımlama profili](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)  

   c. Uygulamayı yayımlamak için kullanmak istediğiniz aboneliği seçip "Yeni"ye tıklayın: 

      ![VS - Yayımlama profili - app service](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)  

   d. **Uygulama Hizmetini Oluştur** iletişim kutusunun tüm alanlarının yüklenmesi için birkaç saniye bekledikten sonra aşağıdaki alanları değiştirin:
   
      Alan | Açıklama
      ---|---
      **Uygulama Adı** | [Uygulamayı Azure AD’ye kaydetme](#register-the-application-with-azure-ad) bölümünün 3. adımında kullandığınız Azure AD uygulama kaydı adıyla değiştirin. 
      **Kaynak Grubu** | **Yeni...** düğmesini kullanarak **Uygulama Adı** alanıyla aynı olacak şekilde değiştirin.
      **App Service Planı** | **Yeni...** düğmesini kullanarak **Uygulama Adı** alanıyla aynı olacak şekilde değiştirin.

      Tamamladığınızda **Oluştur**’a tıklayın. Azure kaynakları oluşturulurken sol alttaki **Dışarı Aktar** düğmesi "Dağıtılıyor:" olarak değiştirilir. Kaynaklar oluşturulduktan sonra önceki iletişim kutusu açılır. 

      ![VS - Yayımlama profili - yeni app service ekle](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service-create.png)  

   e. **Yayımla** iletişim kutusuna döndükten sonra **Yayımlama yöntemi** ayarının "Web dağıtımı" olduğundan emin olun. Ayrıca **Hedef URL**’yi Azure AD uygulama kaydıyla aynı olması için `https` yerine `http` kullanacak şekilde değiştirin. Ardından web uygulamasını dağıtmak için "Yayımla"ya tıklayın:  

      ![VS - Web uygulamasını yayımla - app service yayımla](media/tutorial-create-tsi-sample-spa/vs-publish-publish.png)  

   f. Visual Studio **Çıkış** penceresinde yayımlamanın başarılı olduğunu belirten bir günlük girişi olması gerekir. Dağıtım gerçekleştirildikten sonra Visual Studio web uygulamasını tarayıcı sekmesinde açar ve oturum açmanızı ister. Oturum açma işlemi başarılı olduktan sonra tüm TSI denetimlerinin verilerle doldurulmuş olduğunu göreceksiniz:  

      [![VS - Web uygulamasını yayımla - yayımlama günlüğü çıktısı](media/tutorial-create-tsi-sample-spa/vs-publish-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-output.png#lightbox)

      ![TSI SPA uygulaması - oturum açma](media/tutorial-create-tsi-sample-spa/tsispaapp-azurewebsites-net.png)  

## <a name="troubleshooting"></a>Sorun giderme  

Hata kodu/durumu | Açıklama
---------------------| -----------
*AADSTS50011: Uygulama için kayıtlı yanıt adresi yok.* | Azure AD kaydında "Reply URL" özelliği yok. Azure AD uygulama kaydınızın **Ayarlar** / **Yanıt URL’leri** sayfasına gidin. [Uygulamayı Azure AD’ye kaydetme](#register-the-application-with-azure-ad) bölümünün 3. adımında **Oturum açma** URL’sinin belirtildiğinden emin olun. 
*AADSTS50011: Yanıt URL'si istekte belirtilen uygulama için yapılandırılan yanıt URL'lerinden eşleşmiyor: '\<Uygulama kimliği GUID >'.* | [Web uygulamasını derleme ve yayımlama](#build-and-publish-the-web-application) bölümünün 4.b adımında belirtilen `postLogoutRedirectUri` ile Azure AD uygulama kaydınızın **Ayarlar** / **Yanıt URL’leri** özelliğinde belirtilen değerin eşleşmesi gerekir. Ayrıca adım 5.e’deki gibi, **Hedef URL**’yi `https` kullanacak şekilde değiştirdiğinizden emin olun. [Web uygulamasını derleme ve yayımlama](#build-and-publish-the-web-application).
Web uygulaması yükleniyor ancak stil içermeyen, beyaz arka plana sahip salt metin oturum açma sayfası var. | [Web uygulamasını derleme ve yayımlama](#build-and-publish-the-web-application) bölümünün 4.a adımında belirtilen yolların doğru olduğundan emin olun. Web uygulaması .css dosyalarını bulamadığında sayfa stili doğru şekilde uygulanmaz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici çalışan birkaç Azure hizmeti oluşturur. Bu öğretici serisini tamamlamayı planlamıyorsanız, gereksiz maliyetlerin doğmasını önlemek için tüm kaynakları silmeniz önerilir. 

Azure portalında soldaki menüden:

1. **Kaynak grupları** simgesine tıklayın, ardından TSI Ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın. 
1. **Kaynak grupları** simgesine tıklayın, ardından cihaz simülasyonu çözüm hızlandırıcısı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında **Kaynak grubunu sil**’e tıklayın, kaynak grubunun adını girin, ardından **Sil**’e tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure Active Directory’ye (AD) kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme 

Bu öğretici, erişim belirteci alma amacıyla oturum açmış olan kullanıcının kimliğini kullanarak Azure AD ile tümleşir. TSI API’sine bir hizmet/daemon uygulamasının kimliğini kullanarak erişmeyi öğrenmek için bkz.

> [!div class="nextstepaction"]
> [Azure Time Series Insights API’si için kimlik doğrulaması ve yetkilendirme](time-series-insights-authentication-and-authorization.md)
