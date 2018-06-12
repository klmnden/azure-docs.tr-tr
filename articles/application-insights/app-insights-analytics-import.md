---
title: Azure Application Insights analizleri için verilerinizi alma | Microsoft Docs
description: Uygulama telemetri ile katılmak için statik verileri almak veya Analytics sorgusu için ayrı veri akışı alın.
services: application-insights
keywords: Şema, veri alma açın
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: 688d620e19a8a6f536d134d9c4d7c837ec06bbdc
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293630"
---
# <a name="import-data-into-analytics"></a>İçeri aktarma veri analizi

Hiç tablo verisi içine alın [Analytics](app-insights-analytics.md), kendisiyle katılmaya ya da [Application Insights](app-insights-overview.md) , uygulamanızdan alınan telemetri veya böylece ayrı bir akış olarak çözümleyebilirsiniz. Analytics telemetri yüksek hacimli zaman damgalı akışları çözümleme için oldukça uygun bir güçlü sorgu dildir.

Veri kendi Şeması'nı kullanarak Analytics aktarabilirsiniz. İstek veya izleme gibi standart Application Insights şemalar kullanılacak sahip değil.

JSON veya DSV (sınırlayıcı virgülle ayrılmış değerler - virgül, noktalı virgül veya sekme) içe aktarabilirsiniz dosyaları.

Analitik alma yararlı olduğu üç durumlar vardır:

* **Uygulama telemetriyle katılın.** Örneğin, URL'ler, Web sitesinden daha okunabilir sayfa başlıkları eşleşen bir tablo içe aktarılamadı. Analizleri, on en popüler sayfa, Web sitenizdeki gösteren bir Pano grafiği rapor oluşturabilirsiniz. Şimdi bu URL'leri yerine sayfa başlıklarını gösterebilir.
* **Uygulama telemetrinin bağıntısını** ağ trafiğini gibi diğer kaynaklar ile sunucu verilerini ya da CDN günlük dosyaları.
* **Analiz için ayrı veri akışı uygulayın.** Uygulama Öngörüler Analytics seyrek, zaman damgalı akışları - SQL birçok durumda daha iyi ile iyi çalışan bir güçlü aracıdır. Bu tür bir akış başka bir kaynak sunucudan varsa Analytics ile analiz edebilirsiniz.

Veri kaynağına veri gönderilirken kolaydır. 

1. (Bir saat) Verilerinizi 'veri kaynağında' şeması tanımlayın.
2. (Düzenli aralıklarla) Verilerinizi Azure Storage'a yükler ve yeni veri alımı için bekliyor bize bildirin için REST API çağrısı. Birkaç dakika içinde veri analizi sorgu için kullanılabilir.

Karşıya yükleme frekansı sizin tarafınızdan ve ne kadar hızlı verilerinizi sorgular için kullanılabilir olmasını istediğiniz tanımlanır. Daha büyük öbek, ancak 1 GB'den büyük verileri yüklemek için daha verimli olur.

> [!NOTE]
> *Çok sayıda çözümlemek için veri kaynakları var mı?* [*Kullanmayı* logstash *verilerinizi uygulama Öngörüler sevk etmek.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Başlamadan önce

Gerekenler:

1. Microsoft Azure Application Insights kaynağı.

 * Verilerinizden ayrı olarak diğer telemetri analiz etmek isterseniz [yeni bir Application Insights kaynağı oluşturma](app-insights-create-new-resource.md).
 * Birleştirme ya da Application Insights ile zaten ayarlanmış bir uygulamadan telemetri verilerinizle karşılaştırma, bu uygulama için kaynak kullanabilirsiniz.
 * Bu kaynak katkıda bulunan veya sahibi erişim.
 
2. Azure depolama. Azure depolama alanına yükleme ve analiz verilerinizi buradan alır. 

 * Bloblarınızın için ayrılmış depolama hesabı oluşturma öneririz. Diğer işlemlerle bloblarınızın paylaştırılmışsa bloblarınızın okumak bizim işlemleri için daha uzun sürer.


## <a name="define-your-schema"></a>Şemanızı tanımlayın

Veri almadan önce tanımlamalısınız bir *veri kaynağı* verilerinizi şeması belirtir.
Tek bir Application Insights kaynağı en fazla 50 veri kaynaklarına sahip olabilir

1. Veri kaynağı Sihirbazı'nı başlatın. "Yeni veri kaynağı Ekle" düğmesini kullanın. Alternatif olarak - sağ üst köşedeki ayarlar düğmesine tıklayın ve açılan menüde "Veri kaynakları" seçin.

    ![Yeni veri kaynağı ekleme](./media/app-insights-analytics-import/add-new-data-source.png)

    Yeni veri kaynağı için bir ad sağlayın.

2. Karşıya yükleyecek dosyalarının biçimi tanımlayın.

    Biçim manuel olarak tanımlamak veya bir örnek dosya karşıya yükleme.

    Veri CSV biçiminde ise, örnek ilk satırında sütun üst bilgileri olabilir. Sonraki adımda alan adlarını değiştirebilirsiniz.

    Örnek, en az 10 satır veya veri kayıtlarının içermesi gerekir.

    Sütun veya alan adı alfasayısal adlarını (olmadan, boşluk veya noktalama) olması gerekir.

    ![Örnek dosya karşıya yükleme](./media/app-insights-analytics-import/sample-data-file.png)


3. Sihirbaz var şema gözden geçirin. Bir örnek türlerinden çıkarımı yapılan sütunların oluşturulursa türlerini ayarlamak gerekebilir.

    ![Çıkarsanan şema gözden geçirin](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (İsteğe bağlı.) Bir şema tanımı karşıya yükleyin. Aşağıdaki biçimde bakın.

 * Bir zaman damgası seçin. Tüm veriler Analytics, zaman damgası alanı olması gerekir. Türü olmalıdır `datetime`, ancak 'timestamp' adlandırılmış olması sahip değil. Verilerinizi bir tarih ve saati ISO biçiminde içeren bir sütun varsa, bu zaman damgası sütunu olarak seçin. Aksi takdirde, "veri olarak gelen" seçin ve içeri aktarma işlemi zaman damgası alanı ekler.

5. Veri kaynağı oluşturun.

### <a name="schema-definition-file-format"></a>Şema tanımlama dosyası biçimi

UI şemada düzenlemek yerine, bir dosyanın şema tanımını yükleyebilirsiniz. Şema tanım biçimi aşağıdaki gibidir: 

Sınırlandırılmış biçimi 
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
 
Her sütun, adını ve konumunu türü tarafından tanımlanır. 

* Konum – ayrılmış dosya biçimlendirmek için eşlenen değer konumu değil. JSON için onu eşlenen anahtar jpath biçimidir.
* Adı – sütunun görüntülenen adı.
* Türü – bu sütunun veri türü.
 
Örnek verileri kullanıldı ve dosya biçimi ayrılmış durumda, şema tanımı tüm sütunları eşleme ve sonunda yeni sütunlar eklemeniz gerekir. 

JSON veri kısmi bir eşleme sağlar, bu nedenle örnek verilerde bulunan her anahtar eşlemek JSON biçimini şema tanımı yok. Ayrıca, örnek verileri parçası olmayan sütunları de eşleyebilirsiniz. 

## <a name="import-data"></a>Veri içeri aktarma

Veri almak için Azure depolama alanına yükleme, bir erişim anahtarı oluşturmak ve ardından bir REST API çağrısı yapın.

![Yeni veri kaynağı ekleme](./media/app-insights-analytics-import/analytics-upload-process.png)

El ile aşağıdaki işlemi gerçekleştirin veya düzenli aralıklarla yapmak için otomatikleştirilmiş bir sistemi ayarlarsınız. Veri içeri aktarmak istediğiniz her bir bloğunda için aşağıdaki adımları izlemeniz gerekir.

1. Verileri karşıya [Azure blob depolama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * BLOB'ları olabilir herhangi en fazla 1 GB sıkıştırılmamış boyut. Yüzlerce MB büyük BLOB'lar performans açısından idealdir.
 * Karşıya yükleme zamanı ve verilerin sorgu için kullanılabilir olması gecikme süresini artırmak için Gzip ile sıkıştırabilirsiniz. Kullanım `.gz` dosya adı uzantısı.
 * Farklı hizmetlerde performans yavaşlamasının gelen çağrıları önlemek için bu amaç için ayrı bir depolama hesabı kullanmak en iyisidir.
 * Yüksek yoğunlukta veri gönderirken, her birkaç saniyede birden fazla depolama hesabı, performans nedenleriyle kullanmak için önerilir.

 
2. [Blob için bir paylaşılan erişim imzası anahtar oluşturma](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). Bu anahtar, bir sona erme süresi bir gün sahip ve okuma erişimi sağlar.
3. Veri bekliyor Application Insights bildirmek için REST çağrısı yapın.

 * Uç noktası: `https://dc.services.visualstudio.com/v2/track`
 * HTTP yöntemini: POST
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

* `Blob URI with Shared Access Key`: Bu yordamdan bir anahtar oluşturmak için size. Blob özeldir.
* `Schema ID`:, Tanımlı bir şeması için oluşturulan şema kimliği. Bu blob verileri şemaya uygun olmalıdır.
* `DateTime`: Hangi istek gönderildikten saati UTC. Biz bu biçimlerini kabul: ISO8601 (gibi "2016-01-01 13:45:01"); Rfc822 ("Çar, 14 Ara 16 14:57:01 + 0000"); RFC850 ("Çarşamba, 14 Ara 16 14:57:00 UTC"); RFC1123 ("Çar, 14 Ara 2016 14:57:00 + 0000").
* `Instrumentation key` Application Insights kaynağınıza.

Veri analizleri birkaç dakika sonra kullanılabilir.

## <a name="error-responses"></a>Hata yanıtları

* **400 Hatalı istek**: isteği yükü geçersiz olduğunu belirtir. Denetleme:
 * Doğru izleme anahtarı.
 * Geçerli saat değeri. Şimdi, UTC saat olmalıdır.
 * JSON olayın şemasıyla uyumlu.
* **403 Yasak**: gönderilen blob erişilebilir değil. Paylaşılan erişim anahtarı geçerli değil ve süresi geçmemiş emin olun.
* **404 Bulunamadı**:
 * Blob yok.
 * SourceId yanlış olur.

Yanıt hata iletisinde daha ayrıntılı bilgi bulabilirsiniz.


## <a name="sample-code"></a>Örnek kod

Bu kodu kullanır [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet paketi.

### <a name="classes"></a>Sınıfları

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

Bu kodu her bir blob için kullanın. 

```csharp
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Günlük analizi sorgu dili turu](app-insights-analytics-tour.md)
* Logstash kullanıyorsanız, [Logstash eklentisi için Application Insights veri göndermek için](https://github.com/Microsoft/logstash-output-application-insights)
