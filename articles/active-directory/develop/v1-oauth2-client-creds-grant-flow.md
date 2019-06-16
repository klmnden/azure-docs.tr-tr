---
title: OAuth2.0 kullanarak azure AD hizmet kimlik doğrulaması | Microsoft Docs
description: Bu makalede, HTTP iletileri OAuth2.0 istemci kimlik bilgileri verme akışı kullanarak hizmet kimlik doğrulaması uygulamak için kullanmayı açıklar.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: ryanwi
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d734db7fbedaf3e3f3cd71c31f9391a2237f5b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545245"
---
# <a name="service-to-service-calls-using-client-credentials-shared-secret-or-certificate"></a>Hizmetten hizmete çağrılar (paylaşılan gizli diziyi veya sertifika) istemci kimlik bilgileri kullanma

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

OAuth 2.0 istemci kimlik bilgileri yetki akışı bir web hizmeti izin verir (*gizli istemci*) başka bir web hizmetini çağırırken bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmak için. Bu senaryoda istemci genellikle bir orta katman web hizmeti, arka plan programı hizmeti veya web sitesi olur. Daha yüksek bir güvence düzeyi için Azure AD (yerine, paylaşılan gizlilik) bir sertifika bir kimlik bilgisi olarak kullanılacak arama hizmeti de olanak tanır.

## <a name="client-credentials-grant-flow-diagram"></a>İstemci kimlik bilgileri verme akışı diyagramı
Aşağıdaki diyagramda, istemci kimlik bilgileri akışı çalışır, Azure Active Directory'de (Azure AD) nasıl vermek açıklar.

![OAuth2.0 istemci kimlik bilgileri yetki akışı](./media/v1-oauth2-client-creds-grant-flow/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. İstemci uygulaması, Azure AD belirteç yayınında uç noktaya doğrular ve bir erişim belirteci ister.
2. Azure AD belirteç yayınında uç noktası, erişim belirteci verir.
3. Erişim belirteci güvenli bir kaynak için kimlik doğrulaması için kullanılır.
4. Güvenli kaynaktan veri istemci uygulamaya döndürülür.

## <a name="register-the-services-in-azure-ad"></a>Hizmetleri, Azure AD'ye kaydetme
Arama hizmeti hem alma hizmeti, Azure Active Directory (Azure AD) kaydedin. Ayrıntılı yönergeler için bkz. [uygulamaları Azure Active Directory ile tümleştirme](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="request-an-access-token"></a>İstek bir erişim belirteci
Bir erişim belirteci istemek için kiracıya özgü bir HTTP POST kullanan Azure AD uç noktası.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Hizmetten hizmete erişim belirteci isteği
İstemci uygulaması paylaşılan bir gizli dizi veya bir sertifika tarafından güvenli hale seçti olup olmadığına bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk Durum: Paylaşılan gizlilik ile erişim belirteci isteği
Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli |İstenen izin verme türü belirtir. İstemci kimlik bilgileri verme akışı bir değer olmalıdır **client_credentials**. |
| client_id |Gerekli |Çağıran web hizmeti Azure AD istemci kimliğini belirtir. Çağıran uygulamanın istemci Kimliğini bulmak için [Azure portalında](https://portal.azure.com), tıklayın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamaya tıklayın. Client_id olduğu *uygulama kimliği* |
| client_secret |Gerekli |Çağıran web hizmet veya yordam uygulamanın Azure AD'de kayıtlı bir anahtar girin. Azure portalında bir anahtar oluşturmak için tıklayın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamaya tıklayın, tıklayın **ayarları**, tıklayın **anahtarları** , ve bir anahtar ekleyin.  URL olarak kodlamak, sağlarken bu gizli dizi. |
| resource |Gerekli |Alıcı web hizmeti uygulama kimliği URI'si girin. Uygulama Kimliği URI'si Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklayın **uygulama kayıtları**hizmet uygulaması'nı tıklatın ve ardından **ayarları** ve  **Özellikleri**. |

#### <a name="example"></a>Örnek
Aşağıdaki HTTP POST isteklerini bir [erişim belirteci](access-tokens.md) için https://service.contoso.com/ web hizmeti. `client_id` Erişim belirteci ister web hizmeti tanımlar.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Bir sertifika ile erişim belirteci isteği
Bir sertifika ile hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli |İstenen yanıt türü belirtir. İstemci kimlik bilgileri verme akışı bir değer olmalıdır **client_credentials**. |
| client_id |Gerekli |Çağıran web hizmeti Azure AD istemci kimliğini belirtir. Çağıran uygulamanın istemci Kimliğini bulmak için [Azure portalında](https://portal.azure.com), tıklayın **Azure Active Directory**, tıklayın **uygulama kayıtları**, uygulamaya tıklayın. Client_id olduğu *uygulama kimliği* |
| client_assertion_type |Gerekli |Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Gerekli | Onaylama (bir JSON Web belirteci) oluşturmak ve sertifika ile imzalamak için gereken kimlik bilgileri olarak uygulamanız için kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanız ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| resource | Gerekli |Alıcı web hizmeti uygulama kimliği URI'si girin. Uygulama Kimliği URI'si Azure Portalı'nda bulmak için tıklatın **Azure Active Directory**, tıklayın **uygulama kayıtları**hizmet uygulaması'nı tıklatın ve ardından **ayarları** ve  **Özellikleri**. |

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizli diziyi isteğiyle olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

#### <a name="example"></a>Örnek
Aşağıdaki HTTP POST istekleri için bir erişim belirteci https://service.contoso.com/ web hizmeti bir sertifikayla. `client_id` Erişim belirteci ister web hizmeti tanımlar.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### <a name="service-to-service-access-token-response"></a>Hizmetten hizmete erişim belirteci yanıtı

Başarılı yanıt, aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıtındaki içerir:

| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim belirteci. Çağıran web hizmeti, alıcı web hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekleyen tek tür **taşıyıcı**. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: Taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| expires_on |Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır. |
| not_before |Erişim belirteci kullanılabilir duruma süre. Tarih 1970'ten saniye sayısı temsil edilen-01-01T0:0:0Z UTC belirtecin geçerlilik süresini kadar.|
| resource |Alıcı web hizmeti uygulama kimliği URI'si. |

#### <a name="example-of-response"></a>Örnek yanıt
Aşağıdaki örnek, bir web hizmeti için bir erişim belirteci isteği için başarılı yanıtı gösterir.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD'nin OAuth 2.0](v1-protocols-oauth-code.md)
* [Örnek C# paylaşılan gizlilik ile hizmetten hizmete çağrı](https://github.com/Azure-Samples/active-directory-dotnet-daemon) ve [örnek C# bir sertifika ile hizmetten hizmete çağrı](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
