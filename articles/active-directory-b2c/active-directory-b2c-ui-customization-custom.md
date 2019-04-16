---
title: Azure Active Directory B2C'de bir özel ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme | Microsoft Docs
description: Özel bir ilke kullanarak Azure Active Directory B2C'de bir kullanıcı arabirimi özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7463a61945524672c5124966db2464c036559db7
ms.sourcegitcommit: e89b9a75e3710559a9d2c705801c306c4e3de16c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59571337"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de bir özel ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makaleyi tamamladıktan sonra marka ve görünüm ile kaydolma ve oturum açma özel İlkesi gerekir. Azure Active Directory B2C ile kullanıcılar için sunulan HTML ve CSS içeriğin neredeyse tam denetim elde (Azure AD B2C). Özel bir ilke kullandığınızda, kullanıcı arabirimini özelleştirme XML denetimleri Azure Portalı'nda kullanmak yerine yapılandırdığınız. 

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md). Kaydolma ve oturum açma ile yerel hesaplar için bir çalışma özel ilke olması gerekir.

## <a name="page-ui-customization"></a>Sayfa UI Özelleştirmesi

Sayfa UI özelleştirmesi özelliğini kullanarak, herhangi bir özel ilke görünümünü özelleştirebilirsiniz. Ayrıca bu sayede uygulamanızla Azure AD B2C arasında marka ve görsel tutarlılığı sağlayabilirsiniz.

Şekli aşağıda verilmiştir: Azure AD B2C kod müşterinizin tarayıcıda çalışan ve modern bir yaklaşımı adlı kullanır [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/). İlk olarak, özelleştirilmiş HTML içerikli özel ilkesindeki bir URL belirtin. Azure AD B2C, kullanıcı arabirimi öğelerini URL'nizden yüklenen HTML içeriğiyle birleştirdikten sonra sayfayı müşteriye gösterir.

## <a name="create-your-html5-content"></a>HTML5 içerik oluşturma

HTML başlığında ürününüzün marka adı ile içerik oluşturun.

1. Aşağıdaki HTML kod parçacığını kopyalayın. İyi biçimlendirilmiş adlı boş bir öğe ile HTML5 *\<div kimliği = "API"\>\</div\>* içinde bulunan *\<gövdesi\>* etiketler. Bu öğe, Azure AD B2C içeriğin eklenecek olduğu gösterir.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

2. Kopyalanmış kod parçacığı bir metin düzenleyiciye yapıştırın ve ardından dosyayı farklı Kaydet *özelleştirme ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob Depolama hesabı oluşturma

>[!NOTE]
> Bu makalede, Azure Blob Depolama içeriklerimizde barındırmak için kullanırız. Bir web sunucusunda içeriğinizi barındırmak seçebilirsiniz, ancak gerekir [web sunucunuzda CORS'yi etkinleştirme](https://enable-cors.org/server.html).

Blob depolama alanındaki bu HTML içeriğini barındırmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üzerinde **Hub** menüsünde **yeni** > **depolama** > **depolama hesabı**.
3. Benzersiz bir girin **adı** depolama hesabınız için.
4. **Dağıtım modeli** kalabileceği **Resource Manager**.
5. Değişiklik **hesap türü** için **Blob Depolama**.
6. **Performans** kalabileceği **standart**.
7. **Çoğaltma** kalabileceği **RA-GRS**.
8. **Erişim katmanı** kalabileceği **etkin**.
9. **Depolama hizmeti şifrelemesi** kalabileceği **devre dışı bırakılmış**.
10. Seçin bir **abonelik** depolama hesabınız için.
11. Oluşturma bir **kaynak grubu** veya var olan bir hesabı seçin.
12. Seçin **coğrafi konum** depolama hesabınız için.
13. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.  
    Dağıtım tamamlandıktan sonra **depolama hesabı** dikey penceresi otomatik olarak açılır.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Blob Depolama alanında ortak bir kapsayıcı oluşturmak için aşağıdakileri yapın:

1. Tıklayın **genel bakış** sekmesi.
2. Tıklayın **kapsayıcı**.
3. İçin **adı**, türü **$root**.
4. Ayarlama **erişim türü** için **Blob**.
5. Tıklayın **$root** yeni bir kapsayıcı açın.
6. **Karşıya Yükle**'ye tıklayın.
7. Klasör simgesine tıklayın **bir dosya seçin**.
8. Git **özelleştirme ui.html**, sayfa UI özelleştirmesi bölümünde daha önce oluşturduğunuz.
9. **Karşıya Yükle**'ye tıklayın.
10. Karşıya yüklediğiniz Özelleştir ui.html blob seçin.
11. Yanındaki **URL**, tıklayın **kopyalama**.
12. Bir tarayıcıda, kopyalanan URL'yi yapıştırın ve sitesine gidin. Site erişilemez durumdaysa emin kapsayıcı erişim türü ayarlanır **blob**.

## <a name="configure-cors"></a>CORS Yapılandırma

BLOB Depolama aşağıdakileri yaparak çıkış noktaları arası kaynak paylaşımı için yapılandırın:

1. Menüde **CORS**.
2. İçin **çıkış noktaları**, girin `https://your-tenant-name.b2clogin.com`. Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı. Örneğin, `https://fabrikam.b2clogin.com`. Kiracı adınızın girerken tamamen küçük harf kullanmanız gerekir.
3. İçin **izin verilen yöntemleri**, her ikisini de seçin `GET` ve `OPTIONS`.
4. İçin **izin verilen üstbilgileri**, yıldız işareti (*) girin.
5. İçin **kullanıma sunulan üst bilgiler**, yıldız işareti (*) girin.
6. İçin **Maksimum yaş**, 200 girin.
7. **Kaydet**’e tıklayın.

## <a name="test-cors"></a>CORS'yi test etme

Aşağıdakileri yaparak hazır kimliğinizi doğrulayın:

1. Git [www.test-cors.org](https://www.test-cors.org/) Web sitesine gidin ve sonra URL'yi yapıştırın **uzak URL** kutusu.
2. Tıklayın **gönderme isteği**.  
    Bir hata alırsanız, emin olun, [CORS ayarları](#configure-cors) doğrudur. Tarayıcı önbelleğini temizlemeniz veya Ctrl + Shift + P tuşlarına basarak bir özel tarama oturumu açmak gerekebilir.

## <a name="modify-the-extensions-file"></a>Uzantıları dosyasını değiştirme

UI Özelleştirme yapılandırmak için kopyalama **ContentDefinition** ve uzantıları dosyasının temel dosyasından onun alt öğeleri.

1. İlkenizin temel dosyasını açın. Örneğin, *TrustFrameworkBase.xml*.
2. İçin arama yapın ve tüm içeriğini kopyalayın **ContentDefinitions** öğesi.
3. Uzantı dosyası açın. Örneğin, *TrustFrameworkExtensions.xml*. Arama **BuildingBlocks** öğesi. Öğe yoksa, bunu ekleyin.
4. Tüm içeriğini yapıştırın **ContentDefinitions** öğesi alt öğesi olarak kopyaladığınız **BuildingBlocks** öğesi. 
5. Arama **ContentDefinition** öğesini içeren `Id="api.signuporsignin"` kopyaladığınız XML.
6. Değiştirin **LoadUri** depolama alanına yüklenir HTML dosyasının URL'si. Örneğin, `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.
    
    Özel ilkenizi aşağıdaki gibi görünmelidir:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

7. Uzantıları dosyayı kaydedin.

## <a name="upload-your-updated-custom-policy"></a>Güncelleştirilmiş özel ilkenizi karşıya yükleme

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kimlik deneyimi çerçevesi**.
2. Tıklayın **tüm ilkeleri**.
3. Tıklayın **karşıya yükleme İlkesi**.
4. Daha önce değiştirdiğiniz extensions dosyasını yükleyin.

## <a name="test-the-custom-policy-by-using-run-now"></a>Özel ilke kullanarak test **Şimdi Çalıştır**

1. Üzerinde **Azure AD B2C** dikey penceresinde, Git **tüm ilkeleri**.
2. Yüklenmiş ve'ı tıklatın özel bir ilkeyi seçin **Şimdi Çalıştır** düğmesi.
3. Bir e-posta adresi kullanarak kaydolun olması gerekir.

## <a name="reference"></a>Başvuru

Örnek şablonları UI özelleştirmesi burada bulabilirsiniz:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Aşağıdaki HTML dosyaları sample_templates/wingtip klasör içerir:

| HTML5 şablonu | Açıklama |
|----------------|-------------|
| *phonefactor.HTML* | Bu dosya için çok faktörlü kimlik doğrulaması sayfası şablon olarak kullanın. |
| *ResetPassword.HTML* | Bu dosya için bir şablon olarak kullanmak bir parolamı unuttum sayfası. |
| *selfasserted.html* | Bu dosya, bir sosyal hesap kaydolma sayfası, bir yerel hesap kaydolma sayfası veya bir yerel hesap oturum açma sayfası için bir şablon olarak kullanın. |
| *Unified.HTML* | Bu dosya bir birleşik kaydolma veya oturum açma sayfası için bir şablon olarak kullanın. |
| *updateprofile.html* | Bu dosya için bir profil güncelleştirme sayfasını şablon olarak kullanın. |

İçerik tanımı için yapılandırdığınız Değiştir kaydolma veya oturum açma özel bir ilke bölümü `api.idpselections`. İçerik kümesinin aşağıdaki tabloda, Azure AD B2C kimlik deneyimi çerçevesi ve açıklamalarının tarafından tanınan tanım kimlikleri şunlardır:

| İçerik tanımı kimliği | Açıklama | 
|-----------------------|-------------|
| *api.error* | **Hata sayfası**. Bu sayfa, bir özel durum veya hata ile karşılaşıldığında görüntülenir. |
| *api.idpselections* | **Kimlik sağlayıcısı seçim sayfası**. Bu sayfa, oturum açma sırasında kullanıcı seçebileceği kimlik sağlayıcılarının bir listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar seçeneklerdir. |
| *api.idpselections.signup* | **Kimlik sağlayıcısı seçim için kaydolma**. Bu sayfa kullanıcı kayıt sırasında seçebileceği kimlik sağlayıcılarının bir listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar seçeneklerdir. |
| *api.localaccountpasswordreset* | **Parolanızı mı unuttunuz sayfasını**. Bu sayfa kullanıcı parola sıfırlama başlatmak için tamamlaması gereken form içerir.  |
| *api.localaccountsignin* | **Yerel hesap oturum açma sayfası**. Bu sayfa, bir e-posta adresi veya kullanıcı adına göre yerel bir hesap ile oturum imzalamak için bir oturum açma formunu içerir. Form, metin girişi kutusunu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | **Yerel hesap kaydolma sayfası**. Bu sayfa, bir e-posta adresi veya bir kullanıcı adı temel alan bir yerel hesap kaydolma için bir kayıt formunu içerir. Form, metin girişi kutusunu, parola girişi kutusu, radyo düğmesi, tekli seçim açılır kutuları ve çoklu seçim onay kutularını gibi çeşitli giriş denetimleri içerebilir. |
| *api.phonefactor* | **Çok faktörlü kimlik doğrulaması sayfası**. Bu sayfada, kullanıcıların kendi telefon numaralarını (metin ya da ses) kaydolma veya oturum açma sırasında doğrulayabilirsiniz. |
| *api.selfasserted* | **Sosyal hesap kaydolma sayfası**. Bu sayfa, bunlar bir sosyal kimlik sağlayıcısı Facebook veya Google + gibi var olan bir hesap ile oturum açarken kullanıcıların tamamlamalısınız kaydolma form içerir. Bu sayfa, önceki sosyal hesap kaydolma sayfası, parola giriş alanları dışında benzerdir. |
| *api.selfasserted.profileupdate* | **Profili güncelleştirme sayfasını**. Bu sayfa, kullanıcıların kendi profilini güncelleştirmek için kullanabileceği bir form içerir. Bu sayfa, parola giriş alanları dışında sosyal hesap kaydolma sayfası benzerdir. |
| *api.signuporsignin* | **Birleşik kaydolma veya oturum açma sayfası**. Bu sayfa hem kaydolma ve oturum açma Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanan kullanıcıların işler.  |

## <a name="next-steps"></a>Sonraki adımlar

Özelleştirilebilir kullanıcı Arabirimi öğeleri hakkında ek bilgi için bkz: [başvuru kılavuzu için yerleşik ilkeleri için kullanıcı Arabirimi özelleştirme](active-directory-b2c-reference-ui-customization.md).
