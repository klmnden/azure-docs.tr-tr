---
title: Azure İzleyici veri toplayıcı API'si ile veri işlem hattı oluşturma | Microsoft Docs
description: POST JSON verileri REST API çağrısı herhangi bir istemciden Log Analytics çalışma alanınıza eklemek için Azure İzleyici HTTP veri toplayıcı API'sini kullanabilirsiniz. Bu makalede, otomatik bir şekilde dosyalarında depolanan verileri karşıya yükleme işlemini açıklar.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/09/2018
ms.author: magoedte
ms.openlocfilehash: 53457a044f5c69af7bf68561f24732e8f02219d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65603242"
---
# <a name="create-a-data-pipeline-with-the-data-collector-api"></a>Veri Toplayıcı API'si ile veri işlem hattı oluşturma

[Azure İzleyici, veri toplayıcı API'sini](data-collector-api.md) Azure İzleyici'de bir Log Analytics çalışma alanına herhangi bir özel günlük veri almanızı sağlar. Verileri JSON biçimli ve 30 MB veya parçaları daha az Böl tek gerekenlerdir. İçine birçok şekilde takılabilen tamamen esnek bir mekanizma budur: doğrudan uygulamanızdan gönderilen verilerden için tek seferlik geçici yükler. Bu makalede, yaygın bir senaryo için bazı başlangıç noktaları özetler: normal, otomatik olarak dosyalarında depolanan verileri karşıya yüklemek için gereken. İşlem hattı sunulan burada kullanılamaz en yüksek performanslı veya aksi halde en iyi duruma getirilmiş, bu bir üretim hattının kendi oluşturmaya yönelik bir başlangıç noktası olarak hizmet verecek içindir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="example-problem"></a>Örnek sorun
Bu makalenin geri kalanında için sayfa görüntüleme verilerini Application ınsights'da inceleyeceğiz. Özel verilere dünyanın her ülke/bölge popülasyonu burada biz harcama belirlemek amacıyla içeren Application Insights SDK'sı tarafından varsayılan olarak toplanan coğrafi bilgileri ilişkilendirmek istediğimiz kuramsal senaryomuzdaki ise en pazarlama dolar. 

Genel veri kaynağı gibi kullandığımız [kaldırma dünya popülasyon yaklaşımları](https://esa.un.org/unpd/wpp/) bu amaç için. Veriler aşağıdaki basit şema olacaktır:

![Örnek basit şema](./media/create-pipeline-datacollector-api/example-simple-schema-01.png)

Bizim örneğimizde, kullanılabilir duruma geldiğinde size yeni bir dosya ile en son yılın verilerini yükleyeceği varsayıyoruz.

## <a name="general-design"></a>Genel Tasarım
Bizim ardışık düzen tasarlamak için bir Klasik ETL türü mantıksal kullanıyoruz. Mimari aşağıdaki gibi görünür:

![Veri toplama işlem hattı mimarisi](./media/create-pipeline-datacollector-api/data-pipeline-dataflow-architecture.png)

Bu makalede veri oluşturma kapsamaz veya [bir Azure Blob Depolama hesabına yükleme](../../storage/blobs/storage-upload-process-images.md). Yeni bir dosya için blob karşıya yüklendikten hemen sonra bunun yerine, biz akışı değiştirebilmenizdir. Buradan:

1. İşlem, yeni verilerin yüklendiğini algılar.  Örneğimizde bir [Azure Logic App](../../logic-apps/logic-apps-overview.md), kullanılabilir olan yeni veriler bir blob olarak karşıya yüklenen algılamak için bir tetikleyici.

2. Bir işlemci bu yeni verileri okur ve JSON değerine dönüştüren, biçimi Azure İzleyici ile bu örnek gerekli, kullandığımız bir [Azure işlevi](../../azure-functions/functions-overview.md) işleme kodumuz yürütmenin basit ve ekonomik bir yolu olarak. İşlev algılamak için kullandığımız aynı mantıksal uygulama tarafından başlatılır yeni bir veri.

3. Son olarak, JSON nesnesi kullanılabilir duruma geldikten sonra Azure İzleyicisi'ne gönderilir. Aynı mantıksal uygulama, Azure İzleyici oluşturulmuş bir Log Analytics Veri Toplayıcı etkinliği kullanarak verileri gönderir.

Blob depolama, mantıksal uygulama veya Azure işlevi, ayrıntılı Kurulum, bu makalede belirtilmeyen karşın, belirli ürünlerin sayfalarında ayrıntılı yönergeler kullanılabilir.

Bu işlem hattını izlemek için Application Insights, Azure işlevi izlemek için kullanırız [burada ayrıntıları](../../azure-functions/functions-monitoring.md)ve mantıksal uygulamamızı izlemek için Azure İzleyici [burada ayrıntıları](../../logic-apps/logic-apps-monitor-your-logic-apps-oms.md). 

## <a name="setting-up-the-pipeline"></a>İşlem hattı ayarlama
İşlem hattı ayarlamak için ilk oluşturduğunuza ve yapılandırdığınıza, blob kapsayıcısına sahip emin olun. Benzer şekilde, veri göndermesini istediğiniz Log Analytics çalışma alanı oluşturulduktan emin olun.

## <a name="ingesting-json-data"></a>JSON veri alma
Logic Apps ile Önemsiz JSON verilerini almak ve hiçbir dönüştürme gerçekleşmesi gerekli olduğundan, biz işlem hattının tamamı tek bir mantıksal uygulamada parantezleri içerir durumda. Blob kapsayıcısı hem de Log Analytics çalışma alanı yapılandırıldıktan sonra yeni bir mantıksal uygulama oluşturun ve şu şekilde yapılandırın:

![Logic apps iş akışı örneği](./media/create-pipeline-datacollector-api/logic-apps-workflow-example-01.png)

Mantıksal uygulamanızı kaydedin ve test etmek için devam edin.

## <a name="ingesting-xml-csv-or-other-formats-of-data"></a>XML, CSV veya diğer biçimlere veri almak
Logic Apps bugün kolayca XML, CSV veya diğer türleri JSON biçimine dönüştürmek için yerleşik özelliklere sahip değil. Bu nedenle, biz bu dönüşümü tamamlamak için başka bir araç kullanmanız gerekir. Bu makale için Bunun yapılması, bir çok hafif ve maliyet kolay şekilde Azure işlevleri'nin sunucusuz hesaplama özellikleri kullanırız. 

Bu örnekte, biz bir CSV dosyası ayrıştırılamadı, ancak herhangi bir dosya türünü benzer şekilde işlenebilir. Yalnızca seri durumdan çıkarılırken bölümü Azure işlevi, özel veri türü için doğru mantıksal yansıtacak şekilde değiştirin.

1.  Yeni bir Azure işlev çalışma zamanı v1 ile Tüketim tabanlı olduğunda işlev, oluşturma istenir.  Seçin **HTTP tetikleyicisi** hedeflenmiş kılarız gibi bağlamalarınızı yapılandırır bir başlangıç noktası olarak C# şablonu. 
2.  Gelen **dosyaları görüntüle** adlı yeni bir dosya oluşturun, sağ bölmede sekme **project.json** kullanıyoruz NuGet paketlerini aşağıdaki kodu yapıştırın:

    ![Azure işlevleri örnek proje](./media/create-pipeline-datacollector-api/functions-example-project-01.png)
    
    ``` JSON
    {
      "frameworks": {
        "net46":{
          "dependencies": {
            "CsvHelper": "7.1.1",
            "Newtonsoft.Json": "11.0.2"
          }  
        }  
       }  
     }  
    ```

3. Geçiş **run.csx** sağ bölme ve varsayılan kodu şu kodla değiştirin. 

    >[!NOTE]
    >Projeniz için kayıt modeli ("PopulationRecord" sınıf) ile kendi veri şemasını değiştirmeniz gerekir.
    >

    ```   
    using System.Net;
    using Newtonsoft.Json;
    using CsvHelper;
    
    class PopulationRecord
    {
        public String Location { get; set; }
        public int Time { get; set; }
        public long Population { get; set; }
    }

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        string filePath = await req.Content.ReadAsStringAsync(); //get the CSV URI being passed from Logic App
        string response = "";

        //get a stream from blob
        WebClient wc = new WebClient();
        Stream s = wc.OpenRead(filePath);         

        //read the stream
        using (var sr = new StreamReader(s))
        {
            var csvReader = new CsvReader(sr);
    
            var records = csvReader.GetRecords<PopulationRecord>(); //deserialize the CSV stream as an IEnumerable
    
            response = JsonConvert.SerializeObject(records); //serialize the IEnumerable back into JSON
        }    

        return response == null
            ? req.CreateResponse(HttpStatusCode.BadRequest, "There was an issue getting data")
            : req.CreateResponse(HttpStatusCode.OK, response);
     }  
    ```

4. İşlevinizi kaydedin.
5. İşlev kodunu doğru şekilde çalıştığından emin olmak için test edin. Geçiş **Test** sekmesine sağ bölmede test şu şekilde yapılandırma. Örnek verileriniz ile bir blob için bir bağlantı yerleştirin **istek gövdesi** metin. ' I tıklattıktan sonra **çalıştırmak**, JSON görmelisiniz çıkış **çıkış** kutusu:

    ![İşlev uygulamaları test kodu](./media/create-pipeline-datacollector-api/functions-test-01.png)

Şimdi geri dönüp alınır ve JSON biçimine dönüştürülen verileri dahil etmek için daha önce oluşturmaya başladığımız mantıksal uygulamayı değiştirmek ihtiyacımız var.  Görünüm Tasarımcısı'nı kullanarak, aşağıda açıklandığı şekilde yapılandırın ve ardından mantıksal uygulamanızı kaydedin:

![Logic Apps iş akışı tam örnek](./media/create-pipeline-datacollector-api/logic-apps-workflow-example-02.png)

## <a name="testing-the-pipeline"></a>İşlem hattını test etme
Şimdi yeni bir dosya önceden yapılandırdığınız bloba yüklemek ve mantıksal uygulamanız tarafından izlenen sahip. Yakında, mantıksal uygulama clark'i yeni bir örneğini görmek Azure işlevinizi duyurmak ve verileri Azure İzleyicisi'ne başarıyla Gönder gerekir. 

>[!NOTE]
>Bu, verilerin Azure İzleyici'de yeni bir veri türü gönderdiğiniz ilk kez görünmesi 30 dakikaya kadar sürebilir.


## <a name="correlating-with-other-data-in-log-analytics-and-application-insights"></a>Log Analytics ve Application Insights diğer verilerle ilişkilendirmek
Application Insights sayfa görünümü verilerini size sunduğumuz özel veri kaynağından alınan nüfus verileri ile ilişkilendirmek, Hedefimiz tamamlamak için Application Insights Analytics penceresinden veya Log Analytics çalışma alanı'ndan aşağıdaki sorguyu çalıştırın:

``` KQL
app("fabrikamprod").pageViews
| summarize numUsers = count() by client_CountryOrRegion
| join kind=leftouter (
   workspace("customdatademo").Population_CL
) on $left.client_CountryOrRegion == $right.Location_s
| project client_CountryOrRegion, numUsers, Population_d
```

İki veri çıkış göstermelidir kaynakları artık katılmış.  

![Arama sonucu örnek verileri üyeliğinden ilişkilendirme](./media/create-pipeline-datacollector-api/correlating-disjoined-data-example-01.png)

## <a name="suggested-improvements-for-a-production-pipeline"></a>Bir üretim hattının iyileştirme önerileri
Bu makalede, ardındaki mantığı doğru üretim kalitesinde çözüm uygulanabilir bir çalışma prototip sunulur. Böyle bir üretim kalitesinde çözümü için aşağıdaki geliştirmeleri önerilir:

* Hata işleme ekleyin ve yeniden deneme mantığı, mantıksal uygulama ve işlev.
* 30 MB/tek Log Analytics alımı API çağrısı sınırı aşılmadığını emin olmak için mantık ekleyin. Gerekirse, verileri daha küçük parçalara bölün.
* Blob depolama alanınızın bir temizleme İlkesi ayarlama. Ham verileri arşivleme amacıyla tutmak istediğiniz sürece Log Analytics çalışma alanına başarıyla gönderdikten sonra bunu depolamak için bir neden yoktur. 
* İzleme ve izleme noktaları ekleme tam işlem hattı etkin uyarıları uygun şekilde doğrulayın.
* Mantıksal uygulama ve işlev kodunu yönetmek için kaynak denetimi yararlanın.
* Şema değişirse, işlevi ve Logic Apps uygun şekilde değiştirilir, uygun değişiklik yönetimi ilke düzenin uygulandığından emin olun.
* Birden çok farklı veri türleri yüklüyorsanız, bunları blob kapsayıcınızın içindeki tek tek klasörlere ayırmak ve mantıksal çıkış veri türüne göre fanı mantığını oluşturun. 


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [veri toplayıcı API'sini](data-collector-api.md) herhangi bir REST API istemcisinden Log Analytics çalışma alanına veri yazmak için.
