---
title: "Özel ilkeler - Azure AD B2C kullanarak bir UI Özelleştirme | Microsoft Docs"
description: "Azure AD B2C'de özel ilkeler kullanırken bir kullanıcı arabirimi (UI) özelleştirme hakkında bilgi edinin."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: dcd8b6df68a68f5feb428b4fd98aee938b3bfe6c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: Kullanıcı Arabirimi özelleştirme özel bir ilke yapılandırın.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede tamamladıktan sonra kaydolma ve oturum açma özel bir ilke ile marka ve görünümüne sahip olur. Azure Active Directory B2C ile kullanıcılara sunulan HTML ve CSS içeriğin neredeyse tam denetim elde (Azure AD B2C). Özel bir ilke kullandığınızda, kullanıcı arabirimini özelleştirme XML'de Azure portalında denetimleri kullanmak yerine yapılandırın. 

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md). Çalışma özel bir ilke için kaydolma ve oturum açma yerel hesaplara sahip olmalıdır.

## <a name="page-ui-customization"></a>Sayfa UI Özelleştirmesi

Sayfanın UI Özelleştirme özelliğini kullanarak, herhangi bir özel ilke görünümünü özelleştirebilirsiniz. Ayrıca, marka ve uygulama ve Azure AD B2C arasında visual tutarlılık koruyabilirsiniz.

İşte nasıl çalışır?: Azure AD B2C, müşterinizin tarayıcıda kodu çalıştırır ve adlı modern bir yaklaşım kullanır [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/). İlk olarak, özelleştirilmiş HTML içerikle özel ilkesindeki bir URL belirtin. Azure AD B2C, URL'den yüklenir ve ardından sayfanın müşteriye görüntüleyen HTML içeriğe sahip kullanıcı Arabirimi öğeleri birleştirir.

## <a name="create-your-html5-content"></a>HTML5 içerik oluşturma

HTML başlığında ürününüzün marka adıyla içerik oluşturun.

1. Aşağıdaki HTML kod parçası kopyalayın. Doğru biçimlendirilmiş boş bir öğesiyle HTML5 adlı  *\<div ID "API" =\>\</div\>*  içinde bulunan  *\<gövde\>*  etiketler. Bu öğe, Azure AD B2C içerik eklenecek nerede gösterir.

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

   >[!NOTE]
   >Güvenlik nedenleriyle, JavaScript kullanımı için özelleştirme şu anda engelleniyor.

2. Bir metin düzenleyicisinde kopyalanan parçacığını yapıştırın ve dosyayı farklı Kaydet *özelleştirme ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob storage hesabı oluşturma

>[!NOTE]
> Bu makalede, Azure Blob Depolama bizim içeriğini barındırmak için kullanırız. İçeriğinizi bir web sunucusunda barındırmak seçebilirsiniz, ancak gerekir [web sunucunuz üzerinde CORS'yi etkinleştirmeniz](https://enable-cors.org/server.html).

Blob depolama alanındaki bu HTML içeriğini barındırmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üzerinde **Hub** menüsünde, select **yeni** > **depolama** > **depolama hesabı**.
3. Benzersiz bir girin **adı** depolama hesabınız için.
4. **Dağıtım modeli** kalabileceği **Resource Manager**.
5. Değişiklik **hesap türü** için **Blob storage**.
6. **Performans** kalabileceği **standart**.
7. **Çoğaltma** kalabileceği **RA-GRS**.
8. **Erişim katmanı** kalabileceği **etkin**.
9. **Depolama hizmeti şifrelemesi** kalabileceği **devre dışı**.
10. Seçin bir **abonelik** depolama hesabınız için.
11. Oluşturma bir **kaynak grubu** veya varolan bir tanesini seçin.
12. Seçin **coğrafi konum** depolama hesabınız için.
13. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.  
    Dağıtım tamamlandıktan sonra **depolama hesabı** dikey penceresinde otomatik olarak açılır.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Blob depolama alanına genel bir kapsayıcı oluşturmak için aşağıdakileri yapın:

1. Tıklatın **genel bakış** sekmesi.
2. Tıklatın **kapsayıcı**.
3. İçin **adı**, türü **$root**.
4. Ayarlama **erişim türüne** için **Blob**.
5. Tıklatın **$root** yeni kapsayıcı açın.
6. **Karşıya Yükle**'ye tıklayın.
7. Klasör simgesine tıklayın **bir dosya seçin**.
8. Git **özelleştirme ui.html**, daha önce içinde oluşturulan [sayfa UI özelleştirme](#the-page-ui-customization-feature) bölüm.
9. **Karşıya Yükle**'ye tıklayın.
10. Karşıya yüklediğiniz Özelleştir ui.html blob seçin.
11. Yanına **URL**, tıklatın **kopya**.
12. Bir tarayıcıda, kopyalanan URL'sini yapıştırın ve sitesine gidin. Site erişilemez, emin olun kapsayıcı erişim türü ayarlanırsa **blob**.

## <a name="configure-cors"></a>CORS'yi yapılandırın

BLOB Depolama çıkış noktaları arası kaynak paylaşımı için aşağıdakileri yaparak yapılandırın:

>[!NOTE]
>Bizim örnek HTML ve CSS içeriğini kullanarak kullanıcı Arabirimi özelleştirme özelliği denemek mi istiyorsunuz? Sağladık [basit Yardımcısı aracı](active-directory-b2c-reference-ui-customization-helper-tool.md) karşıya yükleyen ve Blob Depolama hesabınıza bizim örnek içeriği yapılandırır. Aracı kullanırsanız, İleri için atlayabilirsiniz [oturum açma veya kaydolma özel ilkenizi değiştirmek](#modify-your-sign-up-or-sign-in-custom-policy).

1. Üzerinde **depolama** dikey altında **ayarları**, açık **CORS**.
2. **Ekle**'ye tıklayın.
3. İçin **çıkış izin**, bir yıldız işareti girin (\*).
4. İçinde **fiillere izin** açılan listesinden, select **almak** ve **seçenekleri**.
5. İçin **üstbilgileri izin**, bir yıldız işareti girin (\*).
6. İçin **sunulan üstbilgileri**, bir yıldız işareti girin (\*).
7. İçin **en uzun geçerlilik süresi (saniye)**, türü **200**.
8. **Ekle**'ye tıklayın.

## <a name="test-cors"></a>Test CORS

Aşağıdakileri yaparak hazırsınız olduğunu doğrulama:

1. Git [www.test-cors.org](http://www.test-cors.org/) Web sitesine gidin ve URL yapıştırın **uzak URL** kutusu.
2. Tıklatın **gönderme isteği**.  
    Bir hata alırsanız, emin olun, [CORS ayarları](#configure-cors) doğrudur. Tarayıcı önbelleğini temizlemek veya Ctrl + Shift + P tuşuna basarak özel gözatma oturumu açmak gerekebilir.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Oturum açma veya kaydolma özel ilkesini değiştirme

Üst düzey altında  *\<TrustFrameworkPolicy\>*  etiketi, bulmanız gerekir  *\<BuildingBlocks\>*  etiketi. İçinde  *\<BuildingBlocks\>*  etiketler eklemek bir  *\<ContentDefinitions\>*  aşağıdaki örnekte kopyalayarak etiketi. Değiştir *your_storage_account* depolama hesabınızın adı.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Güncelleştirilmiş özel ilkeniz karşıya yükle

1. İçinde [Azure portal](https://portal.azure.com), [geçiş Azure AD B2C kiracınızın bağlamına](active-directory-b2c-navigate-to-b2c-context.md)ve ardından açın **Azure AD B2C** dikey.
2. Tıklatın **tüm ilkeleri**.
3. Tıklatın **karşıya İlkesi**.
4. Karşıya yükleme `SignUpOrSignin.xml` ile  *\<ContentDefinitions\>*  daha önce eklediğiniz etiketi.

## <a name="test-the-custom-policy-by-using-run-now"></a>Özel ilke kullanarak test **Şimdi Çalıştır**

1. Üzerinde **Azure AD B2C** dikey penceresinde, Git **tüm ilkeler**.
2. Karşıya yüklenen ve'ı tıklatın özel ilkeyi seçin **Şimdi Çalıştır** düğmesi.
3. Bir e-posta adresi kullanarak kaydolabilirsiniz olması gerekir.

## <a name="reference"></a>Başvuru

Buraya kullanıcı arabirimini özelleştirme örnek şablonları bulabilirsiniz:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Aşağıdaki HTML dosyaları sample_templates/wingtip klasör içerir:

| HTML5 şablonu | Açıklama |
|----------------|-------------|
| *phonefactor.html* | Bu dosyayı bir çok faktörlü kimlik doğrulaması sayfası için bir şablon olarak kullanın. |
| *resetpassword.html* | Bu dosya için bir şablon olarak kullanmak bir parolanızı mı unuttunuz sayfasını. |
| *selfasserted.html* | Bu dosya, sosyal hesap kayıt sayfası, yerel hesap kaydolma sayfası veya bir yerel hesap oturum açma sayfası için bir şablon olarak kullanın. |
| *unified.html* | Bu dosyayı bir birleşik kayıt veya oturum açma sayfası için bir şablon olarak kullanın. |
| *updateprofile.html* | Bu dosya için bir profil güncelleştirme sayfası şablon olarak kullanın. |

İçinde [, oturum açma veya kaydolma özel ilke bölümünü değiştirme](#modify-your-sign-up-or-sign-in-custom-policy), içerik tanımı yapılandırılmış `api.idpselections`. Tam bir içerik kümesinin Azure AD B2C kimlik deneyimi framework ve açıklamalarının tarafından tanınan tanımı kimlikleri aşağıdaki tabloda verilmiştir:

| İçerik tanımı kimliği | Açıklama | 
|-----------------------|-------------|
| *api.error* | **Hata sayfası**. Bir özel durum ya da hatayla karşılaşıldığında bu sayfada görüntülenir. |
| *api.idpselections* | **Kimlik sağlayıcısı seçimi sayfası**. Bu sayfa, oturum açma sırasında seçebileceği kimlik sağlayıcıları listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar seçeneklerdir. |
| *api.idpselections.signup* | **Kimlik sağlayıcısı seçimi için kaydolma**. Bu sayfa kullanıcı kayıt sırasında seçebileceği kimlik sağlayıcıları listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar seçeneklerdir. |
| *api.localaccountpasswordreset* | **Parolanızı mı unuttunuz sayfasını**. Bu sayfa kullanıcı parola sıfırlama başlatmak için tamamlamanız gereken bir form içerir.  |
| *api.localaccountsignin* | **Yerel hesap oturum açma sayfası**. Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre yerel bir hesap ile oturum imzalamak için bir oturum açma formu içerir. Form, metin giriş kutusu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | **Yerel hesap kayıt sayfasına**. Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre yerel bir hesap için kaydolduğunuz için bir kayıt formu içerir. Form, metin giriş kutusu, bir parola giriş kutusu, bir radyo düğmesi, tek seçimlik açılan kutuları ve çoklu seçim onay kutuları gibi çeşitli giriş denetimlerini içerebilir. |
| *api.phonefactor* | **Çok faktörlü kimlik doğrulaması sayfası**. Bu sayfada, kullanıcıların telefon numaralarına (metin veya sesli kullanarak) kaydolma veya oturum açma sırasında doğrulayabilirsiniz. |
| *api.selfasserted* | **Sosyal hesap kayıt sayfası**. Bu sayfa, bunlar Facebook veya Google + gibi sosyal kimlik sağlayıcısından var olan bir hesabı kullanarak kaydolduğunuzda, kullanıcılar tamamlamalısınız bir kayıt formu içerir. Bu sayfa, önceki sosyal hesap kayıt sayfası, parola girdi alanlarının dışında benzerdir. |
| *api.selfasserted.profileupdate* | **Profil güncelleştirme sayfası**. Bu sayfa, kullanıcıların kendi profilini güncelleştirmek için kullanabileceği bir form içerir. Bu sayfa, sosyal hesap kaydolma sayfası, parola girdi alanlarının dışında benzerdir. |
| *api.signuporsignin* | **Birleşik kayıt veya oturum açma sayfası**. Bu sayfa, hem kaydolma ve oturum açma Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanan kullanıcıların işler.  |

## <a name="next-steps"></a>Sonraki adımlar

Özelleştirilebilir kullanıcı Arabirimi öğeleri hakkında ek bilgi için bkz: [yerleşik ilkeleri için kullanıcı Arabirimi özelleştirme için başvuru kılavuzu](active-directory-b2c-reference-ui-customization.md).
