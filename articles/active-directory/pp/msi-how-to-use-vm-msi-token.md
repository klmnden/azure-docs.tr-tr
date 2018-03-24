---
title: Bir kullanıcı tarafından atanan yönetilen hizmet kimliği bir VM'de bir erişim belirteci almak için nasıl kullanılacağını.
description: Adım adım yönergeler ve bir OAuth almak için bir Azure VM gelen bir kullanıcı tarafından atanan MSI kullanımına ilişkin örnekler belirteci erişin.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 6c6422bc2b13c0c40e48dabf0470c821b13e7851
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="acquire-an-access-token-for-a-vm-user-assigned-managed-service-identity-msi"></a>Yönetilen hizmet kimliği (MSI) kullanıcı tarafından atanan bir VM için bir erişim belirteci alma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]
Bu makalede, belirteç edinme yanı sıra işleme belirteci süre sonu ve HTTP hataları gibi önemli konular hakkında yönergeler için çeşitli kod ve komut dosyası örnekler verilmektedir.

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]
Bu makalede Azure PowerShell örneklerini kullanmayı planlıyorsanız, en son sürümünü yüklediğinizden emin olun [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!IMPORTANT]
> - Tüm örnek kodu/komut dosyası bu makalede varsayar istemci MSI özellikli sanal makine üzerinde çalışan. VM "Bağlan" özelliği Azure portalında uzaktan VM'nize bağlanmak için kullanın. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak bir VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md), veya değişken makaleleri (Portal, PowerShell, şablon veya bir Azure SDK'sını kullanarak). 

## <a name="overview"></a>Genel Bakış

Bir istemci uygulaması bir MSI isteyebilir [yalnızca uygulama erişim belirteci](~/articles/active-directory/develop/active-directory-dev-glossary.md#access-token) belirli bir kaynağa erişmek için. Belirteç [MSI hizmet sorumlusu göre](msi-overview.md#how-does-it-work). Bu nedenle, kendi hizmet sorumlusu altında bir erişim belirteci edinmek için istemcinin kendisini kaydetmek gerek yoktur. Belirtecin bir taşıyıcı belirteci olarak kullanmaya uygun [service-to-service çağrıları gerektiren istemci kimlik bilgileri](~/articles/active-directory/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [HTTP kullanarak belirteç alın](#get-a-token-using-http) | MSI belirteç uç noktası için protokol ayrıntıları |
| [CURL kullanarak belirteç alın](#get-a-token-using-curl) | MSI REST uç noktası geçirmesi/CURL istemciden kullanma örneği |
| [Belirteç süre sonu işleme](#handling-token-expiration) | Süresi dolan erişim belirteçleri işlemek için kılavuz |
| [Hata işleme](#error-handling) | MSI belirteç uç noktadan döndürülen HTTP hataları işlemek için kılavuz |
| [Kısıtlama Kılavuzu](#throttling-guidance) | MSI belirteç uç noktası azaltma işlemek için kılavuz |
| [Azure Hizmetleri için kaynak kimlikleri](#resource-ids-for-azure-services) | Kaynak kimlikleri için desteklenen Azure Hizmetleri alınacağı konumu |


## <a name="get-a-token-using-http"></a>HTTP kullanarak belirteç alın 

Bir erişim belirteci alma için temel arabirim REST, HTTP REST çağrılarını yapabilen VM'de çalışan herhangi bir istemci uygulama için erişilebilir hale getirme temel alır. Sanal makineye bir uç nokta istemcinin kullandığı dışında bu Azure AD programlama modeline benzer (vs Azure AD uç noktası).

Örnek meta veri hizmeti (IMDS) uç noktası kullanarak örnek isteği:

```
GET http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1 Metadata: true
```

MSI VM uzantısı uç noktası (yaklaşan kullanımdan) kullanarak örnek isteği:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1 Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktasından verileri almak istediğiniz belirten HTTP fiili. Bu durumda, bir OAuth belirteç erişin. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | MSI hizmeti için uç noktaya örneği meta verileri. |
| `http://localhost:50342/oauth2/token` | Burada 50342 varsayılan bağlantı noktası ve yapılandırılabilir VM uzantısı için MSI bitiş noktası. |
| `api-version`  | IMDS uç noktası için API sürümünü belirten bir sorgu dizesi parametresi.  |
| `resource` | Hedef kaynak uygulama kimliği URI'sini belirten bir sorgu dizesi parametresi. Ayrıca görünür `aud` verilen belirteç talep (İzleyici). Bu örnek Azure Kaynak Yöneticisi'ne erişmek için bir belirteç isteklerini sahip olduğu bir uygulama kimliği URI'sini https://management.azure.com/. |
| `client_id` |  Bir *isteğe bağlı* sorgu dizesi parametresi, istemci kimliği (uygulama kimliği olarak da bilinir) gösteren bir kullanıcı tarafından atanan MSI temsil eden hizmet sorumlusu. Sistem tarafından atanan MSI kullanıyorsanız, bu parametre gerekli değildir. Bu değer döndürülür `clientId` kullanıcı tarafından atanan bir MSI oluşturulması sırasında özelliği. Bu örnek bir belirteç istemci kimliği "712eac09-e943-418 c-9be6-9fd5c91078bl" ister. |
| `Metadata` | Sunucu tarafı istek sahtekarlığı (SSRF) saldırılara karşı azaltma olarak MSI tarafından gerekli bir HTTP isteği üstbilgisi alanının. Bu değer "tüm alt durumda true olarak" olarak ayarlanmalıdır.

Örnek yanıt:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
  "client_id":"712eac09-e943-418c-9be6-9fd5c91078bl"
}
```

| Öğe | Açıklama |
| ------- | ----------- |
| `access_token` | İstenen erişim belirteci. Belirteç katıştırılmış olduğu güvenli bir REST API'si çağrılırken `Authorization` isteği üstbilgisi alanının arayan kimliğini doğrulamak API izin verme "bearer" Token olarak. | 
| `expires_in` | Erişim belirteci, verme zamandan süresi dolmadan önce geçerli olmaya devam saniye sayısı. Verme süresini belirtecin içinde bulunabilir `iat` talep. |
| `expires_on` | Erişim belirtecinin süresi dolduğunda timespan. Tarih saniyeden sayısı olarak temsil edilir "1970'ten-01-01T0:0:0Z UTC" (belirtecin karşılık gelen `exp` talep). |
| `not_before` | Erişim belirteci etkili olur ve kabul edilebilir timespan. Tarih saniyeden sayısı olarak temsil edilir "1970'ten-01-01T0:0:0Z UTC" (belirtecin karşılık gelen `nbf` talep). |
| `resource` | Erişim belirteci için hangi eşleşen istenen kaynak `resource` sorgu dizesi parametresi istek. |
| `token_type` | Kaynak bu belirteç taşıyıcı için erişmenizi anlamına gelir "Bearer" erişim belirteci Belirtecin türü. |
| `client_id` | Belirteç istendi kullanıcı tarafından atanan MSI temsil eden hizmet sorumlusu istemci kimliği (uygulama kimliği olarak da bilinir). |

## <a name="get-a-token-using-curl"></a>CURL kullanarak belirteç alın

İstemci Kimliğini (uygulama kimliği olarak da bilinir), kullanıcı tarafından atanan MSI hizmet sorumlusu için değiştirdiğinizden emin olun <MSI CLIENT ID> değerini `client_id` parametresi. Bu değer döndürülür `clientId` kullanıcı tarafından atanan bir MSI oluşturulması sırasında özelliği.
  
Örnek meta veri hizmeti (IMDS) uç noktası kullanarak örnek isteği:

   ```bash
   response=$(curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>")
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```
   
MSI VM uzantısı uç noktası (yaklaşan kullanımdan) kullanarak örnek isteği:

   ```bash
   response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=<MSI CLIENT ID>" -H Metadata:true -s)
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```

   Örnek yanıtları:

   ```bash
   user@vmLinux:~$ response=$(curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=9d484c98-b99d-420e-939c-z585174b63bl")
   user@vmLinux:~$ access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   user@vmLinux:~$ echo The MSI access token is $access_token
   The MSI access token is eyJ0eXAiOiJKV1QiLCJhbGciO...
   ```

## <a name="handling-token-expiration"></a>Belirteç süre sonu işleme

MSI alt sistemi belirteçleri önbelleğe alır. Bu nedenle, istediğiniz ve yalnızca Azure AD üzerinde hat çağrısına sonuçları kadar çağırabilirsiniz:
- önbellek isabetsizliği önbelleğinde herhangi bir belirteci nedeniyle oluşur
- belirtecin süresi doldu

Kodunuzda belirteç önbelleğe alma, burada belirtecinin süresi doldu kaynağı gösterir senaryoları işlemek için hazırlıklı olmalıdır.

## <a name="error-handling"></a>Hata işleme 

MSI uç noktası HTTP yanıt iletisi üst bilgisi, durum kodu alanı aracılığıyla hataları 4xx veya 5xx hataları olarak işaret eder:

| Durum Kodu | Hata açıklaması | Nasıl yapılacağını |
| ----------- | ------------ | ------------- |
| İstek 4xx hatası. | Bir veya daha fazla istek parametreleri yanlış. | Yeniden denemeyin.  Daha fazla bilgi için hata ayrıntılarını inceleyin.  Tasarım zamanı hataları 4xx hatalardır.|
| Hizmet geçici hatadan 5XX. | MSI alt sistemi veya Azure Active Directory geçici bir hata döndürdü. | En az 1 saniye bekledikten sonra yeniden denemek güvenlidir.  Çok hızlı bir şekilde veya çok sık yeniden deneyin, Azure AD hızı sınırı hatası (429) döndürebilir.|

Bir hata oluşursa, karşılık gelen HTTP yanıt gövdesi JSON ile hata ayrıntıları içerir:

| Öğe | Açıklama |
| ------- | ----------- |
| error   | Hata tanımlayıcı. |
| error_description | Hatanın ayrıntılı açıklamasını. **Hata açıklaması, dilediğiniz zaman değiştirebilirsiniz. Dalları hata açıklamasında değerlere göre kod yazma.**|

### <a name="http-response-reference"></a>HTTP yanıt başvurusu

Bu bölümde, olası hata yanıtları belgeler. A "200 Tamam" durumu başarılı bir yanıt ve erişim belirteci yanıt gövdesinde JSON, access_token öğesi bulunur.

| Durum kodu | Hata | Hata Açıklaması | Çözüm |
| ----------- | ----- | ----------------- | -------- |
| 400 Hatalı istek | invalid_resource | AADSTS50001: uygulama adlı *\<URI\>* adlı Kiracı bulunamadı  *\<KİRACI kimliği\>*. Uygulama değil Kiracı Yöneticisi tarafından yüklendikten veya Kiracı herhangi bir kullanıcı tarafından izin verdiği gerçekleşebilir. Yanlış Kiracı kimlik doğrulama isteği gönderilen. \ | (Yalnızca Linux) |
| 400 Hatalı istek | bad_request_102 | Gerekli meta veriler üstbilgisi belirtilmedi | Her iki `Metadata` isteği üstbilgisi alanının isteğinizden eksik veya yanlış biçimlendirilmiş. Değer olarak belirtilmelidir `true`, tüm alt durumda. "Örnek istek" bölümüne bakın [HTTP kullanarak bir belirteç almak](#get-a-token-using-http) bir örnek bölüm.|
| 401 Yetkisiz | unknown_source | Bilinmeyen kaynak  *\<URI\>* | HTTP GET isteği URI doğru biçimlendirildiğinden emin olun. `scheme:host/resource-path` Bölümü olarak belirtilmelidir `http://169.254.169.254/metadata/identity/oath2/token` veya `http://localhost:50342/oauth2/token`. "Örnek istek" bölümüne bakın [HTTP kullanarak bir belirteç almak](#get-a-token-using-http) bir örnek bölüm.|
|           | invalid_request | İstek gerekli bir parametre eksik, geçersiz bir parametre değeri içerir, bir parametresi birden çok kez içerir veya aksi halde bozuk. |  |
|           | unauthorized_client | İstemci bu yöntemi kullanarak bir erişim belirteci istemek için yetkili değil. | Yerel bir geri döngü uzantısı çağırmak için kullanmadı bir istek tarafından ya da doğru şekilde yapılandırılmış bir MSI sahip olmayan bir VM'de neden oldu. Bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md) VM yapılandırması yardıma ihtiyacınız varsa. |
|           | access_denied | Kaynak sahibi veya yetkilendirme sunucusu isteğini reddetti. |  |
|           | unsupported_response_type | Yetkilendirme sunucusu bu yöntemi kullanarak bir erişim belirteci alma desteklemez. |  |
|           | invalid_scope | İstenen kapsamı geçersiz, bilinmeyen veya hatalı biçimlendirilmiş olabilir. |  |
| 500 İç sunucu hatası | bilinmiyor | Active Directory'den belirteci alınamadı. Ayrıntılar için günlükleri Bkz  *\<dosya yolu\>* | MSI VM etkin olduğunu doğrulayın. Bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md) VM yapılandırması yardıma ihtiyacınız varsa.<br><br>Ayrıca, HTTP GET isteği URI özellikle URI sorgu dizesinde belirtilen kaynak doğru biçimlendirildiğinden emin olun. "Örnek istek" bölümüne bakın [HTTP kullanarak bir belirteç almak](#get-a-token-using-http) bölüm bir örnek veya [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) Hizmetleri ve bunların ilgili kaynak kimlikleri listesi.

## <a name="throttling-guidance"></a>Kısıtlama Kılavuzu 

Azaltma sınırları MSI IMDS uç noktasına yapılan çağrı sayısı için geçerlidir. Azaltma eşiği aşıldığında, kısıtlama etkinken MSI IMDS uç noktası başka bir istek sınırlar. Bu süre boyunca MSI IMDS uç noktası HTTP durum kodu 429 döndürür ("çok sayıda istek"), ve istekleri başarısız olur. 

Yeniden deneme için aşağıdaki stratejisi öneririz: 

| **Yeniden deneme stratejisi** | **Ayarlar** | **Değerler** | **Nasıl çalışır?** |
| --- | --- | --- | --- |
|ExponentialBackoff |Yeniden deneme sayısı<br />En düşük geri alma<br />En yüksek geri alma<br />Delta geri alma<br />İlk hızlı yeniden deneme |5<br />0 sn<br />60 sn<br />2 sn<br />false |Deneme 1 - 0 sn gecikme<br />Deneme 2 - yaklaşık 2 sn gecikme<br />Deneme 3 - yaklaşık 6 sn gecikme<br />Deneme 4 - yaklaşık 14 sn gecikme<br />Deneme 5 - yaklaşık 30 sn gecikme |

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) Azure AD destekleyen ve MSI ile test kaynakları ve bunların ilgili kaynak kimlikleri listesi.


## <a name="next-steps"></a>Sonraki adımlar

- Azure VM'de MSI etkinleştirmek için bkz: [Azure CLI kullanarak bir VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.








