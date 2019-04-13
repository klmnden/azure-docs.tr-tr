---
title: Azure Media Services REST API çağrıları için Postman'ı yapılandırma
description: Media Services REST API çağrıları için Postman'ı yapılandırmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2019
ms.author: juliako
ms.openlocfilehash: a2171ff8a4354a59ec2f790f9bf38b7a687419ca
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543885"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Media Services REST API çağrıları için Postman'ı yapılandırma

Bu makalede nasıl yapılacağı gösterilmektedir **Postman** böylece Azure Media Services (AMS) REST API'leri çağırmak için kullanılabilir. Makale dosyalarına ortam ve koleksiyon içeri aktarma **Postman**. Koleksiyon, Azure Media Services (AMS) REST API'lerini çağırma HTTP isteklerinin gruplandırılmış tanımları içerir. Koleksiyon tarafından kullanılan değişkenleri ortam dosyası içerir.

Geliştirmeye başlamadan önce gözden [Media Services v3 API'leri ile geliştirme](media-services-apis-overview.md).

## <a name="prerequisites"></a>Önkoşullar

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- İçin gereken bilgileri elde [API'lere erişim](access-api-cli-how-to.md)
- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer alternatifler: **Visual Studio Code** REST eklentisiyle veya **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Postman dosyalarını indirme

Postman koleksiyonunu ve ortam dosyalarını içeren bir GitHub deposunu kopyalayın.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="configure-postman"></a>Postman'i yapılandırma

Bu bölümde Postman yapılandırılmaktadır.

### <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. **Postman**'ı açın.
2. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Ortamı yönetme](./media/develop-with-postman/postman-import-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
2. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Azure Media Service v3 Environment.postman_environment.json` dosyasına gidin.
6. **Azure Media Service v3 Environment** ortamı eklenir.

    > [!Note]
    > Erişim değişkenlerini yukarıdaki **Media Services API'sine erişme** bölümünden aldığınız değerlerle güncelleştirin.

7. Seçili dosyayı çift tıklatın ve erişimi API adımları izleyerek aldığınız değerleri girin.
8. İletişim kutusunu kapatın.
9. Aşağı açılan listeden **Azure Media Service v3 Environment** ortamını seçin.

    ![Ortamı seçme](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
1. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Media Services v3.postman_collection.json` dosyasına gidin
3. **Media Services v3.postman_collection.json** dosyasını seçin.

    ![Dosya içe aktarma](./media/develop-with-postman/postman-import-collection.png)

## <a name="get-azure-ad-token"></a>Azure AD Belirteci alma 

AMS v3 kaynakları düzenleme başlamadan önce almak ve hizmet sorumlusu kimlik doğrulaması için Azure AD belirteç ayarlamak gerekir.

1. Postman sol penceresinde, seçin "1. adım: AAD kimlik doğrulaması belirteci alma".
2. Sonra, "Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma"'yı seçin.
3. **Gönder**’e basın.

    Aşağıdaki **POST** işlemi gönderilir.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Yanıt belirteç ile gelir ve "AccessToken" ortam değişkenini belirteç değerine ayarlar.  

    ![AAD belirteci alma](./media/develop-with-postman/postman-get-aad-auth-token.png)

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services hesabına - REST dosya yükleme](upload-files-rest-how-to.md)
- [Medya Hizmetleri - REST ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [Azure Resource Manager tabanlı REST API](https://github.com/Azure-Samples/media-services-v3-arm-templates)

## <a name="next-steps"></a>Sonraki adımlar

- [Dosyaları REST ile Stream](stream-files-tutorial-with-rest.md).  
- [Öğretici: Uzak dosya tabanlı URL kodlama ve video akışı yapma - REST](stream-files-tutorial-with-rest.md)
