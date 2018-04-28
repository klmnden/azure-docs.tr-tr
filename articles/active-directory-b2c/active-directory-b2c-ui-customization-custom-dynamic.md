---
title: 'Azure Active Directory B2C: özel ilkeler kullanarak Azure AD B2C kullanıcı arabirimi (UI) dinamik olarak özelleştirebilirsiniz.'
description: Çalışma zamanında dinamik olarak değişir HTML5/CSS içeriğe sahip birden çok marka deneyimlerini destekler.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 09/20/2017
ms.author: davidmu
ms.openlocfilehash: 77f6ae1df8a364eecc5e7d6d6fe3a07dd215ac16
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-active-directory-b2c-configure-the-ui-with-dynamic-content-by-using-custom-policies"></a>Azure Active Directory B2C: kullanıcı Arabirimi ile dinamik içerik özel ilkeler kullanarak yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C kullanarak (Azure AD B2C) özel ilkeler, bir parametre bir sorgu dizesi olarak gönderebilirsiniz. HTML uç noktanızı parametresini geçirerek, sayfa içeriği dinamik olarak değiştirebilirsiniz. Örneğin, web ya da mobil uygulama geçirdiğiniz parametre temel Azure AD B2C kaydolma veya oturum açma sayfasında, arka plan resmi değiştirebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede Azure AD B2C kullanıcı arabirimiyle özelleştirmeyi odaklanır *dinamik içerik* özel ilkeler kullanarak. Başlamak için bkz: [özel bir ilke kullanıcı Arabirimi özelleştirmesinde](active-directory-b2c-ui-customization-custom.md). 

>[!NOTE]
>Azure AD B2C makaleyi [özel bir ilke yapılandırma kullanıcı Arabirimi özelleştirmesinde](active-directory-b2c-ui-customization-custom.md), aşağıdaki temelleri açıklanır:
> * Sayfa kullanıcı arabirimi (UI) özelleştirme özelliği.
> * Bizim örnek içeriği kullanarak sayfası kullanıcı Arabirimi özelleştirme özelliğini test etmek için gerekli araçları sağlar.
> * Çekirdek Kullanıcı Arabirimi öğeleri her bir sayfa türü.
> * Bu özelliği uygulamak için en iyi yöntemler.

## <a name="add-a-link-to-html5css-templates-to-your-user-journey"></a>HTML5/CSS şablonları kullanıcı Yolculuğunuzun için bir bağlantı ekleme

Özel bir ilke bir içerik tanımı HTML5 sayfasında belirtilen UI adım (örneğin, oturum açma veya kaydolma sayfaları) için kullanılan URI tanımlar. Temel ilke, varsayılan görünüm bir URI, HTML5 dosyalarında (CSS) göstererek tanımlar. Uzantı İlkesi'nde, Görünüm ve kullanımında LoadUri HTML5 dosyası için geçersiz kılarak değiştirebilirsiniz. HTML5/CSS dosyaları uygun şekilde hazırlayın tarafından tanımlanan dış içerik için URL'leri içerik tanımları içerir. 

`ContentDefinitions` Bölüm içeren bir dizi `ContentDefinition` XML öğeleri. ID özniteliği `ContentDefinition` öğesi içerik tanımına ilişkili sayfa türü belirtir. Diğer bir deyişle, öğe uygulamak için özel bir HTML5/CSS şablon geçiyor bağlamı tanımlar. Aşağıdaki tabloda içerik tanımı IEF altyapısı ve bunlara ile ilgili sayfa türleri tarafından tanınan kimlikleri açıklar.

| İçerik tanımı kimliği | Varsayılan HTML5 şablonu| Açıklama | 
|-----------------------|--------|-------------|
| *api.error* | [Exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Hata sayfası**. Bir özel durum ya da hatayla karşılaşıldığında bu sayfada görüntülenir. |
| *api.idpselections* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcısı seçimi sayfası**. Bu sayfa, oturum açma sırasında kullanıcıların seçebileceği kimlik sağlayıcıları listeler. Genellikle Kurumsal kimlik sağlayıcıları, Facebook ve Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları seçeneklerdir. |
| *api.idpselections.signup* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcısı seçimi için kaydolma**. Bu sayfa, kullanıcıların kayıt sırasında seçebileceği kimlik sağlayıcıları listeler. , Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar seçeneklerdir. |
| *api.localaccountpasswordreset* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Parolanızı mı unuttunuz sayfasını**. Bu sayfa, kullanıcıların bir parola sıfırlama başlatmak için tamamlamanız gereken bir form içerir.  |
| *api.localaccountsignin* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap oturum açma sayfası**. Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre yerel bir hesap ile oturum imzalama için bir form içerir. Form, metin giriş kutusu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap kayıt sayfasını**. Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre yerel bir hesap için kaydolduğunuz için bir form içerir. Formu gibi çeşitli giriş denetimlerini içerebilir: bir metin kutusu, bir parola giriş kutusu, bir radyo düğmesi, tek seçimlik açılan kutuları giriş ve çoklu seçim onay kutularını. |
| *api.phonefactor* | [çok faktörlü 1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Çok faktörlü kimlik doğrulaması sayfası**. Bu sayfada, kullanıcıların telefon numaralarına (metin veya sesli kullanarak) kaydolma veya oturum açma sırasında doğrulayabilirsiniz. |
| *api.selfasserted* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Sosyal hesap kayıt sayfası**. Bu sayfa, bunlar bir sosyal kimlik sağlayıcısından var olan bir hesabı kullanarak kaydolduğunuzda, kullanıcılar tamamlamalısınız form içerir. Bu sayfa, önceki sosyal hesap kayıt sayfası, parola girdi alanlarının dışında benzerdir. |
| *api.selfasserted.profileupdate* | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Profil güncelleştirme sayfası**. Bu sayfa, kullanıcıların kendi profilini güncelleştirmek için erişebileceği bir form içerir. Bu sayfa, sosyal hesap kaydolma sayfası, parola girdi alanlarının dışında benzerdir. |
| *api.signuporsignin* | [Unified.HTML](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Birleşik kayıt veya oturum açma sayfası**. Bu sayfa kullanıcı kaydolma ve oturum açma işlemini gerçekleştirir. Kullanıcıların Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanabilirsiniz.  |

## <a name="serving-dynamic-content"></a>Dinamik içerik hizmet veren
İçinde [özel bir ilke yapılandırma kullanıcı Arabirimi özelleştirmesinde](active-directory-b2c-ui-customization-custom.md) makale, Azure Blob depolama alanına HTML5 dosyaları karşıya yükleme. HTML5 dosyaları statik ve aynı HTML her istek için içerik oluşturmak. 

Bu makalede, sorgu dizesi parametreleri kabul edebilir ve uygun şekilde yanıt bir ASP.NET web uygulamasını kullanın. 

Bu kılavuzda:
* HTML5 şablonlarınızı barındıran bir ASP.NET Core web uygulaması oluşturun. 
* Özel bir HTML5 Şablon Ekle _unified.cshtml_. 
* Web uygulamanızı Azure App Service'e yayımlayın. 
* Çıkış noktaları arası kaynak paylaşımını (CORS) web uygulamanız için ayarlayın.
* Geçersiz kılma `LoadUri` HTML5 dosyanıza işaret edecek şekilde öğeleri.

## <a name="step-1-create-an-aspnet-web-app"></a>1. adım: bir ASP.NET web uygulaması oluşturma

1. Visual Studio'da seçerek bir proje oluşturun **dosya** > **yeni** > **proje**.

2. İçinde **yeni proje** penceresinde, seçin **Visual C#** > **Web** > **ASP.NET çekirdek Web uygulaması (.NET Core)**.

3. Uygulama adı (örneğin, *Contoso.AADB2C.UI*) ve ardından **Tamam**.

    ![Yeni Visual Studio projesi oluşturma](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project1.png)

4. Seçin **Web uygulaması** şablonu.

5. Kimlik doğrulamasını ayarlamak **doğrulaması yok**.

    ![Web uygulaması şablonunu seçin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project2.png)

6. Seçin **Tamam** projesi oluşturmak için.

## <a name="step-2-create-mvc-view"></a>2. adım: MVC görünümü oluşturma
### <a name="step-21-download-the-b2c-built-in-html5-template"></a>2.1. adım: B2C yerleşik HTML5 şablonu indirme
Özel HTML5 şablonunuzun Azure AD B2C yerleşik HTML5 şablona dayalıdır. İndirebilirsiniz [unified.html dosya](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) veya şablonu indirme [başlangıç paketi](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip). Birleşik bir kayıt veya oturum açma sayfası oluşturmak için bu HTML5 dosyasını kullanın.

### <a name="step-22-add-the-mvc-view"></a>2.2. adım: MVC görünümü ekleme
1. Görünümler/giriş klasörünü sağ tıklatın ve ardından **Ekle** > **yeni öğe**.

    ![MVC Yeni Öğe Ekle](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view1.png)

2. İçinde **Yeni Öğe Ekle - Contoso.AADB2C.UI** penceresinde, seçin **Web > ASP.NET**.

3. Seçin **MVC görüntüleme sayfası**.

4. İçinde **adı** kutusunda, adına değiştirme **unified.cshtml**.

5. **Add (Ekle)** seçeneğini belirleyin.

    ![MVC görünümü ekleme](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view2.png)

6. Varsa *unified.cshtml* dosyası değil açık zaten, açmak için dosyaya çift tıklayın ve sonra dosya içeriğini temizleyin.

7. Bu kılavuz için Düzen sayfası başvurusunu kaldırın. Aşağıdaki kod parçacığını ekleyin _unified.cshtml_:

    ```csharp
    @{
        Layout = null;
    }
    ```

8. Açık _unified.cshtml_ Azure AD B2C yerleşik HTML5 şablondan karşıdan dosya.

9. Tek işaretlerini değiştirin (@) çift işaretlerini (@@) sahip.

10. Dosyanın içeriğini kopyalayın ve düzen tanımı yapıştırın. Kod gibi görünmelidir:

    ![HTML5 ekledikten sonra unified.cshtml dosyası](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-edit-view1.png)

### <a name="step-23-change-the-background-image"></a>2.3. adım: arka plan resmi değiştirme

Bulun `<img>` içeren öğeyi `ID` değeri *background_background_image*ve ardından Değiştir `src` değerini **https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1** veya diğer kullanmak istediğiniz arka plan resmi.

![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-static-background.png)

### <a name="step-24-add-your-view-to-the-mvc-controller"></a>2.4. adım: MVC denetleyicisi görünümü ekleme

1. Açık **Controllers\HomeController.cs**, yöntemi ekleyin: 

    ```C
    public IActionResult unified()
    {
        return View();
    }
    ```
    Bu kod yöntemi kullanması gerektiğini belirtir bir *Görünüm* tarayıcı yanıta işlemek için şablon dosyası. Biz açıkça adını belirtmeyin çünkü *Görünüm* şablon dosyası, MVC varsayılan olarak kullanmaya _unified.cshtml_ görünüm dosyasında */görünümler/giriş* klasör.
    
    Eklediğiniz sonra _birleştirilmiş_ yöntemi, kodunuzu gibi görünmelidir:
    
    ![Görünümü işlemek için denetleyici değiştirme](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-controller-view.png)

2. Web uygulamanızın hatalarını ayıklama ve olduğundan emin olun _birleştirilmiş_ sayfasıdır erişilebilir (örneğin, `http://localhost:<Port number>/Home/unified`).

### <a name="step-25-publish-to-azure"></a>2.5. adım: Azure'a yayımlama
1. İçinde **Çözüm Gezgini**, sağ **Contoso.AADB2C.UI** proje ve ardından **Yayımla**.

    ![Microsoft Azure App Service'te yayımlama](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish1.png)

2. Seçin **Microsoft Azure App Service** kutucuğuna ve ardından **Yayımla**.

    ![Yeni Microsoft Azure App Service oluştur](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish2.png)

    **App Service Oluştur** penceresi açılır. İçinde Azure'da ASP.NET web uygulamasını çalıştırmak için gereken tüm Azure kaynakların oluşturmaya başlayabilirsiniz.

    > [!NOTE]
    > Yayımlama hakkında daha fazla bilgi için bkz: [bir ASP.NET web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3. İçinde **Web uygulaması adı** benzersiz uygulama adı yazın (geçerli karakterler: a-z, A-Z, 0-9 ve tire (-). Web uygulamasının URL'si `http://<app_name>.azurewebsites.NET` şeklindedir; burada `<app_name>`, web uygulamanızın adıdır. Otomatik oluşturulmuş benzersiz adı kabul edebilirsiniz.

4. Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

    ![Uygulama hizmeti özellikleri sağlar](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish3.png)

    Oluşturma işlemi tamamlandıktan sonra sihirbazın ASP.NET web uygulaması için Azure yayımlar ve ardından varsayılan tarayıcıda uygulamasını başlatıyor.

5. URL'sini kopyalayın _birleştirilmiş_ sayfa (örneğin, _https://<app_name>.azurewebsites.net/home/unified_).

## <a name="step-3-configure-cors-in-azure-app-service"></a>3. adım: Azure App Service'de CORS'yi yapılandırın
1. İçinde [Azure portal](https://portal.azure.com/)seçin **uygulama hizmetleri**ve ardından API uygulamanızın adı seçin.

    ![Azure portalda API uygulamasını seçin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS1.png)

2. İçinde **ayarları** bölümünde **API** bölümünde, select **CORS**.

    ![CORS ayarlarını seçin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS2.png)

3. İçinde **CORS** penceresi, **izin verilen çıkış noktası** kutusunda, aşağıdakilerden birini yapın:

    * URL veya JavaScript çağrılarını alınmasına izin vermek istediğiniz URL'leri girin.
    * Tüm kaynak etki alanlarının kabul edildiğini belirtmek üzere bir yıldız işareti (*) girin.

4. **Kaydet**’i seçin.

    ![CORS penceresi](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS3.png)

    Siz seçtikten sonra **kaydetmek**, API uygulaması belirtilen URL'lerden JavaScript çağrılarını kabul eder. 

## <a name="step-4-html5-template-validation"></a>4. adım: HTML5 şablonu doğrulama
HTML5 şablonunuzu kullanılmaya hazırdır. Ancak, kullanılabilir değil `ContentDefinition` kodu. Ekleyebilmeniz için önce `ContentDefinition` özel ilkeniz emin olun:
* İçeriğinizi HTML5 uyumlu ve erişilebilir olduğunu.
* İçeriğinize CORS için etkinleştirilir.

    >[!NOTE]
    >Burada barındırma içeriğinizi site CORS etkin olduğundan ve CORS isteklerini test edebilirsiniz doğrulamak için Git [test cors.org](http://test-cors.org/) Web sitesi. 

* Sunulacak içeriğinizi üzerinden güvenli **HTTPS**.
* Kullanmakta olduğunuz *mutlak URL'ler*, gibi *https://yourdomain/content*, tüm bağlantılar, CSS içerik ve görüntüler.

## <a name="step-5-configure-your-content-definition"></a>Adım 5: içerik tanımınızı yapılandırma
Yapılandırmak için `ContentDefinition`, aşağıdakileri yapın:
1. İlkeniz temel dosyasını açın (örneğin, *TrustFrameworkBase.xml*).

2. Arama `<ContentDefinitions>` öğesini ve tüm içeriğini kopyalayın `<ContentDefinitions>` düğümü.

3. Uzantı dosyasını açın (örneğin, *TrustFrameworkExtensions.xml*) arayın ve sonra `<BuildingBlocks>` öğesi. Öğe yoksa, ekleyin.

4. Tüm içeriğini yapıştırın `<ContentDefinitions>` bir alt öğesi olarak kopyaladığınız düğümü `<BuildingBlocks>` öğesi. 

5. Arama `<ContentDefinition>` içeren düğümü `Id="api.signuporsignin"` kopyaladığınız XML.

6. Değerini değiştirme `LoadUri` gelen _~/tenant/default/unified_ için _https://<app_name>.azurewebsites.net/home/unified_.  
    Özel ilkeniz aşağıdaki gibi görünmelidir:
    
    ![İçerik tanımınızı](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-content-definition.png)

## <a name="step-6-upload-the-policy-to-your-tenant"></a>6. adım: ilke kiracınız için karşıya yükleme
1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.

2. Seçin **kimlik deneyimi Framework**.

3. Seçin **tüm ilkeleri**.

4. Seçin **karşıya İlkesi**.

5. Seçin **varsa ilkesi üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkExtensions.xml* dosya ve doğrulama başarılı olduğundan emin olun.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>7. adım: Test Şimdi Çalıştır kullanarak özel İlkesi
1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.

    >[!NOTE]
    >Çalışma şimdi Kiracı'preregistered için en az bir uygulama için gereklidir. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.

2. Açık **B2C_1A_signup_signin**, karşıya ve ardından bağlı olan taraf (RP) özel ilkesini **Şimdi Çalıştır**.  
    Daha önce oluşturduğunuz arka plana sahip özel, HTML5 görebilmeniz gerekir.

    ![Kaydolma veya oturum açma ilkesi](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo1.png)

## <a name="step-8-add-dynamic-content"></a>8. adım: dinamik içerik ekleme
Adlandırılmış sorgu dizesi parametresi temelinde arka planını değiştirin _campaignId_. RP uygulamanız (web ve mobil uygulamalar) Azure AD B2C'ye parametresi gönderir. İlkeniz parametresi okur ve HTML5 şablonunuzu değeri gönderir. 

### <a name="step-81-add-a-content-definition-parameter"></a>8.1. adım: bir içerik tanımı parametre ekleme

Ekleme `ContentDefinitionParameters` aşağıdakileri yaparak öğe:
1. Açık *SignUpOrSignin* ilkenizin dosya (örneğin, *SignUpOrSignin.xml*).

2. Arama `<DefaultUserJourney>` düğümü. 

3. İçinde `<DefaultUserJourney>` düğümü, aşağıdaki XML parçacığını ekleyin:  

    ```XML
    <UserJourneyBehaviors>
        <ContentDefinitionParameters>
            <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ```

### <a name="step-82-change-your-code-to-accept-a-query-string-parameter-and-replace-the-background-image"></a>8.2. adım: kodunuzu bir sorgu dizesi parametresi kabul edecek şekilde değiştirmeniz ve arka plan resmi değiştirin
HomeController değiştirme `unified` campaignId parametresini kabul yöntemi. Yöntem parametresi sonra denetler değer adı ve ayarlar `ViewData["background"]` değişkeni buna göre.

1. Açık *Controllers\HomeController.cs* dosyasını bulun ve sonra değiştirmek `unified` aşağıdaki kod parçacığını ekleyerek yöntemi:

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

2. Bulun `<img>` kimlikli öğe `background_background_image`ve yerine `src` değerini `@ViewData["background"]`.

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-dynamic-background.png)

### <a name="83-upload-the-changes-and-publish-your-policy"></a>8.3: değişiklikleri karşıya yüklemek ve ilkeniz yayımlama
1. Visual Studio projenizi Azure App Service'e yayımlayın.

2. Karşıya yükleme *SignUpOrSignin.xml* ilke Azure AD B2C.

3. Açık **B2C_1A_signup_signin**, karşıya ve ardından RP özel ilkesini **Şimdi Çalıştır**.  
    Daha önce gösterilen aynı arka plan görüntüsü görmeniz gerekir.

4. Tarayıcınızın adres çubuğundan URL'yi kopyalayın.

5. Ekleme _campaignId_ sorgu dizesi parametresi URI. Örneğin, ekleyin `&campaignId=hawaii`, aşağıdaki görüntüde gösterildiği gibi:

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-campaignId-param.png)

6. Seçin **Enter** Havai arka plan görüntüsü görüntülemek için.

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo2.png)

7. Bir değerle değiştirmek *Tokyo*ve ardından **Enter**.  
    Tarayıcı, Tokyo arka plan görüntüler.

    ![Sayfa arka planını değiştirin](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo3.png)

## <a name="step-9-change-the-rest-of-the-user-journey"></a>9. adım: kullanıcı gezisine kalan değiştirme
Seçerseniz **şimdi kaydolun** oturum açma sayfasında, tarayıcı bağlantı görüntüler varsayılan arka plan görüntüsü, tanımladığınız görüntü değil. Bu davranış, yalnızca kaydolma veya oturum açma sayfası değiştirdiyseniz doğurur. Kendi kendine Assert içerik tanımları kalan değiştirmek için:
1. "Adım 2'ye" geri dönün ve aşağıdakileri yapın:

    a. Karşıdan *selfasserted* dosya.

    b. Dosya içeriğini kopyalayın.

    c. Yeni bir görünüm oluşturma *selfasserted*.

    d. Ekleme *selfasserted* için **giriş** denetleyicisi.

2. "Adım 4" geri dönün ve aşağıdakileri yapın: 

    a. Uzantı ilkenizde Bul `<ContentDefinition>` içeren düğümü `Id="api.selfasserted"`, `Id="api.localaccountsignup"`, ve `Id="api.localaccountpasswordreset"`.

    b. Ayarlama `LoadUri` özniteliğini, *selfasserted* URI.

3. "Adım 8.2" geri dönün ve sorgu dizesi parametreleri, ancak bu kez kabul etmek için kodunu değiştirmeniz *selfasserted* işlevi. 

4. Karşıya yükleme *TrustFrameworkExtensions.xml* İlkesi ve doğrulama başarılı olduğundan emin olun.

5. İlke testini çalıştırmak ve ardından **şimdi kaydolun** sonuçları görmek için.

## <a name="optional-download-the-complete-policy-files-and-code"></a>(İsteğe bağlı) Kod ve tam ilke dosyaları indirme
* Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).
* Tam koddan indirebilirsiniz [başvurusu için örnek Visual Studio çözümü](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).




