---
title: "Azure Active Directory B2C: Sayfa UI Özelleştirme Yardımcısı aracı | Microsoft Docs"
description: "Azure Active Directory B2C sayfası kullanıcı Arabirimi özelleştirme özelliğindeki göstermek için kullanılan bir yardımcı aracı"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: ae935d52-3520-4a94-b66e-b35bb40e7514
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: swkrish
ms.openlocfilehash: e0c2d827553567ddbc7d006192dc35574e66f1cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: sayfası kullanıcı arabirimi (UI) özelleştirme özelliğini göstermek için kullanılan yardımcı aracı
Bu makalede bir yardımcı olan [ana UI Özelleştirme makalesi](active-directory-b2c-reference-ui-customization.md) Azure Active Directory (Azure AD) B2C içinde. Aşağıdaki adımları sayfası kullanıcı Arabirimi özelleştirme özelliğini sağladık örnek HTML ve CSS içerik kullanarak çalışma açıklar.

## <a name="get-an-azure-ad-b2c-tenant"></a>Bir Azure AD B2C kiracısı edinme
Herhangi bir şey özelleştirebilmeniz gerekecektir [bir Azure AD B2C kiracısı edinme](active-directory-b2c-get-started.md) zaten yoksa.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Bir kayıt veya oturum açma ilkesi oluşturma
Sunulmuştur örnek içeriği customze iki sayfaları için kullanılabilir bir [kaydolma veya oturum açma ilkesini](active-directory-b2c-reference-policies.md): [birleştirilmiş oturum açma sayfası](active-directory-b2c-reference-ui-customization.md) ve [kendi kendine sürülen öznitelikler sayfasını](active-directory-b2c-reference-ui-customization.md). Zaman [kaydolma veya oturum açma ilkesi oluşturma](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), yerel hesap (e-posta adresi), Facebook, Google ve Microsoft eklemek **kimlik sağlayıcıları**. Bizim örnek HTML içeriğini kabul yalnızca IDPs bunlar.  İsterseniz, bu IDPs kümesini de ekleyebilirsiniz.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme
Etmeniz [bir uygulamayı kaydetme](active-directory-b2c-app-registration.md) B2C kiracınızda ilkeniz yürütmek için kullanılabilir. Uygulamanızı kaydolduktan sonra aslında kaydolma ilkenizde çalıştırmak için kullanabileceğiniz birkaç seçeneğiniz vardır:

* Hızlı Başlangıç uygulamaları "kullanmaya başlayın" bölümünde listelenen Azure AD B2C birini yapı [oturum ayarlama ve tüketicilerinizin uygulamanıza oturum](active-directory-b2c-overview.md#get-started).
* Önceden derlenmiş kullanmak [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) uygulama. Playground kullanmayı seçerseniz, bir uygulama B2C kiracınızın kullanarak kaydetmelisiniz **yeniden yönlendirme URI'si** `https://aadb2cplayground.azurewebsites.net/`.
* Kullanım **Şimdi Çalıştır** ilkenizde düğmesinde [Azure portal](https://portal.azure.com/).

## <a name="customize-your-policy"></a>İlkeniz özelleştirme
İlkeniz görünümünü özelleştirmek için önce belirli kuralları Azure AD B2C kullanarak HTML ve CSS dosyaları oluşturmanız gerekir. Azure AD B2C erişebilmesi için statik içerik genel kullanıma açık bir konuma karşıya yükleyebilirsiniz. Bu, kendi özel web sunucusu, Azure Blob Storage, Azure içerik teslim ağı veya tüm diğer statik kaynak barındırma sağlayıcı olabilir. Yalnızca içeriğiniz HTTPS üzerinden kullanılabilir ve CORS kullanarak erişilebilir gereksinimleridir. Statik içeriğinizi web üzerinde açığa sonra bu konuma gelin ve o içeriği müşterilerinize sunmak için ilkeniz düzenleyebilirsiniz. [Ana UI Özelleştirme makalesi](active-directory-b2c-reference-ui-customization.md) ayrıntılı olarak Azure AD B2C özelleştirme özelliğinin nasıl çalıştığı açıklanmaktadır.

Bu öğreticinin amaçları doğrultusunda zaten bazı örnek içeriği oluşturulan artık ve Azure Blob Depolama alanında barındırılan. Örnek çok basit tema özelleştirmesinde kurgusal şirketimizin, "Wingtip Toys" içeriktir. Kendi ilkesinde denemek için aşağıdaki adımları izleyin:

1. Oturum kiracınız için [Azure portal](https://portal.azure.com/) ve B2C özellikleri dikey penceresine gidin.
2. Tıklatın **oturum açma veya kaydolma ilkeleri** tıkladıktan sonra ilkenizin (örneğin, "b2c\_1\_oturum\_yukarı\_oturum\_içinde").
3. Tıklatın **sayfa UI özelleştirme** ve ardından **Unified kaydolma veya oturum açma sayfası**.
4. İki durumlu **kullanım özel sayfa** geçiş **Evet**. İçinde **özel sayfa URI** alanına, `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. **Tamam** düğmesine tıklayın.
5. Tıklatın **yerel hesap kayıt sayfasına**. İki durumlu **özel şablonu kullan** geçiş **Evet**. İçinde **özel sayfa URI** alanına, `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. İçin aynı adımı tekrarlayın **sosyal hesap kayıt sayfası**.
   Tıklatın **Tamam** iki kez UI Özelleştirme dikey pencereleri kapatın.
7. **Kaydet** düğmesine tıklayın.

Şimdi, özelleştirilmiş ilke deneyebilirsiniz. İstediğiniz, ancak kısaca tıklayabilirsiniz kendi uygulamanız veya Azure AD B2C playground kullanabilirsiniz **Şimdi Çalıştır** ilke dikey penceresinde komutu. Aşağı açılan kutusunda uygulamanızı seçin ve uygun yeniden yönlendirme URI'sini belirtin. Tıklatın **Şimdi Çalıştır** düğmesi. Yeni bir tarayıcı sekmesi açar ve yerinde yeni içerikle, uygulamanız için kaydolan kullanıcı deneyimi aracılığıyla çalıştırabilirsiniz!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Örnek içeriğine Azure Blob depolama alanına karşıya yükle
Sayfa içeriğini barındırmak için Azure Blob Storage kullanmak istiyorsanız, kendi depolama hesabı oluşturma ve dosyalarınızı karşıya yüklemek için sunduğumuz B2C yardımcı aracı kullanın.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklatın **+ yeni** > **veri + depolama** > **depolama hesabı**. Bir Azure Blob Storage hesabı oluşturmak için Azure aboneliği gerekir. Ücretsiz deneme sırasında yukarı imzalayabilirsiniz [Azure Web sitesi](https://azure.microsoft.com/pricing/free-trial/).
3. Sağlayan bir **adı** depolama hesabı (örneğin, "contoso") ve için uygun seçimleri çekme **fiyatlandırma katmanı**, **kaynak grubu** ve **abonelik**. Bilgisayarınızda yüklü olduğundan emin olun **başlangıç panosuna Sabitle** iade seçeneği. **Oluştur**'a tıklayın.
4. Panosuna geri dönün ve yeni oluşturduğunuz depolama hesabı'nı tıklatın.
5. İçinde **Özet** 'yi tıklatın **kapsayıcıları**ve ardından **+ Ekle**.
6. Sağlayan bir **adı** seçin ve kapsayıcı (örneğin, "b2c") için **Blob** olarak **erişim türüne**. **Tamam** düğmesine tıklayın.
7. Listede görünür, oluşturduğunuz kapsayıcı **BLOB'lar** dikey. Kapsayıcı URL yazma; Örneğin, bunu şuna benzemelidir `https://contoso.blob.core.windows.net/b2c`. Kapat **BLOB'lar** dikey.
8. Depolama hesabı dikey penceresinde **anahtarları** ve değerlerini yazma **depolama hesabı adı** ve **birincil erişim anahtarını** alanları.

> [!NOTE]
> **Birincil erişim anahtarını** önemli güvenlik kimlik bilgileri.
> 
> 

### <a name="download-the-helper-tool-and-sample-files"></a>Yardımcı aracı ve örnek dosyalarını indirme
İndirebilirsiniz [.zip dosyası olarak Azure Blob Storage yardımcı aracı ve örnek dosyalar](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) veya Github'dan kopyalayın:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Bu depo içeren bir `sample_templates\wingtip` örnek HTML, CSS ve görüntüleri içeren dizini. Bu şablonlar, kendi Azure Blob Storage hesabı başvurmak HTML dosyaları düzenlemeniz gerekir. Açık `unified.html` ve `selfasserted.html` ve tüm örneklerini Değiştir `https://localhost` önceki adımda yazdığınız kendi kapsayıcı URL'si ile. Bu durumda, HTML etki alanı altında Azure AD tarafından sunulacak çünkü HTML dosyalarının mutlak yolunda kullanmalısınız `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Örnek dosya karşıya yükleme
Aynı Havuzda sıkıştırmasını `B2CAzureStorageClient.zip` çalıştırıp `B2CAzureStorageClient.exe` içinde dosya. Bu program yalnızca depolama hesabınıza belirttiğiniz dizindeki tüm dosyaları karşıya yükleme ve bu dosyaları için CORS erişimi etkinleştirin. Yukarıdaki adımları izlediyseniz, HTML ve CSS dosyaları depolama hesabınıza şimdi işaret. Depolama hesabınızın adını önündeki bir parçası olduğunu unutmayın `blob.core.windows.net`; Örneğin, `contoso`. İçerik doğru bir şekilde erişmek deneyerek yüklendiğini doğrulayabilirsiniz `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` bir tarayıcıdan. Aynı zamanda [http://test-cors.org/](http://test-cors.org/) içeriği artık CORS'yi olduğundan emin olmak için. (Ara "XHR durumu: 200" sonuç.)

### <a name="customize-your-policy-again"></a>İlkeniz, yeniden özelleştirme
Örnek içeriğine kendi depolama hesabınıza yüklediğiniz, ona başvurmak için kaydolma ilkeniz düzenlemeniz gerekir. Yer alan adımları yineleyin ["ilkenizi özelleştirmek"](#customize-your-policy) yukarıdaki kendi depolama hesabının URL'leri kullanarak bu kez bölüm. Örneğin, konumunu, `unified.html` dosya olacaktır `<url-of-your-container>/wingtip/unified.html`.

Kullanabileceğiniz artık **Şimdi Çalıştır** düğmesini veya ilkeniz tekrar yürütmek için kendi uygulamanızı. Sonuç neredeyse tam olarak aynı--, kullanılan görünmelidir aynı örnek HTML ve CSS her iki durumda da. Ancak, ilkelerinizi kendi örneğini Azure Blob Storage şimdi başvuruyor ve dosyaları yeniden olarak Lütfen karşıya yükleyin ve düzenlemek boş. HTML ve CSS özelleştirme hakkında daha fazla bilgi için bkz [ana UI Özelleştirme makalesi](active-directory-b2c-reference-ui-customization.md).

