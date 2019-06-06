---
title: Özel ilkeleri kullanarak Azure Active Directory B2C'yi kullanıcı arabirimini (UI) dinamik olarak özelleştirme | Microsoft Docs
description: Çalışma zamanında dinamik olarak değiştirir, HTML5/CSS içeriğiyle birden çok marka deneyimleri destekler.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e1abdfa8bc47f42f7373760370588c0bc41fc1dc
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507773"
---
# <a name="azure-active-directory-b2c-configure-the-ui-with-dynamic-content-by-using-custom-policies"></a>Azure Active Directory B2C: Özel ilkeler kullanarak dinamik içerik ile kullanıcı arabirimini yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C kullanarak (Azure AD B2C) özel ilkeler, bir sorgu dizesi parametresi gönderebilirsiniz. Parametreyi HTML uç noktanıza ileterek sayfa içeriğini dinamik olarak değiştirebilirsiniz. Örneğin web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü değiştirebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede Azure AD B2C'yi kullanıcı arabirimiyle özelleştirme odaklanır *dinamik içerik* özel ilkeler kullanarak. Başlamak için bkz: [UI özelleştirmesi özel bir ilkede](active-directory-b2c-ui-customization-custom.md). 

>[!NOTE]
>Azure AD B2C makaleyi [özel bir ilke yapılandırma kullanıcı Arabirimi özelleştirmesinde](active-directory-b2c-ui-customization-custom.md), aşağıdaki temelleri açıklanır:
> * Sayfa kullanıcı arabirimi (UI) özelleştirme özelliği.
> * Bizim örnek içerik kullanarak sayfa UI özelleştirmesi özelliği test etmek için gereken araçları sağlar.
> * Her sayfanın temel kullanıcı Arabirimi öğeleri yazın.
> * Bu özelliği uygulamak için en iyi yöntemler.

## <a name="add-a-link-to-html5css-templates-to-your-user-journey"></a>HTML5/CSS şablonları, kullanıcı yolculuğu için bir bağlantı ekleyin

Özel bir ilke içerik tanımı HTML5 sayfasında belirtilen UI adımı (örneğin, oturum açma veya kaydolma sayfaları) için kullanılan URI tanımlar. Temel ilke için bir URI, HTML5 dosyalarında (CSS) işaret ederek varsayılan görünümünü tanımlar. Uzantı ilkesinde LoadUri HTML5 dosyası için geçersiz kılarak görünümünü değiştirebilirsiniz. İçerik tanımlarını uygun şekilde HTML5/CSS dosyaları hazırlayın tarafından tanımlanan dış içerik için URL'leri içeriyor. 

`ContentDefinitions` Bölümü içeren bir dizi `ContentDefinition` XML öğeleri. ID özniteliği `ContentDefinition` öğesi için içerik tanımı ilişkili sayfa türü belirtir. Diğer bir deyişle, öğeyi uygulamak için özel bir HTML5/CSS şablon geçiyor bağlamı tanımlar. Aşağıdaki tablo, içerik tanımı IEF altyapısı ve bunlara ile ilgili sayfa türleri tarafından tanınan kimlik kümesini açıklar.

| İçerik tanımı kimliği | Varsayılan HTML5 şablonu| Açıklama | 
|-----------------------|--------|-------------|
| *api.error* | [Exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Hata sayfası**. Bu sayfa, bir özel durum veya hata ile karşılaşıldığında görüntülenir. |
| *api.idpselections* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcısı seçim sayfası**. Bu sayfa, oturum açma sırasında kullanıcıların seçebileceği kimlik sağlayıcıları listeler. Seçenekler, genellikle Kurumsal kimlik sağlayıcıları, Facebook ve Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları şunlardır. |
| *api.idpselections.signup* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcısı seçim için kaydolma**. Bu sayfa, kullanıcıların kayıt sırasında seçebileceği kimlik sağlayıcıları listeler. Seçenekler şunlardır: Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar. |
| *api.localaccountpasswordreset* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Parolanızı mı unuttunuz sayfasını**. Bu sayfa, kullanıcılar parola sıfırlama başlatmak için tamamlaması gereken form içerir.  |
| *api.localaccountsignin* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap oturum açma sayfası**. Bu sayfa, bir e-posta adresi veya kullanıcı adına göre yerel bir hesap ile oturum imzalamak için bir form içerir. Form, metin girişi kutusunu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap kaydolma sayfası**. Bu sayfa, bir e-posta adresi veya kullanıcı adına göre yerel bir hesap için kaydolmaya yönelik bir form içerir. Form gibi çeşitli giriş denetimleri içerebilen: bir metin girişi kutusunu, parola girişi kutusu, radyo düğmesi, tekli seçim açılır kutuları ve çoklu seçim kutuları işaretleyin. |
| *api.phonefactor* | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Çok faktörlü kimlik doğrulaması sayfası**. Bu sayfada, kullanıcıların kendi telefon numaralarını (metin ya da ses) kaydolma veya oturum açma sırasında doğrulayabilirsiniz. |
| *api.selfasserted* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Sosyal hesap kaydolma sayfası**. Bu sayfa, bunlar bir sosyal kimlik sağlayıcısı var olan bir hesap ile oturum açarken kullanıcıların tamamlamalısınız bir form içerir. Bu sayfa, önceki sosyal hesap kaydolma sayfası, parola giriş alanları dışında benzerdir. |
| *api.selfasserted.profileupdate* | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Profili güncelleştirme sayfasını**. Bu sayfa, kullanıcıların kendi profilini güncelleştirmek için erişebileceği bir form içerir. Bu sayfa, parola giriş alanları dışında sosyal hesap kaydolma sayfası benzerdir. |
| *api.signuporsignin* | [Unified.HTML](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Birleşik kaydolma veya oturum açma sayfası**. Bu sayfa kullanıcı kaydolma ve oturum açma işlemini gerçekleştirir. Kullanıcıların Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını kullanabilirsiniz.  |

## <a name="serving-dynamic-content"></a>Dinamik içerik sunma
İçinde [özel bir ilke yapılandırma kullanıcı Arabirimi özelleştirmesinde](active-directory-b2c-ui-customization-custom.md) makale, Azure Blob depolama alanına HTML5 dosyaları karşıya yükleme. HTML5 dosyaları statik olan ve aynı HTML her istek için içerik işleme. 

Bu makalede, sorgu dizesi parametrelerini kabul etmesi ve uygun şekilde yanıt bir ASP.NET web uygulaması kullanın. 

Bu kılavuzda:
* HTML5 şablonlarınızı barındıran bir ASP.NET Core web uygulaması oluşturun. 
* Özel bir HTML5 şablon eklemek _unified.cshtml_. 
* Web uygulamanızı Azure App Service'te yayımlayın. 
* Çıkış noktaları arası kaynak paylaşımı (CORS) web uygulamanız için ayarlayın.
* Geçersiz kılma `LoadUri` HTML5 dosyanıza işaret edecek şekilde öğeleri.

## <a name="step-1-create-an-aspnet-web-app"></a>1. adım: ASP.NET web uygulaması oluşturma

1. Visual Studio'da seçerek bir proje oluşturun **dosya** > **yeni** > **proje**.

2. İçinde **yeni proje** penceresinde **Visual C#**  > **Web** > **ASP.NET Core Web uygulaması (.NET Core)** .

3. Uygulama adı (örneğin, *Contoso.AADB2C.UI*) ve ardından **Tamam**.

    ![Yeni Visual Studio projesi oluşturma](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project1.png)

4. Seçin **Web uygulaması** şablonu.

5. Kimlik doğrulamasını ayarlamak **kimlik doğrulaması yok**.

    ![Web uygulaması şablonunu seçin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project2.png)

6. Seçin **Tamam** projeyi oluşturmak için.

## <a name="step-2-create-mvc-view"></a>2. adım: MVC görünümü oluşturma
### <a name="step-21-download-the-b2c-built-in-html5-template"></a>2.1. adım: B2C yerleşik HTML5 şablonunu indirme
Azure AD B2C'yi yerleşik HTML5 şablonu temel alan özel HTML5 şablonunuzu. İndirebileceğiniz [unified.html dosya](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) veya şablondan indirme [başlangıç paketi](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip). Birleşik kaydolma veya oturum açma sayfası oluşturmak için bu HTML5 dosyasını kullanın.

### <a name="step-22-add-the-mvc-view"></a>2.2. adım: MVC Görünümü Ekle
1. Görünümler/giriş klasörünü sağ tıklatın ve ardından **Ekle** > **yeni öğe**.

    ![MVC Yeni Öğe Ekle](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view1.png)

2. İçinde **Yeni Öğe Ekle - Contoso.AADB2C.UI** penceresinde **Web > ASP.NET**.

3. Seçin **MVC sayfası görüntüleme**.

4. İçinde **adı** kutusunda, ada değiştirin **unified.cshtml**.

5. **Add (Ekle)** seçeneğini belirleyin.

    ![MVC Görünümü Ekle](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view2.png)

6. Varsa *unified.cshtml* dosya açık değilse zaten, açmak için dosyaya çift tıklayın ve ardından dosya içerikleri temizleyin.

7. Bu kılavuz için şu düzen sayfası başvurusunu kaldırın. İçin aşağıdaki kod parçacığını ekleyin _unified.cshtml_:

    ```csharp
    @{
        Layout = null;
    }
    ```

8. Açık _unified.cshtml_ Azure AD B2C'yi yerleşik HTML5 şablondan indirilen dosya.

9. Tek işaretlerini değiştirin (@) ile iki et işareti (@@).

10. Dosyasının içeriğini kopyalayın ve düzen tanımı yapıştırın. Kod gibi görünmelidir:

    ![HTML5 ekledikten sonra unified.cshtml dosyası](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-edit-view1.png)

### <a name="step-23-change-the-background-image"></a>Adım 2.3: Arka plan resmi Değiştir

Bulun `<img>` öğesini içeren `ID` değer *background_background_image*ve ardından `src` değerini **https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1** veya diğer kullanmak istediğiniz arka plan resmi.

![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-static-background.png)

### <a name="step-24-add-your-view-to-the-mvc-controller"></a>2.4. adım: MVC denetleyicisi için Görünüm Ekle

1. Açık **Controllers\HomeController.cs**, yöntemi ekleyin: 

    ```C
    public IActionResult unified()
    {
        return View();
    }
    ```
    Bu kod yöntemi kullanması gerektiğini belirtir. bir *görünümü* tarayıcı yanıt oluşturmak için şablon dosyası. Biz açıkça adını belirtmeyin çünkü *görünümü* şablon dosyası, MVC varsayılan olarak kullanılacak _unified.cshtml_ görünümü dosyasında */görünümler/giriş* klasör.
    
    Siz ekledikten sonra _birleşik_ yöntemi, kod gibi görünmelidir:
    
    ![Denetleyici görünümü işlemek için değiştirme](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-controller-view.png)

2. Web uygulamanızda hata ayıklama ve emin _birleşik_ sayfa erişilebilir (örneğin, `http://localhost:<Port number>/Home/unified`).

### <a name="step-25-publish-to-azure"></a>2.5. adım: Azure'a Yayımlama
1. İçinde **Çözüm Gezgini**, sağ **Contoso.AADB2C.UI** proje ve ardından **Yayımla**.

    ![Microsoft Azure App Service'e yayımlama](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish1.png)

2. Seçin **Microsoft Azure App Service** kutucuğuna ve ardından **Yayımla**.

    ![Yeni Microsoft Azure App Service oluştur](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish2.png)

    **App Service Oluştur** penceresi açılır. İçinde ASP.NET web uygulamasını Azure'da çalıştırmak için tüm gerekli Azure kaynaklarını oluşturmaya başlayabilirsiniz.

    > [!NOTE]
    > Yayımlama hakkında daha fazla bilgi için bkz. [Azure'da bir ASP.NET web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

3. İçinde **Web uygulaması adı** benzersiz bir uygulama adı yazın (geçerli karakterler: a-z, A-Z, 0-9 ve tire (-). Web uygulamasının URL'si `http://<app_name>.azurewebsites.NET` şeklindedir; burada `<app_name>`, web uygulamanızın adıdır. Otomatik oluşturulmuş benzersiz adı kabul edebilirsiniz.

4. Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

    ![App Service özellikleri sağlar](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish3.png)

    Oluşturma işlemi tamamlandıktan sonra sihirbazın ASP.NET web uygulamasını Azure'da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

5. URL'sini kopyalayın _birleşik_ sayfa (örneğin, _https://<app_name>.azurewebsites.net/home/unified_).

## <a name="step-3-configure-cors-in-azure-app-service"></a>3. adım: Azure uygulama Hizmeti'nde CORS'yi yapılandırın
1. İçinde [Azure portalında](https://portal.azure.com/)seçin **uygulama hizmetleri**ve ardından API uygulamanızın adını seçin.

    ![Azure portalda API uygulamasını seçin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS1.png)

2. İçinde **ayarları** bölümündeki **API** bölümünden **CORS**.

    ![CORS ayarlarını seçin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS2.png)

3. İçinde **CORS** penceresi içinde **izin verilen çıkış noktaları** kutusunda, aşağıdakilerden birini yapın:

    * URL veya JavaScript çağrılarını alınmasına izin vermek istediğiniz URL'leri girin. Tüm küçük harfleri girdiğiniz URL'lerinde kullanmanız gerekir.
    * Tüm kaynak etki alanlarının kabul edildiğini belirtmek için yıldız işareti (*) girin.

4. **Kaydet**’i seçin.

    ![CORS penceresi](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS3.png)

    Seçtikten sonra **Kaydet**, API uygulaması belirtilen URL'lerden JavaScript çağrılarını kabul eder. 

## <a name="step-4-html5-template-validation"></a>4. Adım: HTML5 şablonu doğrulaması
HTML5 şablonunuzu kullanıma hazırdır. Ancak, kullanıma sunulmadı `ContentDefinition` kod. Ekleyebilmeniz için önce `ContentDefinition` özel ilkenizi emin olun:
* İçeriğinizi HTML5 uyumlu ve erişilebilir olduğunu.
* İçerik sunucunuz için CORS etkinleştirilir.

    >[!NOTE]
    >Burada barındırma içeriğinizi site CORS etkinleştirilmiş ve CORS istekleri sınayabilirsiniz doğrulamak için Git [test cors.org](https://test-cors.org/) Web sitesi. 

* Served içeriğinizi üzerinden güvenli **HTTPS**.
* Kullanmakta olduğunuz *mutlak URL'ler*, gibi `https://yourdomain/content`, tüm bağlantılar, CSS içeriği ve görüntüler.

## <a name="step-5-configure-your-content-definition"></a>5. Adım: İçerik tanımını yapılandırma
Yapılandırmak için `ContentDefinition`, aşağıdakileri yapın:
1. İlkenizin temel dosyasını açın (örneğin, *TrustFrameworkBase.xml*).

2. Arama `<ContentDefinitions>` öğenin ve tüm içeriğini kopyalayın `<ContentDefinitions>` düğümü.

3. Uzantı dosyasını açın (örneğin, *TrustFrameworkExtensions.xml*) ve ardından arama `<BuildingBlocks>` öğesi. Öğe yoksa, bunu ekleyin.

4. Tüm içeriğini yapıştırın `<ContentDefinitions>` alt öğesi olarak kopyaladığınız düğüm `<BuildingBlocks>` öğesi. 

5. Arama `<ContentDefinition>` içeren düğüm `Id="api.signuporsignin"` kopyaladığınız XML.

6. Değiştirin `LoadUri` gelen _~/tenant/default/unified_ için _https://<app_name>.azurewebsites.net/home/unified_.  
    Özel ilkenizi aşağıdaki gibi görünmelidir:
    
    ![İçerik tanımınızı](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-content-definition.png)

## <a name="step-6-upload-the-policy-to-your-tenant"></a>6. Adım: Kiracınız için ilkeyi karşıya yükle
1. İçinde [Azure portalında](https://portal.azure.com), geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.

2. Seçin **kimlik deneyimi çerçevesi**.

3. Seçin **tüm ilkeleri**.

4. Seçin **karşıya yükleme İlkesi**.

5. Seçin **ilke varsa üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkExtensions.xml* dosya ve doğrulama başarılı olun.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>7. Adım: Şimdi Çalıştır'i kullanarak özel bir ilkeyi test
1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi çerçevesi**.

    >[!NOTE]
    >Çalıştırma artık Kiracı'da önceden kayıtlı için en az bir uygulama gerektirir. Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.

2. Açık **B2C_1A_signup_signin**, yüklenmiş ve ardından bağlı olan taraf (RP) özel ilke **Şimdi Çalıştır**.  
    Daha önce oluşturduğunuz arka plan ile özel, HTML5 görebilmeniz gerekir.

    ![Kaydolma veya oturum açma ilkenizin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo1.png)

## <a name="step-8-add-dynamic-content"></a>8. adım: Dinamik İçerik Ekle
Adlandırılmış sorgu dizesi parametresini temel alan arka planını değiştirin _campaignId_. (Web ve mobil uygulamalar), RP uygulaması, Azure AD B2C'ye parametresi gönderir. İlkeniz, parametre okur ve HTML5 şablonunuza değeri gönderir. 

### <a name="step-81-add-a-content-definition-parameter"></a>8.1. adım: İçerik tanımı parametre ekleme

Ekleme `ContentDefinitionParameters` aşağıdakileri yaparak öğesi:
1. Açık *SignUpOrSignin* ilkenizin dosya (örneğin, *SignUpOrSignin.xml*).

2. Altında `<DefaultUserJourney>` düğümü Ekle `UserJourneyBehaviors` düğüm:  

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <UserJourneyBehaviors>
        <ContentDefinitionParameters>
          <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        </ContentDefinitionParameters>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```

### <a name="step-82-change-your-code-to-accept-a-query-string-parameter-and-replace-the-background-image"></a>8.2. adım: Bir sorgu dizesi parametresi kabul etmek için kodunuzu değiştirmeniz ve arka plan resmi Değiştir
HomeController değiştirme `unified` campaignId parametre kabul etmek için yöntemi. Yöntemin parametre ardından denetler. değer kullanıcının ve ayarlar `ViewData["background"]` değişkeni buna göre.

1. Açık *Controllers\HomeController.cs* dosyasını ve ardından değiştirmek `unified` aşağıdaki kod parçacığını ekleyerek yöntemi:

    ```csharp
    public IActionResult unified(string campaignId)
    {
        // If campaign ID is Hawaii, show Hawaii background
        if (campaignId != null && campaignId.ToLower() == "hawaii")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1";
        }
        // If campaign ID is Tokyo, show Tokyo background
        else if (campaignId != null && campaignId.ToLower() == "tokyo")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19666_en_1";
        }
        // Default background
        else
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/18983_en_1";
        }

        return View();
    }

    ```

2. Bulun `<img>` Kimliğine sahip öğe `background_background_image`ve yerine `src` değerini `@ViewData["background"]`.

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-dynamic-background.png)

### <a name="83-upload-the-changes-and-publish-your-policy"></a>8.3: Değişiklikleri karşıya yükleme ve ilkeniz yayımlama
1. Visual Studio projenizi Azure App Service'te yayımlayın.

2. Karşıya yükleme *SignUpOrSignin.xml* Azure AD B2C ilkesi.

3. Açık **B2C_1A_signup_signin**, yüklenmiş ve ardından RP özel ilke **Şimdi Çalıştır**.  
    Daha önce gösterilen aynı arka plan resminin görmeniz gerekir.

4. Tarayıcınızın adres çubuğundan URL'yi kopyalayın.

5. Ekleme _campaignId_ sorgu dizesi parametresi için URI. Örneğin, ekleme `&campaignId=hawaii`, aşağıdaki görüntüde gösterildiği gibi:

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-campaignId-param.png)

6. Seçin **Enter** Hawaii arka plan resmi görüntülenecek.

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo2.png)

7. Bir değerle değiştirmek *Tokyo*ve ardından **Enter**.  
    Tarayıcı Tokyo arka plan görüntüler.

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo3.png)

## <a name="step-9-change-the-rest-of-the-user-journey"></a>9. adım: Kullanıcı yolculuğu geri kalanını değiştirme
Seçerseniz **şimdi kaydolun** görüntü tanımladığınız, oturum açma sayfasında, tarayıcı bağlantısı görüntüler varsayılan arka plan resmi. Bu davranış, yalnızca kaydolma veya oturum açma sayfası değiştirdiğinizi doğurur. Kendini onaylama içerik tanımlarını geri kalanını değiştirmek için:
1. "Adım 2'ye" geri dönün ve aşağıdakileri yapın:

    a. İndirme *selfasserted* dosya.

    b. Dosya içeriğini kopyalayın.

    c. Yeni bir görünüm oluşturma *selfasserted*.

    d. Ekleme *selfasserted* için **giriş** denetleyicisi.

2. "Adım 4" geri dönün ve aşağıdakileri yapın: 

    a. Uzantı ilkenizi bulun `<ContentDefinition>` içeren düğüm `Id="api.selfasserted"`, `Id="api.localaccountsignup"`, ve `Id="api.localaccountpasswordreset"`.

    b. Ayarlama `LoadUri` özniteliğini, *selfasserted* URI.

3. "Adım 8.2 için" geri dönün ve sorgu dizesi parametreleri, ancak bu sefer kabul etmek üzere kod değişikliğiniz *selfasserted* işlevi. 

4. Karşıya yükleme *TrustFrameworkExtensions.xml* İlkesi, doğrulama başarılı olun.

5. İlke testi çalıştırın ve ardından **şimdi kaydolun** sonuçları görmek için.

## <a name="optional-download-the-complete-policy-files-and-code"></a>(İsteğe bağlı) Tüm ilke dosyaları ve kodu indirin
* Tamamladıktan sonra [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) izlenecek yol, öneririz senaryonuz kendi özel ilke dosyalarını kullanarak oluşturun. Referans olması açısından sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).
* Tam koddan indirebileceğiniz [başvuru için örnek Visual Studio çözümü](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).




