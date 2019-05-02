---
title: Öğretici - özelleştirebilir kullanıcı deneyimleri - Azure Active Directory B2C | Microsoft Docs
description: Azure portalını kullanarak uygulamalarınızı Azure Active Directory B2C, kullanıcı arabirimini özelleştirmeyi öğrenin.
services: B2C
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: c2a84bf72ab68937224ac93bd9ffd035e32c603d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702555"
---
# <a name="tutorial-customize-the-interface-of-user-experiences-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanıcı deneyimleri arabirimini özelleştirme

Daha yaygın kullanıcı deneyimleri için gibi kaydolma, oturum açma ve profil düzenleme, kullanabileceğiniz [kullanıcı akışları](active-directory-b2c-reference-policies.md) Azure Active Directory (Azure AD) B2C'de. Bu öğreticide bilgiler edinin yardımcı olur nasıl [kullanıcı arabirimini (UI) özelleştirmek](customize-ui-overview.md) kendi HTML ve CSS dosyaları kullanarak bu deneyimleri.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * UI özelleştirme dosyaları oluşturma
> * Güncelleştirme dosyaları kullanmak için kullanıcı akışı
> * Özelleştirilmiş UI testi

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Kullanıcı akışı Oluştur](tutorial-create-user-flows.md) etkinleştirme kullanıcıların kaydolmak ve uygulamanız için oturum açın.

## <a name="create-customization-files"></a>Özelleştirme dosyaları oluşturma

Bir Azure depolama hesabı ve kapsayıcı oluşturur ve ardından kapsayıcıda temel HTML ve CSS dosyaları yerleştirebilirsiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu öğretici için birçok yolla dosyalarınızı depolayabilirsiniz olsa da, onları depolamak [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure aboneliğinizi içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve aboneliğinizi içeren dizini seçin. Bu dizin, Azure B2C kiracınızın içerdiğinden hesaptan farklıdır.
3. Tüm hizmetleri Azure portalının sol üst köşedeki seçin, arayın ve seçin **depolama hesapları**. 
4. **Add (Ekle)** seçeneğini belirleyin.
5. Altında **kaynak grubu**seçin **Yeni Oluştur**yeni kaynak grubu için bir ad girin ve ardından **Tamam**.
6. Depolama hesabı için bir ad girin. Seçtiğiniz ad Azure’da benzersiz olmalı, uzunluğu 3 ile 24 karakter arasında olmalı ve yalnızca sayı ile küçük harf içermelidir.
7. Depolama hesabının konumu seçin veya varsayılan konumu kabul edin. 
8. Diğer tüm varsayılan değerleri kabul edin, seçin **gözden geçir + Oluştur**ve ardından **Oluştur**.
9. Depolama hesabı oluşturulduktan sonra seçin **kaynağa Git**.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

1. Depolama hesabı genel bakış sayfasında **Blobları**.
2. Seçin **kapsayıcı**, kapsayıcı için bir ad girin, seçin **Blob (yalnızca BLOB'lar için anonim okuma erişimi)** ve ardından **Tamam**.

### <a name="enable-cors"></a>CORS'yi etkinleştirme

 Bir tarayıcıda Azure AD B2C kod modern ve standart bir yaklaşım, bir kullanıcı akışı belirttiğiniz URL'den özel içerik yüklemek için kullanır. Çıkış noktaları arası kaynak paylaşımı (CORS) başka etki alanlarından istenmesi için bir web sayfasında sınırlı kaynaklar sağlar.

1. Menüde **CORS**.
2. İçin **çıkış noktaları**, girin `https://your-tenant-name.b2clogin.com`. Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı. Örneğin, `https://fabrikam.b2clogin.com`. Kiracı adınızın girerken tamamen küçük harf kullanmanız gerekir.
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

## <a name="update-the-user-flow"></a>Kullanıcı akışı güncelleştir

1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
2. Seçin **kullanıcı akışları (ilke)** ve ardından *B2C_1_signupsignin1* kullanıcı akışı.
3. Seçin **sayfa düzenleri**ve ardından altındaki **birleşik kaydolma veya oturum açma sayfası**, tıklayın **Evet** için **özel sayfa içeriği kullan**.
4. İçinde **özel sayfa URI'si**, URI'sini girin *özel ui.html* daha önce kaydettiğiniz dosya.
5. Sayfanın üst kısmında seçin **Kaydet**.

## <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Azure AD B2C kiracınızı seçin **kullanıcı akışları** seçip *B2C_1_signupsignin1* kullanıcı akışı.
2. Sayfanın üst kısmında tıklayın **kullanıcı akışı çalıştırma**.
3. Tıklayın **kullanıcı akışı çalıştırma** düğmesi.

    ![Kaydolma veya oturum açma kullanıcı akışı çalıştırma](./media/tutorial-customize-ui/run-user-flow.png)

    Oluşturduğunuz CSS dosyasını temel alan bir sayfa Orta öğeleriyle aşağıdaki örneğe benzer görmeniz gerekir:

    ![Kullanıcı akışı sonuçları](./media/tutorial-customize-ui/run-now.png) 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * UI özelleştirme dosyaları oluşturma
> * Güncelleştirme dosyaları kullanmak için kullanıcı akışı
> * Özelleştirilmiş UI testi

> [!div class="nextstepaction"]
> [Azure Active Directory B2C'de dil özelleştirme](active-directory-b2c-reference-language-customization.md)
