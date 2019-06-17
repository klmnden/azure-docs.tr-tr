---
title: Azure İzleyici HTTP veri toplayıcı API'si | Microsoft Docs
description: POST JSON verileri REST API çağrısı herhangi bir istemciden bir Log Analytics çalışma alanınıza eklemek için Azure İzleyici HTTP veri toplayıcı API'sini kullanabilirsiniz. Bu makalede API'SİNİN nasıl kullanılacağı açıklanır ve farklı programlama dillerini kullanarak veri yayımlama örnekleri vardır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: ''
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: bwren
ms.openlocfilehash: 0f5a996d68c80fd9b1f55a36de37579ea245d99d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64922776"
---
# <a name="send-log-data-to-azure-monitor-with-the-http-data-collector-api-public-preview"></a>Azure İzleyici HTTP veri toplayıcı API'sini (genel Önizleme) ile günlük verileri gönderin
Bu makalede günlük verilerini Azure İzleyici için bir REST API istemcisinden göndermek için HTTP veri toplayıcı API'sini kullanmayı gösterir.  Bu betik ya da uygulama tarafından toplanan verileri biçimlendirme, bir isteğe ekleyin ve bu isteği Azure İzleyici tarafından yetkilendirilmiş olması açıklar.  PowerShell, C# ve Python için örnek verilmiştir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
> Azure İzleyici HTTP veri toplayıcı API'si genel önizlemeye sunuldu.

## <a name="concepts"></a>Kavramlar
Günlük verileri Log Analytics çalışma alanına Azure İzleyici REST API'sine çağrı yapmadan herhangi bir istemciden göndermek için HTTP veri toplayıcı API'sini kullanabilirsiniz.  Bu runbook olabilir yönetim toplar Azure Automation'da Azure veya başka bir bulut ya da verileri birleştirmek ve günlük verilerini analiz etmek için Azure İzleyici kullanan bir alternatif yönetim sistemi olabilir.

Log Analytics çalışma alanındaki tüm verileri, belirli bir kayıt türü içeren bir kayıt olarak depolanır.  HTTP veri toplayıcı API'si için birden çok kayıt JSON olarak göndermek için veri biçimi.  Veri gönderdiğinde, depo istek yükü her kayıt için tek bir kayıt oluşturulur.


![HTTP veri toplayıcı genel bakış](media/data-collector-api/overview.png)



## <a name="create-a-request"></a>Bir isteği oluştur
HTTP veri toplayıcı API'sini kullanmak için JavaScript nesne gösterimi (JSON) gönderilecek verileri içeren bir POST isteği oluşturun.  Sonraki üç tablolarda her istek için gerekli olan öznitelikler listelenir. Her bir öznitelik makalenin ilerleyen bölümlerinde daha ayrıntılı olarak açıklanmaktadır.

### <a name="request-uri"></a>İstek URI'si
| Öznitelik | Özellik |
|:--- |:--- |
| Yöntem |POST |
| URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| İçerik türü |uygulama/json |

### <a name="request-uri-parameters"></a>İstek URI parametreleri
| Parametre | Açıklama |
|:--- |:--- |
| CustomerID |Log Analytics çalışma alanı için benzersiz tanımlayıcı. |
| Resource |API kaynak adı: / api/günlükleri. |
| API Sürümü |Bu istekle kullanılacak API sürümü. Şu anda bu 2016-04-01 olur. |

### <a name="request-headers"></a>İstek üst bilgileri
| Üstbilgi | Açıklama |
|:--- |:--- |
| Yetkilendirme |Yetkilendirme imzası. Makalenin sonraki bölümlerinde bir HMAC SHA256 üst bilgisi oluşturma hakkında okuyabilirsiniz. |
| Günlük türü |Gönderiliyor verileri kayıt türünü belirtin. Bu parametre için boyut sınırı 100 karakterdir. |
| x-ms-tarih |İstek işlendiği, RFC 1123 biçiminde tarih. |
| x-ms-AzureResourceId | Azure veri kaynağının kaynak kimliği ile ilişkilendirilir. Bu doldurur [_ResourceId](log-standard-properties.md#_resourceid) özelliği ve dahil edilecek verileri sağlayan [kaynak odaklı](manage-access.md#access-modes) sorgular. Bu alan belirtilmezse, veri kaynağı merkezli sorgularda dahil edilmez. |
| saat oluşturulan alanı | Zaman damgası veri öğesinin içerdiği verileri bir alanın adı. Bir alanı belirtmeniz sonra içeriği için kullanılan **TimeGenerated**. Bu alan belirtilmezse, varsayılan **TimeGenerated** ileti alınan zamandır. Mesaj alanına içeriğini ISO 8601 biçimi YYYY izlemelidir-aa-ssZ. |

## <a name="authorization"></a>Yetkilendirme
Azure İzleyici HTTP veri toplayıcı API'sini yapılan tüm istekleri bir yetkilendirme üst bilgisi içermesi gerekir. Bir isteğin kimliğini doğrulamak için birincil veya ikincil anahtarı isteği yapan çalışma alanı için istekle oturum açmanız gerekir. Ardından, bu imza, isteğin bir parçası geçirin.   

Yetkilendirme üst bilgisi biçimi şu şekildedir:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*Çalışma alanı kimliği* Log Analytics çalışma alanı için benzersiz tanımlayıcı. *İmza* olduğu bir [karma tabanlı ileti kimlik doğrulama kodu (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) istekten oluşturulur ve ardından kullanarak hesaplanan [SHA256 algoritmasını](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Daha sonra bunu Base64 kodlaması kullanarak kodlayın.

Kodlamak için bu biçimi kullanmak **SharedKey** imza dize:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

Bir imza dize örneği şöyledir:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

İmza dize olduğunda, UTF-8 olarak kodlanmış dize üzerinde HMAC SHA256 algoritmasını kullanarak kodlamak ve ardından sonucu olarak Base64 kodlama. Bu biçimi kullanın:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Örnekler sonraki bölümlerde, bir yetkilendirme üst bilgisi oluşturmanıza yardımcı olması için örnek kod vardır.

## <a name="request-body"></a>İstek gövdesi
İletisinin gövdesini JSON biçiminde olmalıdır. Bu biçimde bir veya daha fazla özellik ad ve değer çiftlerini kayıtlarla içermesi gerekir:

```json
[
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    }
]
```

Aşağıdaki biçimi kullanarak tek bir istekte birden çok kayıt toplu iş. Tüm kayıtlarda aynı kayıt türü olmalıdır.

```json
[
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    },
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    }
]
```

## <a name="record-type-and-properties"></a>Kayıt türü ve özellikleri
Azure İzleyici HTTP veri toplayıcı API'si aracılığıyla veri gönderdiğinde, özel bir kayıt türü tanımlarsınız. Şu anda, diğer veri türleri ve çözümler tarafından oluşturulan mevcut kayıt türlerinin veri yazamıyor. Azure İzleyici, gelen verileri okur ve girdiğiniz değer veri türleri eşleşen özelliklere oluşturur.

Veri Toplayıcı API'sini kullanarak her isteğin içermelidir bir **günlük türü** üstbilgiyle kayıt türünün adı. Sonek **_CL** otomatik olarak eklenir adı için özel bir günlük günlük diğer türlerden ayırt etmek için bilgi girin. Örneğin adını girin, **MyNewRecordType**, Azure izleyici türü ile bir kayıt oluşturur **MyNewRecordType_CL**. Bu kullanıcı tarafından oluşturulan tür adları hem de geçerli veya gelecek Microsoft çözümleri sevk arasında çakışmalar olduğundan emin olun yardımcı olur.

Azure İzleyici bir özelliğin veri türünü tanımlamak için özellik adına bir sonek ekler. Bir özellik null bir değer içeriyorsa, bu kayıt özelliği dahil edilmez. Bu tabloda, karşılık gelen sonek ve özellik verilerinin türü listelenmiştir:

| Özellik verilerinin türü | Son eki |
|:--- |:--- |
| String |_s |
| Boolean |_b |
| Double |_d |
| Tarih/saat |_t |
| GUID |_g |

Yeni kayıt için kayıt türü zaten var olup üzerinde her özelliği için Azure İzleyici kullanan veri türüne bağlıdır.

* Azure İzleyici, kayıt türü yoksa yeni kayıt için her bir özellik için veri türünü belirlemek için JSON tür çıkarımı kullanarak yeni bir oluşturur.
* Kayıt türü yoksa, Azure İzleyici mevcut özelliklerine bağlı olarak yeni bir kayıt oluşturmak çalışır. Yeni kayıttaki bir özellik için veri türü eşleşmiyor ve mevcut türüne dönüştürülemiyor veya kayıt mevcut olmayan bir özellik varsa, Azure İzleyici ilgili sonekine sahip yeni bir özellik oluşturur.

Örneğin, bu gönderme giriş üç özellik ile bir kayıt oluşturacak **number_d**, **boolean_b**, ve **string_s**:

![Kayıt örneği 1](media/data-collector-api/record-01.png)

Bu sonraki giriş ardından tüm değerleri dize olarak biçimlendirilmiş gönderdiyseniz özelliklerini değiştirmemesi. Bu değerler, mevcut veri türlerine dönüştürülebilir:

![Örnek kaydı 2](media/data-collector-api/record-02.png)

Ancak, ardından bu sonraki gönderim yaptıysanız, Azure İzleyici'yeni özellikleri oluşturacak **boolean_d** ve **string_d**. Bu değerleri dönüştürülemez:

![Örnek kayıt 3](media/data-collector-api/record-03.png)

Kayıt türü oluşturulmadan önce şu girişi, ardından gönderdiyseniz, Azure İzleyici üç özellik bir kayıt oluşturacak **başarı sayısı**, **boolean_s**, ve **string_s**. Bu girdiye her ilk değeri bir dize olarak biçimlendirilmiş:

![Örnek kayıt 4](media/data-collector-api/record-04.png)

## <a name="reserved-properties"></a>Ayrılmış Özellikler
Aşağıdaki özellikler ayrılmış ve bir özel kayıt türü kullanılmamalıdır. Bu özellik adlarının herhangi yükünüzü içeriyorsa bir hata alırsınız.

- tenant

## <a name="data-limits"></a>Veri sınırları
Azure İzleyicisi veri koleksiyonu API'sini için gönderilen veriler etrafında bazı kısıtlamalar vardır.

* Azure İzleyici, veri toplayıcı API'sini gönderi başına en fazla 30 MB. Tek bir gönderi için boyut sınırı budur. Tek bir veri gönderirseniz 30 MB aşıyor, daha küçük boyutlu öbeklere verileri bölün ve eşzamanlı olarak gönderin.
* En fazla 32 KB sınırını alan değerleri için. Alan değeri, 32 KB'den büyükse, verileri kesilecek.
* Verilen tür için alanları önerilen en yüksek sayısını 50'dir. Bu, bir kullanılabilirlik ve arama deneyimi açısından pratik bir sınırdır.  
* Bir Log Analytics çalışma alanında bir tablo yalnızca 500'e kadar sütun (Bu makalede bir alan olarak adlandırılır) destekler. 
* Sütun adı karakter sayısı 500'dür.

## <a name="return-codes"></a>Dönüş kodları
HTTP durum kodu 200 istek işleme için alındı anlamına gelir. Bu işlem başarıyla tamamlandığını gösterir.

Bu tabloda eksiksiz hizmet döndürebilir durum kodları listelenmiştir:

| Kod | Durum | Hata kodu | Açıklama |
|:--- |:--- |:--- |:--- |
| 200 |Tamam | |İstek başarıyla kabul edildi. |
| 400 |Hatalı istek |InactiveCustomer |Çalışma alanı kapatıldı. |
| 400 |Hatalı istek |InvalidApiVersion |Belirtilen API sürümü, hizmet tarafından tanınmadı. |
| 400 |Hatalı istek |InvalidCustomerId |Belirtilen çalışma alanı kimliği geçersiz. |
| 400 |Hatalı istek |InvalidDataFormat |Geçersiz JSON gönderildi. Yanıt gövdesi, hatanın nasıl düzeltileceği hakkında daha fazla bilgi içerebilir. |
| 400 |Hatalı istek |InvalidLogType |Kapsanan özel karakterler veya sayısal değerleri belirtilen günlük türü. |
| 400 |Hatalı istek |MissingApiVersion |API sürümü belirtilmedi. |
| 400 |Hatalı istek |MissingContentType |İçerik türü belirtilmedi. |
| 400 |Hatalı istek |MissingLogType |Gerekli değer günlük türü belirtilmedi. |
| 400 |Hatalı istek |UnsupportedContentType |İçerik türü değil olarak ayarlandı **application/json**. |
| 403 |Yasak |InvalidAuthorization |Hizmet, isteğin kimliğini doğrulayamadı. Çalışma alanı kimliği ve bağlantı anahtarı geçerli olduğunu doğrulayın. |
| 404 |Bulunamadı | | Sağlanan URL yanlış veya isteği çok büyük. |
| 429 |Çok Fazla İstek | | Hizmet hesabınızdan veri hacmi yüksek yaşıyor. Lütfen istek daha sonra yeniden deneyin. |
| 500 |İç sunucu hatası |UnspecifiedError |Hizmet bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin. |
| 503 |Hizmet kullanılamıyor |ServiceUnavailable |Hizmet isteklerini almak şu anda kullanılamıyor. Lütfen isteğinizi yeniden deneyin. |

## <a name="query-data"></a>Verileri sorgulama
Azure İzleyici HTTP veri toplayıcı API'sini, arama ile kayıt tarafından gönderilen veri **türü** eşit olan **LogType** , belirttiğiniz değer eklenmiş olan **_CL**. Örneğin, kullandıysanız **MyCustomLog**, tüm kayıtları döndürecekti sonra `MyCustomLog_CL`.

## <a name="sample-requests"></a>Örnek istekler
Sonraki bölümlerde, farklı programlama dillerini kullanarak Azure İzleyici HTTP veri toplayıcı API'sini kullanarak veri göndermek nasıl örnekleri bulabilirsiniz.

Her örnek için yetkilendirme üst bilgisi için değişkenleri ayarlamak için aşağıdaki adımları uygulayın:

1. Azure portalında Log Analytics çalışma alanınızın bulun.
2. Seçin **Gelişmiş ayarlar** ardından **bağlı kaynakları**.
2. Sağındaki **çalışma alanı kimliği**, Kopyala simgesini seçin ve ardından kimlik değeri olarak yapıştırın **Müşteri Kimliği** değişkeni.
3. Sağındaki **birincil anahtar**, Kopyala simgesini seçin ve ardından kimlik değeri olarak yapıştırın **paylaşılan anahtar** değişkeni.

Alternatif olarak, günlük türü ve JSON verilerini değişkenleri değiştirebilirsiniz.

### <a name="powershell-sample"></a>PowerShell örneği
```powershell
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# You can use an optional field to specify the timestamp from the data. If the time field is not specified, Azure Monitor assumes the time is the message ingestion time
$TimeStampField = ""


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-LogAnalyticsData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-LogAnalyticsData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C# örneği
```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId to your Log Analytics workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Azure Monitor
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Azure Monitor assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            var jsonBytes = Encoding.UTF8.GetBytes(json);
            string stringToHash = "POST\n" + jsonBytes.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-2-sample"></a>Python 2 örneği
```python
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Log Analytics workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```
## <a name="alternatives-and-considerations"></a>Seçenekler ve önemli noktalar
Veri Toplayıcı API'sini kullanarak Azure günlüklerine serbest biçimli verileri toplamak için gereksinimlerinizi çoğunu kapsamalıdır, ancak bazı sınırlamaları API'sinin aşmak için bir alternatif burada gerekebilir örnekler vardır. Tüm seçenekleri, dahil önemli konular şunlardır:

| Alternatif | Açıklama | İçin en uygun |
|---|---|---|
| [Özel olaylar](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#properties): Application ınsights SDK tabanlı yerel alma | Application Insights, genellikle, uygulamanızda bulunan bir SDK'sı aracılığıyla izlenen özel verilerine özel olayları göndermek için özelliği sunar. | <ul><li> Uygulamanızın içinde oluşturulur, ancak varsayılan veri türlerinden biri aracılığıyla SDK'sı tarafından toplanmış değil veri (IE: istekleri, bağımlılıklar, özel durumlar, vb.).</li><li> Genellikle diğer uygulama verilerini Application ınsights'da ilişkili verileri </li></ul> |
| [Veri Toplayıcı API'si](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) Azure İzleyici günlüklerine | Azure İzleyici günlüklerine veri toplayıcı API'sini kullanarak veri alımı için tamamen açık uçlu bir yoludur. Bir JSON nesnesi biçimlendirilen herhangi bir veri burada gönderilebilir. Gönderdikten sonra işleme alınır ve kullanılabilir olmasını günlüklerindeki diğer verilerle günlükleri veya diğer Application Insights karşı veri bağıntılı. <br/><br/> Veri dosyaları olarak bir Azure Blob bloba, gelen burada bu dosyaları işlenmesi ve Log Analytics'e karşıya yüklemek oldukça kolaydır. Lütfen [bu](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) makalede bu tür bir işlem hattı için bir örnek uygulama için. | <ul><li> Her zaman içinde Application Insights izleme eklenmiş uygulama içinde oluşturulmayan verileri.</li><li> Örnekler, arama ve olgu tabloları, başvuru verileri, önceden toplu istatistikleri vb. içerir. </li><li> Diğer Azure İzleyici verileri (örneğin, Application Insights, diğer günlükler veri türleri, Güvenlik Merkezi, Azure İzleyici kapsayıcılar/VMs, vb.) karşı çıkarılacak veriler için yöneliktir. </li></ul> |
| [Azure Veri Gezgini](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview) | Azure Veri Gezgini (ADX), Application Insights Analytics ve Azure İzleyici günlüklerine güç veri platformudur. Artık genellikle bir veri platformu kullanarak ham biçimde Availabile ("GA"), tam esneklik (ancak yönetiminin getirdiği ek yüke gerek) küme üzerinde (RBAC, elde tutma oranı, şema, vb.) sağlar. ADX sağlayan birçok [alımı seçenekleri](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview#ingestion-methods) dahil olmak üzere [CSV, TSV ve JSON](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master) dosyaları. | <ul><li> Application Insights veya günlükleri altında herhangi bir veri ilişkilendirilmez verileri. </li><li> Veri alımı Gelişmiş gerektiren veya Azure İzleyici günlüklerine bugün kullanılabilir özellikleri işleme. </li></ul> |


## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [günlük arama API'si](../log-query/log-query-overview.md) Log Analytics çalışma alanından veri alınamadı.

- Hakkında daha fazla bilgi [veri işlem hattı ile veri toplayıcı API'sini oluşturma](create-pipeline-datacollector-api.md) Azure İzleyici için Logic Apps iş akışı kullanarak.
