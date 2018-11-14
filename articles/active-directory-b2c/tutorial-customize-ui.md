---
title: Öğretici - Azure Active Directory B2C uygulamalarınızın, kullanıcı arabirimini özelleştirme | Microsoft Docs
description: Azure portalını kullanarak uygulamalarınızı Azure Active Directory B2C, kullanıcı arabirimini özelleştirmeyi öğrenin.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ee6d7735a2983f642eff82a7dabe036af100e60e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622678"
---
# <a name="tutorial-customize-the-user-interface-of-your-applications-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C uygulamalarınızın, kullanıcı arabirimini özelleştirme

Daha yaygın kullanıcı deneyimleri için gibi kaydolma, oturum açma ve profil düzenleme, kullanabileceğiniz [yerleşik ilkeleri](active-directory-b2c-reference-policies.md) Azure Active Directory (Azure AD) B2C'de. Bu öğreticide bilgiler edinin yardımcı olur nasıl [kullanıcı arabirimini (UI) özelleştirmek](customize-ui-overview.md) kendi HTML ve CSS dosyaları kullanarak bu deneyimleri.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * UI özelleştirme dosyaları oluşturma
> * Dosyaları kullanan bir kaydolma ve oturum açma ilkesi oluşturma
> * Özelleştirilmiş UI testi

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Henüz kendi oluşturmadıysanız [Azure AD B2C Kiracısı](tutorial-create-tenant.md), şimdi oluşturun. Önceki bir öğreticide oluşturduysanız, mevcut bir kiracınız kullanabilirsiniz.

## <a name="create-customization-files"></a>Özelleştirme dosyaları oluşturma

Bir Azure depolama hesabı ve kapsayıcı oluşturur ve ardından kapsayıcıda temel HTML ve CSS dosyaları yerleştirebilirsiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu öğretici için birçok yolla dosyalarınızı depolayabilirsiniz olsa da, onları depolamak [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md).

1. Azure aboneliğinizi içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve aboneliğinizi içeren dizini seçin. Bu dizin, Azure B2C kiracınızın içerdiğinden hesaptan farklıdır.

    ![Abonelik dizinine geçin](./media/tutorial-customize-ui/switch-directories.png)

2. Tüm hizmetleri Azure portalının sol üst köşedeki seçin, arayın ve seçin **depolama hesapları**. 
3. **Add (Ekle)** seçeneğini belirleyin.
4. Altında **kaynak grubu**seçin **Yeni Oluştur**yeni kaynak grubu için bir ad girin ve ardından **Tamam**.
5. Depolama hesabı için bir ad girin. Seçtiğiniz ad Azure’da benzersiz olmalı, uzunluğu 3 ile 24 karakter arasında olmalı ve yalnızca sayı ile küçük harf içermelidir.
6. Depolama hesabının konumu seçin veya varsayılan konumu kabul edin. 
7. Diğer tüm varsayılan değerleri kabul edin, seçin **gözden geçir + Oluştur**ve ardından **Oluştur**.
8. Depolama hesabı oluşturulduktan sonra seçin **kaynağa Git**.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

1. Depolama hesabı genel bakış sayfasında **Blobları**.
2. Seçin **kapsayıcı**, kapsayıcı için bir ad girin, seçin **Blob (yalnızca BLOB'lar için anonim okuma erişimi)** ve ardından **Tamam**.

### <a name="enable-cors"></a>CORS'yi etkinleştirme

 Bir tarayıcıda Azure AD B2C kod modern ve standart bir yaklaşım, ilkede belirttiğiniz URL'den özel içerik yüklemek için kullanır. Çıkış noktaları arası kaynak paylaşımı (CORS) başka etki alanlarından istenmesi için bir web sayfasında sınırlı kaynaklar sağlar.

1. Menüde **CORS**.
2. İçin **çıkış noktaları**, girin `your-tenant-name.b2clogin.com`. Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı. Örneğin, `fabrikam.b2clogin.com`.
3. İçin **izin verilen yöntemleri**, her ikisini de seçin `GET` ve `OPTIONS`.
4. İçin **izin verilen üstbilgileri**, yıldız işareti (*) girin.
5. İçin **kullanıma sunulan üst bilgiler**, yıldız işareti (*) girin.
6. İçin **Maksimum yaş**, 200 girin.

    ![CORS'yi etkinleştirme](./media/tutorial-customize-ui/enable-cors.png)

5. **Kaydet**’e tıklayın.

### <a name="create-the-customization-files"></a>Özelleştirme dosyaları oluşturma

Oturum açma deneyimi, kullanıcı arabirimini özelleştirmek için basit bir HTML ve CSS dosyası oluşturarak başlayın. Dilediğiniz gibi ancak sahip olmalıdır, HTML yapılandırabileceğiniz bir **div** tanımlayıcısına sahip öğe `api`. Örneğin, `<div id="api"></div>`. Azure AD B2C öğeye eklediği `api` sayfası görüntülendiğinde, kapsayıcı.

1. Yerel bir klasörde aşağıdaki dosyayı oluşturun ve, değiştirdiğinizden emin olun `your-storage-account` depolama hesabı adını ve `your-container` oluşturduğunuz kapsayıcı adı. Örneğin, `https://store1.blob.core.windows.net/b2c/style.css`.

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>My B2C Application</title>
        <link rel="stylesheet" href="https://your-storage-account.blob.core.windows.net/your-container/style.css">
      </head>
      <body>  
        <h1>My B2C Application</h1>
        <div id="api"></div>
      </body>
    </html>
    ```

    Sayfa, istediğiniz herhangi bir şekilde tasarlanabilir ancak **API** DIV öğesi, oluşturduğunuz tüm HTML özelleştirme dosyası için gereklidir. 

3. Dosyayı Farklı Kaydet *özel ui.html*.
4. Kaydolma veya oturum açma sayfasında, Azure AD B2C eklediği öğeleri dahil olmak üzere tüm öğeleri merkezi aşağıdaki basit CSS oluşturun.

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

5. Dosyayı Farklı Kaydet *style.css*.

### <a name="upload-the-customization-files"></a>Özelleştirme dosyaları karşıya yükleme

Bu öğreticide, Azure AD B2C bunlara erişebilmesi için depolama hesabında oluşturulan dosyalarını depolar.

1. Seçin **tüm hizmetleri** Azure portalının sol üst köşedeki arayın ve seçin **depolama hesapları**.
2. Oluşturduğunuz depolama hesabını seçin, **Blobları**ve ardından oluşturduğunuz kapsayıcıyı seçin.
3. Seçin **karşıya**, bulun ve seçin *özel ui.html* dosya ve ardından **karşıya**.

    ![Özelleştirme dosyaları karşıya yükleme](./media/tutorial-customize-ui/upload-blob.png)

4. Daha sonra öğreticide kullanmak üzere karşıya yüklediğiniz dosyanın URL'sini kopyalayın.
5. İçin 3 ve 4 adımı yineleyin *style.css* dosya.

## <a name="create-a-sign-up-and-sign-in-policy"></a>Kaydolma ve oturum açma ilkesi oluşturma

Bu öğreticideki adımları tamamlamak için Azure AD B2C'de bir test uygulaması ve kaydolma veya oturum açma ilkesi oluşturmanız gerekir. Profil düzenleme gibi diğer kullanıcı deneyimleri için Bu öğreticide açıklanan ilkeler uygulayabilirsiniz.

### <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C'yi uygulama oluşturma

Azure AD B2C ile iletişim kiracınızda oluşturduğunuz bir uygulama üzerinden gerçekleşir. Aşağıdaki adımlar, döndürülen yetkilendirme belirteci yeniden yönlendiren bir uygulama oluşturur [ https://jwt.ms ](https://jwt.ms).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. İçin **Web uygulaması / Web API'sini**seçin `Yes`yazıp enter `https://jwt.ms` için **yanıt URL'si**.
7. **Oluştur**’a tıklayın.

### <a name="create-the-policy"></a>İlke oluşturma

Özelleştirme dosyaları test etmek için daha önce oluşturduğunuz uygulamayı kullanan yerleşik bir kaydolma veya oturum açma ilkesi oluşturun.

1. Azure AD B2C kiracınızı seçin **oturum açma veya kaydolma ilkeleri'ni**ve ardından **Ekle**.
2. İlke için bir ad girin. Örneğin, *signup_signin*. Önek *B2C_1* adına ilke oluşturulduğunda otomatik olarak eklenir.
3. Seçin **kimlik sağlayıcıları**ayarlayın **kayıt e-posta** için bir yerel hesap ve ardından **Tamam**.
4. Seçin **kaydolma özniteliklerini**, müşteriden kayıt sırasında toplamak istediğiniz öznitelikleri seçin. Örneğin, **ülke/bölge**, **görünen ad**, ve **posta kodu**ve ardından **Tamam**.
5. Seçin **uygulama taleplerini**, başarılı bir kaydolma veya oturum açma deneyiminden sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **görünen ad**, **kimlik sağlayıcısı**, **posta kodu**, **kullanıcı yeni** ve **kullanıcının nesne kimliği** ve ardından **Tamam**.
6. Seçin **sayfa UI özelleştirmesini**seçin **birleşik kaydolma veya oturum açma sayfası**ve ardından **Evet** için **özel sayfa kullan**.
7. İçinde **özel sayfa URI'si**, için URL girin *özel ui.html* daha önce kaydettiğiniz dosya ve ardından **Tamam**.
8. **Oluştur**’a tıklayın.

## <a name="test-the-policy"></a>Test İlkesi

1. Azure AD B2C kiracınızı seçin **oturum açma veya kaydolma ilkeleri'ni**ve ardından oluşturduğunuz ilkeyi seçin. Örneğin, *B2C_1_signup_signin*.
2. Oluşturduğunuz uygulama, seçili olduğundan emin olun **uygulama seçin**ve ardından **Şimdi Çalıştır**.

    ![Kaydolma veya oturum açma ilkesi çalıştırın](./media/tutorial-customize-ui/signup-signin.png)

    Oluşturduğunuz CSS dosyasını temel alan bir sayfa Orta öğeleriyle aşağıdaki örneğe benzer görmeniz gerekir:

    ![İlke Sonuçları](./media/tutorial-customize-ui/run-now.png) 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * UI özelleştirme dosyaları oluşturma
> * Dosyaları kullanan bir kaydolma ve oturum açma ilkesi oluşturma
> * Özelleştirilmiş UI testi

> [!div class="nextstepaction"]
> [Azure Active Directory B2C'de dil özelleştirme](active-directory-b2c-reference-language-customization.md)