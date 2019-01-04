---
title: Azure dijital çiftleri için Postman'ı yapılandırma | Microsoft Docs
description: Azure dijital çiftleri için Postman'ı yapılandırma
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: adgera
ms.openlocfilehash: 32c56a2ac3df9f386300a6ee8207a76c8031ab10
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016724"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Azure dijital çiftleri için Postman'ı yapılandırma

Bu makalede etkileşime geçmek ve Azure dijital İkizlerini yönetim API'leri test etmek için Postman REST istemcisinin nasıl yapılandırılacağını açıklar.

Makale bir Azure Active Directory uygulamasının OAuth 2.0 örtülü izin akışı kullanmak için nasıl yapılandırılacağını gösterir. Ayrıca, yönetim Apı'leriniz için belirteci seçtiğiniz HTTP isteğinde bulunmak için Postman REST istemcisinin nasıl yapılandırılacağını açıklar.

## <a name="postman-summary"></a>Postman özeti

Azure dijital İkizlerini üzerinde bir REST istemcisi aracı gibi kullanarak başlama [Postman](https://www.getpostman.com/) yerel test ortamınızı hazırlamak için. Postman istemci karmaşık HTTP isteklerine hızlı bir şekilde oluşturulmasına yardımcı olur. Giderek Postman istemci Masaüstü sürümünü indirin [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) bir REST test etme anahtar HTTP isteği işlevlerini yararlı Masaüstü ve GUI eklentisi tabanlı bulur aracı. Postman istemcinize çözümleri geliştiricilerin HTTP isteği türünü belirtebilirsiniz (*POST*, *alma*, *güncelleştirme*, *düzeltme eki*ve  *SİLME*), SSL kullanımını ve çağırmak için API uç noktası. Postman, HTTP isteği üstbilgileri ekleme, parametreleri, form verilerini ve gövdeleri de destekler.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>OAuth 2.0 örtülü izin akışı kullanmak için Azure Active Directory'yi yapılandırma

OAuth 2.0 örtülü izin akışı kullanmak için Azure Active Directory uygulamanızı yapılandırın.

1. Bağlantısındaki [Bu hızlı başlangıçta](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) tür yerel bir Azure AD uygulaması oluşturmak için. Veya var olan bir yerel uygulama kaydı yeniden kullanabilirsiniz.

1. Altında **gerekli izinler**seçin **Ekle** girin **Azure dijital İkizlerini** altında **API erişimi Ekle**. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın. Ardından, **izinler > Temsilcili izinler** ve **Bitti**.

    ![Azure Active Directory Uygulama kayıtları API ekleme](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

1. Tıklayın **bildirim** uygulamanız için uygulama bildirimini açın. Ayarlama *oauth2AllowImplicitFlow* için `true`.

      ![Azure Active Directory örtük akış][1]

1. Yapılandırma bir **yanıt URL'si** için `https://www.getpostman.com/oauth2/callback`.

      ![Azure Active Directory yanıt URL'si][2]

1. Kopyalayın ve tutun **uygulama kimliği** Azure Active Directory uygulama. Aşağıda kullanılır.

### <a name="configure-the-postman-client"></a>Postman istemciyi Yapılandırma

Ardından, ayarlama ve Azure Active Directory belirteci almak için Postman'ı yapılandırma. Ardından, kimliği doğrulanmış bir HTTP isteği Azure dijital alınan belirteciyle İkizlerini olun:

1. Git [www.getpostman.com]([https://www.getpostman.com/) uygulamayı indirmek için.
1. Doğrulayın, **yetkilendirme URL'si** doğrudur. Bu biçim sürer:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Ad  | Şununla değiştir | Örnek |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | Kiracı veya kuruluş adı | `microsoft` |

1. Seçin **yetkilendirme** sekmesinde **OAuth 2.0**ve ardından **yeni erişim belirteci Al**.

    | Alan  | Değer |
    |---------|---------|
    | İzin Verme Türü | `Implicit` |
    | Geri çağırma URL'si | `https://www.getpostman.com/oauth2/callback` |
    | Kimlik doğrulama URL'si | Kullanım **yetkilendirme URL'si** gelen yukarıdaki 2. adım |
    | İstemci Kimliği | Kullanım **uygulama kimliği** oluşturulmuş veya başka bir amaçla kullanılması önceki bölümden Azure Active Directory uygulaması |
    | Kapsam | Boş bırakın |
    | Durum | Boş bırakın |
    | İstemci Kimlik Doğrulaması | `Send as Basic Auth header` |

1. İstemci gibi görünmelidir:

   ![Postman istemci örneği][3]

1. Seçin **belirteç iste**.

    >[!NOTE]
    >"2 OAuth tamamlanamadı" hata iletisini alırsanız, aşağıdakileri deneyin:
    > * Postman, kapatın ve yeniden açın ve yeniden deneyin.
  
1. Aşağı kaydırın ve select **kullanım belirteci**.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim API'leri ile kimlik doğrulaması hakkında bilgi edinmek için [API'leri ile kimlik doğrulama](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-configure-postman/implicit-flow.png
[2]: media/how-to-configure-postman/reply-url.png
[3]: media/how-to-configure-postman/postman-oauth-token.png
