---
title: Azure Stream Analytics Jeo-uzamsal işlevleri'ne giriş
description: Bu makalede, Azure Stream Analytics işlerinde kullanılan Jeo-uzamsal işlevleri açıklanmaktadır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: ad789a597da759b9a2d58138c7ed441389a12adb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61479992"
---
# <a name="introduction-to-stream-analytics-geospatial-functions"></a>Stream Analytics Jeo-uzamsal işlevleri'ne giriş

Azure Stream analytics'te Jeo-uzamsal İşlevler, Jeo-uzamsal veri akışı üzerinde gerçek zamanlı analizler sağlar. Yalnızca birkaç kod satırıyla karmaşık senaryolar için üretim sınıf çözüm geliştirebilirsiniz. 

Jeo-uzamsal işlevleri yararlanabilir senaryolarına örnekler şunlardır:

* Kıl paylaşma
* Filo Yönetimi
* Varlık izleme
* Şirketin coğrafı
* Hücre siteler arasında telefon izleme

Stream Analytics sorgu dili yedi yerleşik Jeo-uzamsal işlevler sahiptir: **CreateLineString**, **CreatePoint**, **CreatePolygon**, **ST_DISTANCE**, **ST_OVERLAPS**, **ST_ KESİŞEN**, ve **ST_WITHIN**.

## <a name="createlinestring"></a>CreateLineString

`CreateLineString` İşlevi noktaları kabul eder ve hangi harita üzerinde bir çizgi olarak çizilen bir GeoJSON LineString, döndürür. LineString oluşturmak için en az iki nokta olmalıdır. LineString noktaları sırada bağlanırsınız.

Aşağıdaki sorguda kullandığı `CreateLineString` üç nokta kullanarak LineString oluşturmak için. İlk noktası, diğer iki el ile oluşturulduğu sırada giriş, akış verilerinden oluşturulur.

```SQL 
SELECT  
     CreateLineString(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5))  
FROM input  
```  

### <a name="input-example"></a>Örnek Giriş  
  
|Enlem|Boylam|  
|--------------|---------------|  
|3,0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Çıkış örneği  

 {"type": "LineString", "koordinatları": [[-10.2, 3.0], [10.0, 10.0], [10.5, 10.5]]}

 {"type": "LineString", "koordinatları": [[20.2321,-87.33], [10.0, 10.0], [10.5, 10.5]]}

Daha fazla bilgi için ziyaret [CreateLineString](https://msdn.microsoft.com/azure/stream-analytics/reference/createlinestring) başvuru.

## <a name="createpoint"></a>CreatePoint

`CreatePoint` İşlevi bir enlem ve boylam kabul eder ve bir haritada çizilen bir GeoJSON noktasını döndürür. Latitudes ve longitudes olmalıdır bir **float** veri türü.

Aşağıdaki örnek sorgu kullanan `CreatePoint` latitudes ve giriş akış verilerinden longitudes kullanarak bir noktası oluşturmak için.

```SQL 
SELECT  
     CreatePoint(input.latitude, input.longitude)  
FROM input 
```  

### <a name="input-example"></a>Örnek Giriş  
  
|Enlem|Boylam|  
|--------------|---------------|  
|3,0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Çıkış örneği
  
 {"type": "Nokta", "koordinatları": [-10.2, 3.0]}  
  
 {"type": "Nokta", "koordinatları": [20.2321,-87.33]}  

Daha fazla bilgi için ziyaret [CreatePoint](https://msdn.microsoft.com/azure/stream-analytics/reference/createpoint) başvuru.

## <a name="createpolygon"></a>CreatePolygon

`CreatePolygon` İşlevi noktaları kabul eder ve bir GeoJSON poligon kaydı döndürür. Sağ taraftaki zil yönlendirme, noktaları sırasını izlemelidir veya saat yönünün. Bir noktadan diğerine bildirilen sırada walking düşünün. Çokgenin Orta Sol için tüm zaman olacaktır.

Aşağıdaki örnek sorgu kullanan `CreatePolygon` Çokgen üç noktaları oluşturmak için. İlk iki noktalarını el ile oluşturulan ve son noktası giriş verilerinden oluşturulur.

```SQL 
SELECT  
     CreatePolygon(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5), CreatePoint(input.latitude, input.longitude))  
FROM input  
```  

### <a name="input-example"></a>Örnek Giriş  
  
|Enlem|Boylam|  
|--------------|---------------|  
|3,0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Çıkış örneği  

 {"type": "Çokgen", "koordinatları": [[[-10.2, 3.0], [10.0, 10.0], [10.5, 10.5], [-10.2, 3.0]]]}
 
 {"type": "Çokgen", "koordinatları": [[[20.2321,-87.33], [10.0, 10.0], [10.5, 10.5], [20.2321,-87.33]]]}

Daha fazla bilgi için ziyaret [CreatePolygon](https://msdn.microsoft.com/azure/stream-analytics/reference/createpolygon) başvuru.


## <a name="stdistance"></a>ST_DISTANCE
`ST_DISTANCE` İşlevi metre olarak iki nokta arasındaki uzaklığı döndürür. 

Aşağıdaki sorguda kullandığı `ST_DISTANCE` benzin istasyonunu küçüktür 10 km araba gelen bir olay oluşturmak için.

```SQL
SELECT Cars.Location, Station.Location 
FROM Cars c  
JOIN Station s ON ST_DISTANCE(c.Location, s.Location) < 10 * 1000
```

Daha fazla bilgi için ziyaret [ST_DISTANCE](https://msdn.microsoft.com/azure/stream-analytics/reference/st-distance) başvuru.

## <a name="stoverlaps"></a>ST_OVERLAPS
`ST_OVERLAPS` İşlevi iki çokgenler karşılaştırır. İşlev, çokgenler çakışırsa, 1 döndürür. Çokgenler çakışmadığından işlev 0 döndürür. 

Aşağıdaki sorguda kullandığı `ST_OVERLAPS` bir yapı içinde bölge taşmasını olası bir olay oluşturmak için.

```SQL
SELECT Building.Polygon, Building.Polygon 
FROM Building b 
JOIN Flooding f ON ST_OVERLAPS(b.Polygon, b.Polygon) 
```

Aşağıdaki örnek sorguda, storm, bir araba başlık bir olay oluşturur.

```SQL
SELECT Cars.Location, Storm.Course
FROM Cars c, Storm s
JOIN Storm s ON ST_OVERLAPS(c.Location, s.Course)
```

Daha fazla bilgi için ziyaret [ST_OVERLAPS](https://msdn.microsoft.com/azure/stream-analytics/reference/st-overlaps) başvuru.

## <a name="stintersects"></a>ST_INTERSECTS
`ST_INTERSECTS` İşlevi iki LineString karşılaştırır. INTERSECT LineString, işlev 1 döndürür. LineString kesişen yoksa işlev 0 döndürür.

Aşağıdaki örnek sorgu kullanan `ST_INTERSECTS` asfalt yol kir yol kesişip olmadığını belirlemek için.

```SQL 
SELECT  
     ST_INTERSECTS(input.pavedRoad, input.dirtRoad)  
FROM input  
```  

### <a name="input-example"></a>Örnek Giriş  
  
|datacenterArea|stormArea|  
|--------------------|---------------|  
|{"type": "LineString", "koordinatları": [[-10.0, 0.0], [0.0, 0.0], [10.0, 0.0]]}|{"type": "LineString", "koordinatları": [[0.0, 10.0], [0.0, 0.0], [0.0,-10.0]]}|  
|{"type": "LineString", "koordinatları": [[-10.0, 0.0], [0.0, 0.0], [10.0, 0.0]]}|{"type": "LineString", "koordinatları": [[-10.0, 10.0], [0.0, 10.0], [10.0, 10.0]]}|  
  
### <a name="output-example"></a>Çıkış örneği  

 1  
  
 0  

Daha fazla bilgi için ziyaret [ST_INTERSECTS](https://msdn.microsoft.com/azure/stream-analytics/reference/st-intersects) başvuru.

## <a name="stwithin"></a>ST_WITHIN
`ST_WITHIN` İşlevi içinde bir Çokgen noktası veya Çokgen olup olmadığını belirler. Çokgen noktası veya Çokgen içeriyorsa, işlev 1 döndürür. Noktası veya Çokgen içinde bildirilen Çokgen bulunduğu değilse işlev 0 döndürür.

Aşağıdaki örnek sorgu kullanan `ST_WITHIN` teslim hedef noktasını belirli ambarı Çokgen içinde olup olmadığını belirlemek için.

```SQL 
SELECT  
     ST_WITHIN(input.deliveryDestination, input.warehouse)  
FROM input 
```  

### <a name="input-example"></a>Örnek Giriş  
  
|deliveryDestination|Ambarı|  
|-------------------------|---------------|  
|{"type": "Point", "coordinates": [76.6, 10,1]}|{"type": "Çokgen", "koordinatları": [[0.0, 0.0], [10.0, 0.0], [10.0, 10.0], [0.0, 10.0], [0.0, 0.0]]}|  
|{"type": "Point", "coordinates": [15.0, 15.0]}|{"type": "Çokgen", "koordinatları": [[10.0, 10.0], [20,0, 10.0], [20.0, 20.0], [10.0, 20,0], [10.0, 10.0]]}|  
  
### <a name="output-example"></a>Çıkış örneği  

 0  
  
 1  

Daha fazla bilgi için ziyaret [ST_WITHIN](https://msdn.microsoft.com/azure/stream-analytics/reference/st-within) başvuru.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)