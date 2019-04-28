---
title: Azure İzleyici günlük sorgusu için Splunk | Microsoft Docs
description: Azure İzleyici günlük sorguları ilginizi Splunk ile ilgili bilgi sahibi olan kullanıcılar için yardımcı olur.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: fb637197139001c67a4cfa773f897e6701dc1e9c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61425143"
---
# <a name="splunk-to-azure-monitor-log-query"></a>Azure İzleyici günlük sorgusu için Splunk

Bu makale, Azure İzleyici'de günlük sorguları yazma Kusto sorgu dili öğrenmek Splunk hakkında bilgi sahibi olan kullanıcılara yardımcı olmak için hazırlanmıştır. Doğrudan karşılaştırmalar iki önemli farkları ve ayrıca benzerlikler arasında burada mevcut bilginiz yararlanabileceğiniz yapılır.

## <a name="structure-and-concepts"></a>Yapı ve kavramları

Aşağıdaki tabloda, kavramlar ve veri yapıları Splunk ve Azure İzleyici günlüklerine arasında karşılaştırır.

 | Kavram  | Splunk | Azure İzleyici |  Açıklama
 | --- | --- | --- | ---
 | Dağıtım birimi  | küme |  küme |  Azure İzleyici, küme sorguları rastgele sağlar. Splunk izin vermez. |
 | Veri önbelleklerini |  Demet  |  Önbelleğe alma ve elde tutma ilkeleri |  Dönem ve veri düzeyi önbelleğe alma denetler. Doğrudan bu ayarı, sorguların performansını ve dağıtım maliyetini etkiler. |
 | Mantıksal bölüm veri  |  index  |  veritabanı  |  Mantıksal ayrılığı veri sağlar. Hem uygulamalar, birleşimler ve bu bölümler arasında birleştirme izin verir. |
 | Yapılandırılmış olay meta verileri | Yok | tablo |  Splunk olay meta verilerinin arama dilinin kullanıma sunulan kavramı yoktur. Azure İzleyici günlüklerine sütuna sahip bir tablo kavramı vardır. Her olay örneği bir satıra eşlendi. |
 | Veri kaydı | event | satır |  Yalnızca terminolojisi değiştirin. |
 | Veri kaydı özniteliği | Alan |  Sütun |  Azure İzleyici'de, bu tablo yapısı bir parçası olarak önceden tanımlanmıştır. Splunk içinde her olay alan kendi kümesine sahiptir. |
 | Türler | veri türü |  veri türü |  Azure İzleyici türleri sütunlarda ayarlarken daha açık. Hem de veri türleri ve JSON desteği dahil olmak üzere veri türleri kabaca kümesini dinamik olarak çalışmak üzere sahipsiniz. |
 | Sorgu ve arama  | ara | sorgu |  Kavramları temelde hem Azure İzleyici hem de Splunk arasında aynıdır. |
 | Olay alma saati | Sistem saati | ingestion_time() |  Splunk içinde her olay bir sistem zaman damgası olay dizini zaman alır. Azure İzleyici'de ingestion_time() işlevi ile başvurulan bir sistem sütunu gösteren ingestion_time adlı bir ilke tanımlayabilirsiniz. |

## <a name="functions"></a>İşlevler

Aşağıdaki tabloda Azure İzleyici'de Splunk işlevleri için eşdeğer işlevleri belirtir.

|Splunk | Azure İzleyici |Açıklama
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|if     | iff()   | (1) |
|tonumber | ToDouble()<br>tolong()<br>toint() | (1) |
|üst<br>Daha düşük |toupper()<br>tolower()|(1) |
| Değiştir | replace() | (1)<br> Ayrıca bu süre unutmayın `replace()` farklı parametreleri her iki ürün de üç parametre alır. |
| substr | substring() | (1)<br>Ayrıca Splunk tabanlı dizinleri kullandığına dikkat edin. Azure İzleyici, sıfır tabanlı dizin notlar. |
| tolower |  tolower() | (1) |
| toupper | toupper() | (1) |
| eşleşme | Normal ifade ile eşleşir |  (2)  |
| regex | Normal ifade ile eşleşir | Splunk'ta, `regex` işleçtir. Azure İzleyici'de, ilişkisel bir işlecidir. |
| searchmatch | == | Splunk'ta, `searchmatch` tam dize için arama sağlar.
| rastgele | rand()<br>rand(n) | Splunk'ın işlevi, 2 sıfırdan bir sayıyı döndürür<sup>31</sup>-1. Azure İzleyici ' 0,0 ile 1,0, arasında bir sayı döndürür veya 0 ile n-1 arasında sağlanan bir parametre değilse.
| şimdi | now() | (1)
| relative_time | ToTimeSpan() | (1)<br>Azure İzleyici'de (datetimeVal, offsetVal) relative_time Splunk'ın denk datetimeVal + totimespan(offsetVal) şeklindedir.<br>Örneğin, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> olur <code>...  &#124; extend myTime = now() - totimespan("1d")</code>.

(1 Splunk) işlevi ile çağrılan `eval` işleci. Azure İzleyici'de bir parçası olarak kullanıldığı `extend` veya `project`.<br>(2 Splunk) işlevi ile çağrılan `eval` işleci. Azure İzleyici'de, kullanılabilir `where` işleci.


## <a name="operators"></a>İşleçler

Aşağıdaki bölümlerde, Azure İzleyici ile Splunk arasındaki farklı işleçleri kullanma örnekleri sağlar.

> [!NOTE]
> Aşağıdaki örnekte, Splunk alanın amacıyla _kural_ Azure İzleyici'de bir tablo eşlenir ve günlükleri analiz Splunk'ın varsayılan zaman damgası eşlenir _ingestion_time()_ sütun.

### <a name="search"></a>Arama
Splunk içinde atlayabilirsiniz `search` anahtar sözcüğü tırnak işareti olmayan bir dize belirtin. Azure İzleyici'de her sorgu ile başlamalıdır `find`, bir sütun adı tırnak işareti olmayan bir dizedir ve arama değeri tırnak içine alınmış bir dize olmalıdır. 

| |  | |
|:---|:---|:---|
| Splunk | **Arama** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| Azure İzleyici | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |
| | |

### <a name="filter"></a>Filtre
Azure İzleyici günlük sorguları başından tablosal sonuçtaki ayarlamak nerede filtre. Splunk içinde filtreleme varsayılan geçerli dizindeki bir işlemdir. Ayrıca `where` işleci Splunk, ancak önerilmez.

| |  | |
|:---|:---|:---|
| Splunk | **Arama** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| Azure İzleyici | **Burada** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |
| | |


### <a name="getting-n-eventsrows-for-inspection"></a>İnceleme için n olayları satırları alma 
Azure İzleyici günlük sorguları da destek `take` için diğer ad olarak `limit`. Splunk'ta sonuçları sıralanır, `head` ilk n sonuç döndürür. Azure İzleyici'de sınırı sıralı değil, ancak bulunan ilk n satırları döndürür.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| Azure İzleyici | **Sınırı** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |
| | |



### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>İlk n olaylar/alan/sütuna göre sıralanmış satırları alma
Splunk içinde alt sonuçları elde etmek için kullanmanız `tail`. Azure İzleyici'de sıralama yönünü belirtmek `asc`.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| Azure İzleyici | **Sayfanın Üstü** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |
| | |




### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Yeni alanlar/sütunlarla sonucu genişletme ayarlayın
Splunk de sahip bir `eval` ile karşılaştırılabilir olmaması gereken işlevi `eval` işleci. Her iki `eval` splunk'ta işleci ve `extend` Azure İzleyicisi'nde işleci yalnızca skaler işlevler ve aritmetik işleçler destekler.

| |  | |
|:---|:---|:---|
| Splunk | **Değerlendirme** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| Azure İzleyici | **Genişletme** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |
| | |


### <a name="rename"></a>Yeniden Adlandır 
Azure İzleyici, yeniden adlandırmak ve yeni bir alan oluşturmak için işleç kullanır. Splunk sahip iki ayrı işleçler `eval` ve `rename`.

| |  | |
|:---|:---|:---|
| Splunk | **Yeniden adlandırma** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| Azure İzleyici | **Genişletme** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend exception = Date_Exception</code> |
| | |




### <a name="format-resultsprojection"></a>Biçim sonuçları/projeksiyonu
Splunk sahip bir işleç benzer şekilde görünen değil `project-away`. Dışarıda alanlarını filtrelemek için kullanıcı Arabirimi kullanabilirsiniz.

| |  | |
|:---|:---|:---|
| Splunk | **Tablo** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| Azure İzleyici | **Proje**<br>**Proje koyma** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |
| | |



### <a name="aggregation"></a>Toplama
Bkz: [toplamalar Azure İzleyici'de oturum sorguları](aggregations.md) farklı toplama işlevleri için.

| |  | |
|:---|:---|:---|
| Splunk | **İstatistikleri** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| Azure İzleyici | **Özetleme** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |
| | |



### <a name="join"></a>Birleştir
Splunk birleştirme önemli sınırlamaları vardır. Alt sorgu 10000 sonuçları (dağıtım yapılandırma dosyasında ayarlanır) sınırı vardır ve sınırlı sayıda birleştirme özellikleri vardır.

| |  | |
|:---|:---|:---|
| Splunk | **join** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias \| join Client.Id max=0 [search earliest=-24h Event.Rule="150310.0" Data.Hresult=-2147221040]</code> |
| Azure İzleyici | **join** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |
| | |



### <a name="sort"></a>Sırala
Splunk'ta kullanmalısınız artan düzende sıralamak için `reverse` işleci. Azure da null değerlere, başında veya sonunda yerleştirileceği yeri tanımlama destekler izleyin.

| |  | |
|:---|:---|:---|
| Splunk | **Sıralama** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| Azure İzleyici | **Sıralama ölçütü** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |
| | |



### <a name="multivalue-expand"></a>Birden çok değerli genişletin
Bu, hem Azure İzleyici, hem de Splunk benzer bir işlecidir.

| |  | |
|:---|:---|:---|
| Splunk | **mvexpand** |  `mvexpand foo` |
| Azure İzleyici | **mvexpand** | `mvexpand foo` |
| | |




### <a name="results-facets-interesting-fields"></a>Sonuçları modelleri, ilginç alanları
Azure portalında log Analytics'te, yalnızca ilk sütununu kullanıma sunulur. Tüm sütunlar API aracılığıyla kullanılabilir.

| |  | |
|:---|:---|:---|
| Splunk | **Alanları** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| Azure İzleyici | **modelleri** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |
| | |




### <a name="de-duplicate"></a>XML'deki yinelenen
Kullanabileceğiniz `summarize arg_min()` yerine hangi kayıt seçilen sırasını tersine çevirmek için.

| |  | |
|:---|:---|:---|
| Splunk | **Yinelenenleri kaldırma** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| Azure İzleyici | **summarize arg_max()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |
| | |




## <a name="next-steps"></a>Sonraki adımlar

- Ders ile devam [Azure İzleyici'de günlük sorguları yazma](get-started-queries.md).
