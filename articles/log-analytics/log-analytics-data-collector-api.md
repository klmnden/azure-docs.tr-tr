---
title: "Analytics HTTP veri toplayıcı API oturum | Microsoft Docs"
description: "Günlük analizi HTTP veri toplayıcı API REST API'si çağırabilirsiniz herhangi bir istemciden günlük analizi depoya POST JSON verilerini eklemek için kullanabilirsiniz. Bu makalede API kullanmayı açıklar ve farklı programlama dillerini kullanarak veri yayımlama örnekleri vardır."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: bwren
ms.openlocfilehash: 5c6f2b35b48988af533612cb48da8fe79a838cf6
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="send-data-to-log-analytics-with-the-http-data-collector-api-public-preview"></a>HTTP veri toplayıcı API (genel Önizleme) ile günlük analizi veri Gönder
Bu makalede HTTP veri toplayıcı API'sini bir REST API istemciden için günlük analizi veri göndermek için nasıl kullanılacağı gösterilmektedir.  Bu komut dosyası veya uygulama tarafından toplanan veri biçimi, bir istekte içerir ve günlük analizi tarafından yetkili bu istekte açıklar.  PowerShell, C# ve Python için örnekler verilmiştir.

> [!NOTE]
> Günlük analizi HTTP veri toplayıcı API Genel önizlemede değil.

## <a name="concepts"></a>Kavramlar
HTTP veri toplayıcı API'sini bir REST API'si çağırabilirsiniz herhangi bir istemciden için günlük analizi veri göndermek için kullanabilirsiniz.  Bu runbook olabilir Azure Otomasyonu'nda yönetim toplar Azure veya başka bir bulut ya da verileri birleştirmek ve verileri çözümlemek için günlük analizi kullanan bir alternatif yönetim sistemi olabilir.

Günlük analizi depodaki tüm verileri belirli bir kayıt türü içeren bir kayıt olarak depolanır.  HTTP veri toplayıcı API'sine JSON içinde birden çok kayıt olarak göndermek için verilerinizi biçimlendirin.  Veri gönderdiğinde, tek bir kaydın depo istek yükünde her kayıt için oluşturulur.


![HTTP veri toplayıcı genel bakış](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a>Bir isteği oluşturun
HTTP veri toplayıcı API kullanmak için JavaScript nesne gösterimi (JSON) göndermek için verileri içeren bir POST isteği oluşturun.  Sonraki üç tablolarda her istek için gerekli olan öznitelikler listelenir. Biz her özniteliği makalenin sonraki bölümlerinde daha ayrıntılı açıklanmıştır.

### <a name="request-uri"></a>İstek URI'si
| Öznitelik | Özellik |
|:--- |:--- |
| Yöntem |YAYINLA |
| URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| İçerik türü |uygulama/json |

### <a name="request-uri-parameters"></a>İstek URI parametreleri
| Parametre | Açıklama |
|:--- |:--- |
| CustomerID |Günlük analizi çalışma alanı için benzersiz tanımlayıcı. |
| Kaynak |API kaynak adı: / api/günlükleri. |
| API sürümü |Bu istekle kullanmak için API sürümü. Şu anda, 2016-04-01 değil. |

### <a name="request-headers"></a>İstek üst bilgileri
| Üst bilgi | Açıklama |
|:--- |:--- |
| Yetkilendirme |Yetkilendirme imzası. Makalenin sonraki bölümlerinde HMAC SHA256 üstbilgi oluşturma hakkında bilgi edinebilirsiniz. |
| Günlük türü |Gönderiliyor veri kaydı türünü belirtin. Şu anda, yalnızca alfasayısal karakterler günlük türünü destekler. Sayısal türler veya özel karakterler desteklemez. |
| x-ms-date |İstek işlendi, RFC 1123 biçiminde tarih. |
| saat oluşturulan alanı |Veri öğesinin zaman damgası içeren veri bir alanın adı. Bir alan belirtin sonra içeriği için kullanılan **TimeGenerated**. Bu alan belirtilmezse, varsayılan **TimeGenerated** ileti alınan saattir. Mesaj alanına içeriğini ISO 8601 biçimi YYYY izlemelisiniz-aa-: ssZ. |

## <a name="authorization"></a>Yetkilendirme
Günlük analizi HTTP veri toplayıcı API için herhangi bir istek bir authorization üstbilgisi eklemeniz gerekir. Bir istek kimliğini doğrulamak için birincil veya ikincil anahtarı isteği yapan çalışma alanı için istekle imzalamanız gerekir. Daha sonra bu imza isteğin bir parçası geçirin.   

Authorization üstbilgisi biçimi şöyledir:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*Workspaceıd* günlük analizi çalışma alanı için benzersiz tanıtıcıdır. *İmza* olan bir [karma tabanlı ileti kimlik doğrulama kodu (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) istekten oluşturulan ve kullanılarak hesaplanan [SHA256 algoritmasını](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Ardından, onu Base64 kodlaması kullanarak kodlayın.

Kodlanacak şu biçimi kullanın **SharedKey** imza dizesi:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

İmza dize örneği şöyledir:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

İmza dize olduğunda, üzerinde UTF-8 kodlu dize HMAC SHA256 algoritmasını kullanarak kodlamak ve sonucu Base64 olarak kodlayın. Şu biçimi kullanın:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Sonraki bölümlerde bulunan örnekleri bir authorization üstbilgisi oluşturmanıza yardımcı olması için örnek koduna sahip.

## <a name="request-body"></a>İstek gövdesi
İleti gövdesi JSON'de olması gerekir. Bu biçimde özelliği ad ve değer çiftleri ile bir veya daha fazla kayıt içermesi gerekir:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Aşağıdaki biçimi kullanarak tek bir istekte birden çok kayıt toplu. Tüm kayıtları aynı kayıt türü olmalıdır.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Kayıt türü ve özellikleri
Günlük analizi HTTP veri toplayıcı API'si aracılığıyla veri gönderdiğinde, özel bir kayıt türü tanımlarsınız. Şu anda, diğer veri türleri ve çözümleri tarafından oluşturulan mevcut kayıt türleri veri yazamıyor. Günlük analizi gelen verileri okur ve girdiğiniz değerleri veri türlerini eşleşen özelliklere oluşturur.

Her istek için günlük analizi API içermelidir bir **günlük türü** kayıt türü için bir ad ile üstbilgisi. Sonek **_CL** otomatik olarak eklenir adı için özel bir günlük diğer günlük türlerinden ayırt etmek için girin. Örneğin adını girin, **MyNewRecordType**, günlük analizi türüyle bir kayıt oluşturur **MyNewRecordType_CL**. Bu, kullanıcı tarafından oluşturulan tür adları ve bunlar geçerli veya gelecek Microsoft çözümlerinde sevk arasında çakışmalar olduğundan emin olun yardımcı olur.

Bir özelliğin veri türünü tanımlamak için günlük analizi özellik adına bir sonek ekler. Bir özellik boş bir değer içeriyorsa, özelliği bulunan ve bu kaydı bulunmaz. Bu tablo karşılık gelen sonek ve özellik veri türünü listeler:

| Özellik veri türü | Sonek |
|:--- |:--- |
| Dize |_s |
| Boole |_b |
| Çift |_d |
| Tarih/saat |_t |
| GUID |_g |

Yeni kayıt için kayıt türü zaten var olup üzerinde her bir özellik için günlük analizi kullanır veri türüne bağlıdır.

* Kayıt türü mevcut değilse yeni bir günlük analizi oluşturur. Günlük analizi JSON tür çıkarımı yeni kayıt için her bir özellik için veri türünü belirlemek için kullanır.
* Kayıt türü var olan özelliğe göre yeni bir kayıt oluşturmak günlük analizi çalışır. İçin veri türü yeni kayıttaki özelliğinde eşleşmiyor ve varolan türüne dönüştürülemiyor veya kayıt mevcut olmayan bir özellik varsa, günlük analizi ilgili son ekine sahip yeni bir özellik oluşturur.

Örneğin, bir kayıt üç özellik ile bu gönderimi giriş oluşturursunuz **number_d**, **boolean_b**, ve **string_s**:

![Kayıt örneği 1](media/log-analytics-data-collector-api/record-01.png)

Ardından tüm değerleri dize olarak biçimlendirilmiş bu sonraki giriş gönderdiyseniz özellikleri değişmediği. Bu değerler var olan veri türüne dönüştürülebilir:

![Örnek kaydı 2](media/log-analytics-data-collector-api/record-02.png)

Ancak, günlük analizi yeni özellikleri oluşturur. ardından bu sonraki gönderme yaptıysanız, **boolean_d** ve **string_d**. Bu değerleri dönüştürülemiyor:

![Örnek kayıt 3](media/log-analytics-data-collector-api/record-03.png)

Kayıt türü oluşturulmadan önce şu girdiyi sonra gönderdiyseniz, günlük analizi üç özellik ile bir kayıt oluşturacak **başarı sayısı**, **boolean_s**, ve **string_s**. Bu giriş, her ilk değeri bir dize olarak biçimlendirilmiş:

![Örnek kayıt 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a>Veri sınırları
Günlük analizi veri toplama API gönderilen veriler etrafında bazı kısıtlamalar vardır.

* Günlük analizi veri toplayıcı API post başına en fazla 30 MB. Tek bir posta için boyut sınırı budur. Tek bir veri gönderme, 30 MB aşıyor, küçük boyutlu öbekleri kadar veri bölme ve eşzamanlı olarak gönderin.
* En fazla 32 KB sınırını alan değerleri için. Alan değeri 32 KB'den büyükse, verileri kesilecek.
* Önerilen alanı sayısı için belirli bir türde 50'dir. Kullanılabilirlik ve arama deneyimi perspektifinden pratik sınır budur.  

## <a name="return-codes"></a>Dönüş kodları
HTTP durum kodu 200 istek işleme için alındı anlamına gelir. Bu işlemin başarıyla tamamlandığını belirtir.

Bu tabloda tamamını hizmet döndürebilir durum kodları listelenmektedir:

| Kod | Durum | Hata kodu | Açıklama |
|:--- |:--- |:--- |:--- |
| 200 |Tamam | |İsteği başarıyla kabul edildi. |
| 400 |Hatalı istek |InactiveCustomer |Çalışma alanı kapatıldı. |
| 400 |Hatalı istek |InvalidApiVersion |Belirtilen API sürümü, hizmet tarafından tanınmadı. |
| 400 |Hatalı istek |InvalidCustomerId |Belirtilen çalışma alanı kimliği geçersiz. |
| 400 |Hatalı istek |InvalidDataFormat |Geçersiz JSON gönderildi. Yanıt gövdesi hatanın nasıl düzeltileceği hakkında daha fazla bilgi içerebilir. |
| 400 |Hatalı istek |InvalidLogType |Kapsanan özel karakter veya sayı belirtilen günlük türü. |
| 400 |Hatalı istek |MissingApiVersion |API sürümü belirtilmedi. |
| 400 |Hatalı istek |MissingContentType |İçerik türü belirtilmedi. |
| 400 |Hatalı istek |MissingLogType |Gerekli değer günlük türü belirtilmedi. |
| 400 |Hatalı istek |UnsupportedContentType |İçerik türü ayarlanmadı **uygulama/json**. |
| 403 |Yasak |InvalidAuthorization |Hizmet, isteğin kimliğini doğrulayamadı. Çalışma alanı kimliği ve bağlantı anahtarı geçerli olduğunu doğrulayın. |
| 404 |Bulunamadı | | Sağlanan URL yanlış ya da istek çok büyük. |
| 429 |Çok fazla istek | | Hizmet hesabınızdan veri hacmi yüksek yaşıyor. Lütfen isteği daha sonra yeniden deneyin. |
| 500 |İç sunucu hatası |UnspecifiedError |Hizmet dahili bir hatayla karşılaştı. Lütfen isteği yeniden deneyin. |
| 503 |Hizmet Kullanılamıyor |ServiceUnavailable |Hizmet isteklerini almak şu anda kullanılamıyor. Lütfen isteğinizi yeniden deneyin. |

## <a name="query-data"></a>Verileri sorgulama
Günlük analizi HTTP veri toplayıcı API, arama ile kayıt tarafından gönderilen verileri sorgulayamadı **türü** eşit olan **LogType** , belirttiğiniz değer eklenmiş olan **_CL**. Örneğin, kullandığınız **MyCustomLog**, tüm kayıtları döndürecekti sonra **türü MyCustomLog_CL =**.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.

> `MyCustomLog_CL`

## <a name="sample-requests"></a>Örnek istek
Sonraki bölümlerde, farklı programlama dillerini kullanarak günlük analizi HTTP veri toplayıcı API veri göndermek nasıl örnekleri bulabilirsiniz.

Her bir örnek için yetkilendirme üst bilgisi için değişkenleri ayarlamak için bu adımları uygulayın:

1. Azure portalında günlük analizi çalışma alanınız bulun.
2. Seçin **Gelişmiş ayarları** ve ardından **bağlı kaynakları**.
2. Sağındaki **çalışma alanı kimliği**Kopyala simgesini seçin ve kimlik değeri olarak yapıştırın **Müşteri Kimliği** değişkeni.
3. Sağındaki **birincil anahtar**Kopyala simgesini seçin ve kimlik değeri olarak yapıştırın **paylaşılan anahtar** değişkeni.

Alternatif olarak, günlük türü ve JSON verilerini değişkenleri değiştirebilirsiniz.

### <a name="powershell-sample"></a>PowerShell örnek
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


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
        -fileName $fileName `
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
```
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

        // LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            var jsonBytes = Encoding.UTF8.GetBytes(message);
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

### <a name="python-sample"></a>Python örneği
```
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

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [günlük arama API](log-analytics-log-search-api.md) günlük analizi depodan veri alınamadı.
