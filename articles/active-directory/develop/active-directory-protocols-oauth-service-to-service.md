---
title: OAuth2.0 kullanarak azure AD hizmet Auth | Microsoft Docs
description: "Bu makalede HTTP iletileri OAuth2.0 istemci kimlik bilgileri verin akışı kullanarak hizmet kimlik doğrulaması uygulamak için nasıl kullanılacağını açıklar."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: cc30a54cd56c0cb03a67f86e4552398baa764e58
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# İstemci kimlik bilgilerini (paylaşılan gizliliği veya sertifika) kullanarak hizmeti çağrıları hizmetine
Bir web hizmeti OAuth 2.0 istemci kimlik bilgileri verin akış verir (*gizli istemci*) başka bir web hizmeti çağrılırken kimliğini doğrulamak için bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmak için. Bu senaryoda istemci genellikle bir orta katman web hizmeti, arka plan programı hizmeti veya web sitesi olur. Daha yüksek bir güvence düzeyi için Azure AD kimlik bilgisi olarak (yerine bir paylaşılan gizlilik) bir sertifika kullanmak arama hizmeti de sağlar.

## Akış Diyagramı istemci kimlik bilgileri verin
Aşağıdaki diyagramda, nasıl akış works Azure Active Directory'de (Azure AD) istemci kimlik bilgileri vermenizi açıklanmaktadır.

![OAuth2.0 istemci kimlik bilgileri verin akışı](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. İstemci uygulaması, Azure AD belirteci verme uç noktasına kimliğini doğrular ve bir erişim belirteci ister.
2. Azure AD belirteci verme endpoint erişim belirteci yayımlar.
3. Erişim belirteci güvenli kaynak kimliğini doğrulamak için kullanılır.
4. Güvenli kaynak verilerini web uygulamasına döndürülür.

## Azure AD'de Hizmetleri Kaydet
Arama hizmeti ve alıcı hizmeti Azure Active Directory (Azure AD) kaydedin. Ayrıntılı yönergeler için bkz: [uygulamaları Azure Active Directory ile tümleştirme](active-directory-integrating-applications.md).

## İstek bir erişim belirteci
Bir erişim belirteci istemek için kiracıya özgü bir HTTP POST kullanan Azure AD uç noktası.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## Hizmetten hizmete erişim belirteci isteği
İstemci uygulaması olup bir paylaşılan gizlilik ya da bir sertifika tarafından güvenli hale seçti bağlı olarak iki durumlar vardır.

### İlk durumda: bir paylaşılan gizlilik ile erişim belirteci isteği
Paylaşılan gizlilik kullanırken, hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli |İstenen verme türünü belirtir. Bir istemci kimlik bilgileri sağlama akışında değer olmalıdır **client_credentials**. |
| client_id |Gerekli |Çağrıyı yapan web hizmeti Azure AD istemci kimliğini belirtir. Çağıran uygulamanın istemci Kimliğini bulmak için [Azure portal](https://portal.azure.com), tıklatın **Active Directory**, anahtar dizini, uygulamayı tıklatın. Client_id olan *uygulama kimliği* |
| client_secret |Gerekli |Çağıran web hizmeti veya arka plan programı uygulaması için Azure AD'de kayıtlı bir anahtar girin. Azure portalında bir anahtar oluşturmak için tıklatın **Active Directory**, dizin anahtarı, uygulama'yı tıklatın, tıklatın **ayarları**, tıklatın **anahtarları**, ve bir anahtar ekleyin.|
| Kaynak |Gerekli |Alıcı web hizmeti uygulama kimliği URI'sini girin. Uygulama Kimliği URI'sini Azure Portalı'nda bulmak için tıklatın **Active Directory**anahtar dizini, hizmet uygulaması'nı tıklatın ve ardından **ayarları** ve **özellikleri** |

#### Örnek
Bir erişim belirteci https://service.contoso.com/ web hizmeti için aşağıdaki HTTP POST istekleri. `client_id` Erişim belirteci istekleri web hizmeti tanımlar.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### İkinci durumda: bir sertifika ile erişim belirteci isteği
Hizmetten hizmete erişim belirteci isteği bir sertifika ile aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |Gerekli |İstenen yanıt türünü belirtir. Bir istemci kimlik bilgileri sağlama akışında değer olmalıdır **client_credentials**. |
| client_id |Gerekli |Çağrıyı yapan web hizmeti Azure AD istemci kimliğini belirtir. Çağıran uygulamanın istemci Kimliğini bulmak için [Azure portal](https://portal.azure.com), tıklatın **Active Directory**, anahtar dizini, uygulamayı tıklatın. Client_id olan *uygulama kimliği* |
| client_assertion_type |Gerekli |Değer olmalıdır`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Gerekli | Oluşturma ve sertifika ile imzalamak için gereken bir onaylama işlemi (bir JSON Web belirteci) uygulamanız için kimlik bilgileri olarak kayıtlı. Hakkında bilgi edinin [sertifika kimlik bilgileri](active-directory-certificate-credentials.md) sertifikanızı ve onaylama biçimi kaydetme hakkında bilgi edinmek için.|
| Kaynak | Gerekli |Alıcı web hizmeti uygulama kimliği URI'sini girin. Uygulama Kimliği URI'sini Azure Portalı'nda bulmak için tıklatın **Active Directory**dizini tıklatın, uygulama'yı tıklatın ve ardından **yapılandırma**. |

Client_secret parametresi tarafından iki parametre değiştirilir dışında parametreler neredeyse aynı paylaşılan gizliliği isteğiyle durumunda olduğu gibi olduğuna dikkat edin: client_assertion_type ve client_assertion.

#### Örnek
Bir sertifika ile https://service.contoso.com/ web hizmeti için bir erişim belirteci aşağıdaki HTTP POST isteği gönderir. `client_id` Erişim belirteci istekleri web hizmeti tanımlar.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### Hizmetten hizmete erişim belirteci yanıtı

Aşağıdaki parametrelerle bir JSON OAuth 2.0 yanıt başarılı yanıtı içerir:

| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim belirteci. Çağrıyı yapan web hizmeti alıcı web hizmeti için kimlik doğrulaması için bu belirteci kullanabilirsiniz. |
| token_type |Belirteç türü değeri gösterir. Azure AD destekler yalnızca türü **taşıyıcı**. Taşıyıcı belirteçlerini hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteci kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil. |
| expires_on |Erişim belirtecinin süresi dolduğunda süre. Tarih 1970'ten saniyeyi temsil edilir-01-01T0:0:0Z UTC sona erme zamanı kadar. Bu değer, önbelleğe alınan belirteç ömrü belirlemek için kullanılır. |
| not_before |Erişim belirteci kullanılabilir hale süre. Tarih 1970'ten saniyeyi temsil edilir-01-01T0:0:0Z UTC geçerlilik süresini belirteç için kadar.|
| Kaynak |Alıcı web hizmeti uygulama kimliği URI'si. |

#### Yanıt örneği
Aşağıdaki örnek, bir başarı isteğine yanıt olarak bir web hizmeti için bir erişim belirteci için gösterir.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## Ayrıca bkz.
* [Azure AD'de OAuth 2.0](active-directory-protocols-oauth-code.md)
* [Örnek C# bir paylaşılan gizlilik ile hizmet aramasının](https://github.com/Azure-Samples/active-directory-dotnet-daemon) ve [örnek C# bir sertifika ile hizmet çağrısı](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
