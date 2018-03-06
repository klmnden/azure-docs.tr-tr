---
title: "Sınırları ve yapılandırması - Azure Logic Apps | Microsoft Docs"
description: "Hizmet sınırları ve Azure mantıksal uygulamaları için yapılandırma değerleri"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 54a35607e107a09188373cc5f71bb3068b4c6bab
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="logic-apps-limits-and-configuration"></a>Logic Apps sınırları ve yapılandırma

Bu makalede, Azure mantıksal uygulamaları için geçerli sınırlarını ve yapılandırma ayrıntılarını açıklanmaktadır.

## <a name="limits"></a>Sınırlar

### <a name="http-request-limits"></a>HTTP istek sınırları

Tek bir HTTP isteğinin ya da bir bağlayıcı çağrı sınırlarını şunlardır:

#### <a name="timeout"></a>Zaman Aşımı

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İstek zaman aşımına uğradı | 120 saniye | Bir [zaman uyumsuz desen](../logic-apps/logic-apps-create-api-app.md) veya [döngü kadar](logic-apps-control-flow-loops.md) gerektiğinde dengeleyebilirsiniz | 
|||| 

#### <a name="message-size"></a>İleti boyutu

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İleti boyutu | 100 MB | Bazı bağlayıcılar ve API 100 MB desteklemeyebilir. | 
| İfade değerlendirme sınırı | 131.072 karakterleri | `@concat()`, `@base64()`, `string` bu sınırdan daha uzun olamaz. | 
|||| 

#### <a name="retry-policy"></a>Yeniden deneme ilkesi

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Yeniden deneme sayısı | 90 | Varsayılan değer 4'tür. İle yapılandırabilirsiniz [yeniden deneme ilkesi parametresi](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| En uzun gecikme yeniden deneyin | 1 gün | İle yapılandırabilirsiniz [yeniden deneme ilkesi parametresi](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Yeniden deneme Min gecikmesi | 5 saniye | İle yapılandırabilirsiniz [yeniden deneme ilkesi parametresi](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

### <a name="run-duration-and-retention"></a>Çalışma süresi ve bekletme

Çalıştıran tek bir mantıksal uygulama sınırlarını şunlardır:

| Ad | Sınır | 
| ---- | ----- | 
| Çalışma süresi | 90 gün | 
| Depolama bekletme | Başlangıç zamanı çalışmanın 90 gün | 
| En az yinelenme aralığı | 1 saniye </br>Logic apps ile bir uygulama hizmeti planı için: 15 saniye | 
| En fazla yineleme aralığı | 500 gün | 
||| 

Çalışma süresi veya depolama bekletme, normal işlem akışında sınırları aşan [Logic Apps ekibine başvurun](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım.

### <a name="looping-and-debatching-limits"></a>Döngü ve sınırları debatching

Çalıştıran tek bir mantıksal uygulama sınırlarını şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| ForEach öğeleri | 100,000 | Kullanabileceğiniz [sorgu eylem](../connectors/connectors-native-query.md) gerektiği gibi daha büyük diziler filtre uygulamak için. | 
| Kadar yineleme | 5.000 | | 
| SplitOn öğeleri | 100,000 | | 
| ForEach paralellik | 50 | Varsayılan değer 20'dir. <p>ForEach döngüde paralellik belirli bir düzeyde ayarlamak için ayarlayın `runtimeConfiguration` özelliğinde `foreach` eylem. <p>ForEach döngüsü sıralı olarak çalışacak şekilde ayarlanmış `operationOptions` "Sıralı" özelliğine `foreach` eylem. | 
|||| 

### <a name="throughput-limits"></a>İşleme sınırları

Bir tek mantığı uygulama örneği için sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ----- | ----- | ----- | 
| Eylemler yürütmeleri 5 dakika başına | 100,000 | 300000 için sınırı artırmak için bir mantıksal uygulama çalıştırabilirsiniz `High Througput` modu. Yüksek verimlilik modu altında yapılandırmak için `runtimeConfiguration` iş akışı kaynak ayarlanmış `operationOptions` özelliğine `OptimizedForHighThroughput`. <p>**Not**: yüksek verimlilik modudur önizlemede. Ayrıca, birden çok uygulamalarında gerektiği gibi bir iş yükü dağıtabilirsiniz. | 
| Eylemler eşzamanlı giden çağrıları | ~2,500 | Eşzamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: eşzamanlı gelen çağrıları |~1,000 | Eşzamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı okuma  | 60,000 | İş yükü, gerektiğinde birden çok uygulama arasında dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı çağırma| 45,000 |İş yükü, gerektiğinde birden çok uygulama arasında dağıtabilirsiniz. | 
|||| 

Bu sınırlar aşabilir bu sınırları normal işleme veya çalıştırılan bir yük testi aşmayı [Logic Apps ekibine başvurun](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım.

### <a name="logic-app-definition-limits"></a>Mantıksal uygulama tanımını sınırları

Bir tek mantıksal uygulama tanımını sınırlarını şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İş akışı başına Eylemler | 500 | Bu sınır genişletmek için gerektiği gibi iç içe geçmiş iş akışları ekleyebilirsiniz. |
| Eylem iç içe geçirme derinliği izin verilen | 8 | Bu sınır genişletmek için gerektiği gibi iç içe geçmiş iş akışları ekleyebilirsiniz. | 
| Her Abonelikteki bölge başına iş akışları | 1000 | | 
| İş akışı başına Tetikleyicileri | 10 | | 
| Anahtar kapsam durumlarda sınırı | 25 | | 
| İş akışı başına değişkenleri sayısı | 250 | | 
| İfade başına en fazla karakter | 8,192 | | 
| Max `trackedProperties` karakter cinsinden boyut | 16,000 | 
| `action`/`trigger` ad sınırı | 80 | | 
| `description` uzunluk sınırı | 256 | | 
| `parameters` Sınırı | 50 | | 
| `outputs` Sınırı | 10 | | 
|||| 

<a name="custom-connector-limits"></a>

### <a name="custom-connector-limits"></a>Özel bağlayıcı sınırları

Bu sınırları web API'leri oluşturabileceğiniz özel bağlayıcılar için geçerlidir.

| Ad | Sınır | 
| ---- | ----- | 
| Oluşturabileceğiniz özel bağlayıcı sayısı | Azure aboneliği başına 1000 | 
| Özel bir bağlayıcı tarafından oluşturulan her bağlantı için dakika başına istek sayısı | Bağlayıcı tarafından oluşturulan her bağlantı için 500 istek |
||| 

### <a name="integration-account-limits"></a>Tümleştirme hesabı sınırları

Bir tümleştirme hesabına ekleyebilirsiniz yapıları için sınırlar şunlardır.

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Şema | 8 MB | Kullanabileceğiniz [URI blob](../logic-apps/logic-apps-enterprise-integration-schemas.md) 2 MB'den büyük dosyaları karşıya yüklemek için. | 
| Harita (XSLT dosyası) | 2 MB | | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı okuma | 60,000 | İş yükü, gerektiğinde arasında birden fazla hesap dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı çağırma | 45,000 | İş yükü, gerektiğinde arasında birden fazla hesap dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı izleme | 45,000 | İş yükü, gerektiğinde arasında birden fazla hesap dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: eşzamanlı çağrıları engelleme | ~1,000 | Eşzamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
|||| 

Bu sınırlar bir tümleştirme hesabına ekleyebilirsiniz yapıların sayısını uygulanır.

#### <a name="free-pricing-tier"></a>Ücretsiz fiyatlandırma katmanı

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Sözleşmeler | 10 | | 
| Diğer yapı türleri | 25 | İş ortakları, şemalar, sertifikalar ve haritalar yapı türleri içerir. Her tür yapıları maksimum sayıya olabilir. | 
|||| 

#### <a name="standard-pricing-tier"></a>Standart fiyatlandırma katmanı

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Herhangi bir türde yapı | 500 | Yapı türleri anlaşmaları, iş ortakları, şemalar, sertifikalar ve eşlemeleri içerir. Her tür yapıları maksimum sayıya olabilir. | 
|||| 

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>B2B protokolleri (AS2, X12, EDIFACT) boyutu iletisi

B2B protokollerini Uygula sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Kodlanacağını ve uygular | 
| X12 | 50 MB | Kodlanacağını ve uygular | 
| EDIFACT | 50 MB | Kodlanacağını ve uygular | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Yapılandırması: IP adresleri

### <a name="logic-apps-service"></a>Logic Apps hizmeti

Bir mantıksal uygulama doğrudan diğer bir deyişle, yaptığı çağrıları aracılığıyla [HTTP](../connectors/connectors-native-http.md) veya [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) veya diğer HTTP isteklerini gelebilir bu listedeki IP adreslerinden.

|Logic Apps bölge|Giden IP|
|-----------------|-----------|
|Avustralya Doğu|13.75.149.4, 104.210.91.55, 104.210.90.241|
|Avustralya Güneydoğu|13.73.114.207, 13.77.3.139, 13.70.159.205|
|Güney Brezilya|191.235.82.221, 191.235.91.7, 191.234.182.26|
|Orta Kanada|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Doğu Kanada|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Orta Hindistan|52.172.154.168, 52.172.186.159, 52.172.185.79|
|Orta ABD|13.67.236.125, 104.208.25.27, 40.122.170.198|
|Doğu Asya|13.75.94.173, 40.83.127.19, 52.175.33.254|
|Doğu ABD|13.92.98.111, 40.121.91.41, 40.114.82.191|
|Doğu ABD 2|40.84.30.147, 104.208.155.200, 104.208.158.174|
|Japonya Doğu|13.71.158.3, 13.73.4.207, 13.71.158.120|
|Japonya Batı|40.74.140.4, 104.214.137.243, 138.91.26.45|
|Orta Kuzey ABD|168.62.248.37, 157.55.210.61, 157.55.212.238|
|Kuzey Avrupa|40.113.12.95, 52.178.165.215, 52.178.166.21|
|Orta Güney ABD|104.210.144.48, 13.65.82.17, 13.66.52.232|
|Güneydoğu Asya|13.76.133.155, 52.163.228.93, 52.163.230.166|
|Güney Hindistan|52.172.50.24, 52.172.55.231, 52.172.52.0|
|Batı Orta ABD|52.161.27.190, 52.161.18.218, 52.161.9.108|
|Batı Avrupa|40.68.222.65, 40.68.209.23, 13.95.147.65|
|Batı Hindistan|104.211.164.80, 104.211.162.205, 104.211.164.136|
|Batı ABD|52.160.92.112, 40.118.244.241, 40.118.241.243|
|Batı ABD 2|13.66.210.167, 52.183.30.169, 52.183.29.132|
|Birleşik Krallık Güney|51.140.74.14, 51.140.73.85, 51.140.78.44|
|Birleşik Krallık Batı|51.141.54.185, 51.141.45.238, 51.141.47.136|
| | |

### <a name="connectors"></a>Bağlayıcılar

Çağrılar, [Bağlayıcılar](../connectors/apis-list.md) bu listedeki IP adreslerinden gelen olun.

|Logic Apps bölge|Giden IP|
|-----------------|-----------|
|Avustralya Doğu|40.126.251.213|
|Avustralya Güneydoğu|40.127.80.34|
|Güney Brezilya|191.232.38.129|
|Orta Kanada|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Doğu Kanada|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Orta Hindistan|104.211.98.164|
|Orta ABD|40.122.49.51|
|Doğu Asya|23.99.116.181|
|Doğu ABD|191.237.41.52|
|Doğu ABD 2|104.208.233.100|
|Japonya Doğu|40.115.186.96|
|Japonya Batı|40.74.130.77|
|Orta Kuzey ABD|65.52.218.230|
|Kuzey Avrupa|104.45.93.9|
|Orta Güney ABD|104.214.70.191|
|Güneydoğu Asya|13.76.231.68|
|Güney Hindistan|104.211.227.225|
|Batı Avrupa|40.115.50.13|
|Batı Hindistan|104.211.161.203|
|Batı ABD|104.40.51.248|
|Birleşik Krallık Güney|51.140.80.51|
|Birleşik Krallık Batı|51.141.47.105|
| | | 

## <a name="next-steps"></a>Sonraki adımlar  

* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* [Yayın örnekleri ve senaryoları](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Video: Logic Apps ile iş süreçlerini otomatikleştirmek](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Video: sistemlerinizi Logic Apps ile tümleştirme](http://channel9.msdn.com/Events/Build/2016/P462)