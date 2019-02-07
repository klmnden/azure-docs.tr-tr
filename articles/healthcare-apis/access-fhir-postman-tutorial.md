---
title: Azure Öğreticisi - Azure API FHIR için postman FHIR sunucusu
description: Bu makalede, bir Postman ile FHIR API'sine açıklar.
services: healthcare-apis
ms.service: healthcare-apis
ms.topic: tutorial
ms.reviewer: dseven
ms.author: mihansen
author: hansenms
ms.date: 02/07/2019
ms.openlocfilehash: 7009fa175e19ad484c83ec3a988d6c33b67fefcd
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823441"
---
# <a name="tutorial-access-fhir-api-with-postman"></a>Öğretici: Postman ile FHIR API'ye erişme

Bir istemci uygulaması FHIR API üzerinden eriştiğiniz bir [REST API](https://www.hl7.org/fhir/http.html). Ayrıca, uygulama, örneğin, hata ayıklama amacıyla oluşturdukça doğrudan FHIR sunucusu ile etkileşim kurmak isteyebilirsiniz. Bu öğreticide, biz kullanmak için gereken adımlarda size yol gösterir [Postman](https://www.getpostman.com/) FHIR sunucusuna erişmek için. Postman, genellikle API'lere uygulamaları oluştururken hata ayıklama için kullanılan bir araçtır.

## <a name="prerequisites"></a>Önkoşullar

- Azure'da FHIR uç. Yönetilen Azure API FHIR veya açık kaynak FHIR server için Azure kullanarak ayarlayabilirsiniz. FHIR kullanmak için yönetilen Azure API ayarlama [Azure portalında](fhir-paas-portal-quickstart.md), [PowerShell](fhir-paas-powershell-quickstart.md), veya [Azure CLI](fhir-paas-cli-quickstart.md).
- Postman yüklü. Buradan edinin [https://www.getpostman.com](https://www.getpostman.com)

## <a name="fhir-server-and-authentication-details"></a>FHIR server ve kimlik doğrulaması ayrıntıları

Postman'ı kullanmak için aşağıdaki ayrıntıları bilmeniz gerekir:

- FHIR sunucusu URL'nizi, örneğin, `https://MYFHIRSERVICE.azurewebsites.net` veya `https://MYACCOUNT.azurehealthcareapis.com`
- Kimlik sağlayıcısı `Authority` FHIR sunucunuzun, örneğin, `https://login.microsoftonline.com/{TENANT-ID}`
- Yapılandırılmış `audience`. Azure API FHIR için budur `https://azurehealthcareapis.com` ve Azure için açık kaynak FHIR sunucusu için bu ayarlanır [Azure AD'ye kaynak uygulama kaydı](register-resource-azure-ad-client-app.md).
- `client_id` (Veya uygulama kimliği), [istemci uygulaması](register-confidential-azure-ad-client-app.md) FHIR hizmete erişmek için kullanacaksınız.
- `client_secret` (Veya uygulama gizli anahtarı) istemci uygulaması.

Son olarak, kontrol `https://www.getpostman.com/oauth2/callback` istemci uygulamanız için kayıtlı yanıt URL'dir.

## <a name="connect-to-fhir-server"></a>FHIR sunucuya bağlanın

Postman kullanarak bunu bir `GET` isteği `https://fhir-server-url/metadata`:

![Postman meta veri özellik ifadesi](media/tutorial-postman/postman-metadata.png)

Bu örnekte FHIR sunucu URL'si: `https://fhirdocsmsft.azurewebsites.net` ve yetenek ifadesi sunucunun kullanılabilir `https://fhirdocsmsft.azurewebsites.net/metadata`. Bu uç nokta kimlik doğrulaması olmadan erişilebilir olmalıdır.

Kısıtlanmış kaynaklara erişmeyi denerseniz, ardından bir "Kimlik doğrulaması başarısız oldu" yanıt gerekir:

![Kimlik doğrulaması başarısız oldu](media/tutorial-postman/postman-authentication-failed.png)

## <a name="obtaining-an-access-token"></a>Bir erişim belirteci alma

Geçerli erişim belirteci almak için "Yetkilendirme" ve türü seçin "OAuth 2.0":

![OAuth 2.0 ayarlayın](media/tutorial-postman/postman-select-oauth2.png)

İsabet "Yeni erişim belirteci Al" ve bir iletişim kutusu görüntülenir:

![Yeni bir erişim belirteci isteği](media/tutorial-postman/postman-request-token.png)

Bazı ayrıntılar ihtiyacınız olacak:
| Alan                 | Örnek Değer                                                                                                   | Açıklama                    |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------|
| Belirteç adı            | MYTOKEN                                                                                                         | Seçtiğiniz bir adı          |
| İzin Verme Türü            | Yetkilendirme kodu                                                                                              |                            |
| Geri çağırma URL'si          | `https://www.getpostman.com/oauth2/callback`                                                                      |                            |
| Kimlik doğrulama URL'si              | `https://login.microsoftonline.com/{TENANT-ID}/oauth2/authorize?resource=<audience>` | `audience` olan `https://azurehealthcareapis.com` FHIR için Azure API ve `https://MYFHIRSERVICE.azurewebsites.net` OSS FHIR sunucusu |
| Erişim belirteci URL'si      | `https://login.microsoftonline.com/{TENANT ID}/oauth2/token`                                                      |                            |
| İstemci Kimliği             | `XXXXXXXX-XXX-XXXX-XXXX-XXXXXXXXXXXX`                                                                            | Uygulama Kimliği             |
| İstemci Gizli Anahtarı         | `XXXXXXXX`                                                                                                        | İstemci gizli anahtarı          |
| Durum                 | `1234`                                                                                                            |                            |
| İstemci Kimlik Doğrulaması | İstemci kimlik bilgileri gövdesinde Gönder                                                                                 |                 

İsabet "istek Token" ve Azure Active Directory kimlik doğrulaması akışını destekli ve bir belirteç için Postman döndürülür. Sorunlarla karşılaşırsanız, Postman konsoldan ("Görünümü göster Postman Konsolu ->" menü öğesi) açın.

Döndürülen belirteç ekranda aşağı kaydırın ve "kullanım Token" basın:

![Belirteci kullanın](media/tutorial-postman/postman-use-token.png)

Belirteci şimdi "Access Token" alanında doldurulur ve "Kullanılabilir belirteçler" belirteçleri seçebilirsiniz. "Tekrar yinelenecek gönderdiğiniz ise" `Patient` kaynak arama durumu almanız gerekir `200 OK`:

![200 TAMAM](media/tutorial-postman/postman-200-OK.png)

Bu durumda, veritabanında hiçbir hastalara vardır ve boş bir aramadır.

Bir aracı ile erişim belirteci İnceleme hoşlanıyorsanız [ https://jwt.ms ](https://jwt.ms), içeriği gibi görmeniz gerekir:

```json
{
  "aud": "https://azurehealthcareapis.com",
  "iss": "https://sts.windows.net/{TENANT-ID}/",
  "iat": 1545343803,
  "nbf": 1545343803,
  "exp": 1545347703,
  "acr": "1",
  "aio": "AUQAu/8JXXXXXXXXXdQxcxn1eis459j70Kf9DwcUjlKY3I2G/9aOnSbw==",
  "amr": [
    "pwd"
  ],
  "appid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "oid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "appidacr": "1",

  ...// Truncated
}
```

İçinde doğru hedef kitleye olduğunu doğrulama gibi durumlarda sorun giderme (`aud` talep) başlatmak için iyi bir yerdir. Yönetilen Azure API FHIR kullanımlar için [kimlik nesne kimlikleri](find-identity-object-ids.md) hizmete erişimi kısıtlamak için. Emin olun `oid` talep belirtecin, izin verilen nesne kimlikleri listesinden bir nesne kimliği içerir.

## <a name="inserting-a-patient"></a>Bir hastanın ekleme

Geçerli bir erişim belirteciyle sahip olduğunuza göre. Yeni bir Hasta ekleyebilirsiniz. Yöntemi "POST" arasında geçiş ve istek gövdesinde şu JSON belgesini ekleyin:

[!code-json[](samples/sample-patient.json)]

"Gönder" isabet ve hasta başarıyla oluşturulduğunu görürsünüz:

![Hasta oluşturuldu](media/tutorial-postman/postman-patient-created.png)

Hasta için arama tekrarlama, Hasta kayıt şimdi görmeniz gerekir:

![Hasta oluşturuldu](media/tutorial-postman/postman-patient-found.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide postman kullanarak bir FHIR API eriştiğiniz. Azure için Microsoft FHIR Server'da desteklenen API özellikleri okuyun.
 
>[!div class="nextstepaction"]
>[Desteklenen FHIR özellikleri](fhir-features-supported.md)