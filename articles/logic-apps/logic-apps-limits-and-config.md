---
title: Sınırları ve yapılandırması - Azure Logic Apps | Microsoft Docs
description: Hizmet sınırları ve Azure mantıksal uygulamaları için yapılandırma değerleri
services: logic-apps
author: ecfan
manager: cfowler
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 713312725c92c6c78241e1cdcaa06280282c7777
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831855"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Sınırları ve Azure mantıksal uygulamaları için yapılandırma bilgilerini

Bu makalede, sınırları ve oluşturma ve Azure Logic Apps ile çalışan otomatik iş akışları için yapılandırma ayrıntılarını açıklanmaktadır. Microsoft Flow için bkz: [sınırlarını ve Microsoft Flow yapılandırmasını](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Tanımı sınırları

Bir tek mantıksal uygulama tanımını sınırlarını şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İş akışı başına Eylemler | 500 | Bu sınır genişletmek için gerektiği gibi iç içe geçmiş iş akışları ekleyebilirsiniz. |
| Eylemler için iç içe geçirme derinliği izin verilen | 8 | Bu sınır genişletmek için gerektiği gibi iç içe geçmiş iş akışları ekleyebilirsiniz. | 
| Her Abonelikteki bölge başına iş akışları | 1000 | | 
| İş akışı başına Tetikleyicileri | 10 | Kod görünümünde Tasarımcısı çalışırken | 
| Anahtar kapsam durumlarda sınırı | 25 | | 
| İş akışı başına değişkenleri | 250 | | 
| İfade başına karakter | 8,192 | | 
| En büyük boyutu `trackedProperties` | 16.000 karakterleri | 
| İçin ad `action` veya `trigger` | 80 karakter | | 
| uzunluğu `description` | 256 karakteri | | 
| Maksimum `parameters` | 50 | | 
| Maksimum `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Çalışma süresi ve saklama sınırları

Çalıştıran tek bir mantıksal uygulama sınırlarını şunlardır:

| Ad | Sınır | Notlar | 
|------|-------|-------| 
| Çalışma süresi | 90 gün | Bu sınırı değiştirmek için bkz: [değişiklik çalıştırma süresi](#change-duration). | 
| Depolama bekletme | Başlangıç zamanı çalışmanın 90 gün | Bu sınırı değiştirmek için bkz: [depolama bekletme değiştirmek](#change-retention). | 
| En az yinelenme aralığı | 1 saniye | | 
| En fazla yineleme aralığı | 500 gün | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Çalışma süresini ve depolama bekletme değiştirme

Bu sınır 7 gün ve 90 gün arasında bir değer değiştirebilirsiniz. Üst sınır gitmek için [Logic Apps ekibine başvurun](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım.

1. Azure portalında mantığı uygulamanızın menüsünde seçin **iş akışı ayarları**. 

2. Altında **çalışma zamanı seçeneklerini**, gelen **geçmiş saklama gün içinde çalıştırmak** listesinde, seçin **özel**. 

3. Girin veya istediğiniz gün sayısı için kaydırıcıyı sürükleyin.

<a name="looping-debatching-limits"></a>

## <a name="looping-and-debatching-limits"></a>Döngü ve sınırları debatching

Çalıştıran tek bir mantıksal uygulama sınırlarını şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Kadar yineleme | 5.000 | | 
| ForEach öğeleri | 100,000 | Kullanabileceğiniz [sorgu eylem](../connectors/connectors-native-query.md) gerektiği gibi daha büyük diziler filtre uygulamak için. | 
| ForEach paralellik | 50 | Varsayılan değer 20'dir. <p>ForEach döngüsü içinde bu varsayılan düzeyini değiştirmek için ayarlanmış `runtimeConfiguration` özelliğinde `foreach` eylem. <p>ForEach döngüsü sıralı olarak çalışacak şekilde ayarlanmış `operationOptions` "Sıralı" özelliğine `foreach` eylem. | 
| SplitOn öğeleri | 100,000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>İşleme sınırları

Çalıştıran tek bir mantıksal uygulama sınırlarını şunlardır:

| Ad | Sınır | Notlar | 
| ----- | ----- | ----- | 
| Eylemler yürütmeleri 5 dakika başına | 100,000 | 300000 için sınırı artırmak için bir mantıksal uygulama çalıştırabilirsiniz `High Throughput` modu. Yüksek verimlilik modu altında yapılandırmak için `runtimeConfiguration` iş akışı kaynak ayarlanmış `operationOptions` özelliğine `OptimizedForHighThroughput`. <p>**Not**: yüksek verimlilik modudur önizlemede. Ayrıca, gerektiğinde arasında birden fazla uygulama bir iş yükü dağıtabilirsiniz. | 
| Eylemler eşzamanlı giden çağrıları | ~2,500 | Eşzamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: eşzamanlı gelen çağrıları | ~1,000 | Eşzamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı okuma  | 60,000 | İş yükü, gerektiğinde arasında birden fazla uygulama dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı çağırma| 45,000 | İş yükü, gerektiğinde arasında birden fazla uygulama dağıtabilirsiniz. | 
|||| 

Normal işlem bu sınırları yukarıda gidin veya bu sınırları gitmiş olabilecek yük testi çalıştırmak için [Logic Apps ekibine başvurun](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>HTTP istek sınırları

Tek HTTP isteği ya da zaman uyumlu bağlayıcı çağrısı için sınırları şunlardır:

#### <a name="timeout"></a>Zaman Aşımı

Bazı bağlayıcı işlemleri zaman uyumsuz çağrılar yapın veya bu işlemler için zaman aşımı bu sınırlardan daha uzun olması Web kancası isteklerini dinlemek. Daha fazla bilgi için belirli bir bağlayıcının için teknik ayrıntılara bakın ve ayrıca [iş akışı tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Giden istek | 120 saniye | Uzun çalışan işlemleri için bir [zaman uyumsuz yoklama düzeni](../logic-apps/logic-apps-create-api-app.md#async-pattern) veya bir [döngü kadar](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Zaman uyumlu yanıt | 120 saniye | İç içe geçmiş iş akışı olarak başka bir mantıksal uygulama gerektirmediği sürece özgün istek yanıt almak için sınırı içinde yanıt tüm adımları bitmesi gerekir. Daha fazla bilgi için bkz: [çağrısı, tetikleyici veya logic apps iç içe](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>İleti boyutu

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İleti boyutu | 100 MB | Bu sınır olarak çözmek için bkz: [Öbekleme ile büyük iletileri işlemek](../logic-apps/logic-apps-handle-large-messages.md). Ancak, bazı bağlayıcılar ve API Öbekleme desteklemez veya varsayılan sınır bile. | 
| Öbekleme ile ileti boyutu | 1 GB | Bu sınır, yerel olarak Öbekleme desteklemek ya da, çalışma zamanı yapılandırmalarında Öbekleme etkinleştirmenize olanak eylemleri uygular. Daha fazla bilgi için bkz: [Öbekleme ile büyük iletileri işlemek](../logic-apps/logic-apps-handle-large-messages.md). | 
| İfade değerlendirme sınırı | 131.072 karakterleri | `@concat()`, `@base64()`, `@string()` İfadeleri bu sınırdan daha uzun olamaz. | 
|||| 

#### <a name="retry-policy"></a>Yeniden deneme ilkesi

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Yeniden deneme sayısı | 90 | Varsayılan değer 4'tür. Varsayılan değeri değiştirmek için kullanmak [yeniden deneme ilkesi parametresi](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| En uzun gecikme yeniden deneyin | 1 gün | Varsayılan değeri değiştirmek için kullanmak [yeniden deneme ilkesi parametresi](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Yeniden deneme Min gecikmesi | 5 saniye | Varsayılan değeri değiştirmek için kullanmak [yeniden deneme ilkesi parametresi](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Özel bağlayıcı sınırları

Burada, web API'leri oluşturabileceğiniz özel bağlayıcıların sınırları bulunmaktadır.

| Ad | Sınır | 
| ---- | ----- | 
| Özel bağlayıcılar sayısı | Azure aboneliği başına 1000 | 
| Özel bir bağlayıcı tarafından oluşturulan her bağlantı için dakika başına istek sayısı | Bağlantı başına 500 istekleri |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Tümleştirme hesabı sınırları

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Dışlayıcı sınırlar tümleştirme hesap başına

Burada, yapıları her tümleştirme hesabı sayısı sınırlamaları bulunmaktadır. Daha fazla bilgi için bkz: [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). 

*Ücretsiz katmanı*

Ücretsiz katmanı yalnızca keşif senaryoları için üretim senaryoları kullanın. Bu katman üretilen iş ve kullanımını kısıtlar ve hiçbir hizmet düzeyi sözleşmesi (SLA) sahiptir.

| Yapı | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari ortaklar | 25 | | 
| EDI ticaret sözleşmeleri | 10 | | 
| Haritalar | 25 | | 
| Şemalar | 25 | 
| Derlemeler | 10 | | 
| Toplu iş yapılandırmaları | 5 | 
| Sertifikalar | 25 | | 
|||| 

*Temel katman*

| Yapı | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari ortaklar | 2 | | 
| EDI ticaret sözleşmeleri | 1 | | 
| Haritalar | 500 | | 
| Şemalar | 500 | 
| Derlemeler | 25 | | 
| Toplu iş yapılandırmaları | 1 | | 
| Sertifikalar | 2 | | 
|||| 

*Standart katman*

| Yapı | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari ortaklar | 500 | | 
| EDI ticaret sözleşmeleri | 500 | | 
| Haritalar | 500 | | 
| Şemalar | 500 | 
| Derlemeler | 50 | | 
| Toplu iş yapılandırmaları | 5 |  
| Sertifikalar | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Yapı Kapasitesi sınırları

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Şema | 8 MB | 2 MB'den daha büyük bir dosya yüklemek için kullandığınız [URI blob](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Harita (XSLT dosyası) | 2 MB | | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı okuma | 60,000 | Gerektiği gibi birden fazla hesap arasında iş yükünü dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı çağırma | 45,000 | Gerektiği gibi birden fazla hesap arasında iş yükünü dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrı izleme | 45,000 | Gerektiği gibi birden fazla hesap arasında iş yükünü dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: eşzamanlı çağrıları engelleme | ~1,000 | Eşzamanlı istek sayısını azaltın veya gerektiği gibi süresini azaltın. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B Protokolü (AS2, X12, EDIFACT) ileti boyutu

B2B protokollerini Uygula sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Kodlanacağını ve uygular | 
| X12 | 50 MB | Kodlanacağını ve uygular | 
| EDIFACT | 50 MB | Kodlanacağını ve uygular | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Yapılandırması: IP adresleri

### <a name="azure-logic-apps-service"></a>Azure mantıksal uygulamaları hizmeti

Bir bölgedeki tüm logic apps IP adreslerinin aynı aralıkları kullanın. Logic apps ile doğrudan olun çağrılarını desteklemek üzere [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)ve bu giden ve gelen IP adreslerini göre eklemek için güvenlik duvarı yapılandırmalarını ayarlamak diğer HTTP istekleri mantıksal uygulamalarınızı var üzerinde olduğu:

| Logic Apps bölge | Giden IP |
|-------------------|-------------|
| Avustralya | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
| Avustralya Doğu | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Güney Brezilya | 191.235.82.221, 191.235.91.7, 191.234.182.26 |
| Orta Kanada | 52.233.29.92, 52.228.39.241, 52.228.39.244 |
| Doğu Kanada | 52.232.128.155, 52.229.120.45, 52.229.126.25 |
| Orta Hindistan | 52.172.154.168, 52.172.186.159, 52.172.185.79 |
| Orta ABD | 13.67.236.125, 104.208.25.27, 40.122.170.198 |
| Doğu Asya | 13.75.94.173, 40.83.127.19, 52.175.33.254 |
| Doğu ABD | 13.92.98.111, 40.121.91.41, 40.114.82.191 |
| Doğu ABD 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174 |
| Japonya Doğu | 13.71.158.3, 13.73.4.207, 13.71.158.120 |
| Japonya Batı | 40.74.140.4, 104.214.137.243, 138.91.26.45 |
| Orta Kuzey ABD | 168.62.248.37, 157.55.210.61, 157.55.212.238 |
| Kuzey Avrupa | 40.113.12.95, 52.178.165.215, 52.178.166.21 |
| Orta Güney ABD | 104.210.144.48, 13.65.82.17, 13.66.52.232 |
| Güney Hindistan | 52.172.50.24, 52.172.55.231, 52.172.52.0 |
| Güneydoğu Asya | 13.76.133.155, 52.163.228.93, 52.163.230.166 |
| Batı Orta ABD | 52.161.27.190, 52.161.18.218, 52.161.9.108 |
| Batı Avrupa | 40.68.222.65, 40.68.209.23, 13.95.147.65 |
| Batı Hindistan | 104.211.164.80, 104.211.162.205, 104.211.164.136 |
| Batı ABD | 52.160.92.112, 40.118.244.241, 40.118.241.243 |
| Batı ABD 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132 |
| Birleşik Krallık Güney | 51.140.74.14, 51.140.73.85, 51.140.78.44 |
| Birleşik Krallık Batı | 51.141.54.185, 51.141.45.238, 51.141.47.136 |
| | |

| Logic Apps bölge | Gelen IP |
|-------------------|------------|
| Avustralya Doğu | 3.75.153.66, 104.210.89.222, 104.210.89.244 |
| Avustralya Güneydoğu | 13.73.115.153, 40.115.78.70, 40.115.78.237 |
| Güney Brezilya | 191.235.86.199, 191.235.95.229, 191.235.94.220 |
| Orta Kanada | 13.88.249.209, 52.233.30.218, 52.233.29.79 |
| Doğu Kanada | 52.232.129.143, 52.229.125.57, 52.232.133.109 |
| Orta Hindistan | 52.172.157.194, 52.172.184.192, 52.172.191.194 |
| Orta ABD | 13.67.236.76, 40.77.111.254, 40.77.31.87 |
| Doğu Asya | 168.63.200.173, 13.75.89.159, 23.97.68.172 |
| Doğu ABD | 137.135.106.54, 40.117.99.79, 40.117.100.228 |
| Doğu ABD 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136 |
| Japonya Doğu | 13.71.146.140, 13.78.84.187, 13.78.62.130 |
| Japonya Batı | 40.74.140.173, 40.74.81.13, 40.74.85.215 |
| Orta Kuzey ABD | 168.62.249.81, 157.56.12.202, 65.52.211.164 |
| Kuzey Avrupa | 13.79.173.49, 52.169.218.253, 52.169.220.174 |
| Orta Güney ABD | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Güney Hindistan | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Güneydoğu Asya | 52.163.93.214, 52.187.65.81, 52.187.65.155 |
| Batı Orta ABD | 52.161.26.172, 52.161.8.128, 52.161.19.82 |
| Batı Avrupa | 13.95.155.53, 52.174.54.218, 52.174.49.6 |
| Batı Hindistan | 104.211.164.112, 104.211.165.81, 104.211.164.25 |
| Batı ABD | 52.160.90.237, 138.91.188.137, 13.91.252.184 |
| Batı ABD 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| Birleşik Krallık Güney | 51.140.79.109, 51.140.78.71, 51.140.84.39 |
| Birleşik Krallık Batı | 51.141.48.98, 51.141.51.145, 51.141.53.164 |
| | |

### <a name="connectors"></a>Bağlayıcılar

Çağrıları desteklemek için [Bağlayıcılar](../connectors/apis-list.md) olun, bu giden IP adresleri eklemek için güvenlik duvarı yapılandırmalarını ayarlamak dayalı üzerindeki mantıksal uygulamalarınızı var olduğu bölgeleri.

> [!IMPORTANT]
>
> Var olan yapılandırmaları varsa, lütfen güncelleştirmenizi **1 Eylül 2018 hemen önce** içerir ve logic apps var olduğu bölgeler için bu listeyi IP adresleriyle eşleşen. 

| Logic Apps bölge | Giden IP | 
|-------------------|-------------|  
| Avustralya Doğu | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213 | 
| Avustralya Güneydoğu | 13.77.50.240 - 13.77.50.255, 13.70.136.174, 40.127.80.34 | 
| Güney Brezilya | 191.233.203.192 - 191.233.203.207, 104.41.59.51, 191.232.38.129 | 
| Orta Kanada | 13.71.170.208 - 13.71.170.223, 13.71.170.224 - 13.71.170.239, 52.237.24.126, 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13 | 
| Doğu Kanada | 40.69.106.240 - 40.69.106.255, 52.242.35.152, 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52 | 
| Orta Hindistan | 104.211.81.192 - 104.211.81.207, 52.172.211.12, 104.211.98.164 | 
| Orta ABD | 13.89.171.80 - 13.89.171.95, 52.173.245.164, 40.122.49.51 | 
| Doğu Asya | 13.75.36.64 - 13.75.36.79, 52.175.23.169, 23.99.116.181 | 
| Doğu ABD | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 191.237.41.52 | 
| Doğu ABD 2 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100 | 
| Japonya Doğu | 13.78.108.0 - 13.78.108.15, 13.71.153.19, 40.115.186.96 | 
| Japonya Batı | 40.74.100.224 - 40.74.100.239, 104.215.61.248, 40.74.130.77 | 
| Orta Kuzey ABD | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230 | 
| Kuzey Avrupa | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9 | 
| Orta Güney ABD | 104.214.19.48 - 104.214.19.63, 13.65.86.57, 104.214.70.191 | 
| Güney Hindistan | 40.78.194.240 - 40.78.194.255, 13.71.125.22, 104.211.227.225 | 
| Güneydoğu Asya | 13.67.8.240 - 13.67.8.255, 52.187.68.19, 13.76.231.68 | 
| Batı Orta ABD | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 52.161.27.108, 52.161.30.5, 52.161.29.35, 52.161.26.212 | 
| Batı Avrupa | 13.69.64.208 - 13.69.64.223, 52.174.88.118, 40.115.50.13 | 
| Batı Hindistan | 104.211.146.224 - 104.211.146.239, 104.211.189.218, 104.211.161.203 | 
| Batı ABD | 40.112.243.160 - 40.112.243.175, 104.42.122.49, 104.40.51.248 | 
| Batı ABD 2 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 13.66.225.219, 13.66.218.78, 13.66.220.135, 13.66.219.14 | 
| Birleşik Krallık Güney | 51.140.148.0 - 51.140.148.15, 51.140.80.51, 51.140.80.51 | 
| Birleşik Krallık Batı | 51.140.211.0 - 51.140.211.15, 51.141.47.105, 51.141.47.105 | 
| | | 

## <a name="next-steps"></a>Sonraki adımlar  

* Bilgi edinmek için nasıl [ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Hakkında bilgi edinin [yayın örnekleri ve senaryoları](../logic-apps/logic-apps-examples-and-scenarios.md)
