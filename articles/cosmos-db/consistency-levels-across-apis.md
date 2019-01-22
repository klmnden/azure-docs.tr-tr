---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API’leri
description: Azure Cosmos DB API'leri arasında tutarlılık düzeylerini anlama.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: a506c696cdb9ca6c6221b54c63d2446b7cb86a69
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430578"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API’leri

Azure Cosmos DB tarafından sunulan beş tutarlılık modeli yerel olarak Azure Cosmos DB SQL API tarafından desteklenir. Azure Cosmos DB kullandığınızda, SQL API'si varsayılandır. 

Azure Cosmos DB ayrıca kablo için yerel destek sağlar popüler veritabanları için protokolü ile uyumlu API'leri. Veritabanlarını, MongoDB, Apache Cassandra, Gremlin ve Azure tablo depolama alanı içerir. Bu veritabanları, tam olarak tanımlanmış tutarlılık modeli veya tutarlılık düzeyleri için SLA destekli garantileri sunmamaktadır. Bunlar genellikle yalnızca bir alt kümesini Azure Cosmos DB tarafından sunulan beş tutarlılık modeli sunar. SQL API, Gremlin API ve tablo API'si için Azure Cosmos DB hesabı yapılandırılmış varsayılan tutarlılık düzeyini kullanılır. 

Aşağıdaki bölümlerde OSS istemci sürücü tarafından istenen Apache Cassandra için veri tutarlılığı arasındaki eşlemeyi Göster 4.x ve MongoDB 3.4. Bu belge, ayrıca Apache Cassandra ve MongoDB için karşılık gelen Azure Cosmos DB tutarlılık düzeyleri gösterilmektedir.

## <a id="cassandra-mapping"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Bu tablo, Azure Cosmos DB'de Apache Cassandra ve tutarlılık düzeyleri tutarlılık eşlemesini gösterir. Her Cassandra okuma ve yazma tutarlılık düzeyleri için karşılık gelen bir Cosmos DB tutarlılık düzeyi daha güçlü sağlayan başka bir deyişle, katı garanti eder.


<table>
<tr> 
  <th rowspan="2">Cassandra tutarlılık düzeyi</th> 
  <th rowspan="2">Cosmos DB tutarlılık düzeyi</th> 
  <th colspan="3">Tutarlılık eşleme yazma</th> 
  <th colspan="3">Okuma tutarlılığı eşleme</th> 
</tr> 


 
 <tr> 
  <th>Cassandra</th> 
  <th>Cosmos DB</th> 
  <th>Garantisi</th> 
  <th>Cassandra</th> 
  <th>Cosmos DB'ye</th> 
  <th>Garantisi</th> 
 </tr> 
 
  <tr> 
  <td rowspan="6">TÜMÜ</td> 
  <td rowspan="6">Güçlü</td> 
  <td>TÜMÜ</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
  <td>TÜM ÇEKİRDEK, SERİ, LOCAL_QUORUM, LOCAL_SERIAL, ÜÇ, İKİ, BİRİ, LOCAL_ONE</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
 </tr> 
 
 <tr> 
  <td rowspan="2">EACH_QUORUM</td> 
  <td rowspan="2">Güçlü</td> 
  <td rowspan="2">Doğrusallaştırma</td> 
  <td>TÜM ÇEKİRDEK, SERİ LOCAL_QUORUM, LOCAL_SERIAL, ÜÇ, İKİ</td> 
  <td>Güçlü</td> 
  <td >Doğrusallaştırma</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE</td>
  <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>
 

 <tr> 
  <td rowspan="2">ÇEKİRDEK SERİ</td> 
  <td rowspan="2">Güçlü</td> 
  <td rowspan="2">Doğrusallaştırma</td> 
  <td>TÜMÜ, ÇEKİRDEK, SERİ</td> 
  <td>Güçlü</td> 
  <td >Doğrusallaştırma</td> 
 </tr> 

 <tr>
   <td>LOCAL_ONE, BİRİ, LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ</td>
   <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>
 
 
 <tr> 
 <td>LOCAL_QUORUM, ÜÇ, İKİ, BİRİ, LOCAL_ONE, <b>ANY</b></td> 
  <td>Tutarlı Ön Ek</td> 
  <td>Genel tutarlı ön ek</td> 
  <td>LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK</td> 
  <td>Tutarlı Ön Ek</td> 
  <td>Genel tutarlı ön ek</td>
 </tr> 
 
 
  <tr> 
  <td rowspan="6">EACH_QUORUM</td> 
  <td rowspan="6">Güçlü</td> 
  <td rowspan="2">EACH_QUORUM</td> 
  <td rowspan="2">Güçlü</td> 
  <td rowspan="2">Doğrusallaştırma</td> 
  <td>TÜM ÇEKİRDEK, SERİ LOCAL_QUORUM, LOCAL_SERIAL, ÜÇ, İKİ</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE</td>
  <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>
 
 
 
 <tr> 
  <td rowspan="2">ÇEKİRDEK SERİ</td> 
  <td rowspan="2">Güçlü</td> 
  <td rowspan="2">Doğrusallaştırma</td> 
  <td>TÜMÜ, ÇEKİRDEK, SERİ</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, BİRİ, LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ</td>
  <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>
 
 
  <tr> 
  <td rowspan="2">LOCAL_QUORUM, ÜÇ, İKİ, BİRİ, LOCAL_ONE, TÜM</td> 
  <td rowspan="2">Tutarlı Ön Ek</td> 
  <td rowspan="2">Genel tutarlı ön ek</td> 
  <td>TÜMÜ</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK</td>
  <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>


  <tr> 
  <td rowspan="4">ÇEKİRDEK</td> 
  <td rowspan="4">Güçlü</td> 
  <td rowspan="2">ÇEKİRDEK SERİ</td> 
  <td rowspan="2">Güçlü</td> 
  <td rowspan="2">Doğrusallaştırma</td> 
  <td>TÜMÜ, ÇEKİRDEK, SERİ</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, BİRİ, LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ</td>
  <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>
 
 
 <tr> 
  <td rowspan="2">LOCAL_QUORUM, ÜÇ, İKİ, BİRİ, LOCAL_ONE, TÜM</td> 
  <td rowspan="2">Tutarlı Ön Ek </td> 
  <td rowspan="2">Genel tutarlı ön ek </td> 
  <td>TÜMÜ</td> 
  <td>Güçlü</td> 
  <td>Doğrusallaştırma</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK</td>
  <td>Tutarlı Ön Ek</td>
   <td>Genel tutarlı ön ek</td>
 </tr>
 
 <tr> 
  <td rowspan="4">LOCAL_QUORUM, ÜÇ, İKİ</td> 
  <td rowspan="4">Sınırlanmış Eskime Durumu</td> 
  <td rowspan="2">LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ</td> 
  <td rowspan="2">Sınırlanmış Eskime Durumu</td> 
  <td rowspan="2">Sınırlanmış eskime durumu.<br/>
En fazla K sürümleri veya t saatten.<br/>
Son yürütülen değere bölgede okuyun. 
</td> 
  
  <td>ÇEKİRDEK LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ</td> 
  <td>Sınırlanmış Eskime Durumu</td> 
  <td>Sınırlanmış eskime durumu.<br/>
En fazla K sürümleri veya t saatten. <br/>
Son yürütülen değere bölgede okuyun. </td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE</td>
  <td>Tutarlı Ön Ek</td>
   <td>Bölge başına tutarlı ön ek</td>
 </tr>
 
 
 <tr> 
  <td>BİR LOCAL_ONE, TÜM</td> 
  <td>Tutarlı Ön Ek </td> 
  <td >Bölge başına tutarlı ön ek </td> 
  <td>LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK</td> 
  <td>Tutarlı Ön Ek</td> 
  <td>Bölge başına tutarlı ön ek</td> 
 </tr> 
</table>

## <a id="mongo-mapping"></a>MongoDB 3.4 ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Aşağıdaki tabloda, Azure Cosmos DB MongoDB 3.4 varsayılan tutarlılık düzeyini arasında "konuları okuyun" eşleme gösterilmektedir. Tabloda, çok bölgeli ve tek bölgeli dağıtımlar gösterilmiştir.

| **MongoDB 3.4** | **Azure Cosmos DB (çok bölgeli)** | **Azure Cosmos DB (tek bölge)** |
| - | - | - |
| Linearizable | Güçlü | Güçlü |
| Çoğunluk | Sınırlanmış eskime durumu | Güçlü |
| Yerel | Tutarlı ön ek | Tutarlı ön ek |

## <a name="next-steps"></a>Sonraki adımlar

Tutarlılık düzeyleri ve açık kaynak API'lerle Azure Cosmos DB API'leri arasındaki uyumluluk hakkında daha fazlasını okuyun. Aşağıdaki makalelere bakın:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [MongoDB için Azure Cosmos DB API tarafından desteklenen MongoDB özellikleri](mongodb-feature-support.md)
* [Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)