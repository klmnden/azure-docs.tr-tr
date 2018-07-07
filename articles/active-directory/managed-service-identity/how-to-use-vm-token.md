---
title: Bir erişim belirteci almak için bir Azure VM yönetilen hizmet Kimliği'ni kullanma
description: Adım adım yönergeler ve örnekler bir OAuth almak için bir Azure VM MSI kullanma belirteci erişin.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: e564f48b4b90cfcaa72ed51d5f210a71a4980360
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902954"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Belirteç edinme için bir Azure VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, işleme belirteci süre sonu ve HTTP hataları gibi önemli konularda rehberlik yanı sıra, belirteç edinme için çeşitli kod ve betik örnekleri sağlar. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell örnekleri kullanmayı planlıyorsanız, en son sürümünü yüklediğinizden emin olun [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).


> [!IMPORTANT]
> - Tüm bu makaledeki örnek kodun/betik varsayar istemci, bir yönetilen hizmet kimliği ile bir sanal makinede çalışıyor. VM "Bağlan" özelliği Azure Portalı'nda uzaktan VM'nize bağlanmak için kullanın. Bir VM'deki MSI etkinleştirme hakkında daha fazla bilgi için bkz [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, bir şablon veya bir Azure SDK'sını kullanarak). 

> [!IMPORTANT]
> - Bir yönetilen hizmet kimliği, güvenlik sınırı olduğundan kaynak üzerinde kullanılıyor. Bir sanal makine üzerinde çalışan tüm kod/betikler, istek ve tüm yönetilen hizmet kimliği üzerinde kullanılabilir belirteçlerini almak. 

## <a name="overview"></a>Genel Bakış

Bir istemci uygulama bir yönetilen hizmet kimliği isteyebilir [salt uygulama erişim belirteci](../develop/active-directory-dev-glossary.md#access-token) belirli bir kaynağa erişmek için. Belirteç [MSI hizmet sorumlusu tabanlı](overview.md#how-does-it-work). Bu nedenle, kendi hizmet sorumlusu altında bir erişim belirteci almak için istemcinin kendisini kaydetmek gerek yoktur. Taşıyıcı belirteç olarak kullanmaya uygun bir belirteçtir [-hizmet çağrıları gerektiren bir istemci kimlik bilgileri](../develop/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [HTTP kullanarak bir belirteç Al](#get-a-token-using-http) | MSI belirteç uç noktası için protokol ayrıntıları |
| [C# kullanarak bir belirteç Al](#get-a-token-using-c) | Bir C# istemciden MSI REST uç noktasını kullanma örneği |
| [Go kullanarak bir belirteç Al](#get-a-token-using-go) | MSI REST uç noktasından bir Git istemcisi kullanma örneği |
| [Azure PowerShell kullanarak bir belirteç Al](#get-a-token-using-azure-powershell) | MSI REST uç noktasından bir PowerShell istemcisi kullanma örneği |
| [CURL kullanarak bir belirteç Al](#get-a-token-using-curl) | MSI REST uç noktasından bir Bash/CURL istemci kullanma örneği |
| [Belirteç önbelleğe işleme](#handling-token-caching) | Süresi dolan erişim belirteçleri işlemek için yönergeler |
| [Hata işleme](#error-handling) | MSI belirteç uç noktadan döndürülen HTTP hata işleme yönergeleri |
| [Azure Hizmetleri için kaynak kimlikleri](#resource-ids-for-azure-services) | Desteklenen Azure Hizmetleri için nereden kaynak kimlikleri |

## <a name="get-a-token-using-http"></a>HTTP kullanarak bir belirteç Al 

Bir erişim belirteci almak için temel arabirimi, HTTP REST çağrılarını yapabilir VM'de çalıştırılan tüm istemci uygulamaları için erişilebilir hale getirme REST'i temel alır. İstemci sanal makineye bir uç nokta kullanır ancak bu Azure AD programlama modeline benzer (vs bir Azure AD uç noktası).

Azure örnek meta veri hizmeti (IMDS) uç noktayı kullanarak örnek istek *(önerilen)*:

```
GET 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' HTTP/1.1 Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktasından veri almak istediğiniz gösteren HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | Örnek meta veri hizmeti için MSI bitiş noktası. |
| `api-version`  | IMDS uç noktası için API sürümü belirten bir sorgu dizesi parametresi. Lütfen API sürümünü kullanın `2018-02-01` veya büyük. |
| `resource` | Hedef kaynağın uygulama kimliği URİ'sini belirten bir sorgu dizesi parametresi. Ayrıca şurada görünür `aud` verilen belirtecin (dinleyici) talep. Bu örnekte Azure Resource Manager'a erişmek için bir belirteç isteyen bir uygulama kimliği URI'si, sahip olduğu https://management.azure.com/. |
| `Metadata` | Sunucu tarafı istek sahteciliği (SSRF) saldırılara karşı bir risk azaltma olarak MSI tarafından gereken bir HTTP isteği üstbilgisi alanının. Bu değer true", tamamen küçük için" olarak ayarlanmalıdır.

Yönetilen hizmet kimliği (MSI) VM uzantısı uç noktasını kullanarak örnek istek *(kullanım dışı için)*:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktasından veri almak istediğiniz gösteren HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://localhost:50342/oauth2/token` | Burada 50342 varsayılan bağlantı noktası ve yapılandırılabilir MSI endpoint. |
| `resource` | Hedef kaynağın uygulama kimliği URİ'sini belirten bir sorgu dizesi parametresi. Ayrıca şurada görünür `aud` verilen belirtecin (dinleyici) talep. Bu örnekte Azure Resource Manager'a erişmek için bir belirteç isteyen bir uygulama kimliği URI'si, sahip olduğu https://management.azure.com/. |
| `Metadata` | Sunucu tarafı istek sahteciliği (SSRF) saldırılara karşı bir risk azaltma olarak MSI tarafından gereken bir HTTP isteği üstbilgisi alanının. Bu değer true", tamamen küçük için" olarak ayarlanmalıdır.


Örnek yanıt:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Öğe | Açıklama |
| ------- | ----------- |
| `access_token` | İstenen erişim belirteci. Güvenli bir REST API'nin çağrılması durumunda, belirteç katıştırılmış `Authorization` isteği üstbilgisi alanının arayan kimliğini doğrulamak API izin vererek "bearer" Token olarak. | 
| `refresh_token` | MSI tarafından kullanılmaz. |
| `expires_in` | Erişim belirteci, verme zamandan süresi dolmadan önce geçerli olması için devam saniye sayısı. Verme süresini belirtecin içinde bulunabilir `iat` talep. |
| `expires_on` | Erişim belirtecinin süresi dolduğunda TimeSpan değeri. Saniyeyi tarih gösterilir "1970-01-01T0:0:0Z UTC" (belirtecinin karşılık gelen `exp` talep). |
| `not_before` | Erişim belirteci etkinleşir ve kabul edilebilir süre. Saniyeyi tarih gösterilir "1970-01-01T0:0:0Z UTC" (belirtecinin karşılık gelen `nbf` talep). |
| `resource` | Erişim belirtecine hangi eşleşmelerini istendi kaynak `resource` sorgu dizesi parametresi istek. |
| `token_type` | Kaynak bu belirtecin taşıyıcı için erişim verebilirsiniz anlamına gelir "Bearer" erişim belirteci olan Belirtecin türü. |

## <a name="get-a-token-using-c"></a>C# kullanarak bir belirteç Al

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

## <a name="get-a-token-using-go"></a>Go kullanarak bir belirteç Al

```
package main

import (
  "fmt"
  "io/ioutil"
  "net/http"
  "net/url"
  "encoding/json"
)

type responseJson struct {
  AccessToken string `json:"access_token"`
  RefreshToken string `json:"refresh_token"`
  ExpiresIn string `json:"expires_in"`
  ExpiresOn string `json:"expires_on"`
  NotBefore string `json:"not_before"`
  Resource string `json:"resource"`
  TokenType string `json:"token_type"`
}

func main() {
    
    // Create HTTP request for MSI token to access Azure Resource Manager
    var msi_endpoint *url.URL
    msi_endpoint, err := url.Parse("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01")
    if err != nil {
      fmt.Println("Error creating URL: ", err)
      return 
    }
    msi_parameters := url.Values{}
    msi_parameters.Add("resource", "https://management.azure.com/")
    msi_endpoint.RawQuery = msi_parameters.Encode()
    req, err := http.NewRequest("GET", msi_endpoint.String(), nil)
    if err != nil {
      fmt.Println("Error creating HTTP request: ", err)
      return 
    }
    req.Header.Add("Metadata", "true")

    // Call MSI /token endpoint
    client := &http.Client{}
    resp, err := client.Do(req) 
    if err != nil{
      fmt.Println("Error calling token endpoint: ", err)
      return
    }

    // Pull out response body
    responseBytes,err := ioutil.ReadAll(resp.Body)
    defer resp.Body.Close()
    if err != nil {
      fmt.Println("Error reading response body : ", err)
      return
    }

    // Unmarshall response body into struct
    var r responseJson
    err = json.Unmarshal(responseBytes, &r)
    if err != nil {
      fmt.Println("Error unmarshalling the response:", err)
      return
    }

    // Print HTTP response and marshalled response body elements to console
    fmt.Println("Response status:", resp.Status)
    fmt.Println("access_token: ", r.AccessToken)
    fmt.Println("refresh_token: ", r.RefreshToken)
    fmt.Println("expires_in: ", r.ExpiresIn)
    fmt.Println("expires_on: ", r.ExpiresOn)
    fmt.Println("not_before: ", r.NotBefore)
    fmt.Println("resource: ", r.Resource)
    fmt.Println("token_type: ", r.TokenType)
}
```

## <a name="get-a-token-using-azure-powershell"></a>Azure PowerShell kullanarak bir belirteç Al

Aşağıdaki örnek MSI REST uç noktasından bir PowerShell istemcisi kullanmayı gösterir:

1. Erişim belirteci alma.
2. Bir Azure Resource Manager REST API çağrısı ve VM hakkında bilgi almak için erişim belirteci kullanın. Yerine, abonelik kimliği, kaynak grubu adı ve sanal makine adı için mutlaka `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>`, ve `<VM-NAME>`sırasıyla.

```azurepowershell
Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Headers @{Metadata="true"}
```

Erişim belirteci yanıt ayrıştırmayı örneğinde:
```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F `
                              -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>CURL kullanarak bir belirteç Al

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s
```


Erişim belirteci yanıt ayrıştırmayı örneğinde:

```bash
response=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="token-caching"></a>Belirteç önbelleğe alma

Kullanılan yönetilen hizmet kimliği (MSI) alt sistemi (IMDS/MSI VM uzantısı) belirteçleri önbelleğe almaz, ancak ayrıca kodunuzda belirteç önbelleğe uygulamak için öneririz. Sonuç olarak, burada kaynak belirtecinin kullanım süresi doldu gösterir senaryoları için hazırlamanız gerekir. 

Azure AD-hat üzerinde çağrısına neden yalnızca zaman:
- önbellek isabetsizliği MSI alt önbelleğinde herhangi bir belirteci dolayı gerçekleşir
- önbelleğe alınan belirteç süresi doldu

## <a name="error-handling"></a>Hata işleme

Yönetilen hizmet kimliği uç nokta 4xx veya 5xx hata olarak HTTP yanıt iletisi üst bilgi, durum kodu alanına aracılığıyla hataları bildirir:

| Durum Kodu | Hata nedeni | Nasıl yapılacağını |
| ----------- | ------------ | ------------- |
| 404 bulunamadı. | IMDS uç noktası güncelleniyor. | Expontential geri alma ile yeniden deneyin. Rehbere bakın. |
| 429 çok fazla istek. |  IMDS azaltma sınırına ulaşıldı. | Üstel geri alma ile yeniden deneyin. Rehbere bakın. |
| isteğinde 4xx hata oluştu. | Bir veya daha fazla istek parametreleri büyük/küçük harf yanlış. | Yeniden denemeyin.  Daha fazla bilgi için hata ayrıntılarını inceleyin.  4xx, tasarım zamanı hataları hatalardır.|
| 5XX hizmetinden geçici hata. | MSI alt sistemi veya Azure Active Directory geçici bir hata döndürdü. | En az 1 saniye bekledikten sonra yeniden denemek güvenlidir.  Çok hızlı bir şekilde veya çok sık tekrar denerseniz, IMDS ve/veya Azure AD oranı sınırı hatası (429) döndürebilir.|
| timeout | IMDS uç noktası güncelleniyor. | Expontential geri alma ile yeniden deneyin. Rehbere bakın. |

Bir hata oluşursa, karşılık gelen HTTP yanıt gövdesi JSON ile hata ayrıntılarını içerir:

| Öğe | Açıklama |
| ------- | ----------- |
| error   | Hata tanımlayıcı. |
| error_description | Hatanın ayrıntılı bir açıklaması. **Hata açıklamaları dilediğiniz zaman değiştirebilirsiniz. Dalları hata açıklamasını değerlere göre kod yazma.**|

### <a name="http-response-reference"></a>HTTP yanıt başvurusu

Bu bölümde, olası hata yanıtları belgeler. Bir "200 Tamam" durumu başarılı bir yanıt ve access_token öğesi yanıt gövdesinde JSON, erişim belirtecini bulunur.

| Durum kodu | Hata | Hata Açıklaması | Çözüm |
| ----------- | ----- | ----------------- | -------- |
| 400 Hatalı istek | invalid_resource | AADSTS50001: uygulama adlı *\<URI\>* adlı kiracıda bulunamadı  *\<KİRACI-kimliği\>*. Uygulama değil Kiracı Yöneticisi tarafından yüklenmemiş veya kiracıdaki herhangi bir kullanıcı tarafından onay varsa bu durum oluşabilir. Kimlik doğrulaması isteğinizi yanlış kiracıya göndermiş olabilirsiniz. \ | (Yalnızca Linux) |
| 400 Hatalı istek | bad_request_102 | Gerekli meta veriler üst bilgisi belirtilmedi | Her iki `Metadata` isteği üstbilgisi alanının isteğinizden eksik veya hatalı biçimlendirilmiş. Değer olarak belirtilmelidir `true`, tüm alt durumda. "Örnek istek" bölümüne bakın [KALAN bölümü önceki](#rest) örneği.|
| 401 Yetkisiz | unknown_source | Bilinmeyen kaynak  *\<URI'si\>* | HTTP GET isteği URI doğru şekilde biçimlendirildiğini doğrulayın. `scheme:host/resource-path` Bölümü olarak belirtilmelidir `http://localhost:50342/oauth2/token`. "Örnek istek" bölümüne bakın [KALAN bölümü önceki](#rest) örneği.|
|           | invalid_request | İstek gerekli parametre eksik, geçersiz bir parametre değeri içerir, birden çok kez bir parametre içerir veya aksi halde yanlış biçimlendirilmiş. |  |
|           | unauthorized_client | Bu yöntemi kullanarak bir erişim belirteci istemek için istemci yetkili değil. | Uzantı çağırmak için yerel bir geri döngü kullanmadı bir istek veya doğru yapılandırılmış bir MSI sahip olmayan bir VM'de neden oldu. Bkz: [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md) VM yapılandırması ile ilgili yardıma ihtiyacınız varsa. |
|           | access_denied | Kaynak sahibi veya yetkilendirme sunucusu isteği reddetti. |  |
|           | unsupported_response_type | Yetkilendirme sunucusu, bu yöntemi kullanarak bir erişim belirteci alma desteklemez. |  |
|           | invalid_scope | İstenen kapsamı geçersiz, bilinmeyen ya da hatalı biçimlendirilmiş. |  |
| 500 İç sunucu hatası | bilinmiyor | Active Directory'den belirteci alınamadı. Günlüklerde ayrıntıları görmek için  *\<dosya yolu\>* | MSI sanal makinede etkin olduğunu doğrulayın. Bkz: [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md) VM yapılandırması ile ilgili yardıma ihtiyacınız varsa.<br><br>Ayrıca, HTTP GET isteği URI özellikle URI sorgu dizesinde belirtilen kaynak doğru şekilde biçimlendirildiğini doğrulayın. "Örnek istek" bölümüne bakın [KALAN bölümü önceki](#rest) bir örnek veya [Azure Hizmetleri söz konusu destek Azure AD kimlik doğrulamasını](services-support-msi.md) hizmetler ve bunların ilgili kaynak kimlikleri listesi için.

## <a name="retry-guidance"></a>Yeniden deneme Kılavuzu 

404 hatası, 429 ve 5xx hata kodu alırsanız yeniden denemek için önerilen (bkz [hata işleme](#error-handling) yukarıda).

IMDS uç noktaya yapılan çağrıların sayısını azaltma sınırları uygulanır. Azaltma eşiği aşıldığında IMDS uç nokta azaltma etkinken ek istekleri kısıtlar. Bu süre boyunca IMDS uç noktası HTTP durum kodu 429 döndürür ("çok fazla istek"), ve istekleri başarısız olur. 

Yeniden deneme için aşağıdaki stratejisi öneririz: 

| **Yeniden deneme stratejisi** | **Ayarlar** | **Değerler** | **Nasıl çalışır?** |
| --- | --- | --- | --- |
|ExponentialBackoff |Yeniden deneme sayısı<br />En düşük geri alma<br />En yüksek geri alma<br />Delta geri alma<br />İlk hızlı yeniden deneme |5<br />0 sn<br />60 sn<br />2 sn<br />false |Deneme 1 - 0 sn gecikme<br />Deneme 2 - yaklaşık 2 sn gecikme<br />Deneme 3 - yaklaşık 6 sn gecikme<br />Deneme 4 - yaklaşık 14 sn gecikme<br />Deneme 5 - yaklaşık 30 sn gecikme |

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri söz konusu destek Azure AD kimlik doğrulamasını](services-support-msi.md) için MSI ile test edilmiştir ve Azure AD Destek kaynakları ve bunların ilgili kaynak kimlikleri listesi.


## <a name="related-content"></a>İlgili içerik

- Azure VM'deki MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.








