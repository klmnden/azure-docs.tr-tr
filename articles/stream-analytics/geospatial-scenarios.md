---
title: Azure Stream Analytics ile sona bölge sınırlaması ve Jeo-uzamsal toplama senaryoları
description: Bu makalede, Azure Stream Analytics için bölge sınırlaması ve Jeo-uzamsal toplama kullanmayı açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: cc301855e4cdcb8eb687e753835577399cfe72b0
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58807286"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>Azure Stream Analytics ile sona bölge sınırlaması ve Jeo-uzamsal toplama senaryoları

Yerleşik Jeo-uzamsal işlevleri ile Azure Stream Analytics Filo yönetimi gibi senaryolar için uygulamalar oluşturun, paylaşımı, bağlı arabalar ve varlık izleme bulutumuzda için kullanabilirsiniz.

## <a name="geofencing"></a>Coğrafi Sınırlama

Azure Stream Analytics, bulutta ve IOT Edge çalışma zamanı düşük gecikme süresi gerçek zamanlı bölge sınırlaması hesaplamalar destekler.

### <a name="geofencing-scenario"></a>Bölge sınırlaması senaryosu

Varlıkları kendi bina izlemek bir üretim şirketi gerekir. Bunlar, her cihaz bir GPS ile donatılmış ve bir cihaz belirli bir alanında ayrılırsa, bildirim almak istiyor.

Bu örnekte kullanılan başvuru verilerinin Binalar ve her binalar, izin verilen cihazlar için bölge sınırının bilgilere sahiptir. Başvuru verileri statik veya yavaş ya da olabileceğini unutmayın. Statik başvuru verileri, bu senaryo için kullanılır. Bir veri akışı, cihaz kimliği ve geçerli konumuna sürekli olarak yayar.

### <a name="define-geofences-in-reference-data"></a>Başvuru verilerinde bölge sınırlarını tanımlama

Bir bölge sınırının bir GeoJSON nesnesi kullanılarak tanımlanabilir. 1.2 ve daha yüksek uyumluluk sürümüyle işler için bölge sınırlarının da olarak da bilinen metin (WKT) kullanılarak tanımlanabilir `NVARCHAR(MAX)`. Bir açık Jeo-uzamsal Consortium (uzamsal verileri metin biçiminde göstermek için kullanılan OGC) standart WKT olur.

Yerleşik Jeo-uzamsal İşlevler, tanımlı bölge sınırlarının ya da belirli bir bölge sınırının Çokgen dışında bir öğe olup olmadığını öğrenmek için kullanabilirsiniz.

Aşağıdaki tabloda, Azure blob depolama veya Azure SQL tablosuna depolanabilir döndürürüz başvuru verilerini örneğidir. Her site tarafından bir Jeo-uzamsal Çokgen temsil edilir ve her cihazda bir izin verilen bir site kimliğiyle ilişkilendirilir

|Site Kimliği|Site adı|Geofence|AllowedDeviceID|
|------|--------|--------|---------------|
|1|"Redmond Building 41"|"ÇOKGEN ((-122.1337357922017 47.63782998329432,-122.13373042778369 47.637634793257305,-122.13346757130023 47.637642022530954,-122.13348902897235 47.637508280806806,-122.13361777500506 47.637508280806806,-122.13361241058703 47.63732393354484,-122.13265754417773 47.63730947490855,-122.13266290859576 47.637519124743164,-122.13302232460376 47.637515510097955,-122.13301696018573 47.63764925180358,-122.13272728161212 47.63764925180358,-122.13274873928424 47.63784082716388,-122.13373579220172 47.63782998329432)) "|"B"|
|2|"Redmond Building 40"|"ÇOKGEN ((-122.1336154507967 47.6366745947009,-122.13361008637867 47.636483015064535,-122.13349206918201 47.636479400347675,-122.13349743360004 47.63636372927573,-122.13372810357532 47.63636372927573,-122.13373346799335 47.63617576323771,-122.13263912671528 47.63616491902258,-122.13264985555134 47.63635649982525,-122.13304682248554 47.636367344000604,-122.13305218690357 47.63650831807564,-122.13276250832996 47.636497473929516,-122.13277323716602 47.63668543881025,-122.1336154507967 47.6366745947009)) "|"A"|
|3|"Redmond Building 22"|"ÇOKGEN ((-122.13611660248233 47.63758544698554,-122.13635263687564 47.6374083293018,-122.13622389084293 47.63733603619712,-122.13622389084293 47.63717699101473,-122.13581619507266 47.63692757827657,-122.13559625393344 47.637046862778135,-122.13569281345798 47.637144458985965,-122.13570890671207 47.637314348246214,-122.13611660248233 47.63758544698554)) "|"C"|

### <a name="generate-alerts-with-geofence"></a>Bölge sınırının uyarılarla oluştur

Cihazları kendi kimliği ve konum dakikada adlı akış aracılığıyla yayabilir `DeviceStreamInput`. Aşağıdaki tabloda, giriş akışıdır.

|Cihaz Kimliği|GeoPosition|
|--------|-----------|
|"A"|"POINT(-122.13292341559497 47.636318374032726)"|
|"B"|"POINT(-122.13338475554553 47.63743531308874)"|
|"C"|"POINT(-122.13354001095752 47.63627622505007)"|

Cihaz akışını döndürürüz başvuru verileriyle birleştirir ve izin verilen bir yapı dışında bir cihaz olduğundan her zaman bir uyarı oluşturur. bir sorgu yazabilirsiniz.

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

Aşağıdaki görüntüde, bölge sınırlarının temsil eder. Giriş akış verilerini çerçevesinde cihazları nerede olduğunu görebilirsiniz.

![Yapı bölge sınırlarının](./media/geospatial-scenarios/building-geofences.png)

Cihaz "C" başvuru verileri göre izin verilmeyen kimliği 2, derleme içinde bulunur. Bu cihaz kimliği 3 derleme içinde bulunmalıdır. Bu işi bu belirli ihlali için bir uyarı oluşturur.

### <a name="site-with-multiple-allowed-devices"></a>Birden çok izin verilen cihazlar ile site

Bir siteye birden fazla cihaza izin veriyorsa, cihaz kimlikleri dizisi tanımlanabilir `AllowedDeviceID` ve bir kullanıcı tanımlı işlev üzerinde kullanılabilir `WHERE` yan akış cihaz kimliği, listedeki herhangi bir cihaz kimliği eşleştiğini doğrulayın. Daha fazla bilgi için görüntüleme [Javascript UDF](stream-analytics-javascript-user-defined-functions.md) bulut işleri için öğretici ve [ C# UDF](stream-analytics-edge-csharp-udf.md) edge işleri için öğretici.

## <a name="geospatial-aggregation"></a>Jeo-uzamsal toplama

Azure Stream Analytics, bulutta ve IOT Edge çalışma zamanı düşük gecikme süreli gerçek zamanlı Jeo-uzamsal toplama destekler.

### <a name="geospatial-aggregation-scenario"></a>Jeo-uzamsal toplama senaryosu

Şu anda yüksek talep yaşayan şehirler alanlarının doğru kılma aranıyor cab sürücülerini yol göstermesi için gerçek zamanlı bir uygulama oluşturmak bir cab şirket istemektedir.

Şirket Şehir mantıksal bölge başvuru verilerini depolar. Her bölgede RegionID, RegionName ve bölge sınırı tarafından tanımlanır.

### <a name="define-the-geofences"></a>Bölge sınırlarını tanımlama

Aşağıdaki tabloda, Azure blob depolama veya Azure SQL tablosuna depolanabilir döndürürüz başvuru verilerini örneğidir. Her bölge, akış verilerinden gelen istekleri ile ilişkilendirmek için kullanılan bir Jeo-uzamsal Çokgen tarafından temsil edilir.

Bu Çokgen yalnızca başvuru amaçlıdır ve mantıksal veya fiziksel bir gerçek Şehir ayrımları temsil etmiyor.

|RegionID|RegionName|Geofence|
|--------|----------|--------|
|1|"SoHo"|"ÇOKGEN ((-74.00279525078275 40.72833625216264,-74.00547745979765 40.721929158663244,-74.00125029839018 40.71893680218994,-73.9957785919998 40.72521409075776,-73.9972377137039 40.72557184584898,-74.00279525078275 40.72833625216264) )"|
|2|"Chinatown"|"ÇOKGEN ((-73.99712367114876 40.71281582267133,-73.9901070123658 40.71336881907936,-73.99023575839851 40.71452359088633,-73.98976368961189 40.71554823078944,-73.99551434573982 40.717337246783735,-73.99480624255989 40.718491949759304,-73.99652285632942 40.719109951574,-73.99776740131233 40.7168005470334,-73.99903340396736 40.71727219249899,-74.00193018970344 40.71938642421256,-74.00409741458748 40.71688186545551,-74.00051398334358 40.71517415773184,-74.0004281526551 40.714377212470005,-73.99849696216438 40.713450141693166,-73.99748845157478 40.71405192594819,-73.99712367114876 40.71281582267133)) "|
|3|"Tribeca"|"ÇOKGEN ((-74.01091641815208 40.72583120006787,-74.01338405044578 40.71436586362705,-74.01370591552757 40.713617702123415,-74.00862044723533 40.711308107057235,-74.00194711120628 40.7194238654018,-74.01091641815208 40.72583120006787))"|

### <a name="aggregate-data-over-a-window-of-time"></a>Bir zaman penceresi içinde toplam veri

Aşağıdaki tablo, "sürmeye.", akış verilerini içerir.

|Kullanıcı Kimliği|FromLocation|ToLocation|TripRequestedTime|
|------|------------|----------|-----------------|
|"A"|"POINT(-74.00726861389182 40.71610611981975)"|"POINT(-73.98615095917779 40.703107386025835)"|"2019-03-12T07:00:00Z"|
|"B"|"POINT(-74.00249841021645 40.723827238895666)"|"POINT(-74.01160699942085 40.71378884930115)"|"2019-03-12T07:01:00Z"|
|"C"|"POINT(-73.99680120565864 40.716439898624024)"|"POINT(-73.98289663412544 40.72582343969828)"|"2019-03-12T07:02:00Z"|
|"D"|"POINT(-74.00741090068288 40.71615626755086)"|"POINT(-73.97999843120539 40.73477895807408)"|"2019-03-12T07:03:00Z"|

Aşağıdaki sorgu cihaz akışını döndürürüz başvuru verileriyle birleştirir ve her dakika 15 dakikalık bir zaman penceresi üzerinde bölge başına istek sayısını hesaplar.

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

Bu sorgu isteği sayısı, şehirde her bölgeye göre son 15 dakika boyunca dakika başı çıkarır. Bu bilgiler, Power BI panosu ile kolayca görüntülenebilir veya tüm sürücüleri için Azure işlevleri gibi hizmetlerle tümleştirme aracılığıyla SMS iletileri olarak yayınlanabilir.

Aşağıdaki görüntüde, Power BI panosuna sorgunun çıkışı gösterir. 

![Power BI Panosu sonucu çıktı](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Stream Analytics Jeo-uzamsal işlevleri'ne giriş](stream-analytics-geospatial-functions.md)
* [Jeo-uzamsal işlevler (Azure Stream Analytics)](https://docs.microsoft.com/stream-analytics-query/geospatial-functions)
