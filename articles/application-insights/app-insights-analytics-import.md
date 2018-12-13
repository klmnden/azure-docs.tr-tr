---
title: Azure Application Insights analiz için verilerinizi içeri aktarma | Microsoft Docs
description: Ayrı veri akışını analiz ile sorgu aktarın veya uygulama telemetrisi ile birleştirmek için statik veri içeri aktarın.
services: application-insights
keywords: şeması, verileri içeri aktarma
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: mbullwin
ms.openlocfilehash: 7160232cf511226b26c15e6476ff75fcdf5ad33e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52866609"
---
# <a name="import-data-into-analytics"></a>Analiz verilerini içeri aktarma

Herhangi bir tablo verileri içine aktarmak [Analytics](app-insights-analytics.md), ile katılmak için ya da [Application Insights](app-insights-overview.md) , uygulamanızdan alınan telemetri veya ayrı bir akış olarak analiz etmek. Analytics telemetri akışlarını yüksek hacimli zaman damgalı çözümleme için uygun bir güçlü sorgu dilidir.
Kendi şemanızı kullanarak Analytics'e verileri içeri aktarabilirsiniz. İstek veya izleme gibi standart Application Insights şemaları kullanın gerekmez.

JSON veya DSV (sınırlayıcı sekmeyle ayrılmış değerler - virgül, noktalı virgül veya sekme) içe aktarabilirsiniz dosyaları.

> [!IMPORTANT]
> Bu makalede olmuştur **kullanım dışı**. Log Analytics'e veri almaya ilişkin önerilen aracılığıyla yöntemdir [Log Analytics Veri Toplayıcı API'si.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api)

Analiz için içeri aktarma kullanışlı olduğu üç durumlar vardır:

* **Uygulama telemetrisi ile katılın.** Örneğin, URL'leri, Web sitesinden daha okunabilir sayfa başlığı eşleyen tablo içe aktaramadı. Analytics'te Web sitenize on en popüler sayfa gösteren bir Pano grafiği rapor oluşturabilirsiniz. Artık bu URL'leri yerine sayfa başlıklarının gösterebilirsiniz.
* **Uygulama telemetrinizi bağıntısını** ağ trafiği gibi diğer kaynaklar ile sunucu verileri veya CDN günlük dosyaları.
* **Analytics, ayrı bir veri akışı için geçerlidir.** Application Insights Analytics, seyrek olarak zaman damgalı akışları - SQL çoğu durumda çok daha iyi çalışan güçlü bir araç olan. Böyle bir akışa başka bir kaynaktan varsa, Analytics ile çözümleyebilirsiniz.

Veri kaynağınıza veri göndermek çok kolaydır. 

1. (Bir kez) 'Veri kaynağında' verilerinizin şemasını tanımlar.
2. (Düzenli aralıklarla) Verilerinizi Azure Depolama'ya yükler ve yeni veri alımı için bekleyen bize bildirmek için REST API çağrısı. Birkaç dakika içinde veri analytics'te sorgu için kullanılabilir.

Karşıya yükleme sıklığını, tarafından ve ne kadar hızlı sorgular için kullanılabilir olması için verilerinizi istediğiniz tanımlanır. Daha büyük öbekler halinde, ancak 1 GB'tan büyük veri yüklemek için daha verimlidir.

> [!NOTE]
> *Analiz etmek için veri kaynakları çok sayıda var mı?* [*Kullanmayı* logstash *Application Insights'a veri göndermeye.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Başlamadan önce

Gerekenler:

1. Microsoft azure'da Application Insights kaynağı.

 * Verilerinizden ayrı olarak diğer telemetriyi analiz etmek istiyorsanız [yeni bir Application Insights kaynağı oluşturun](app-insights-create-new-resource.md).
 * Birleştirme ya da Application Insights ile önceden kurulmuş bir uygulamadan telemetri ile verilerinizi karşılaştırma, bu uygulama için kaynak kullanabilirsiniz.
 * Bu kaynağa erişim katkıda bulunan veya sahip.
 
2. Azure depolama. Azure depolama alanına yükleyin ve Analytics verilerinizi buradan alır. 

 * Bloblarınızın için ayrılmış depolama hesabı oluşturma öneririz. Bloblarınızın diğer işlemlerle paylaşılır, bloblarınızın okumak sunduğumuz işlemleri için daha uzun sürer.


## <a name="define-your-schema"></a>Şemanızı tanımlayın

Verileri içeri aktarmadan önce tanımlamalısınız bir *veri kaynağı* verilerinizin şemasını belirtir.
Tek bir Application Insights kaynağı 50 adede kadar veri kaynaklarında olabilir

1. Veri kaynağı Sihirbazı başlatın. "Yeni veri kaynağı Ekle" düğmesini kullanın. Alternatif olarak - sağ üst köşedeki ayarları düğmesine tıklayın ve açılan menüde "Veri kaynakları" öğesini seçin.

    ![Yeni veri kaynağı Ekle](./media/app-insights-analytics-import/add-new-data-source.png)

    Yeni veri kaynağınız için bir ad sağlayın.

2. Karşıya yükleyeceğiniz dosyalarının biçimi tanımlayın.

    Biçim manuel olarak tanımlamak veya örnek dosya yükleyin.

    CSV biçiminde verilerin ise örnek ilk satırını sütun üst bilgilerini olabilir. Sonraki adımda alan adlarını değiştirebilirsiniz.

    Örnek en az 10 satır veya veri kayıtlarının içermelidir.

    Sütun veya alan adları (olmadan, boşluk veya noktalama işareti) alfasayısal adlara sahip olmalıdır.

    ![Örnek dosya yükleyin](./media/app-insights-analytics-import/sample-data-file.png)


3. Sihirbaz var şema gözden geçirin. Bir örnek türlerinden çıkarılan sütunları çıkarsanan tür ayarlamak gerekebilir.

    ![Çıkarsanan şema gözden geçirin](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (İsteğe bağlı.) Bir şema tanımı karşıya yükleyin. Aşağıdaki biçimde bakın.

 * Bir zaman damgası'ı seçin. Analytics'te tüm verileri, zaman damgası alanı olması gerekir. Türü olmalıdır `datetime`, ancak 'timestamp' olarak adlandırılmasını gerekli değildir. Verilerinizi bir tarih ve saati ISO biçiminde içeren bir sütun varsa, bunu zaman damgası sütununu seçin. Aksi takdirde, "veri gelen"'i seçin ve içeri aktarma işlemi zaman damgası alanı ekler.

5. Veri kaynağı oluşturun.

### <a name="schema-definition-file-format"></a>Şema tanımlama dosyası biçimi

Şema kullanıcı arabiriminde düzenleme yerine, bir dosyadan şema tanımı yükleyebilirsiniz. Şema tanımı biçimi aşağıdaki gibidir: 

Ayrılmış biçimi 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

JSON biçimi 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
Her sütun, konum, adı ve türüne göre tanımlanır.

* Konumu - ayrılmış dosya biçimlendirmek için eşlenen değer konumunu şeklindedir. JSON biçimi, bu eşlenen anahtarının jpath değil.
* Adı - sütunun görüntülenen adı.
* Türü - bu sütunun veri türü.

> [!NOTE]
> Örnek verileri kullanıldı ve dosya biçimi ayrılmış durumda şema tanımı tüm sütunları eşlemeniz ve sonunda yeni sütunlar eklemeniz gerekir.
> 
> JSON verileri kısmi bir eşleme sağlar, bu nedenle örnek verileri bulunan her anahtar eşleştirmek şema tanımı JSON biçiminde yok. Ayrıca, örnek verileri parçası olmayan sütunları da eşleyebilirsiniz. 

## <a name="import-data"></a>Veri içeri aktarma

Verileri içeri aktarmak için Azure depolamaya yükleme, bir erişim anahtarı oluşturun ve sonra bir REST API çağrısı yapın.

![Yeni veri kaynağı Ekle](./media/app-insights-analytics-import/analytics-upload-process.png)

El ile aşağıdaki işlemi gerçekleştirin veya düzenli aralıklarla yapmak için bir Otomatik Sistem ayarlayın. Her içeri aktarmak istediğiniz veri bloğu için şu adımları izlemesi gerekir.

1. Verileri karşıya yükleme [Azure blob depolama](../storage/blobs/storage-quickstart-blobs-dotnet.md). 

 * Blobları olabilir herhangi sıkıştırılmamış en fazla 1 GB boyut. Büyük BLOB'ları, yüzlerce MB, performans açısından bakılırsa idealdir.
 * Karşıya yükleme zamanı ve gecikme süresi verilerin sorgu için kullanılabilir geliştirmek için Gzip ile sıkıştırabilirsiniz. Kullanım `.gz` dosya adı uzantısı.
 * Farklı Hizmetleri performans düşmesi çağrılarından önlemek için bu amaç için ayrı bir depolama hesabı kullanmak en iyisidir.
 * Veri yüksek frekanslı gönderirken her birkaç saniyede performansla ilgili nedenlerden dolayı birden fazla depolama hesabı kullanmak için önerilir.

 
2. [Blob için paylaşılan erişim imzası anahtar oluşturma](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). Bu anahtar, bir günlük bir zaman aşımı süresine sahip olacak ve okuma erişimi sağlar.
3. Application Insights, veri bekleniyor bildirmek için REST çağrısı yapın.

 * Uç noktası: `https://dc.services.visualstudio.com/v2/track`
 * HTTP yöntemi: POST
 * Yük:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

Yer tutucuları şunlardır:

* `Blob URI with Shared Access Key`: Bu yordamdan bir anahtar oluşturmak için sahip olursunuz. Blob için özeldir.
* `Schema ID`:, Tanımlı bir şeması için üretilen şema kimliği. Bu BLOB veri şemasına uygun olmalıdır.
* `DateTime`: Başlangıçtan istek gönderildikten saati UTC. Biz bu biçimlerini kabul: ISO8601 (gibi "2016-01-01 13:45:01"); Rfc822 ("Çarşamba, 14 Ara 16 14:57:01 + 0000"); RFC850 ("Çarşamba, 14 Ara 16 14:57:00 UTC"); RFC1123 ("Çarşamba, 14 Aralık 2016 14:57:00 + 0000").
* `Instrumentation key` Application Insights kaynağınıza.

Veriler, birkaç dakika sonra Analytics'te kullanılabilir.

## <a name="error-responses"></a>Hata yanıtları

* **400 Hatalı istek**: isteği yükü geçersiz olduğunu gösterir. Kontrol edin:
 * Doğru izleme anahtarı.
 * Geçerli saat değeri. Şimdi, UTC zamanı olmalıdır.
 * Olayın bir JSON şemaya uygun.
* **403 Yasak**: gönderdiğiniz blobu erişilebilir değil. Paylaşılan erişim anahtarının geçerli olduğundan ve sona ermediğinden emin olun.
* **404 Bulunamadı Hatası**:
 * Blob yok.
 * SourceId yanlış olur.

Daha ayrıntılı bilgi yanıt hata iletisinde kullanılabilir.


## <a name="sample-code"></a>Örnek kod

Bu kod [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet paketi.

### <a name="classes"></a>Sınıflar

```csharp
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a>Veriyi çekme

Bu kod, her blob için kullanın. 

```csharp
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Log Analytics sorgu dili turu](../azure-monitor/log-query/get-started-portal.md)
* Logstash kullanıyorsanız, [Logstash eklentisini verilerini Application Insights'a gönderme](https://github.com/Microsoft/logstash-output-application-insights)
