---
title: Sayfa UI özelleştirmesi Yardımcısı aracı, Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C sayfa UI özelleştirmesi özelliği göstermek için kullanılan bir yardımcı araç.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 18f921fb718aeb7ae4add2836fbb6ffabd66668f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445067"
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: sayfa kullanıcı arabirimi (UI) özelleştirme özelliği göstermek için kullanılan bir yardımcı aracı
Bu makale için bir yardımcı olan [ana kullanıcı Arabirimi özelleştirme makalesi](active-directory-b2c-reference-ui-customization.md) Azure Active Directory (Azure AD) B2C'de. Aşağıdaki adımlar, bizim sağladığımız örnek HTML ve CSS içeriği kullanarak sayfa UI özelleştirmesi özelliği çalışma açıklanmaktadır.

## <a name="get-an-azure-ad-b2c-tenant"></a>Bir Azure AD B2C kiracısı edinme
Herhangi bir şey özelleştirmeden önce gerekecektir [bir Azure AD B2C kiracısı edinme](active-directory-b2c-get-started.md) zaten yoksa.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi oluşturma
Örnek içerik sunulmuştur vault'un iki sayfaları için kullanılabilir bir [kaydolma veya oturum açma ilkesi](active-directory-b2c-reference-policies.md): [birleştirilmiş oturum açma sayfası](active-directory-b2c-reference-ui-customization.md) ve [otomatik olarak onaylanan öznitelikler sayfasını](active-directory-b2c-reference-ui-customization.md). Zaman [kaydolma veya oturum açma ilkenizin oluşturma](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), yerel bir hesap (e-posta adresi), Facebook, Google ve Microsoft ekleme **kimlik sağlayıcıları**. Bu bizim örnek HTML içeriğini kabul edileceği yalnızca Idp'yi vardır.  İsterseniz bu Idp'yi kümesini de ekleyebilirsiniz.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme
Şunları yapmanız gerekir [bir uygulamayı kaydetme](active-directory-b2c-app-registration.md) B2C kiracınızda ilkenizi yürütmek için kullanılabilir. Uygulamanızı kaydettikten sonra kaydolma ilkenizde çalıştırdığı için kullanabileceğiniz birkaç seçenek vardır:

* Azure AD B2C'in hızlı başlangıç uygulamaları "kullanmaya başlayın" bölümünde listelenen birini yapı [oturum ayarlama ve tüketicilerinizin uygulamanıza oturum açma](active-directory-b2c-overview.md).
* Önceden oluşturulmuş kullanın [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) uygulama. Oyun alanı kullanmayı seçerseniz, bir uygulama kullanarak B2C kiracınıza kaydetmeniz gerekir **yeniden yönlendirme URI'si** `https://aadb2cplayground.azurewebsites.net/`.
* Kullanım **Şimdi Çalıştır** ilkenizde düğmesinde [Azure portalında](https://portal.azure.com/).

## <a name="customize-your-policy"></a>İlkenizi özelleştirme
Görünümünü ilkenizin özelleştirmek için ilk olarak Azure AD B2C'yi belirli kuralları kullanılarak HTML ve CSS dosyaları oluşturmanız gerekir. Azure AD B2C erişebilmesi için statik içerik genel kullanıma açık bir konuma karşıya yükleyebilirsiniz. Bu, kendi özel web sunucusu, Azure Blob Depolama, Azure Content Delivery Network veya diğer tüm statik kaynak barındırma sağlayıcılarına olabilir. Yalnızca içeriğiniz HTTPS üzerinden kullanılabilir ve CORS kullanarak erişilebilir gereksinimleridir. Statik içerik web üzerinde kullanıma sunulan sonra bu konuma gelin ve o içeriği müşterilerinize sunmak için ilkenizi düzenleyebilirsiniz. [Ana kullanıcı Arabirimi özelleştirme makalesi](active-directory-b2c-reference-ui-customization.md) ayrıntılı olarak Azure AD B2C'yi özelleştirme özelliğinin nasıl çalıştığı açıklanmaktadır.

Bu öğreticinin amaçları doğrultusunda, biz zaten bazı örnek içerik oluşturduğunuz ve Azure Blob Depolama alanında barındırılan. Örnek, kurgusal şirketimizin, "Wingtip Toys" çok temel bir özelleştirme temadaki içeriktir. Kendi ilkesinde denemek için aşağıdaki adımları izleyin:

1. Oturum kiracınıza [Azure portalında](https://portal.azure.com/) ve B2C özellikleri dikey penceresine gidin.
2. Tıklayın **oturum açma veya kaydolma ilkeleri'ni**, ilkenizin'a tıklayın ve Düzenle'yi tıklatın (örneğin, "b2c\_1\_oturum\_yukarı\_oturum\_içinde").
3. Tıklayın **sayfa UI özelleştirmesini** ardından **birleşik kaydolma veya oturum açma sayfası**.
4. İki durumlu **özel sayfa kullan** geçin **Evet**. İçinde **özel sayfa URI'si** alanına `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. **Tamam**’a tıklayın.
5. Tıklayın **yerel hesap kaydolma sayfası**. İki durumlu **özel şablon kullan** geçin **Evet**. İçinde **özel sayfa URI'si** alanına `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. İçin aynı adımı tekrarlayın **sosyal hesap kaydolma sayfası**.
   Tıklayın **Tamam** iki kez UI Özelleştirme dikey pencereleri kapatın.
7. **Kaydet**’e tıklayın.

Şimdi, özelleştirilmiş ilke deneyebilirsiniz. İstediğiniz, ancak kısaca tıklayabilirsiniz kendi uygulamanız veya Azure AD B2C playground kullanabilirsiniz **Şimdi Çalıştır** ilke dikey penceresinde komutu. Aşağı açılan kutusunda uygulamanızı seçin ve uygun yeniden yönlendirme URI'Sİ'ı seçin. Tıklayın **Şimdi Çalıştır** düğmesi. Yeni bir tarayıcı sekmesi açar ve yerinde yeni içerikle uygulamanız için kaydolmaya yönelik kullanıcı deneyimi çalıştırabilirsiniz!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Azure Blob depolama alanına örnek içeriği karşıya yükleme
Sayfa içeriğinizi barındırmak için Azure Blob Depolama kullanmak istiyorsanız, kendi depolama hesabı oluşturma ve dosyalarınızı karşıya yüklemek için B2C Yardımcısı aracımızı kullanın.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklayın **+ yeni** > **veri + depolama** > **depolama hesabı**. Bir Azure Blob Depolama hesabının oluşturulacağı bir Azure aboneliği gerekir. Ücretsiz bir deneme sürümü'kurmak oturum [Azure Web sitesi](https://azure.microsoft.com/pricing/free-trial/).
3. Sağlayan bir **adı** depolama hesap (örneğin, "contoso") ve çekme için uygun seçimleri **fiyatlandırma katmanı**, **kaynak grubu** ve  **Abonelik**. Sahip olduğunuzdan emin olun **başlangıç panosuna Sabitle** teslim seçeneği. **Oluştur**’a tıklayın.
4. Başlangıç panosuna geri dönün ve yeni oluşturduğunuz depolama hesabına tıklayın.
5. İçinde **özeti** bölümünde **kapsayıcıları**ve ardından **+ Ekle**.
6. Sağlayan bir **adı** seçin ve kapsayıcı (örneğin, "b2c") için **Blob** olarak **erişim türü**. **Tamam**’a tıklayın.
7. Listede görünür, oluşturduğunuz kapsayıcı **Blobları** dikey penceresi. Kapsayıcı URL'si yazabilirsiniz. Örneğin, benzer şekilde görünmelidir `https://contoso.blob.core.windows.net/b2c`. Kapat **Blobları** dikey penceresi.
8. Depolama hesabı dikey penceresinde tıklayın **anahtarları** ve değerlerini yazma **depolama hesabı adı** ve **birincil erişim anahtarı** alanları.

> [!NOTE]
> **Birincil erişim anahtarı** bir önemli güvenlik kimlik bilgisidir.
> 
> 

### <a name="download-the-helper-tool-and-sample-files"></a>Yardımcısı aracı ve örnek dosyalarını indirme
İndirebileceğiniz [.zip dosyası olarak Azure Blob Depolama Yardımcısı aracı ve örnek dosyaları](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) veya Github'dan kopyalayabilirsiniz:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Bu depo içeren bir `sample_templates\wingtip` örnek HTML, CSS ve resim içeren dizin. Kendi Azure Blob Depolama hesabınızı başvurmak bu şablonları için HTML dosyalarını düzenlemek gerekir. Açık `unified.html` ve `selfasserted.html` ve tüm örneklerini `https://localhost` önceki adımda yazdığınız kendi kapsayıcınızı URL'si ile. Bu durumda, HTML etki alanındaki Azure AD tarafından hizmet verilecek çünkü HTML dosyaları mutlak yolu kullanmalısınız `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Örnek dosyalarını karşıya yükleme
Aynı depoda sıkıştırmasını `B2CAzureStorageClient.zip` çalıştırıp `B2CAzureStorageClient.exe` içinde dosya. Bu program yalnızca depolama hesabınıza belirttiğiniz dizindeki tüm dosyaları karşıya yükleme ve bu dosyaları CORS erişimini etkinleştirin. Yukarıdaki adımları izlediyseniz, HTML ve CSS dosyaları artık depolama hesabınıza işaret edecek. Depolama hesabınızın adını önünde bir parçası olduğunu unutmayın `blob.core.windows.net`; Örneğin, `contoso`. İçeriği doğru erişmeye çalışarak yüklendiğini doğrulayabilirsiniz `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` bir tarayıcıdan. Ayrıca [ http://test-cors.org/ ](http://test-cors.org/) içeriği artık CORS'un etkin olduğundan emin olmak için. (Ara "XHR durumu: 200" Sonuçta.)

### <a name="customize-your-policy-again"></a>İlkenizi yeniden özelleştirme
Örnek içerik kendi depolama hesabınıza yüklediğiniz, buna başvuruda bulunma kaydolma ilkenizde düzenlemeniz gerekir. Bölümündeki adımları tekrar ["ilkenizin özelleştirme"](#customize-your-policy) yukarıdaki kendi depolama hesabının URL'leri kullanarak şu bölüm. Örneğin, konumunu, `unified.html` dosya olacak `<url-of-your-container>/wingtip/unified.html`.

Artık **Şimdi Çalıştır** düğme veya kendi uygulamanızı ilkenizi tekrar yürütün. Sonuç neredeyse aynı--kullanmış görünmelidir aynı örnek HTML ve CSS her iki durumda da. Ancak, ilkelerinizi kendi örneğini Azure Blob Depolama şimdi başvuruyor ve düzenleyin ve dosyaları yeniden olarak Lütfen karşıya yüklemek ücretsizdir. HTML ve CSS özelleştirme hakkında daha fazla bilgi için [ana kullanıcı Arabirimi özelleştirme makalesi](active-directory-b2c-reference-ui-customization.md).

