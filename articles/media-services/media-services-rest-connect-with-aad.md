---
title: Azure Media Services API REST ile erişmek için kimlik doğrulamasını kullan Azure AD | Microsoft Docs
description: Azure Media Services API REST kullanarak Azure Active Directory kimlik doğrulaması ile erişim öğrenin.
services: media-services
documentationcenter: ''
author: willzhan
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/26/2017
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: ed78d6c6d4c695b841dbfbf917cd1681adc44ee7
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
ms.locfileid: "27566815"
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Azure Media Services API REST ile erişmek için Azure AD kimlik doğrulaması kullanın

Azure Media Services ile Azure AD kimlik doğrulaması kullanırken, aşağıdaki iki yöntemden biriyle doğrulayabilir:

- **Kullanıcı kimlik doğrulaması** uygulama etkileşim kurmak için Azure Media Services kaynaklarla kullanan bir kişinin kimliğini doğrular. Etkileşimli uygulama önce kullanıcıdan kimlik bilgilerini ister. Kodlama işleri izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet asıl kimlik doğrulaması** bir hizmetin kimliğini doğrular. Genellikle bu kimlik doğrulama yöntemini kullanan uygulamalar, arka plan programı services, orta katman Hizmetleri veya web uygulamaları, işlev uygulamalarının, logic apps, API veya mikro gibi zamanlanmış işleri çalıştırma uygulamalardır.

    Bu öğretici, Azure AD kullanmayı gösterir **hizmet sorumlusu** AMS API REST ile erişmek için kimlik doğrulaması. 

    > [!NOTE]
    > **Hizmet sorumlusu** önerilen en iyi uygulamadır Azure Media Services'e bağlanma çoğu uygulamalar için. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Portalı'ndan kimlik doğrulama bilgilerini alın
> * Postman kullanarak erişim belirteci alın
> * Test **varlıklar** erişim belirtecini kullanarak API


> [!IMPORTANT]
> Şu anda, Media Services Azure erişim denetimi Hizmetleri kimlik doğrulama modelini destekler. Ancak, erişim denetimi kimlik doğrulaması 1 Haziran 2018 kullanım dışı kalacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [Azure portalını kullanarak Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Gözden geçirme [AAD kimlik doğrulamasına genel bakış ile Azure Media Services API erişme](media-services-use-aad-auth-to-access-ams-api.md) makalesi.
- Yükleme [Postman](https://www.getpostman.com/) bu makalede gösterilen REST API'leri yürütmek için REST istemci. 

    Bu öğreticide, duyuyoruz uring **Postman** ancak herhangi bir REST aracı uygun olacaktır. Diğer seçenekleri şunlardır: **Visual Studio Code** REST eklentisi ile veya **Telerik Fiddler**. 

## <a name="get-the-authentication-information-from-the-azure-portal"></a>Azure Portalı'ndan kimlik doğrulama bilgilerini alın

### <a name="overview"></a>Genel Bakış

Media Services API erişmek için aşağıdaki veri noktaları toplamak gerekir.

|Ayar|Örnek|Açıklama|
|---|-------|-----|
|Azure Active Directory kiracı etki alanı|microsoft.onmicrosoft.com|Aşağıdaki biçimi kullanarak bir güvenli belirteç hizmeti (STS) uç noktası olarak Azure AD oluşturulur: https://login.microsoftonline.com/{your-aad-tenant-name.onmicrosoft.com}/oauth2/token. Azure AD (bir erişim belirteci) kaynaklara erişmek için JWT verir.|
|REST API uç noktası|https://amshelloworld.restv2.westus.media.azure.net/api/|Bu karşı tüm hangi Media Services REST API çağrıları, uygulamanızda yapılan uç noktadır.|
|İstemci kimliği (uygulama kimliği)|f7fbbb29-a02d-4d91-bbc6-59a2579259d2|Azure AD uygulama (istemci) kimliği İstemci Kimliğini ve erişim belirteci almak için gereklidir. |
|İstemci Parolası|+mUERiNzVMoJGggD6aV1etzFGa1n6KeSlLjIq+Dbim0=|Azure AD uygulama anahtarları (gizli). İstemci gizli anahtarı erişim belirteci almak için gereklidir.|

### <a name="get-aad-auth-info-from-the-azure-portal"></a>Azure portalından AAD kimlik doğrulama bilgilerini al

Bilgi edinmek için şu adımları izleyin:

1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. AMS örneğine gidin.
3. Seçin **API erişimini**.
4. Tıklayın **Azure Media Services API hizmet asıl Bağlan**.

    ![API erişimi](./media/connect-with-rest/connect-with-rest01.png)

5. Var olan seçin **Azure AD uygulaması** veya (aşağıda gösterilen) yeni bir tane oluşturun.

    > [!NOTE]
    > Azure Media REST istek başarılı olması arayan kullanıcı olmalıdır bir **katkıda bulunan** veya **sahibi** erişmeye çalışıyor Media Services hesap için rolü. Bildiren bir özel durum alırsanız "uzak sunucusu bir hata döndürdü: (401) yetkisiz" bkz [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control).

    Yeni AD uygulaması oluşturmanız gerekiyorsa, şu adımları izleyin:
    
    1. Tuşuna **Yeni Oluştur**.
    2. Bir ad girin.
    3. Tuşuna **Yeni Oluştur** yeniden.
    4. Tuşuna **kaydetmek**.

    ![API erişimi](./media/connect-with-rest/new-app.png)

    Yeni uygulama sayfasında görüntülenir.

6. Alma **istemci kimliği** (uygulama kimliği ile).
    
    1. Uygulamayı seçin.
    2. Alma **istemci kimliği** sağdaki penceresinden. 

    ![API erişimi](./media/connect-with-rest/existing-client-id.png).

7.  Uygulamanın alma **anahtar** (gizli). 

    1. Tıklatın **uygulamasını Yönet** düğmesini (istemci kimlik bilgileri altında olduğuna dikkat edin **uygulama kimliği**). 
    2. Tuşuna **anahtarları**.
    
        ![API erişimi](./media/connect-with-rest/manage-app.png)
    3. Uygulama anahtarı (gizli) doldurarak oluşturmak **açıklama** ve **EXPIRES** tuşuna basarak **kaydetmek**.
    
        Bir kez **kaydetmek** düğmesine basıldığında, anahtar değeri görüntülenir. Anahtar değeri dikey penceresinde ayrılmadan önce kopyalayın.

    ![API erişimi](./media/connect-with-rest/connect-with-rest03.png)

Kodunuzu daha sonra kullanmak için web.config veya app.config dosyasını AD bağlantı parametreleri için değerler ekleyebilirsiniz.

> [!IMPORTANT]
> **İstemci anahtar** önemli bir parolası ve düzgün bir anahtar kasası veya güvenli hale üretimde şifrelenir.

## <a name="get-the-access-token-using-postman"></a>Postman kullanarak erişim belirteci alın

Bu bölümde nasıl kullanılacağını gösterir **Postman** bir JWT taşıyıcı belirteç (SID) döndüren bir REST API yürütülecek. Bir Media Services REST API çağrısı için çağrıları "Yetkilendirme" üstbilgi ekleyin ve değerini eklemek gereken "taşıyıcı *your_access_token*" (Bu öğreticinin sonraki bölümde gösterildiği gibi) her çağrı için. 

1. Açık **Postman**.
2. **POST**'u seçin.
3. Aşağıdaki biçimi kullanarak Kiracı adınızı içeren bir URL girin: Kiracı adı ile bitmelidir **. onmicrosoft.com** ve URL ile bitmelidir **oauth2/token**: 

    https://login.microsoftonline.com/{your-aad-tenant-name.onmicrosoft.com}/oauth2/token

4. Seçin **üstbilgileri** sekmesi.
5. Girin **üstbilgileri** "Anahtar/değer" veri kılavuzu kullanarak bilgi. 

    ![Veri Kılavuzu](./media/connect-with-rest/headers-data-grid.png)

    Alternatif olarak, **Toplu Düzenle** bağlantı Postman pencerenin sağ tarafta ve aşağıdaki kodu yapıştırın.

        Content-Type:application/x-www-form-urlencoded
        Keep-Alive:true

6. Tuşuna **gövde** sekmesi.
7. "Anahtar/değer" Veri Kılavuzu (Değiştir istemci Kimliğini ve gizli değerler) kullanarak gövde bilgileri girin. 

    ![Veri Kılavuzu](./media/connect-with-rest/data-grid.png)

    Alternatif olarak, **Toplu Düzenle** Yapıştır aşağıdaki gövdesi (Değiştir istemci Kimliğini ve gizli değerler) ve Postman penceresini sağdaki:

        grant_type:client_credentials
        client_id:{Your Client ID that you got from your AAD Application}
        client_secret:{Your client secret that you got from your AAD Application's Keys}
        resource:https://rest.media.azure.net

8. **Gönder**’e basın.

    ![Belirteci alma](./media/connect-with-rest/connect-with-rest04.png)

Döndürülen yanıt içeren **erişim belirteci** , tüm AMS API'lerinin erişmek için kullanmanız gerekebilir.

## <a name="test-the-assets-api-using-the-access-token"></a>Test **varlıklar** erişim belirtecini kullanarak API

Bu bölümde nasıl erişeceğinizi gösterir **varlıklar** API kullanarak **Postman**.

1. Açık **Postman**.
2. **GET**'i seçin.
3. REST API uç noktası (örneğin, yapıştırın https://amshelloworld.restv2.westus.media.azure.net/api/Assets)
4. Seçin **yetkilendirme** sekmesi. 
5. Seçin **taşıyıcı belirteci**.
6. Önceki bölümde oluşturulan belirteç yapıştırın.

    ![Belirteci alma](./media/connect-with-rest/connect-with-rest05.png)

    > [!NOTE]
    > Postman UX Mac ve PC arasında farklı olabilir. Mac sürümünü "Taşıyıcı belirteci" seçeneği yoksa **kimlik doğrulaması** bölümü açılır listesinde, eklemek **yetkilendirme** Mac istemcisini el ile başlığındaki.

   ![Kimlik doğrulama üstbilgisi](./media/connect-with-rest/auth-header.png)

7. Seçin **üstbilgileri**.
5. Tıklatın **Toplu Düzenle** Postman pencerenin sağ tarafta bağlantı.
6. Aşağıdaki üst bilgiler yapıştırın:

        x-ms-version:2.15
        Accept:application/json
        Content-Type:application/json
        DataServiceVersion:3.0
        MaxDataServiceVersion:3.0

7. **Gönder**’e basın.

Döndürülen yanıt hesabınızda olan varlıkları içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Bu örnek kodda deneyin [Azure Media Services erişimi için Azure AD kimlik doğrulamasını: REST API aracılığıyla her ikisi](https://github.com/willzhan/WAMSRESTSoln)
* [.NET ile dosyaları karşıya yükleme](media-services-dotnet-upload-files.md)
