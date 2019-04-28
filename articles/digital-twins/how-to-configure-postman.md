---
title: Azure dijital çiftleri için Postman'ı yapılandırma | Microsoft Docs
description: Azure dijital çiftleri için Postman'ı yapılandırma
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: adgera
ms.openlocfilehash: 49b073952b0923b940204b19680dcc9a1ffa44b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60926199"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Azure dijital çiftleri için Postman'ı yapılandırma

Bu makalede etkileşime geçmek ve Azure dijital İkizlerini yönetim API'leri test etmek için Postman REST istemcisinin nasıl yapılandırılacağını açıklar. Özellikle açıklanmaktadır:

* bir Azure Active Directory uygulamasının OAuth 2.0 örtülü izin akışı kullanmak için yapılandırılır.
* Yönetim Apı'leriniz için belirteci seçtiğiniz HTTP isteğinde bulunmak için Postman REST istemcisini kullanma
* Yönetim Apı'leriniz için çok bölümlü POST isteğinde bulunmak için Postman'ı kullanma

## <a name="postman-summary"></a>Postman özeti

Azure dijital İkizlerini üzerinde bir REST istemcisi aracı gibi kullanarak başlama [Postman](https://www.getpostman.com/) yerel test ortamınızı hazırlamak için. Postman istemci karmaşık HTTP isteklerine hızlı bir şekilde oluşturulmasına yardımcı olur. Giderek Postman istemci Masaüstü sürümünü indirin [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) bir REST test etme anahtar HTTP isteği işlevlerini yararlı Masaüstü ve GUI eklentisi tabanlı bulur aracı. 

Postman istemcinize çözümleri geliştiricilerin HTTP isteği türünü belirtebilirsiniz (*POST*, *alma*, *güncelleştirme*, *düzeltme eki*ve  *SİLME*), SSL kullanımını ve çağırmak için API uç noktası. Postman, HTTP isteği üstbilgileri ekleme, parametreleri, form verilerini ve gövdeleri de destekler.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>OAuth 2.0 örtülü izin akışı kullanmak için Azure Active Directory'yi yapılandırma

OAuth 2.0 örtülü izin akışı kullanmak için Azure Active Directory uygulamanızı yapılandırın.

1. Bağlantısındaki [Bu hızlı başlangıçta](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) tür yerel bir Azure AD uygulaması oluşturmak için. Veya var olan bir yerel uygulama kaydı yeniden kullanabilirsiniz.

1. Altında **gerekli izinler**seçin **Ekle** girin **Azure dijital İkizlerini** altında **API erişimi Ekle**. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın. Ardından, **izinler > Temsilcili izinler** ve **Bitti**.

    ![Azure Active Directory Uygulama kayıtları API ekleme](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

1. Tıklayın **bildirim** uygulamanız için uygulama bildirimini açın. Ayarlama *oauth2AllowImplicitFlow* için `true`.

      ![Azure Active Directory örtük akış][1]

1. Yapılandırma bir **yanıt URL'si** için `https://www.getpostman.com/oauth2/callback`.

      ![Azure Active Directory yanıt URL'si][2]

1. Kopyalayın ve tutun **uygulama kimliği** Azure Active Directory uygulama. İzleyen adımları kullanılır.

## <a name="obtain-an-oauth-20-token"></a>OAuth 2.0 belirteç edinme

Ardından, ayarlama ve Azure Active Directory belirteci almak için Postman'ı yapılandırma. Ardından, kimliği doğrulanmış bir HTTP isteği Azure dijital alınan belirteciyle İkizlerini olun:

1. Git [www.getpostman.com](https://www.getpostman.com/) uygulamayı indirmek için.
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
    | Kimlik doğrulama URL'si | Kullanım **yetkilendirme URL'si** adım 2 |
    | İstemci Kimliği | Kullanım **uygulama kimliği** oluşturulmuş veya başka bir amaçla kullanılması önceki bölümden Azure Active Directory uygulaması |
    | Kapsam | Boş bırakın |
    | Durum | Boş bırakın |
    | İstemci Kimlik Doğrulaması | `Send as Basic Auth header` |

1. Şimdi, istemci olarak görünmesi gerekir:

   ![Postman istemci örneği][3]

1. Seçin **belirteç iste**.

    >[!TIP]
    >"2 OAuth tamamlanamadı" hata iletisini alırsanız, aşağıdakileri deneyin:
    > * Postman, kapatın ve yeniden açın ve yeniden deneyin.
  
1. Aşağı kaydırın ve select **kullanım belirteci**.

<div id="multi"></div>

## <a name="make-a-multipart-post-request"></a>Çok bölümlü bir POST isteği yapmak

Önceki adımları tamamladıktan sonra kimliği doğrulanmış bir HTTP çok bölümlü POST isteğinde bulunmak için: Postman yapılandırın:

1. Altında **üstbilgi** sekmesinde, bir HTTP isteği üstbilgi anahtarı ekleyin **Content-Type** değerle `multipart/mixed`.

   ![İçerik türü çok parçalı/karışık][4]

1. Metin olmayan veri dosyalarıyla serileştirir. JSON verilerini bir JSON dosyası olarak kaydedilmesi.
1. Altında **gövdesi** sekmesi, her dosya atayarak ekleme bir **anahtarı** name, seçerek `file` veya `text`.
1. Her dosyanın üzerinden seçip **Dosya Seç** düğmesi.

   ![Postman istemci örneği][5]

   >[!NOTE]
   > * Postman istemci çok bölümlü öbekleri el ile atanan olmasını gerektirmez **Content-Type** veya **Content-Disposition**.
   > * Her parça için bu üstbilgileri belirtmek gerekmez.
   > * Seçmelisiniz `multipart/mixed` veya başka bir uygun **Content-Type** tüm istek için.

1. Son olarak, tıklayın **Gönder** çok bölümlü HTTP POST isteğinizi gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Dijital İkizlerini yönetim API'leri ve bunların nasıl kullanıldığı hakkında bilgi edinmek için [dijital İkizlerini Azure management API'leri hakkında](how-to-navigate-apis.md).

- Çok bölümlü isteklerine kullanın [blobları Azure dijital İkizlerini varlıklara ekleyin](./how-to-add-blobs.md).

- Yönetim API'leri ile kimlik doğrulaması hakkında bilgi edinmek için [API'leri ile kimlik doğrulama](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-configure-postman/implicit-flow.png
[2]: media/how-to-configure-postman/reply-url.png
[3]: media/how-to-configure-postman/postman-oauth-token.png
[4]: media/how-to-configure-postman/content-type.png
[5]: media/how-to-configure-postman/form-body.png