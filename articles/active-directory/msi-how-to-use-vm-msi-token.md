---
title: "Bir erişim belirteci almak için bir Azure VM yönetilen hizmet kimliği kullanma"
description: "Adım adım yönergeler ve bir OAuth almak için bir Azure VM MSI kullanımına ilişkin örnekler belirteci erişin."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 3d9d4d682a25d11129e81855a6bf149ac1d5cff0
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Belirteç alımı için bir Azure VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, belirteç edinme yanı sıra işleme belirteci süre sonu ve HTTP hataları gibi önemli konular hakkında yönergeler için çeşitli kod ve komut dosyası örnekler verilmektedir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell örneklerini kullanmayı planlıyorsanız, en son sürümünü yüklediğinizden emin olun [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).


> [!IMPORTANT]
> - Tüm örnek kodu/komut dosyası bu makalede varsayar istemci MSI özellikli sanal makine üzerinde çalışan. VM "Bağlan" özelliği Azure portalında uzaktan VM'nize bağlanmak için kullanın. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, şablon veya bir Azure SDK'sını kullanarak). 

## <a name="overview"></a>Genel Bakış

Bir istemci uygulaması bir MSI isteyebilir [yalnızca uygulama erişim belirteci](develop/active-directory-dev-glossary.md#access-token) belirli bir kaynağa erişmek için. Belirteç [MSI hizmet sorumlusu göre](msi-overview.md#how-does-it-work). Bu nedenle, kendi hizmet sorumlusu altında bir erişim belirteci edinmek için istemcinin kendisini kaydetmek gerek yoktur. Belirtecin bir taşıyıcı belirteci olarak kullanmaya uygun [service-to-service çağrıları gerektiren istemci kimlik bilgileri](active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [HTTP kullanarak belirteç alın](#get-a-token-using-http) | MSI belirteç uç noktası için protokol ayrıntıları |
| [C# kullanarak belirteç alın](#get-a-token-using-c) | C# istemciden MSI REST uç noktasını kullanma örneği |
| [Git kullanarak belirteç alın](#get-a-token-using-go) | Bir Git istemciden MSI REST uç noktasını kullanma örneği |
| [Azure PowerShell kullanarak belirteç alın](#get-a-token-using-azure-powershell) | Bir PowerShell istemciden MSI REST uç noktasını kullanma örneği |
| [CURL kullanarak belirteç alın](#get-a-token-using-curl) | MSI REST uç noktası geçirmesi/CURL istemciden kullanma örneği |
| [Belirteç süre sonu işleme](#handling-token-expiration) | Süresi dolan erişim belirteçleri işlemek için kılavuz |
| [Hata işleme](#error-handling) | MSI belirteç uç noktadan döndürülen HTTP hataları işlemek için kılavuz |
| [Azure Hizmetleri için kaynak kimlikleri](#resource-ids-for-azure-services) | Kaynak kimlikleri için desteklenen Azure Hizmetleri alınacağı konumu |

## <a name="get-a-token-using-http"></a>HTTP kullanarak belirteç alın 

Bir erişim belirteci alma için temel arabirim REST, HTTP REST çağrılarını yapabilen VM'de çalışan herhangi bir istemci uygulama için erişilebilir hale getirme temel alır. Sanal makinede localhost endpoint istemcinin kullandığı dışında bu Azure AD programlama modeline benzer (vs Azure AD uç noktası).

Örnek istek:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktasından verileri almak istediğiniz belirten HTTP fiili. Bu durumda, bir OAuth belirteç erişin. | 
| `http://localhost:50342/oauth2/token` | Burada 50342 varsayılan bağlantı noktası ve yapılandırılabilir MSI endpoint. |
| `resource` | Hedef kaynak uygulama kimliği URI'sini belirten bir sorgu dizesi parametresi. Ayrıca görünür `aud` verilen belirteç talep (İzleyici). Bu örnek bir uygulama kimliği URI'sini https://management.azure.com/ olan Azure Kaynak Yöneticisi'ne erişmek için bir belirteç ister. |
| `Metadata` | Sunucu tarafı istek sahtekarlığı (SSRF) saldırılara karşı azaltma olarak MSI tarafından gerekli bir HTTP isteği üstbilgisi alanının. Bu değer "tüm alt durumda true olarak" olarak ayarlanmalıdır.

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
| `access_token` | İstenen erişim belirteci. Belirteç katıştırılmış olduğu güvenli bir REST API'si çağrılırken `Authorization` isteği üstbilgisi alanının arayan kimliğini doğrulamak API izin verme "bearer" Token olarak. | 
| `refresh_token` | MSI tarafından kullanılmaz. |
| `expires_in` | Erişim belirteci, verme zamandan süresi dolmadan önce geçerli olmaya devam saniye sayısı. Verme süresini belirtecin içinde bulunabilir `iat` talep. |
| `expires_on` | Erişim belirtecinin süresi dolduğunda timespan. Tarih saniyeden sayısı olarak temsil edilir "1970'ten-01-01T0:0:0Z UTC" (belirtecin karşılık gelen `exp` talep). |
| `not_before` | Erişim belirteci etkili olur ve kabul edilebilir timespan. Tarih saniyeden sayısı olarak temsil edilir "1970'ten-01-01T0:0:0Z UTC" (belirtecin karşılık gelen `nbf` talep). |
| `resource` | Erişim belirteci için hangi eşleşen istenen kaynak `resource` sorgu dizesi parametresi istek. |
| `token_type` | Kaynak bu belirteç taşıyıcı için erişmenizi anlamına gelir "Bearer" erişim belirteci Belirtecin türü. |

## <a name="get-a-token-using-c"></a>C# kullanarak belirteç alın

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://management.azure.com/");
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

## <a name="get-a-token-using-go"></a>Git kullanarak belirteç alın

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
    msi_endpoint, err := url.Parse("http://localhost:50342/oauth2/token")
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

## <a name="get-a-token-using-azure-powershell"></a>Azure PowerShell kullanarak belirteç alın

Aşağıdaki örnek, MSI REST uç noktası için bir PowerShell istemciden kullanımı gösterilmiştir:

1. Bir erişim belirteci alın.
2. Bir Azure Resource Manager REST API çağrısı ve VM hakkında bilgi almak için erişim belirtecini kullanır. Abonelik kimliği, kaynak grubu adı ve sanal makine adı için değiştirdiğinizden emin olun `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>`, ve `<VM-NAME>`sırasıyla.

```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>CURL kullanarak belirteç alın

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Belirteç süre sonu işleme

Yerel MSI alt sistemi belirteçleri önbelleğe alır. Bu nedenle, istediğiniz ve yalnızca Azure AD üzerinde hat çağrısına sonuçları kadar çağırabilirsiniz:
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
| 400 Hatalı istek | invalid_resource | AADSTS50001: uygulama adlı  *\<URI\>*  adlı Kiracı bulunamadı  *\<KİRACI kimliği\>*. Uygulama değil Kiracı Yöneticisi tarafından yüklendikten veya Kiracı herhangi bir kullanıcı tarafından izin verdiği gerçekleşebilir. Yanlış Kiracı kimlik doğrulama isteği gönderilen. \ | (Yalnızca Linux) |
| 400 Hatalı istek | bad_request_102 | Gerekli meta veriler üstbilgisi belirtilmedi | Her iki `Metadata` isteği üstbilgisi alanının isteğinizden eksik veya yanlış biçimlendirilmiş. Değer olarak belirtilmelidir `true`, tüm alt durumda. "Örnek istek" bölümüne bakın [geri KALAN bölümü önceki](#rest) bir örnek.|
| 401 Yetkisiz | unknown_source | Bilinmeyen kaynak  *\<URI\>* | HTTP GET isteği URI doğru biçimlendirildiğinden emin olun. `scheme:host/resource-path` Bölümü olarak belirtilmelidir `http://localhost:50342/oauth2/token`. "Örnek istek" bölümüne bakın [geri KALAN bölümü önceki](#rest) bir örnek.|
|           | invalid_request | İstek gerekli bir parametre eksik, geçersiz bir parametre değeri içerir, bir parametresi birden çok kez içerir veya aksi halde bozuk. |  |
|           | unauthorized_client | İstemci bu yöntemi kullanarak bir erişim belirteci istemek için yetkili değil. | Yerel bir geri döngü uzantısı çağırmak için kullanmadı bir istek tarafından ya da doğru şekilde yapılandırılmış bir MSI sahip olmayan bir VM'de neden oldu. Bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md) VM yapılandırması yardıma ihtiyacınız varsa. |
|           | access_denied | Kaynak sahibi veya yetkilendirme sunucusu isteğini reddetti. |  |
|           | unsupported_response_type | Yetkilendirme sunucusu bu yöntemi kullanarak bir erişim belirteci alma desteklemez. |  |
|           | invalid_scope | İstenen kapsamı geçersiz, bilinmeyen veya hatalı biçimlendirilmiş olabilir. |  |
| 500 İç sunucu hatası | bilinmiyor | Active Directory'den belirteci alınamadı. Ayrıntılar için günlükleri Bkz  *\<dosya yolu\>* | MSI VM etkin olduğunu doğrulayın. Bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md) VM yapılandırması yardıma ihtiyacınız varsa.<br><br>Ayrıca, HTTP GET isteği URI özellikle URI sorgu dizesinde belirtilen kaynak doğru biçimlendirildiğinden emin olun. "Örnek istek" bölümüne bakın [geri KALAN bölümü önceki](#rest) bir örnek veya [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) Hizmetleri ve bunların ilgili kaynak kimlikleri listesi.

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) Azure AD destekleyen ve MSI ile test kaynakları ve bunların ilgili kaynak kimlikleri listesi.


## <a name="related-content"></a>İlgili içerik

- Azure VM'de MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.








