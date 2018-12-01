---
title: Azure Log analytics'e Splunk | Microsoft Docs
description: Log Analytics sorgu dili öğrenmek Splunk ile ilgili bilgi sahibi olan kullanıcılar için yardımcı olur.
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
ms.openlocfilehash: 61f0cff661c79f994a5b3c20646996f617a31b7e
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52683073"
---
# <a name="splunk-to-log-analytics"></a>Log analytics'e Splunk

Bu makale, Log Analytics sorgu dili öğrenmek Splunk ile ilgili bilgi sahibi olan kullanıcılara yardımcı olmak için hazırlanmıştır. Doğrudan karşılaştırmalar iki önemli farkları ve ayrıca benzerlikler arasında burada mevcut bilginiz yararlanabileceğiniz yapılır.

## <a name="structure-and-concepts"></a>Yapı ve kavramları

Aşağıdaki tabloda, kavramlar ve veri yapıları Splunk ve Log Analytics arasındaki karşılaştırır.

 | Kavram  | Splunk | Log Analytics |  Açıklama
 | --- | --- | --- | ---
 | Dağıtım birimi  | küme |  küme |  Log Analytics'i küme sorguları rastgele sağlar. Splunk izin vermez. |
 | Veri önbelleklerini |  Demet  |  Önbelleğe alma ve elde tutma ilkeleri |  Dönem ve veri düzeyi önbelleğe alma denetler. Doğrudan bu ayarı, sorguların performansını ve dağıtım maliyetini etkiler. |
 | Mantıksal bölüm veri  |  dizin  |  veritabanı  |  Mantıksal ayrılığı veri sağlar. Hem uygulamalar, birleşimler ve bu bölümler arasında birleştirme izin verir. |
 | Yapılandırılmış olay meta verileri | Yok | tablo |  Splunk olay meta verilerinin arama dilinin kullanıma sunulan kavramı yoktur. Log Analytics, sütunları içeren bir tablo, kavramını sahiptir. Her olay örneği bir satıra eşlendi. |
 | Veri kaydı | event | satır |  Yalnızca terminolojisi değiştirin. |
 | Veri kaydı özniteliği | Alan |  Sütun |  Log Analytics'te Bu tablo yapısı bir parçası olarak önceden tanımlanmıştır. Splunk içinde her olay alan kendi kümesine sahiptir. |
 | Türler | veri türü |  veri türü |  Log Analytics veri türleri sütunlarda ayarlarken daha açık. Hem de veri türleri ve JSON desteği dahil olmak üzere veri türleri kabaca kümesini dinamik olarak çalışmak üzere sahipsiniz. |
 | Sorgu ve arama  | ara | sorgu |  Aslında aynı Splunk ve Log Analytics arasındaki kavramlardır. |
 | Olay alma saati | Sistem saati | ingestion_time() |  Splunk içinde her olay bir sistem zaman damgası olay dizini zaman alır. Log Analytics'te ingestion_time() işlevi ile başvurulan bir sistem sütunu gösteren ingestion_time adlı bir ilke tanımlayabilirsiniz. |

## <a name="functions"></a>İşlevler

Aşağıdaki tabloda, Log analytics'te Splunk işlevleri için eşdeğer işlevleri belirtir.

|Splunk | Log Analytics |Açıklama
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|Eğer     | iff()   | (1) |
|tonumber | ToDouble()<br>tolong()<br>toint() | (1) |
|üst<br>Daha düşük |toupper()<br>tolower()|(1) |
| Değiştir | replace() | (1)<br> Ayrıca bu süre unutmayın `replace()` farklı parametreleri her iki ürün de üç parametre alır. |
| substr | substring() | (1)<br>Ayrıca Splunk tabanlı dizinleri kullandığına dikkat edin. Log Analytics, sıfır tabanlı dizin notlar. |
| tolower |  tolower() | (1) |
| toupper | toupper() | (1) |
| eşleşme | Normal ifade ile eşleşir |  (2)  |
| Normal ifade | Normal ifade ile eşleşir | Splunk'ta, `regex` işleçtir. Log Analytics'te, ilişkisel bir işlecidir. |
| searchmatch | == | Splunk'ta, `searchmatch` tam dize için arama sağlar.
| rastgele | rand()<br>rand(n) | Splunk'ın işlevi, 2 sıfırdan bir sayıyı döndürür<sup>31</sup>-1. Log Analytics'e 0,0 ile 1,0, arasında bir sayı döndürür veya 0 ile n-1 arasında sağlanan bir parametre değilse.
| şimdi | now() | (1)
| relative_time | ToTimeSpan() | (1)<br>Log Analytics'te relative_time (datetimeVal, offsetVal) Splunk'ın denk datetimeVal + totimespan(offsetVal) şeklindedir.<br>Örneğin, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> olur <code>...  &#124; extend myTime = now() - totimespan("1d")</code>.

(1 Splunk) işlevi ile çağrılan `eval` işleci. Log Analytics'te bir parçası olarak kullanıldığı `extend` veya `project`.<br>(2 Splunk) işlevi ile çağrılan `eval` işleci. Log Analytics'te ile kullanılabilmesi için `where` işleci.


## <a name="operators"></a>İşleçler

Aşağıdaki bölümlerde Splunk ve Log Analytics arasındaki farklı işleçleri kullanma örnekleri sağlar.

> [!NOTE]
> Aşağıdaki örnekte, Splunk alanın amacıyla _kural_ Azure Log Analytics tabloda eşlenir ve günlükleri analiz Splunk'ın varsayılan zaman damgası eşlenir _ingestion_time()_ sütun.

### <a name="search"></a>Arama
Splunk içinde atlayabilirsiniz `search` anahtar sözcüğü tırnak işareti olmayan bir dize belirtin. Azure Log Analytics'te her arama ile başlamalıdır `find`, bir sütun adı tırnak işareti olmayan bir dizedir ve arama değeri tırnak içine alınmış bir dize olmalıdır. 

| |  | |
|:---|:---|:---|
| Splunk | **Arama** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| Log Analytics | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |
| | |

### <a name="filter"></a>Filtre
Azure Log Analytics sorguları başlangıç ayarlandığı tablosal bir sonuç Filtresi. Splunk içinde filtreleme varsayılan geçerli dizindeki bir işlemdir. Ayrıca `where` işleci Splunk, ancak önerilmez.

| |  | |
|:---|:---|:---|
| Splunk | **Arama** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| Log Analytics | **Burada** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |
| | |


### <a name="getting-n-eventsrows-for-inspection"></a>İnceleme için n olayları satırları alma 
Azure Log Analytics de destekler `take` için diğer ad olarak `limit`. Splunk'ta sonuçları sıralanır, `head` ilk n sonuç döndürür. Azure Log analytics'te sınırı sıralı değil, ancak bulunan ilk n satırları döndürür.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| Log Analytics | **Sınırı** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |
| | |



### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>İlk n olaylar/alan/sütuna göre sıralanmış satırları alma
Splunk içinde alt sonuçları elde etmek için kullanmanız `tail`. Azure Log Analytics'te sıralama yönünü belirtmek `asc`.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| Log Analytics | **Sayfanın Üstü** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |
| | |




### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Yeni alanlar/sütunlarla sonucu genişletme ayarlayın
Splunk de sahip bir `eval` ile karşılaştırılabilir olmaması gereken işlevi `eval` işleci. Her iki `eval` splunk'ta işleci ve `extend` Azure Log analytics'te işleci yalnızca skaler işlevler ve aritmetik işleçler destekler.

| |  | |
|:---|:---|:---|
| Splunk | **Değerlendirme** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| Log Analytics | **Genişletme** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |
| | |


### <a name="rename"></a>Yeniden Adlandır 
Azure Log Analytics, yeniden adlandırmak ve yeni bir alan oluşturmak için işleç kullanır. Splunk sahip iki ayrı işleçler `eval` ve `rename`.

| |  | |
|:---|:---|:---|
| Splunk | **Yeniden adlandırma** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| Log Analytics | **Genişletme** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend exception = Date_Exception</code> |
| | |




### <a name="format-resultsprojection"></a>Biçim sonuçları/projeksiyonu
Splunk sahip bir işleç benzer şekilde görünen değil `project-away`. Dışarıda alanlarını filtrelemek için kullanıcı Arabirimi kullanabilirsiniz.

| |  | |
|:---|:---|:---|
| Splunk | **Tablo** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| Log Analytics | **Proje**<br>**Proje koyma** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |
| | |



### <a name="aggregation"></a>Toplama
Bkz: [Log Analytics sorguları Toplamalara](aggregations.md) farklı toplama işlevleri için.

| |  | |
|:---|:---|:---|
| Splunk | **İstatistikleri** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| Log Analytics | **Özetleme** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |
| | |



### <a name="join"></a>Birleştir
Splunk birleştirme önemli sınırlamaları vardır. Alt sorgu 10000 sonuçları (dağıtım yapılandırma dosyasında ayarlanır) sınırı vardır ve sınırlı sayıda birleştirme özellikleri vardır.

| |  | |
|:---|:---|:---|
| Splunk | **join** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias | Client.Id katılın en fazla 0 = [erken = - 24 h Event.Rule="150310.0 arama" Data.Hresult= 2147221040]</code> |
| Log Analytics | **join** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |
| | |



### <a name="sort"></a>Sırala
Splunk'ta kullanmalısınız artan düzende sıralamak için `reverse` işleci. Azure Log Analytics de destekler null değerlere, başında veya sonunda yerleştirileceği yeri tanımlama.

| |  | |
|:---|:---|:---|
| Splunk | **Sıralama** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| Log Analytics | **Sıralama ölçütü** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |
| | |



### <a name="multivalue-expand"></a>Birden çok değerli genişletin
Bu, hem Log Analytics, hem de Splunk benzer bir işlecidir.

| |  | |
|:---|:---|:---|
| Splunk | **mvexpand** |  `mvexpand foo` |
| Log Analytics | **mvexpand** | `mvexpand foo` |
| | |




### <a name="results-facets-interesting-fields"></a>Sonuçları modelleri, ilginç alanları
Log Analytics portalında, yalnızca ilk sütununu kullanıma sunulur. Tüm sütunlar API aracılığıyla kullanılabilir.

| |  | |
|:---|:---|:---|
| Splunk | **Alanları** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| Log Analytics | **modelleri** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |
| | |




### <a name="de-duplicate"></a>XML'deki yinelenen
Kullanabileceğiniz `summarize arg_min()` yerine hangi kayıt seçilen sırasını tersine çevirmek için.

| |  | |
|:---|:---|:---|
| Splunk | **Yinelenenleri kaldırma** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| Log Analytics | **arg_max() özetleme** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |
| | |




## <a name="next-steps"></a>Sonraki adımlar

- Ders ile devam [Log Analytics'te sorgu yazma](get-started-queries.md).