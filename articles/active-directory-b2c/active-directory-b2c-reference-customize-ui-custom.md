---
title: Bir kullanıcı gezisine özel ilkeler ile UI Özelleştirme | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/25/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0980c79ccd9ebd170e747514bba712c498e1387c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711918"
---
# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>Bir kullanıcı gezisine özel ilkeler ile UI Özelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Bu makalede kullanıcı arabirimini özelleştirme nasıl çalıştığını ve kimlik deneyimi Framework kullanarak Azure AD B2C özel ilkeler ile etkinleştirme Gelişmiş bir tanımıdır.


Kesintisiz kullanıcı deneyimi iş tüketici çözüme yönelik bir anahtardır. Kesintisiz kullanıcı deneyimi aygıt veya tarayıcı, bir kullanıcının gezisine hizmeti aracılığıyla kullandıkları Müşteri Hizmetleri ayırt olduğu bir deneyim açıktır.

## <a name="understand-the-cors-way-for-ui-customization"></a>UI özelleştirme için CORS biçimini anlama

Azure AD B2C Görünüm-ve-yapısını kullanıcı deneyimini (UX), hizmet ve Azure AD B2C tarafından görüntülenen çeşitli sayfalar özelleştirmenize olanak tanır, özel ilkelerini kullanma.

Bunun için Azure AD B2C, tüketicinin tarayıcıda kodu çalıştırır ve modern ve standart bir yaklaşım kullanır [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/) HTML5/CSS şablonlarınızı işaret etmek için özel bir ilke belirttiğiniz belirli bir URL'den özel içerik yüklemek için. CORS kaynak kaynaklandığı etki alanı dışındaki başka bir etki alanından istenmesi için bir web sayfasında yazı tipleri gibi sınırlı kaynakları sağlayan bir mekanizmadır.

Burada burada sınırlı metin sağlanan ve görüntüleri, Düzen ve hissi sınırlı Denetim burada sunulan bir sorunsuz elde etmek için birden fazla ilgili sorunlar baştaki ve deneyimi, CORS biçimini destekler HTML5 ve CSS izin çözüm tarafından şablon sayfalarını ait eski geleneksel şekilde Karşılaştırılan:

- İçeriği barındıracak ve istemci tarafı komut dosyası kullanarak kendi denetimleri çözümü yerleştirir.
- Her piksel düzeni ve hissi üzerinde tam denetime sahiptir.

HTML5/CSS dosyaları uygun şekilde hazırlayın tarafından istediğiniz kadar çok içerik sayfaları sağlayabilir.

> [!NOTE]
> Güvenlik nedenleriyle, JavaScript kullanımı için özelleştirme şu anda engelleniyor. JavaScript engelini kaldırmak için bir özel etki alanı adı, Azure AD B2C kiracınızın kullanımını gereklidir.

Her HTML5/CSS şablonlarınızı sağladığınız bir *bağlantı* gerekli karşılık gelen öğe `<div id=”api”>` HTML veya olarak bundan böyle göstermeye içerik sayfasındaki öğesi. Azure AD B2C tüm içerik sayfalarının bu belirli DIV sahip olmasını gerektirir

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

Sayfaya geri kalanı, size denetimine olsa da azure AD B2C ile ilgili içerik sayfası için bu div eklenen. Azure AD B2C JavaScript kodu, içeriği çeker ve HTML bu belirli div öğesinin yerleştirir. Azure AD B2C aşağıdaki denetimleri uygun olarak yerleştirir: hesabı Seçici denetimini, denetimleri, çok faktörlü (şu anda telefon tabanlı) denetimleri ve öznitelik koleksiyonu denetimleri oturum. Azure AD B2C, tüm denetimler HTML5 uyumlu ve erişilebilir olduğundan, tüm denetimler tam olarak biçimlendirilebilir ve, denetimi sürüm değil ilerletmek sağlar.

Birleştirilen içeriğin sonunda, tüketici dinamik belgeye olarak görüntülenir.

Her şeyin beklendiği gibi çalıştığından emin olmak için yapmanız gerekir:

- İçeriğinizi HTML5 uyumlu ve erişilebilir olduğundan emin olun
- İçeriğinize CORS için etkinleştirildiğinden emin olun.
- HTTPS üzerinden içerik sunmak.
- Mutlak URL'ler gibi kullandığınız https://yourdomain/content tüm bağlantılar ve CSS içeriği.

> [!TIP]
> Barındırma içeriğinizi üzerinde site CORS'yi sahip olduğunu doğrulayın ve CORS isteklerini test etmek için siteyi kullanabilirsiniz http://test-cors.org/. Bu site sayesinde, (CORS destekleniyorsa sınamak için) bir uzak sunucuya CORS isteği gönder veya (CORS belirli özelliklerini keşfetmek için) bir test sunucusuna CORS isteği gönder.

> [!TIP]
> Site http://enable-cors.org/ de CORS yararlı kaynaklara fazla oluşturduğunu.

CORS tabanlı bu yaklaşım sayesinde, son kullanıcılar, uygulamanızı ve Azure AD B2C tarafından sunulan sayfaları arasında tutarlı deneyimleri sahiptir.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir önkoşul olarak bir depolama hesabı oluşturmanız gerekir. Bir Azure Blob Storage hesabı oluşturmak için bir Azure aboneliği gerekir. Ücretsiz deneme sırasında yukarı imzalayabilirsiniz [Azure Web sitesi](https://azure.microsoft.com/pricing/free-trial/).

1. Bir tarayıcı oturumu açın ve gidin [Azure portal](https://portal.azure.com).
2. Yönetici kimlik bilgilerinizle oturum.
3. Tıklatın **kaynak oluşturma** > **depolama** > **depolama hesabı**.  A **depolama hesabı oluşturma** bölmesi açılır.
4. İçinde **adı**, örneğin, depolama hesabı için bir ad sağlayın *contoso369b2c*. Bu değer daha sonra olarak adlandırılır *storageAccountName*.
5. Fiyatlandırma katmanı, kaynak grubu ve abonelik için uygun seçimleri seçin. Bilgisayarınızda yüklü olduğundan emin olun **başlangıç panosuna Sabitle** iade seçeneği. **Oluştur**’a tıklayın.
6. Panosuna geri dönün ve sizin oluşturduğunuz depolama hesabı'nı tıklatın.
7. İçinde **Hizmetleri** 'yi tıklatın **BLOB'lar**. A **Blob hizmeti bölmesi** açılır.
8. Tıklatın **+ kapsayıcı**.
9. İçinde **adı**, örneğin, kapsayıcı için bir ad sağlayın *b2c*. Bu değer, daha sonra olarak adlandırılır *containerName*.
9. Seçin **Blob** olarak **erişim türüne**. **Oluştur**’a tıklayın.
10. Listede görünür, oluşturduğunuz kapsayıcı **Blob hizmeti bölmesi**.
11. Kapat **BLOB'lar** bölmesi.
12. Üzerinde **depolama hesabı bölmesi**, tıklatın **anahtar** simgesi. Bir **erişim anahtarları bölmesinde** açılır.  
13. Değerini yazmak **key1**. Bu değer daha sonra olarak adlandırılır *key1*.

## <a name="downloading-the-helper-tool"></a>Yardımcısı aracı yükleniyor

1.  Yardımcı aracından karşıdan [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Kaydet *B2C-AzureBlobStorage-istemci-master.zip* yerel makinenizde dosya.
3.  Yerel diskinizde B2C-AzureBlobStorage-istemci-master.zip dosyasının içeriği altında örneğin ayıklayın **UI Özelleştirme paketi** oluşturur klasör bir *AzureBlobStorage B2C için istemci ana*klasörü altında.
4.  Bu klasörü açın ve arşiv dosyasını, içeriği ayıklamak *B2CAzureStorageClient.zip* içindeki.

## <a name="upload-the-ui-customization-pack-sample-files"></a>UI Özelleştirme paketi örnek dosyaları karşıya yükleme

1.  Windows Gezgini'ni kullanarak, klasöre gidin *AzureBlobStorage istemci yönetici B2C* altında bulunan *UI Özelleştirme paketi* önceki bölümde oluşturduğunuz klasör.
2.  Çalıştırma *B2CAzureStorageClient.exe* dosya. Bu program, depolama hesabınıza belirtin ve bu dosyaları için CORS erişimini etkinleştirmek dizindeki tüm dosyaları karşıya yükleme.
3.  İstendiğinde, belirtin: bir.  Depolama hesabınızın adını *storageAccountName*, örneğin *contoso369b2c*.
    b.  Azure blob depolama alanınızın, birincil erişim anahtarını *key1*, örneğin *contoso369b2c*.
    c.  Depolama blob depolama kapsayıcısının adı *containerName*, örneğin *b2c*.
    d.  Yolunu *başlangıç paketi* örnek dosyaları, örneğin *... \B2CTemplates\wingtiptoys*.

Yukarıdaki adımları izlediyseniz, HTML5 ve CSS dosyaları *UI Özelleştirme paketi* kurgusal şirket için **wingtiptoys** artık depolama hesabınıza işaret eden.  İçerik doğru Azure portalında ilgili kapsayıcı bölmesini açarak yüklendiğini doğrulayabilirsiniz. Alternatif olarak, içeriği düzgün bir tarayıcıdan sayfa erişerek yüklendiğini doğrulayabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory B2C: sayfası kullanıcı arabirimi (UI) özelleştirme özelliğini göstermek için kullanılan bir yardımcı aracı](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>CORS'yi depolama hesabına sahip olduğundan emin olun

CORS (çıkış noktaları arası kaynak paylaşımı), uç noktası, içeriği yüklemek Azure AD B2C için etkinleştirilmiş olmalıdır. İçeriğinizi Azure AD B2C sayfasından hizmet veren etki alanı farklı bir etki barındırılan olmasıdır.

Üzerinde içeriği barındıran depolama CORS'yi sahip olduğunu doğrulamak için aşağıdaki adımlarla devam edin:

1. Bir tarayıcı oturumu açın ve sayfaya gitmek *unified.html* depolama hesabınızdaki konumunun tam URL'yi kullanarak `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Örneğin, https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. http://test-cors.org sayfasına gidin. Bu site, kullanmakta olduğunuz sayfa CORS'yi sahip olduğunu doğrulamak sağlar.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. İçinde **uzak URL**unified.html içeriğiniz için tam URL'yi girin ve tıklatın **İsteği Gönder**.
4. Doğrulayın çıktıda **sonuçları** bölüm içerir *XHR durumu: 200*, CORS etkin olduğunu gösterir.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
Depolama hesabı artık adlı bir blob kapsayıcı içermelidir *b2c* aşağıdaki wingtiptoys şablonlardan içeren çizimdeki *başlangıç paketi*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

Aşağıdaki tabloda yukarıdaki HTML5 sayfaları amacını açıklar.

| HTML5 şablonu | Açıklama |
|----------------|-------------|
| *phonefactor.HTML* | Bu sayfa bir çok faktörlü kimlik doğrulaması sayfası için bir şablon olarak kullanılabilir. |
| *ResetPassword.HTML* | Bu sayfa için bir şablon olarak kullanılan bir parolanızı mı unuttunuz sayfasını. |
| *selfasserted.html* | Bu sayfa, bir sosyal hesabı için bir şablon oturum gibi bir yerel hesap kayıt sayfasını veya bir yerel hesap oturum açma sayfasını kullanılabilir. |
| *Unified.HTML* | Bu sayfa, bir birleşik kaydolun veya oturum açma sayfası için bir şablon olarak kullanılabilir. |
| *updateprofile.html* | Bu sayfa için bir profil güncelleştirme sayfası şablon olarak kullanılabilir. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>HTML5/CSS şablonlarınızı kullanıcı Yolculuğunuzun için bir bağlantı ekleme

Özel bir ilke doğrudan düzenleyerek kullanıcı Yolculuğunuzun HTML5/CSS şablonlarınızı bir bağlantı ekleyebilirsiniz.

Kullanıcı Yolculuğunuzun kullanmak için özel HTML5/CSS şablonlar bu kullanıcı Yolculuklar kullanılabilir içerik tanımları listesinde belirtilmesi gerekir. Bunun için isteğe bağlı bir *<ContentDefinitions>* XML öğesi bildirildi, altında *<BuildingBlocks>* özel ilke XML dosyasını bölümü.

Aşağıdaki tabloda Azure AD B2C kimliği tarafından tanınan tanımı kimlikleri altyapısı ve bunlara ilişkili sayfaları türünü deneyimi içeriği açıklar.

| İçerik tanımı kimliği | Açıklama |
|-----------------------|-------------|
| *api.error* | **Hata sayfası**. Bir özel durum ya da hatayla karşılaşıldığında bu sayfada görüntülenir. |
| *api.idpselections* | **Kimlik sağlayıcısı seçimi sayfası**. Bu sayfa, oturum açma sırasında seçebileceği kimlik sağlayıcıları listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar (e-posta adresi veya kullanıcı adına göre) sağlayıcılarıdır. |
| *api.idpselections.signup* | **Kimlik sağlayıcısı seçimi için kaydolma**. Bu sayfa kullanıcı kayıt sırasında seçebileceği kimlik sağlayıcıları listesini içerir. Bu, Kurumsal kimlik sağlayıcıları, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar (e-posta adresi veya kullanıcı adına göre) sağlayıcılarıdır. |
| *api.localaccountpasswordreset* | **Parolanızı mı unuttunuz sayfasını**. Bu sayfa, parola sıfırlama başlatmak için doldurmak için kullanıcının sahip olduğu bir form içerir.  |
| *api.localaccountsignin* | **Yerel hesap oturum açma sayfası**. Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre yerel bir hesap ile oturum açarken doldurmak için kullanıcının sahip bir oturum açma formu içerir. Form, metin giriş kutusu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | **Yerel hesap kayıt sayfasına**. Bu sayfa bir e-posta adresi veya bir kullanıcı adı göre yerel bir hesap için kaydolmak doldurmak için kullanıcının sahip bir kayıt formu içerir. Form, metin giriş kutusuna, parola giriş kutusu, radyo düğmesi, tek seçimlik açılan kutuları ve çoklu seçim onay kutuları gibi farklı giriş denetimlerini içerebilir. |
| *api.phonefactor* | **Çok faktörlü kimlik doğrulaması sayfası**. Bu sayfada, kullanıcıların telefon numaralarına (metin veya sesli kullanarak) kaydolma veya oturum açma sırasında doğrulayabilirsiniz. |
| *api.selfasserted* | **Sosyal hesap kayıt sayfası**. Bu sayfa bir sosyal kimlik sağlayıcısından Facebook veya Google + gibi varolan bir hesabı kullanarak oturum açarken doldurmak için kullanıcının sahip bir kayıt formu içerir. Bu sayfa, önceki sosyal hesabı kayıt sayfasına parola giriş alanları dışında benzerdir. |
| *api.selfasserted.profileupdate* | **Profil güncelleştirme sayfası**. Bu sayfa kullanıcının kendi profilini güncelleştirmek için kullanabileceği bir form içerir. Bu sayfa, önceki sosyal hesabı kayıt sayfasına parola giriş alanları dışında benzerdir. |
| *api.signuporsignin* | **Birleşik kayıt veya oturum açma sayfası**.  Bu sayfayı kaydolma hem de işler ve Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları kullanan kullanıcılar, oturum açma.

## <a name="next-steps"></a>Sonraki adımlar
[Başvuru: nasıl özel ilkeler anlamak B2C kimlik deneyimi Framework ile çalışma](active-directory-b2c-reference-custom-policies-understanding-contents.md)
