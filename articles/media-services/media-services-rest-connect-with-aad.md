---
title: "Azure Media Services API REST ile erişmek için kimlik doğrulamasını kullan Azure AD | Microsoft Docs"
description: "Azure Media Services API REST kullanarak Azure Active Directory kimlik doğrulaması ile erişim öğrenin."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: e5d7a5ec1c28a552420aba5e2cd6c8c7bbf4213d
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Azure Media Services API REST ile erişmek için Azure AD kimlik doğrulaması kullanın

Azure Media Services ekip Azure Media Services erişimi için Azure Active Directory (Azure AD) kimlik doğrulama desteği yayımladı. Ayrıca, Media Services erişimi için Azure erişim denetimi hizmeti kimlik doğrulama alanı onaylanamadı planları duyurdu. Her Azure aboneliği ve her medya hizmetleri hesabı için Azure AD kiracısı bağlı olduğundan, Azure AD kimlik doğrulama desteği birçok güvenlik avantajları beraberinde getirir. Bu değişiklik ve (uygulamanız için'de Media Services .NET SDK'sı kullanıyorsanız) geçişi hakkında daha fazla ayrıntı için aşağıdaki blog gönderileri ve makalelere bakın:

- [Azure Media Services, Azure AD için destek sunar ve erişim denetimi kimlik doğrulamasının kullanımdan kaldırma](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Azure AD kimlik doğrulamasını kullanarak erişim Azure Media Services API](media-services-use-aad-auth-to-access-ams-api.md)
- [Microsoft .NET kullanarak Azure Media Services API erişmek için Azure AD kimlik doğrulaması kullanın](media-services-dotnet-get-started-with-aad.md)
- [Azure portalı kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama](media-services-portal-get-started-with-aad.md)

Aşağıdaki kısıtlamalar altında Media Services çözümleri geliştirmek bazı müşteriler gerekir:

*   Microsoft .NET veya C# programlama bir dil kullanın veya çalışma zamanı ortamı Windows değil.
*   Active Directory kimlik doğrulama kitaplıkları gibi Azure AD kitaplıkları için programlama dili kullanılabilir değil veya kendi çalışma zamanı ortamı için kullanılamaz.

Bazı müşteriler, erişim denetimi kimlik doğrulaması ve Azure Media Services erişimi için REST API kullanarak uygulamaları geliştirdik. Bu müşteriler için Azure AD kimlik doğrulaması ve sonraki Azure Media Services erişimi için yalnızca REST API kullanmak için bir yönteme ihtiyacınız vardır. Herhangi bir Azure AD kitaplıkları veya Media Services .NET SDK'sı kalmamanız gerekir. Bu makalede, bir çözüm açıklar ve bu senaryo için örnek kod sağlar. Kod hiçbir bağımlılık tüm Azure AD ile tüm REST API çağrıları olduğundan veya Azure Media Services kitaplık kodu diğer programlama dili için kolay çevrilebilir.

> [!IMPORTANT]
> Şu anda, Media Services Azure erişim denetimi Hizmetleri kimlik doğrulama modelini destekler. Ancak, erişim denetimi kimlik doğrulaması 1 Haziran 2018 kullanım dışı kalacaktır. Azure AD kimlik doğrulama modeline mümkün olan en kısa sürede geçirmek öneririz.


## <a name="design"></a>Tasarlayın

Bu makalede, aşağıdaki kimlik doğrulama ve yetkilendirme tasarım kullanın:

*  Yetkilendirme Protokolü: OAuth 2.0. OAuth 2.0 kimlik doğrulama ve yetkilendirme kapsayan bir web güvenlik standardıdır. Google, Microsoft, Facebook ve PayPal tarafından desteklenir. Ekim 2012 onay. Microsoft, OAuth 2.0 ve Openıd Connect sıkıca destekler. Bu standartlar her ikisinin birden çok hizmet ve istemci kitaplıkları, Azure Active Directory gibi .NET (OWIN) Katana açık Web arabirimi desteklenir ve Azure AD kitaplıkları.
*  Vermek türü: istemci kimlik bilgileri vermenizi türü. İstemci kimlik bilgileri biridir OAuth 2.0 dört grant türler. Genellikle, Azure AD Microsoft Graph API erişimi için de kullanılır.
*  Kimlik doğrulama modu: hizmet sorumlusu. Diğer kimlik doğrulama modu, kullanıcı veya etkileşimli kimlik doğrulaması ' dir.

Azure AD kimlik doğrulama ve yetkilendirme dört uygulamalar veya hizmetler toplam söz konusu Media Services'i kullanarak akışı. Uygulamaları ve Hizmetleri ve akış aşağıdaki tabloda açıklanmıştır:

|Uygulama türü |Uygulama |Akış|
|---|---|---|
|İstemci | Müşteri uygulama ya da çözüm | Bu uygulamayı (aslında, kendi proxy) Azure aboneliği ve medya hizmet hesabının bulunduğu Azure AD kiracısında kaydedilir. Hizmet sorumlusu kayıtlı uygulamanın sahibi veya katkıda bulunan rolü, erişim denetimi (IAM) medya hizmet hesabının sonra verilir. Hizmet sorumlusu uygulama istemci Kimliğini ve istemci gizli anahtarı temsil edilir. |
|Kimlik sağlayıcıyı (IDP) | IDP olarak Azure AD | Kayıtlı uygulama hizmet sorumlusu (istemci kimliği ve istemci gizli anahtarı) IDP olarak Azure AD tarafından doğrulanır. Bu kimlik doğrulaması, dahili ve örtük olarak gerçekleştirilir. İstemci kimlik bilgileri akışının olduğu gibi istemci yerine kullanıcının kimliği doğrulanır. |
|Belirteç Hizmeti (STS) güvenli / OAuth sunucu | STS olarak Azure AD | IDP (veya OAuth sunucusu OAuth 2.0 terimleriyle) göre kimlik doğrulamasından sonra bir erişim belirteci veya JSON Web Token (JWT) STS/OAuth sunucusu olarak Azure AD erişim için Orta katmanda kaynağı tarafından verilir: Bu örnekte, Media Services REST API uç noktası. |
|Kaynak | Media Services REST API'si | Her Media Services REST API çağrısı STS veya OAuth sunucu olarak Azure AD tarafından verilen bir erişim belirteci tarafından yetkilendirildi. |

Örnek kodu çalıştırmak ve yakalama JWT veya bir erişim belirteci, JWT aşağıdaki özniteliklere sahiptir:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

JWT ve özniteliklere dört uygulamalar veya hizmetler önceki tabloda arasındaki eşlemeleri şunlardır:

|Uygulama türü |Uygulama |JWT özniteliği |
|---|---|---|
|İstemci |Müşteri uygulama ya da çözüm |AppID: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6". Bir uygulamanın istemci Kimliğini sonraki bölümde Azure AD ile kaydeder. |
|Kimlik sağlayıcıyı (IDP) | IDP olarak Azure AD |IDP: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/" GUID olan Microsoft kimliği Kiracı (microsoft.onmicrosoft.com). Her bir kiracı kendi, benzersiz kimliği vardır. |
|Belirteç Hizmeti (STS) güvenli / OAuth sunucu |STS olarak Azure AD | ISS: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/". Microsoft kimlik Kiracı (microsoft.onmicrosoft.com) GUID'dir. |
|Kaynak | Media Services REST API'si |aud: "https://rest.media.azure.net". Alıcı ya da erişim belirteci İzleyici. |

## <a name="steps-for-setup"></a>Kurulum adımları

Kaydolun ve Azure Active Directory (AAD) uygulama ayarlama ve Azure Media Services REST API uç noktası çağrılmadan tuşları edinmek için makalesine başvurun [Azure portalını kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama](media-services-portal-get-started-with-aad.md)


## <a name="info-to-collect"></a>Bilgi toplamak için

REST kodlama hazırlamak üzere kod eklemek için aşağıdaki veri noktaları Topla:

*   Bir STS uç noktası olarak Azure AD: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Bu uç noktasından bir JWT erişim belirteci istendi. Bir IDP hizmet veren ek olarak, Azure AD aynı zamanda bir STS olarak görev yapar. Azure AD kaynak erişimi (bir erişim belirteci) için JWT verir. JWT belirteci çeşitli talep vardır.
*   Kaynak veya hedef kitle olarak Azure Media Services REST API: https://rest.media.azure.net.
*   İstemci kimliği: Bkz. Adım 2'de [kurulum adımları](#steps-for-setup).
*   İstemci gizliliği: bkz: adım 2'de [kurulum adımları](#steps-for-setup).
*   Media Services REST API uç noktası şu biçimde hesap:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    Bu karşı tüm hangi Media Services REST API çağrıları, uygulamanızda yapılan uç noktadır. Örneğin, https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Daha sonra kodunuzda kullanmak için web.config veya app.config dosyasını, bu beş parametreleri koyabilirsiniz.

## <a name="sample-code"></a>Örnek kod

Örnek kodda bulabilirsiniz [Azure Media Services erişimi için Azure AD kimlik doğrulamasını: REST API aracılığıyla her ikisi de](https://github.com/willzhan/WAMSRESTSoln).

Örnek kod iki bölümden oluşur:

*   Azure AD kimlik doğrulama ve yetkilendirme için tüm REST API koduna sahip bir DLL Kitaplığı projesi. Ayrıca, erişim belirteci ile Media Services REST API uç REST API çağrıları yapma için bir yöntem vardır.
*   Azure AD kimlik doğrulaması başlatır ve farklı Media Services REST API çağrıları bir konsol test istemcisi.

Örnek Proje üç özelliklere sahiptir:

*   İstemci kimlik bilgileri aracılığıyla Azure AD kimlik doğrulamaları yalnızca REST API kullanarak verirsiniz.
*   Yalnızca REST API kullanarak azure Media Services erişimi.
*   Yalnızca REST (REST API kullanarak bir Media Services hesabı oluşturmak için kullanılan gibi) API kullanarak azure depolama erişim.


## <a name="where-is-the-refresh-token"></a>Yenileme belirteci nerede?

Bazı okuyucular isteyebilir: yenileme belirtecini nerede? Neden bir yenileme belirteci burada kullanabilir?

Bir yenileme belirteci amacı, bir erişim belirteci yenileme değil olmaktır. Son kullanıcı kimlik doğrulamasını atlamak ve bir önceki belirtecinin süresi dolduğunda hala geçerli erişim belirteci almak için tasarlanmıştır. Bir yenileme belirteci için daha iyi bir adı "kullanıcı yeniden-oturumu açma belirteci atlama."gibi bir şey olabilir

Yetki akış (kullanıcı adı ve parola, bir kullanıcı adına hareket) OAuth 2.0 kullanıyorsanız, yenileme belirteci, kullanıcı müdahalesi gerektirmeden yenilenmiş bir erişim belirteci almak yardımcı olur. Ancak, bu makalede açıklanan akış istemci kimlik bilgileri vermenizi OAuth 2.0 için istemci kendi adına hareket eder. Hiçbir kullanıcı etkileşimi gerekmez ve bir yenileme belirteci vermek yetkilendirme sunucusu gerekmez. Hata ayıklama, **GetUrlEncodedJWT** yöntemi, fark belirteç uç noktası yanıttan bir erişim belirteci ancak herhangi bir yenileme belirteci sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-dotnet-upload-files.md).
