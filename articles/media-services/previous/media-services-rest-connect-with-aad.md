---
title: REST ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması | Microsoft Docs
description: REST kullanarak Azure Active Directory kimlik doğrulaması ile Azure Media Services API'sine erişim öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: 4b6bd97d7e87832f774f7a09f7e0deeb4047e695
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598726"
---
# <a name="use-azure-ad-authentication-to-access-the-media-services-api-with-rest"></a>REST ile Media Services API'sine erişmek için Azure AD kimlik doğrulaması kullanın.

Azure Media Services ile Azure AD kimlik doğrulamasını kullanırken, iki yoldan biriyle kimlik doğrulaması yapabilir:

- **Kullanıcı kimlik doğrulaması** kullanan uygulama etkileşim kurmak için Azure Media Services kaynaklarla bir kişinin kimliğini doğrular. Etkileşimli uygulama, kullanıcıdan kimlik bilgilerini ilk isteyecektir. Kodlama işi izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet sorumlusu kimlik doğrulaması** bir hizmetin kimliğini doğrular. Genellikle bu kimlik doğrulama yöntemi kullanan uygulamalar, daemon Hizmetleri, orta katman Hizmetleri ve web uygulamaları, işlev uygulamaları, logic apps, API veya mikro hizmetler gibi zamanlanmış işler çalışan uygulamalardır.

    Bu öğreticide Azure AD'yi kullanacak şekilde gösterilmektedir **hizmet sorumlusu** AMS API'ye REST ile erişmek için kimlik doğrulaması. 

    > [!NOTE]
    > **Hizmet sorumlusu** Azure Media Services'e bağlanma uygulamalarının çoğu için önerilen en iyi yöntem olacaktır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalından kimlik doğrulama bilgilerini alın
> * Postman kullanarak erişim belirteci alma
> * Test **varlıklar** erişim belirtecini kullanarak API


> [!IMPORTANT]
> Şu anda Media Services, Azure erişim denetimi Hizmetleri kimlik doğrulama modeli destekler. Ancak, 1 Haziran 2018'e erişim denetimi kimlik doğrulaması kullanımdan kaldırılacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [Azure portalını kullanarak bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Gözden geçirme [Azure AD kimlik doğrulamasına genel bakış ile Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md) makalesi.
- Yükleme [Postman](https://www.getpostman.com/) bu makalede gösterilen REST API'leri yürütmek için REST istemci. 

    Bu öğreticide, kullanıyoruz **Postman** ancak herhangi bir REST aracı uygun olacaktır. Diğer alternatifler: **Visual Studio Code** REST eklentisiyle veya **Telerik Fiddler**. 

## <a name="get-the-authentication-information-from-the-azure-portal"></a>Azure portalından kimlik doğrulama bilgilerini alın

### <a name="overview"></a>Genel Bakış

Media Services API'sine erişmek için aşağıdaki veri noktaları toplamak gerekir.

|Ayar|Örnek|Açıklama|
|---|-------|-----|
|Azure Active Directory kiracı etki alanı|microsoft.onmicrosoft.com|Azure AD'yi bir güvenli belirteç hizmeti (STS) uç noktası olarak, aşağıdaki biçimi kullanarak oluşturulur: <https://login.microsoftonline.com/{your-ad-tenant-name.onmicrosoft.com}/oauth2/token>. Azure AD, bir JWT (bir erişim belirteci) kaynaklara erişmek için verir.|
|REST API uç noktası|<https://amshelloworld.restv2.westus.media.azure.net/api/>|Bu, tüm medya Hizmetleri REST API çağrıları, uygulamanızda yapılan uç noktadır.|
|İstemci kimliği (uygulama kimliği)|f7fbbb29-a02d-4d91-bbc6-59a2579259d2|Azure AD uygulama (istemci) kimliği. Erişim belirteci almak için istemci kimliği gereklidir. |
|İstemci Gizli Anahtarı|+mUERiNzVMoJGggD6aV1etzFGa1n6KeSlLjIq+Dbim0=|Azure AD uygulama anahtarları (gizli). Erişim belirteci almak için istemci gizli anahtarı gereklidir.|

### <a name="get-aad-auth-info-from-the-azure-portal"></a>Azure Portalı'ndan AAD kimlik doğrulama bilgilerini al

Bilgi edinmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. AMS Örneğinize gidin.
3. Seçin **API erişimi**.
4. Tıklayarak **hizmet sorumlusu ile Azure Media Services API'sine bağlanma**.

    ![API erişimi](./media/connect-with-rest/connect-with-rest01.png)

5. Mevcut bir seçin **Azure AD uygulaması** veya (aşağıda gösterilen) yeni bir tane oluşturun.

    > [!NOTE]
    > Azure medya REST isteği başarılı olması çağıran kullanıcının olmalıdır bir **katkıda bulunan** veya **sahibi** erişmeye çalışıyor Media Services hesap için rolü. Bildiren bir özel durum alırsanız "uzak sunucu bir hata döndürdü: Yetkisiz (401)"konusuna bakın [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control).

    Yeni bir AD uygulaması oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:
    
   1. Tuşuna **Yeni Oluştur**.
   2. Bir ad girin.
   3. Tuşuna **Yeni Oluştur** yeniden.
   4. **Kaydet**’e basın.

      ![API erişimi](./media/connect-with-rest/new-app.png)

      Yeni uygulama sayfasında gösterilir.

6. Alma **istemci kimliği** (uygulama kimliği).
    
   1. Uygulamayı seçin.
   2. Alma **istemci kimliği** sağdaki penceresinden. 

      ![API erişimi](./media/connect-with-rest/existing-client-id.png)

7. Uygulama alma **anahtarı** (gizli). 

   1. Tıklayın **uygulamasını Yönet** düğmesine (istemci kimlik bilgileri altında olduğuna dikkat edin **uygulama kimliği**). 
   2. Tuşuna **anahtarları**.
    
       ![API erişimi](./media/connect-with-rest/manage-app.png)
   3. Uygulama anahtarı (gizli) doldurarak oluşturmak **açıklama** ve **EXPIRES** tuşuna basarak **Kaydet**.
    
       Bir kez **Kaydet** düğmesine basıldığında, anahtar değeri görüntülenir. Dikey ayrılmadan önce anahtar değerini kopyalayın.

   ![API erişimi](./media/connect-with-rest/connect-with-rest03.png)

Kodunuzu daha sonra kullanmak için web.config veya app.config dosyanıza AD bağlantı parametreleri için değerler ekleyebilirsiniz.

> [!IMPORTANT]
> **İstemci anahtarı** önemli bir gizli dizidir ve düzgün bir anahtar Kasası'nda Güvenli veya üretimde şifrelenir.

## <a name="get-the-access-token-using-postman"></a>Postman kullanarak erişim belirteci alma

Bu bölümde, nasıl kullanılacağını gösterir **Postman** bir JWT taşıyıcı belirteç (SID) döndüren bir REST API yürütülecek. Herhangi bir Media Services REST API çağırmak için "Yetkilendirme" üst çağrıları ekleyin ve değeri eklemek gereken "taşıyıcı *your_access_token*" (Bu öğreticinin sonraki bölümünde gösterildiği gibi) her çağrı için. 

1. Açık **Postman**.
2. **POST**'u seçin.
3. Aşağıdaki biçimi kullanarak Kiracı adınızı içeren URL'yi girin: Kiracı adı ile bitmelidir **. onmicrosoft.com** ve URL ile bitmelidir **oauth2/belirtecin**: 

    https://login.microsoftonline.com/{your-aad-tenant-name.onmicrosoft.com}/oauth2/token

4. Seçin **üstbilgileri** sekmesi.
5. Girin **üstbilgileri** "Anahtar/değer" veri kılavuzu kullanarak bilgileri. 

    ![Veri Kılavuzu](./media/connect-with-rest/headers-data-grid.png)

    Alternatif olarak, **toplu düzenleme** bağlantı Postman pencerenin sağ tarafındaki ve aşağıdaki kodu yapıştırın.

        Content-Type:application/x-www-form-urlencoded
        Keep-Alive:true

6. Tuşuna **gövdesi** sekmesi.
7. "Anahtar/değer" Veri Kılavuzu (Değiştir istemci kimliği ve gizli değerler) kullanarak gövde bilgileri girin. 

    ![Veri Kılavuzu](./media/connect-with-rest/data-grid.png)

    Alternatif olarak, **toplu düzenleme** Yapıştır aşağıdaki gövdesi (Değiştir istemci kimliği ve gizli değerler) ve Postman penceresini sağdaki:

        grant_type:client_credentials
        client_id:{Your Client ID that you got from your Azure AD Application}
        client_secret:{Your client secret that you got from your Azure AD Application's Keys}
        resource:https://rest.media.azure.net

8. **Gönder**’e basın.

    ![belirteci alma](./media/connect-with-rest/connect-with-rest04.png)

Döndürülen yanıt içeren **erişim belirteci** , AMS API'lere kullanmanız gerekebilir.

## <a name="test-the-assets-api-using-the-access-token"></a>Test **varlıklar** erişim belirtecini kullanarak API

Bu bölümde nasıl erişeceğinizi gösterir **varlıklar** API'sini kullanarak **Postman**.

1. Açık **Postman**.
2. **GET**'i seçin.
3. REST API uç noktası (örneğin, yapıştırın https://amshelloworld.restv2.westus.media.azure.net/api/Assets)
4. Seçin **yetkilendirme** sekmesi. 
5. Seçin **taşıyıcı belirteç**.
6. Önceki bölümde oluşturulan belirteç yapıştırın.

    ![belirteci alma](./media/connect-with-rest/connect-with-rest05.png)

    > [!NOTE]
    > Postman UX, Mac ve bilgisayar arasında farklı olabilir. Mac sürümü "Taşıyıcı belirteç" seçeneği yoksa **kimlik doğrulaması** bölümü açılır listesinde, eklemelidir **yetkilendirme** el ile Mac istemci üstbilgisi.

   ![Kimlik doğrulama üst bilgisi](./media/connect-with-rest/auth-header.png)

7. Seçin **üstbilgileri**.
5. Tıklayın **toplu düzenleme** Postman pencerenin sağ tarafta bağlayın.
6. Aşağıdaki üst bilgileri yapıştırın:

        x-ms-version:2.15
        Accept:application/json
        Content-Type:application/json
        DataServiceVersion:3.0
        MaxDataServiceVersion:3.0

7. **Gönder**’e basın.

Döndürülen yanıt hesabınızdaki olan varlıklar içeriyor.

## <a name="next-steps"></a>Sonraki adımlar

* Bu örnek kodu deneyin [Azure Media Services erişimi için Azure AD kimlik doğrulaması: REST API aracılığıyla hem de](https://github.com/willzhan/WAMSRESTSoln)
* [.NET ile dosyaları karşıya yükleme](media-services-dotnet-upload-files.md)
