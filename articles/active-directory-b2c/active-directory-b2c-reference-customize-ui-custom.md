---
title: Bir kullanıcı yolculuğunun özel ilkeler ile kullanıcı arabirimini özelleştirme | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1cd3fa11df9bd9c87b84985f7acad6ba0a5e8838
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695769"
---
# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>Bir kullanıcı yolculuğunun özel ilkeler ile kullanıcı arabirimini özelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Bu makalede bir kimlik deneyimi çerçevesi kullanarak Azure AD B2C özel ilkeleri, etkinleştirme kullanıcı Arabirimi özelleştirme nasıl çalıştığını ve Gelişmiş açıklamasıdır.


Kesintisiz kullanıcı deneyimi herhangi bir iş tüketici çözüm için bir anahtardır. Kesintisiz kullanıcı deneyimi bir deneyim cihaz veya Tarayıcı hizmeti aracılığıyla kullanıcı yolculuğu kullandıkları Müşteri Hizmetleri farksız olduğu açıktır.

## <a name="understand-the-cors-way-for-ui-customization"></a>UI özelleştirme için CORS yöntemi öğrenin

Azure AD B2C Görünüm ve hisse sunulan ve Azure AD B2C tarafından görüntülenen çeşitli sayfalar üzerinde kullanıcı deneyimi (UX) özelleştirmenize olanak sağlar, özel ilkeleri kullanarak.

Bu amaç için Azure AD B2C kod, tüketicinin tarayıcıda çalışan ve modern ve standart bir yaklaşım kullanan [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) özel içerik işaret etmek için özel bir ilkede belirttiğiniz belirli bir URL'den yüklenecek HTML5/CSS şablonlarınızı. CORS kaynağı kaynaklandığı etki alanı dışındaki başka bir etki alanından istenmesi için bir web sayfasında yazı tipleri gibi sınırlı kaynaklar sağlayan bir mekanizmadır.

Burada şablon sayfalarını ait Burada sağladığınız sınırlı metin ve görüntüleri çözümüyle, eski geleneksel, yöntemiyle karşılaştırıldığında Düzen genel görünüm ve denetimin sınırlı CORS şekilde sorunsuz bir deneyim elde etmek için birden çok zorluklarla baştaki sunulduğu HTML5 ve CSS destekler ve olanak sağlar:

- İçeriği barındıracak ve istemci tarafı komut dosyası kullanarak denetimlerini çözüm eklediği.
- Düzen genel görünüm ve her piksel üzerinde tam denetime sahiptir.

HTML5/CSS dosyaları uygun şekilde hazırlayın tarafından istediğiniz kadar çok içerik sayfaları sağlayabilir.

> [!NOTE]
> Güvenlik nedenleriyle, JavaScript kullanımını özelleştirmek için şu anda engelleniyor. 

Her HTML5/CSS şablonlarınızı sağladığınız bir *bağlantı* gerekli karşılık gelen öğe `<div id=”api”>` HTML veya olarak bundan böyle gösteren içerik sayfası öğe. Azure AD B2C, tüm içerik sayfalarının bu belirli DIV olmasını gerektirir

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Sayfanın geri kalanını denetimine size ait olduğu sürece azure AD B2C ile ilgili içeriklerin bu div eklenmiş olur. Azure AD B2C JavaScript kodu, içeriğinizi çeker ve bu belirli bir div öğesine HTML yerleştirir. Azure AD B2C'yi aşağıdaki denetimleri uygun olarak yerleştirir: hesap Seçici denetimini, denetimleri, çok faktörlü denetimleri (şu anda telefon tabanlı) ve öznitelik koleksiyonu denetimleri oturum açın. Azure AD B2C, tüm denetimler HTML5 uyumlu ve erişilebilir değil, tüm denetimleri tam olarak biçimlendirilebilir ve denetim sürümü değil ilerletmek sağlar.

Birleştirilen içerik sonunda tüketiciye belgeye dinamik olarak görüntülenir.

Her şeyin beklendiği gibi çalıştığından emin olmak için şunları yapmalısınız:

- İçeriğinizi HTML5 uyumlu ve erişilebilir olduğundan emin olun
- İçerik sunucunuz için CORS etkin olduğundan emin olun.
- HTTPS üzerinden içerik sunma.
- Mutlak URL kullanın `https://yourdomain/content` tüm bağlantılar ve CSS içeriği.

> [!TIP]
> Site üzerinde içerik barındırdığını CORS etkin olduğunu doğrulayın ve CORS istekleri sınamak için site kullanabilirsiniz https://test-cors.org/. Bu site sayesinde, CORS istekleri (CORS destekleniyorsa test etmek için), uzak bir sunucuya göndermek veya CORS istekleri (CORS belirli özelliklerini keşfetmek için) test sunucusuna gönderin.

> [!TIP]
> Site https://enable-cors.org/ de faydalı kaynaklara CORS fazla kabul ettiğiniz anlamına gelir.

CORS tabanlı bu yaklaşım sayesinde, son kullanıcıların uygulamanızla Azure AD B2C tarafından sunulan sayfaları arasında tutarlı deneyimlerine sahip.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir önkoşul olarak bir depolama hesabı oluşturmanız gerekir. Bir Azure Blob Depolama hesabının oluşturulacağı bir Azure aboneliği gerekir. Ücretsiz bir deneme sürümü'kurmak oturum [Azure Web sitesi](https://azure.microsoft.com/pricing/free-trial/).

1. Bir tarayıcı oturumu açın ve gidin [Azure portalında](https://portal.azure.com).
2. Yönetici kimlik bilgilerinizle oturum açın.
3. Tıklayın **kaynak Oluştur** > **depolama** > **depolama hesabı**.  A **depolama hesabı oluşturma** bölmesi açılır.
4. İçinde **adı**, örneğin, depolama hesabı için bir ad sağlamanız *contoso369b2c*. Bu değer daha sonra olarak adlandırılır *storageAccountName*.
5. Fiyatlandırma katmanı, kaynak grubu ve abonelik için uygun seçimleri seçin. Sahip olduğunuzdan emin olun **başlangıç panosuna Sabitle** teslim seçeneği. **Oluştur**’a tıklayın.
6. Başlangıç panosuna geri dönün ve oluşturduğunuz depolama hesabına tıklayın.
7. İçinde **Hizmetleri** bölümünde **Blobları**. A **Blob hizmeti bölmesinde** açılır.
8. Tıklayın **+ kapsayıcı**.
9. İçinde **adı**, örneğin, kapsayıcı için bir ad sağlamanız *b2c*. Bu değer, daha sonra olarak adlandırılır *containerName*.
9. Seçin **Blob** olarak **erişim türü**. **Oluştur**’a tıklayın.
10. Oluşturduğunuz kapsayıcı listede görünür **Blob hizmeti bölmesinde**.
11. Kapat **Blobları** bölmesi.
12. Üzerinde **depolama hesabı bölmesi**, tıklayın **anahtar** simgesi. Bir **erişim anahtarları bölmesinde** açılır.  
13. Değerini yazmak **key1**. Bu değer daha sonra olarak adlandırılır *key1*.

## <a name="downloading-the-helper-tool"></a>Yardımcısı aracı yükleme

1.  Yardımcısı aracı indirin [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Kaydet *B2C-AzureBlobStorage-istemci-master.zip* dosyanın yerel makinenizde.
3.  Örneğin altında yerel diskinizde B2C-AzureBlobStorage-istemci-master.zip dosyasının içeriğini ayıklayın **kullanıcı Arabirimi özelleştirme paketi** oluşturan klasöründe bir *AzureBlobStorage B2C için istemci ana*klasörü altında.
4.  Bu klasörü açın ve Arşiv dosyasının içeriğini ayıklayın *B2CAzureStorageClient.zip* içindeki.

## <a name="upload-the-ui-customization-pack-sample-files"></a>UI Özelleştirme paketi örnek dosyalarını karşıya yükleme

1.  Windows Explorer'ı kullanarak, klasöre gidin *B2C AzureBlobStorage istemci ana* altında bulunan *kullanıcı Arabirimi özelleştirme paketi* önceki bölümde oluşturduğunuz klasör.
2.  Çalıştırma *B2CAzureStorageClient.exe* dosya. Bu program, depolama hesabınıza belirtin ve bu dosyalar için CORS erişimini etkinleştirme dizindeki tüm dosyaları karşıya yükler.
3.  İstendiğinde belirtin: bir.  Depolama hesabınızın adını *storageAccountName*, örneğin *contoso369b2c*.
    b.  Azure blob depolamanızda birincil erişim anahtarı *key1*, örneğin *contoso369b2c*.
    c.  Depolama blob depolama kapsayıcısının adını *containerName*, örneğin *b2c*.
    d.  Yolu *başlangıç paketi* örnek dosyaları, örneğin *... \B2CTemplates\wingtiptoys*.

Yukarıdaki adımları izlediyseniz, HTML5 ve CSS dosyaları *kullanıcı Arabirimi özelleştirme paketi* kurgusal şirket için **wingtiptoys** artık depolama hesabınıza işaret ediyor.  İçeriği doğru Azure portalında ilgili kapsayıcı bölmesini açıp yüklendiğini doğrulayabilirsiniz. Alternatif olarak, içeriği doğru bir tarayıcıdan sayfasına erişerek yüklendiğini doğrulayabilirsiniz. Daha fazla bilgi için [Azure Active Directory B2C: Sayfa kullanıcı arabirimi (UI) özelleştirme özelliği göstermek için kullanılan bir yardımcı araç](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>Depolama hesabı CORS etkin olduğundan emin olun

CORS (çıkış noktaları arası kaynak paylaşımı) uç noktanız içerik yüklemek için Azure AD B2C için etkinleştirilmiş olmalıdır. İçeriğinizi Azure AD B2C sayfasından hizmet etki alanı farklı bir etki barındırılan olmasıdır.

Üzerinde içeriğinizin barındırıldığı depolama alanını CORS etkin olduğunu doğrulamak için aşağıdaki adımlarla devam edin:

1. Bir tarayıcı oturumu açın ve sayfasına giderek *unified.html* , depolama hesabındaki konumunun tam URL'yi kullanarak `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Örneğin, https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. https://test-cors.org sayfasına gidin. Bu site, kullanmakta olduğunuz sayfayı CORS etkin olduğunu doğrulamak sağlar.  
   <!--
   ![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
   -->

3. İçinde **uzak URL**unified.html içeriğiniz için tam URL'yi girin ve tıklatın **İsteği Gönder**.
4. Doğrulayın çıktısında **sonuçları** bölüm içeren *XHR durumu: 200*, CORS etkin olduğunu gösterir.
   <!--
   ![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
   -->
   Depolama hesabı artık adlı bir blob kapsayıcısı içermelidir *b2c* aşağıdaki wingtiptoys şablonlardan içeren çizimdeki *başlangıç paketi*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

Aşağıdaki tabloda, HTML5 sayfaları amacını açıklar.

| HTML5 şablonu | Açıklama |
|----------------|-------------|
| *phonefactor.HTML* | Bu sayfa, bir çok faktörlü kimlik doğrulaması sayfası için bir şablon olarak kullanılabilir. |
| *ResetPassword.HTML* | Bu sayfa için bir şablon olarak kullanılabilecek bir parolamı unuttum sayfası. |
| *selfasserted.html* | Bu sayfa sayfa, bir yerel hesap kaydolma sayfası veya bir yerel hesap oturum açma sayfası bir sosyal hesap için bir şablon oturum olarak kullanılabilir. |
| *Unified.HTML* | Bu sayfa, birleşik kaydolma veya oturum açma sayfası için bir şablon olarak kullanılabilir. |
| *updateprofile.html* | Bu sayfa için bir profil güncelleştirme sayfasını şablon olarak kullanılabilir. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>HTML5/CSS şablonlarınızı, kullanıcı yolculuğu için bir bağlantı ekleyin

Özel bir ilke doğrudan düzenleyerek kullanıcı yolculuğunuza HTML5/CSS şablonlarınızı bir bağlantı ekleyebilirsiniz.

Kullanıcı yolculuğunuza kullanılacak özel HTML5/CSS şablonlar bu kullanıcı yolculuklarından kullanılabilir içerik tanımları listesi belirtilmesi gerekir. Bu amaç için isteğe bağlı  *\<ContentDefinitions >* XML öğesi bildirilen, altında  *\<BuildingBlocks >* , özel ilke XML dosyasının bölümünü.

Aşağıdaki tablo, Azure AD B2C kimlik tarafından tanınan tanım kimlikleri altyapısı ve kendisine ilişkili sayfaları türünü deneyimi içerik kümesini açıklar.

| İçerik tanımı kimliği | Açıklama |
|-----------------------|-------------|
| *api.error* | **Hata sayfası**. Bu sayfa, bir özel durum veya hata ile karşılaşıldığında görüntülenir. |
| *api.idpselections* | **Kimlik sağlayıcısı seçim sayfası**. Bu sayfa, oturum açma sırasında kullanıcı seçebileceği kimlik sağlayıcılarının bir listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar (e-posta adresi veya kullanıcı adına göre) sağlayıcılarıdır. |
| *api.idpselections.signup* | **Kimlik sağlayıcısı seçim için kaydolma**. Bu sayfa kullanıcı kayıt sırasında seçebileceği kimlik sağlayıcılarının bir listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar (e-posta adresi veya kullanıcı adına göre) sağlayıcılarıdır. |
| *api.localaccountpasswordreset* | **Parolanızı mı unuttunuz sayfasını**. Bu sayfa, parola sıfırlama başlatmak için doldurmak için kullanıcının sahip olduğu form içerir.  |
| *api.localaccountsignin* | **Yerel hesap oturum açma sayfası**. Bu sayfa, kullanıcının sahip olduğu bir e-posta adresi veya kullanıcı adına göre yerel bir hesap ile oturum açarken doldurmak için bir oturum açma formunu içerir. Form, metin girişi kutusunu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | **Yerel hesap kaydolma sayfası**. Bu sayfa, kullanıcının sahip olduğu bir e-posta adresi veya kullanıcı adına göre yerel bir hesap için kaydolurken doldurmak için bir kayıt formunu içerir. Form, metin girişi kutusunu, parola girişi kutusu, radyo düğmesi, tekli seçim açılır kutuları ve çoklu seçim onay kutuları gibi farklı giriş denetimleri içerebilir. |
| *api.phonefactor* | **Çok faktörlü kimlik doğrulaması sayfası**. Bu sayfada, kullanıcıların kaydolma veya oturum açma sırasında (metin ya da ses kullanarak) kendi telefon numaralarını doğrulayabilirsiniz. |
| *api.selfasserted* | **Sosyal hesap kaydolma sayfası**. Bu sayfa, kullanıcının bir sosyal kimlik sağlayıcısı gibi Facebook ya da Google + var olan bir hesap kullanarak oturum açarken doldurmak için bir kayıt formunu içerir. Bu sayfa, önceki sosyal hesap kaydolma sayfası parola giriş alanları dışında benzerdir. |
| *api.selfasserted.profileupdate* | **Profili güncelleştirme sayfasını**. Bu sayfa kullanıcı profillerini güncelleştirmek için kullanabileceğiniz bir form içerir. Bu sayfa, önceki sosyal hesap kaydolma sayfası parola giriş alanları dışında benzerdir. |
| *api.signuporsignin* | **Birleşik kaydolma veya oturum açma sayfası**.  Bu sayfa, kaydolma hem de işler & Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanan kullanıcılar, oturum açın.

## <a name="next-steps"></a>Sonraki adımlar
[Başvuru: Nasıl özel ilkelerini anlama, B2C kimlik deneyimi çerçevesi ile çalışma](active-directory-b2c-reference-custom-policies-understanding-contents.md)
