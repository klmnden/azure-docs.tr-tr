---
title: Bir sanal makine doğrulamak için istemci kendi kendine test | Azure Market
description: Bir sanal makine görüntüsü için Azure Market'ten önceden doğrulamak için bir kendi kendine test istemcisi oluşturma
services: Azure, Marketplace, Cloud Partner Portal, Virtual Machine
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: pabutler
ms.openlocfilehash: 117249feea04381b34f8fc1d95f77c2c1a567dba
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938713"
---
# <a name="create-a-self-test-client-to-pre-validate-an-azure-virtual-machine-image"></a>Bir Azure sanal makine görüntüsünü doğrulamak için bir kendi kendine test istemcisi oluşturma

Kendi kendine test API'si, istemci hizmeti oluşturmak için bir kılavuz kullanır gibi bu makaleyi kullanın. Bir sanal makine (VM) en son Azure Marketi'nde yayımlama gereksinimleri karşıladığından emin olmak için önceden doğrulamak için sınama API'sini kullanabilirsiniz. Bu istemci hizmet teklifinizi Microsoft sertifika için göndermeden önce bir VM test etmenizi sağlar.

## <a name="development-and-testing-overview"></a>Geliştirme ve test genel bakış

Kendi kendine test işleminin bir parçası olarak, Azure aboneliğinizde çalıştıran bir VM doğrulamak için Azure Market'te bağlanan yerel bir istemci oluşturacaksınız. VM Windows veya Linux işletim sistemini çalıştırmalıdır.

Yerel istemci, kendi kendine test API'si ile kimlik doğrulaması, bağlantı bilgilerini gönderir ve test sonuçlarını alır bir betiği çalıştırır.

Bir kendi kendine test istemcisi oluşturmak için üst düzey adımlar şunlardır:

1. Uygulamanız için Azure Active Directory (AD) kiracısı seçin.
2. İstemci uygulaması kaydedin.
3. İstemci Azure AD uygulaması için bir belirteç oluşturun.
4. Belirteç kendi kendine test API'sine geçirin.

İstemci oluşturduktan sonra sanal Makinenizin karşı test edebilirsiniz.

### <a name="self-test-client-authorization"></a>İstemci kimlik doğrulaması sınama

Aşağıdaki diyagramda yetkilendirme hizmet çağrıları için istemci kimlik bilgilerini (paylaşılan gizli diziyi veya sertifika) kullanarak nasıl çalıştığını gösterir.

![İstemci Yetkilendirme işlemi](./media/stclient-dev-process.png)

## <a name="the-self-test-client-api"></a>Kendi kendine test İstemcisi API

Kendi kendine test API'si yalnızca POST yöntemini destekleyen tek bir uç nokta içerir.  Bunu, aşağıdaki yapıya sahiptir.

```
Uri:             https://isvapp.azurewebsites.net/selftest-vm
Method:          Post
Request Header:  Content-Type: “application/json”
Authorization:   “Bearer xxxx-xxxx-xxxx-xxxxx”
Request body:    The Request body parameters should use the following JSON format:
                 {
                   "DNSName":"XXXX.westus.cloudapp.azure.com",
                   "User":"XXX",
                   "Password":"XXX@1234567",
                   "OS":"XXX",
                   "PortNo":"22",
                   "CompanyName":"ABCD",
                 }
```

Aşağıdaki tabloda API alanlar açıklanır.


|      Alan         |    Açıklama    |
|  ---------------   |  ---------------  |
|  Yetkilendirme     |  "Taşıyıcı xxxx-xxxx-xxxx-xxxxx" dizesi, PowerShell kullanılarak oluşturulan Azure Active Directory (AD) istemci belirteci içerir.          |
|  DNSName           |  Test etmek için sanal makinenin DNS adı    |
|  Kullanıcı              |  VM'de oturum imzalamak için kullanıcı adı         |
|  Parola          |  VM'de oturum imzalamak için parola          |
|  İşletim Sistemi                |  Sanal makinenin işletim sistemi: ya da `Linux` veya `Windows`          |
|  PortNo            |  VM'ye bağlanmak için bağlantı noktası numarası açın. Bağlantı noktası numarası genellikle olan `22` Linux ve `5986` Windows için.          |
|  |  |

## <a name="consuming-the-api"></a>API kullanma

PowerShell veya cURL kullanarak kendi kendine test API'si kullanabilir.

### <a name="use-powershell-to-consume-the-api-on-the-linux-os"></a>Linux işletim sistemi API'yi kullanmak için PowerShell kullanma

PowerShell'de API'yi çağırmak için aşağıdaki adımları izleyin:

1. Kullanım `Invoke-WebRequest` API'sini çağırmak için komutu.
2. Yöntemi Post ve içerik türünün JSON, aşağıdaki kod örneği ve ekran görüntüsünde gösterildiği gibi.
3. Gövde parametreleri JSON biçiminde belirtin.

Aşağıdaki kod örneği, API için bir PowerShell çağrı gösterir.

```powershell
$accesstoken = “Get token for your Client AAD App”
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Linux"
    "PortNo" = "22"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" –Headers $headers;
$Content = $res | ConvertFrom-Json
```
Aşağıdaki ekran görüntüsü yakalamayı PowerShell'de API'yi çağırmak için bir örnek gösterilmektedir.

![Linux işletim sistemi için PowerShell ile API çağrısı](./media/stclient-call-api-ps-linuxvm.png)

Önceki örneği kullanarak JSON alabilir ve aşağıdaki ayrıntıları almak için ayrıştırılamadı:

```powershell
$testresult = ConvertFrom-Json –InputObject (ConvertFrom-Json –InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++)
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

Aşağıdaki ekran gösteren yakalama `$res.Content`, JSON biçiminde, test sonuçları ayrıntılarını verir.

![Linux için PowerShell çağrısından JSON sonuçları](./media/stclient-pslinux-rescontent-json.png)

Aşağıdaki ekran görüntüsü yakalamayı çevrimiçi bir JSON Görüntüleyicisi'nde görüntülemesini JSON test sonuçlarının bir örneği gösterir (örneğin, [kod Beautify](https://codebeautify.org/jsonviewer) veya [JSON Görüntüleyicisi](https://jsonformatter.org/json-viewer)).

![Linux VM PowerShell çağrısından JSON sonuçları](./media/stclient-consume-api-pslinux-json.png)

### <a name="use-powershell-to-consume-the-api-on-the-windows-os"></a>Windows işletim sistemi API'yi kullanmak için PowerShell kullanma

PowerShell'de API'yi çağırmak için aşağıdaki adımları izleyin:

1. Kullanım `Invoke-WebRequest` API'sini çağırmak için komutu.
2. Yöntemi Post ve içerik türünün JSON, aşağıdaki kod örneği ve ekran görüntüsünde gösterildiği gibi.
3. Gövde parametreleri JSON biçiminde oluşturun.

Aşağıdaki kod örneği, API için bir PowerShell çağrı gösterir.

```powershell
$accesstoken = “Get token for your Client AAD App”
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Windows"
    "PortNo" = "5986"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" –Headers $headers;
$Content = $res | ConvertFrom-Json
```

Aşağıdaki ekran görüntüsü yakalamayı PowerShell'de API'yi çağırmak için bir örnek gösterilmektedir.

![Windows VM için PowerShell ile API çağrısı](./media/stclient-call-api-ps-windowsvm.png)

Önceki örneği kullanarak JSON alabilir ve aşağıdaki ayrıntıları almak için ayrıştırılamadı:

```powershell
$testresult = ConvertFrom-Json –InputObject (ConvertFrom-Json –InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++)
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

Aşağıdaki ekran gösteren yakalama `$res.Content`, JSON biçiminde, test sonuçları ayrıntılarını verir.

![İçin Windows powershell'den JSON sonuçları çağırın](./media/stclient-pswindows-rescontent-json.png)

Aşağıdaki ekran görüntüsü yakalamayı, çevrimiçi bir JSON Görüntüleyicisi'nde görüntülenen test sonuçları gösterilmektedir.
(örneğin, [kod Beautify](https://codebeautify.org/jsonviewer), [JSON Görüntüleyicisi](https://jsonformatter.org/json-viewer))

![Windows VM PowerShell çağrısından JSON sonuçları](./media/stclient-consume-api-pswindows-json.png)

### <a name="use-curl-to-consume-the-api-on-the-linux-os"></a>Linux işletim sistemi API'yi kullanmak için cURL kullanın.

CURL ile API'sini çağırmak için aşağıdaki adımları izleyin:

1. API'yi çağırmak için curl komutunu kullanın.
2. Yöntemi Post ve içerik türünün JSON, aşağıdaki kod parçacığında gösterildiği gibi.

```
CURL POST -H "Content-Type:application/json"
-H "Authorization: Bearer XXXXXX-Token-XXXXXXXX”
https://isvapp.azurewebsites.net/selftest-vm
-d '{ "DNSName":"XXXX.westus.cloudapp.azure.com", "User":"XXX", "Password":"XXXX@123456", "OS":"Linux", "PortNo":"22", "CompanyName":"ABCD"}'
```

Aşağıdaki ekranda API'sini çağırmak için curl kullanarak bir örnek gösterilmektedir.

![Curl komutu kullanarak API çağırın](./media/stclient-consume-api-curl.png)

Aşağıdaki ekran görüntüsü yakalamayı curl çağrısından JSON sonuçları gösterilmektedir.

![Curl çağrısından JSON sonuçları](./media/stclient-consume-api-curl-json.png)


## <a name="choose-the-azure-ad-tenant-for-the-app"></a>Uygulama için Azure AD kiracısı seçin

Uygulamanızı oluşturmak istediğiniz Azure AD kiracısını seçmek için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Üstteki menü çubuğunda, hesabınızı seçin ve dizin listesinde uygulamanızı kaydetmek için istediğiniz Active Directory kiracısı seçin. Ya da seçin **dizin + abonelik** genel abonelik filtresini görmek için simge. Aşağıdaki ekran görüntüsü yakalamayı Bu filtre bir örneği gösterilmektedir.

   ![Abonelik filtresi seçin](./media/stclient-subscription-filter.png)

3. Sol gezinti çubuğunda, seçin **tüm hizmetleri** seçip **Azure Active Directory**.

   Aşağıdaki adımlarda, Kiracı adı (veya dizin adı) gerekebilir veya Kiracı kimliği (veya dizin kimliği).

   **Kiracı bilgilerini almak için:**

   İçinde **Azure Active Directory genel bakış**, "Özellikler" için arama yapın ve ardından **özellikleri**. Örnek olarak aşağıdaki ekran görüntüsü yakalamayı kullanma:

   - **Ad** -Kiracı adı veya dizin adı
   - **Dizin kimliği** -özelliklerini bulmak için Kiracı kimliği veya dizin kimliği veya kaydırma çubuğunu kullanın.

   ![Azure Active Directory Özellikler sayfası](./media/stclient-aad-properties.png)

## <a name="register-the-client-app"></a>İstemci uygulamayı kaydetme

İstemci uygulamayı kaydetmek için aşağıdaki adımları kullanın.

1. Sol gezinti çubuğunda, seçin **tüm hizmetleri** seçip **uygulama kayıtları**.
2. Altında **uygulama kayıtları**seçin **+ yeni uygulama kaydı**.
3. Altında **Oluştur**, aşağıdaki alanlar için gerekli bilgileri sağlayın:

   - **Adı** – uygulama için kolay bir ad girin. Örneğin, "SelfTestClient".
   - **Uygulama türü** – Select **Web uygulaması/API'si**
   - **Oturum açma URL'si** – türü "https:\//isvapp.azurewebsites.net/selftest-vm"

4. **Oluştur**’u seçin.
5. Altında **uygulama kayıtları** veya **kayıtlı uygulama**, kopyalama **uygulama kimliği**.

   ![Uygulama Kimliğini Al](./media/stclient-app-id.png)

6. Kayıtlı uygulama araç çubuğunda, seçin **ayarları**.
7. Seçin **gerekli izinler** uygulamanızın izinlerini yapılandırmak için.
8. Altında **gerekli izinler**seçin **+ Ekle**.
9. Altında **API erişimi Ekle**, çekme **bir API seçin**.
10. Altında **bir API seçin**, türü "Windows Azure Klasik dağıtım modelinden" API için aranacak.
11. Arama sonuçlarında çekme **Windows Azure Klasik dağıtım modeli** ve ardından **seçin**.

    ![Çok kiracılı uygulama için yapılandırma](./media/stclient-select-api.png)

12. Altında **API erişimi Ekle**, çekme **izinleri seçin**.
13. Seçin **erişim "Windows Azure Hizmet Yönetimi API"**.

    ![Uygulama için API erişimini etkinleştirin](./media/stclient-enable-api-access.png)

14. **Seç**'e tıklayın.
15. **Done** (Bitti) öğesini seçin.
16. Altında **ayarları**seçin **özellikleri**.
17. Altında **özellikleri**, ekranı aşağı kaydırarak **çok kiracılı**. Seçin **Evet**.

    ![Çok kiracılı uygulama için yapılandırma](./media/stclient-yes-multitenant.png)

18. **Kaydet**’i seçin.
19. Altında **ayarları**seçin **anahtarları**.
20. Anahtar'ı seçerek bir gizli anahtar oluşturma **açıklama** metin. Aşağıdaki alanları yapılandırın:

    - Bir anahtar adı yazın. Örneğin, "selftestclient"
    - Üzerinde **EXPIRES** açılan listesinde, "1 yıl içinde" seçin.
    - Seçin **Kaydet** anahtarı oluşturmak için.
    - Altında **değer**, anahtarı kopyalayın.

      >[!Important]
      >Çıktıktan sonra anahtar değeri açamazsınız **anahtarları** formu.

    ![Anahtar değeri form](./media/stclient-create-key.png)

## <a name="create-the-token-for-the-client-app"></a>İstemci uygulaması için belirteci oluşturma

Aşağıdaki programlardan herhangi biriyle oluşturmak ve OAuth REST API kullanarak bir belirteç almak için kullanabilirsiniz:

- Postman
- Linux'ta cURL
- C&#35;
- PowerShell

### <a name="to-create-and-get-a-token-using-postman"></a>Oluşturma ve Postman kullanarak bir belirteç almak için

 Auth0 belirteçleri, yetkili uygulamalardan herhangi biri için sormak için bir gönderme işlemi için gerçekleştirmek [ https://login.microsoftonline.com/common/oauth2/token ](https://login.microsoftonline.com/common/oauth2/token) aşağıdaki biçimde bir yükü uç noktası:

```
Method Type : POST
Base Url: https://login.microsoftonline.com/common/oauth2/token
```

İstek gövdesinde Parametreler:

```
Body Content-Type: x-www-form-urlencoded
client_id: XXX (Paste your Application ID of Web App/API Type client AD App)
grant_type: client_credentials
client_secret: XXX (Paste your Secret Key of Web App/API Type client AD App)
resource: https://management.core.windows.net
```

İstek üst bilgisinde Parametreler:

```
Content-Type: application/x-www-form-urlencoded
```

Aşağıdaki ekran görüntüsü yakalamayı bir belirteç almak için Postman'ı kullanarak bir örnek gösterilmektedir.

![Postman ile belirteci alma](./media/stclient-postman-get-token.png)

### <a name="to-create-and-get-a-token-using-curl-in-linux"></a>Oluşturma ve Linux'ta cURL kullanarak bir belirteç almak için

Auth0 belirteçleri, yetkili uygulamalardan herhangi biri için sormak için bir gönderme işlemi için gerçekleştirmek [ https://login.microsoftonline.com/common/oauth2/token ](https://login.microsoftonline.com/common/oauth2/token) aşağıdaki biçimde bir yükü uç noktası:

```
Request:

curl --request POST \
                --url 'https://login.microsoftonline.com/common/oauth2/token' \
                --header "Content-Type:application/x-www-form-urlencoded" \
                --data "grant_type=client_credentials&client_id=XXXXX-XXXX-XXXX-XXXXXXX&client_secret=XXXXXXXXX&resource=https://management.core.windows.net"

Response:

{"token":"UClCUUKxUlkdbhE1cHLz3kyjbIZYVh9eB34A5Q21Y3FPqKGSJs","expires":"2014-02-17 18:46:08"}
```

Aşağıdaki ekran görüntüsü yakalamayı bir belirteç almak üzere curl komutunu kullanarak bir örnek gösterilmektedir.

![Curl komutuyla belirteci alma](./media/stclient-curl-get-token.png)

### <a name="to-create-and-get-a-token-using-c35"></a>Oluşturma ve C kullanarak bir belirteç almak için&#35;

Auth0 belirteçleri, yetkili uygulamalardan herhangi biri için sormak için ve https için bir gönderme işlemi gerçekleştirin:\/aşağıdaki biçimde bir yükü /soamtenant.auth0.com/oauth/token uç noktası:

```csharp
string clientId = "Your Application Id";
string clientSecret = "Your Application Secret";
string audience = "https://management.core.windows.net";
string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, "https://login.microsoftonline.com/common/oauth2/token");
string grantType = "client_credentials";
var client = new RestClient(authority);
var request = new RestRequest(Method.POST);
       request.AddHeader("content-type", "application/x-www-form- urlencoded");

string requestBody = "grant_type=" + grantType + "&" + "client_id=" +   clientId + "&" + "client_secret=" + clientSecret + "&" + "resource=" +  audience;

request.AddParameter("application/x-www-form-urlencoded", requestBody,  ParameterType.RequestBody);

IRestResponse response = client.Execute(request);
var content = response.Content;
var token = JObject.Parse(content)["access_token"];
```

### <a name="to-create-and-get-a-token-using-powershell"></a>Oluşturma ve PowerShell kullanarak bir belirteç almak için

Auth0 belirteçleri, yetkili uygulamalardan herhangi biri için sormak için ve https için bir gönderme işlemi gerçekleştirin:\/aşağıdaki biçimde bir yükü /soamtenant.auth0.com/oauth/token uç noktası:

```powershell
$clientId = "Application Id of AD Client APP";
$clientSecret = "Secret Key of AD Client APP “
$audience = "https://management.core.windows.net";
$authority = "https://login.microsoftonline.com/common/oauth2/token"
$grantType = "client_credentials";

$requestBody = "grant_type=" + $grantType + "&" + "client_id=" +  $clientId   + "&" + "client_secret=" + $clientSecret + "&" + "resource=" + $audience;

$headers = New-Object  "System.Collections.Generic.Dictionary[[String],[String]]"
             $headers.Add("ContentType", "application/x-www-form-urlencoded")
resp = Invoke-WebRequest -Method Post -Uri $authority -Headers $headers -ContentType 'application/x-www-form-urlencoded' -Body $requestBody

$token = $resp.Content | ConvertFrom-Json
$token.AccessToken
```

## <a name="pass-the-client-app-token-to-the-api"></a>İstemci uygulama belirteci API'sine geçirin.

Yetkilendirme üst bilgisinde aşağıdaki kodu kullanarak kendi kendine test API'sine belirtecin geçip:

```powershell
$redirectUri = ‘https://isvapp.azurewebsites.net/selftest-vm’
$accesstoken = ‘place your token here’

$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body =
@{
      'DNSName'="XXXX.cloudapp.azure.com";
      'User'="XXX";
      'Password'="XXXX@12345";
      'OS'="Linux";
      'PortNo'="22"
  } | ConvertTo-Json

$result=Invoke-WebRequest -Method Post -Uri $redirectUri -Headers $headers -ContentType 'application/json' -Body $Body
$result
Write-Output 'Test Results:'
$result.Content
```

## <a name="test-your-self-test-client"></a>Kendi test istemcinizi test

İstemciyi test etme için bu adımları izleyin:

1. Test etmek istediğiniz VM dağıtın.
2. Yetkilendirme için istemci uygulama belirteci kullanarak kendini sınama API'sini çağırın.
3. Test sonuçları JSON biçiminde alın.

### <a name="test-result-examples"></a>Test sonucu örnekleri

Aşağıdaki kod parçacıkları, test sonuçları JSON biçiminde gösterir.

**Bir Windows VM için test sonuçları:**

```json
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Microsoft Windows Server 2016 Datacenter",
  "OSVersion": "10.0.14393",
  "CreatedDate": "06/01/2018 07:48:04",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "Tes tID": "1.1.4",
      "TestCaseName": "OS Architecture",
      "Description": "Azure supports 64bit Operating System Only.",
      "Result": "Passed",
      "ActualValue": "64 Bit",
      "Re quiredValue": "OS Should be 64 bit"
    },
    {
      "TestID": "1.1.5",
      "TestCaseName": "User account dependency",
      "Description": "Application execution should not have de pendency on administrator account.",
      "Result": "Passed",
      "ActualValue": "isv",
      "RequiredValue": "Logged in user should not be an administrator"
    },
    {
      "TestID": " 1.1.6",
      "TestCaseName": "Failover Cluster",
      "Description": "Windows Server Failover Clustering feature is not yet supported, Application should not have dependency on this feature.",
      "Result": "Passed",
      "ActualValue": "Disabled",
      "RequiredValue": "Failover Clustering feature is disabled"
    },
```

**Bir Linux VM'si için test sonuçları:**

```json
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Ubuntu  16.04",
  "OSVersion": "16.04",
  "CreatedDate": "06/01/2018 07:04:24",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "TestID": "48",
      "TestCaseName": "Bash Hi story",
      "Description": "Bash history files should be cleared before creating the VM image.",
      "Result": "Passed",
      "ActualValue": "No file Exist",
      "RequiredVal ue": "1024"
    },
    {
      "TestID": "39",
      "TestCaseName": "Linux Agent Version",
      "Description": "Azure Linux Agent 2.2.10 and above should be installed. ",
      "Result": "Pas sed",
      "ActualValue": "2.2.20",
      "RequiredValue": "2.2.10"
    },
    {
      "TestID": "40",
      "TestCaseName": "Required Kernel Parameters",
      "Description": "Verifies the following  kernel parameters are set console=ttyS0, earlyprintk=ttyS0, rootdelay=300",
      "Result": "Passed",
      "ActualValue": "Matched Parameter: console=ttyS0,earlypri ntk=ttyS0,rootdelay=300",
      "RequiredValue": "console=ttyS0#earlyprintk=ttyS0#rootdelay=300"
    },
```

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal makine başarıyla test ettikten sonra [teklifi yayımlama](./cpp-publish-offer.md).
