---
title: Limitler ve yapılandırma - Azure Logic Apps | Microsoft Docs
description: Hizmet sınırlamaları ve Azure Logic Apps için yapılandırma değerleri
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d153fa495c82103460dd8e4e2aae6000e49eb3eb
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283598"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limitler ve yapılandırma bilgilerini Azure Logic Apps

Bu makalede, sınırlar ve oluşturma ve Azure Logic Apps ile otomatik iş akışları çalıştırma için yapılandırma ayrıntılarını açıklar. Microsoft Flow için bkz: [limitler ve yapılandırma, Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Tanım limitleri

Bir tek bir mantıksal uygulama tanımını sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İş akışı başına Eylemler | 500 | Bu sınırı genişletmek için gerektiği şekilde iç içe geçmiş iş akışları ekleyebilirsiniz. |
| İzin verilen eylemleri için iç içe geçme derinliği | 8 | Bu sınırı genişletmek için gerektiği şekilde iç içe geçmiş iş akışları ekleyebilirsiniz. | 
| Her Abonelikteki bölge başına iş akışları | 1000 | | 
| İş akışı başına Tetikleyicileri | 10 | Tasarımcı, kod görünümü çalışırken | 
| Geçiş kapsamı çalışmaları sınırı | 25 | | 
| İş akışı başına değişkenleri | 250 | | 
| Karakter başına ifadesi | 8,192 | | 
| En büyük boyutu `trackedProperties` | 16.000 karakter | 
| Adında `action` veya `trigger` | 80 karakter | | 
| Uzunluğu `description` | 256 karakter | | 
| En fazla `parameters` | 50 | | 
| En fazla `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Çalışma süresi ve bekletme sınırları

Bir tek bir mantıksal uygulama çalıştırması sınırları şunlardır:

| Ad | Sınır | Notlar | 
|------|-------|-------| 
| Çalıştırma süresi | 90 gün | Bu sınırı değiştirmek için bkz: [değişiklik çalıştırma süresini](#change-duration). | 
| Depolama bekletme | 90 gün içinde çalıştırmanın başlangıç saati | 7 gün-90 gün arasında bir değer bu sınırı değiştirmek için bkz: [depolama bekletmeyi değiştirme](#change-retention). | 
| En az yinelenme aralığı | 1 saniye | | 
| En fazla yinelenme aralığı | 500 gün | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Çalışma süresi ve depolama bekletmeyi değiştirme

Varsayılan sınır 7 gün ve 90 gün arasında değiştirmek için aşağıdaki adımları izleyin. Maksimum sınırı Git gerekiyorsa [Logic Apps ekibiyle](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım için.

1. Azure portalında mantıksal uygulama menüsünde seçin **iş akışı ayarları**. 

2. Altında **çalışma zamanı seçenekleri**, gelen **geçmişinin saklanacağı gün içinde çalıştırma** listesinde **özel**. 

3. Girin veya istediğiniz gün sayısı için kaydırıcıyı sürükleyin.

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Eşzamanlılık, döngü ve limitleri

Bir tek bir mantıksal uygulama çalıştırması sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Tetikleyici eşzamanlılık | 50 | Varsayılan sınırı 20'dir. Aynı anda ya da paralel mantıksal uygulama örneği sayısı bu sınırı açıklar. <p><p>Varsayılan sınır 1 ile 50 arasında bir değer aralığında değiştirmek için bkz: [değişiklik tetikleyici eşzamanlılık](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) veya [tetikleme örnekleri sırayla](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger). | 
| En fazla bekleme çalıştırmalar | 100 | Varsayılan sınır 10'dur. Mantıksal uygulamanızı zaten en fazla eşzamanlı örneği çalışırken çalıştırmak için bekleyebileceği mantıksal uygulama örneği sayısı bu sınırı açıklar. <p><p>Varsayılan sınırı 0 ile 100 arasında bir değer aralığında değiştirmek için bkz: [değişiklik bekleme çalıştırmaları sınırlamak](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs). | 
| Foreach öğeleri | 100.000 | Bu sınır en fazla bir "for each" döngüsü işleyebilen dizi öğe sayısını açıklar. <p><p>Daha büyük dizileri filtrelemek için kullanabileceğiniz [sorgu eylemi](../connectors/connectors-native-query.md). | 
| Foreach yineleme | 50 | Varsayılan sınırı 20'dir. "For each" en fazla sayısı bu sınırı açıklar aynı anda ya da paralel yineleme döngüsü. <p><p>Varsayılan sınır 1 ile 50 arasında bir değer aralığında değiştirmek için bkz. ["for each" eşzamanlılık değiştirmek](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) veya ["for each" çalıştırma sırayla döngü](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each). | 
| SplitOn öğeleri | 100.000 | | 
| Yinelemelere kadar | 5.000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>İşleme sınırları

Bir tek bir mantıksal uygulama çalıştırması sınırları şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Eylem: Yürütme 5 dakika başına | 300,000 | Varsayılan sınır 100. 000 ' dir. Varsayılan sınırı değiştirmek için bkz ["yüksek aktarım hızı" modunda mantıksal uygulamanızı çalıştırın](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode), Önizleme aşamasında olduğu. Veya iş yükü, gerektiğinde birden fazla mantıksal uygulama arasında dağıtabilirsiniz. | 
| Eylem: Eş zamanlı giden çağrılar | ~2,500 | Eş zamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: eş zamanlı gelen çağrılar | ~1,000 | Eş zamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
| Çalışma zamanı uç noktası: çağrılar 5 dakika başına okuma  | 60,000 | İş yükü, gerektiğinde birden fazla uygulama arasında dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrıları | 45,000 | İş yükü, gerektiğinde birden fazla uygulama arasında dağıtabilirsiniz. | 
| İçerik aktarım hızı 5 dakika başına | 600 MB | İş yükü, gerektiğinde birden fazla uygulama arasında dağıtabilirsiniz. | 
|||| 

Normal işleme bu sınırların üzerinde gidin veya bu sınırların üzerinde geçebilir, yük testi çalıştırmak için [Logic Apps ekibiyle](mailto://logicappsemail@microsoft.com) gereksinimlerinizi ile ilgili Yardım için.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>HTTP istek sınırları

Tek HTTP isteği veya zaman uyumlu bağlayıcı çağrı sınırları şunlardır:

#### <a name="timeout"></a>Zaman Aşımı

Bazı bağlayıcı işlemler zaman uyumsuz çağrıları yapmak veya bu işlemler için zaman aşımı limitler uzun olabilir. Bu nedenle, Web kancası isteğini, dinleme. Daha fazla bilgi için özel bağlayıcı için teknik ayrıntıları bakın ve ayrıca [iş akışı tetikleyici ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Giden istek | 120 saniye | Uzun çalışan işlemleri için bir [yoklama zaman uyumsuz desen](../logic-apps/logic-apps-create-api-app.md#async-pattern) veya [until döngüsü](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Zaman uyumlu yanıt | 120 saniye | İç içe geçmiş iş akışı olarak başka bir mantıksal uygulama çağırmadığınız sürece özgün istek yanıt almak tüm adımları yanıt sınırı içinde tamamlanmalıdır. Daha fazla bilgi için [çağrı, tetikleyici veya iç içe mantıksal uygulamalar](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>İleti boyutu

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| İleti boyutu | 100 MB | Bu sınırını çözmek için bkz: [Öbekleme ile büyük iletileri işleyen](../logic-apps/logic-apps-handle-large-messages.md). Ancak, bazı bağlayıcılar ve API Öbekleme desteklemez veya varsayılan sınır bile. | 
| Öbekleme ile ileti boyutu | 1 GB | Bu limit, yerel olarak destekleyen parçalama veya çalışma zamanı yapılandırmalarını Öbekleme etkinleştirmenize izin eylemler için geçerlidir. Daha fazla bilgi için [Öbekleme ile büyük iletileri işleyen](../logic-apps/logic-apps-handle-large-messages.md). | 
| İfade değerlendirme limiti | 131.072 karakter | `@concat()`, `@base64()`, `@string()` İfadeleri bu sınırdan daha uzun olamaz. | 
|||| 

#### <a name="retry-policy"></a>Yeniden deneme ilkesi

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Yeniden deneme sayısı | 90 | Varsayılan 4'tür. Varsayılan değiştirmek için kullanın [ilke parametresi yeniden](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| En uzun gecikme yeniden deneyin | 1 gün | Varsayılan değiştirmek için kullanın [ilke parametresi yeniden](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Yeniden deneme gecikmesi Min | 5 saniye | Varsayılan değiştirmek için kullanın [ilke parametresi yeniden](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Özel bağlayıcı sınırlamaları

Web API'leri oluşturabileceğiniz özel bağlayıcılar için sınırlar aşağıda verilmiştir.

| Ad | Sınır | 
| ---- | ----- | 
| Özel bağlayıcılar sayısı | Azure aboneliği başına 1000 | 
| Özel bir bağlayıcı tarafından oluşturulan her bağlantı için dakika başına istek sayısı | Bağlantı başına 500 istek |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Tümleştirme hesabı sınırları

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Tümleştirme hesabı başına yapıt sınırlar

Her bir tümleştirme hesabı yapıtları sayısına yönelik sınırlar aşağıda verilmiştir. Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). 

*Ücretsiz katmanı*

Ücretsiz katmanı, yalnızca keşif senaryolarda, üretim senaryolarında kullanın. Bu katman, aktarım hızı ve kullanımını kısıtlayan ve hiçbir hizmet düzeyi sözleşmesi (SLA) sahiptir.

| Yapıt | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari iş ortakları | 25 | | 
| EDI ticari sözleşmeleri | 10 | | 
| Haritalar | 25 | | 
| Şemalar | 25 | 
| Derlemeler | 10 | | 
| Toplu iş yapılandırmaları | 5 | 
| Sertifikalar | 25 | | 
|||| 

*Temel katman*

| Yapıt | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari iş ortakları | 2 | | 
| EDI ticari sözleşmeleri | 1 | | 
| Haritalar | 500 | | 
| Şemalar | 500 | 
| Derlemeler | 25 | | 
| Toplu iş yapılandırmaları | 1 | | 
| Sertifikalar | 2 | | 
|||| 

*Standart katman*

| Yapıt | Sınır | Notlar | 
|----------|-------|-------| 
| EDI ticari iş ortakları | 500 | | 
| EDI ticari sözleşmeleri | 500 | | 
| Haritalar | 500 | | 
| Şemalar | 500 | 
| Derlemeler | 50 | | 
| Toplu iş yapılandırmaları | 5 |  
| Sertifikalar | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Yapıt kapasite sınırları

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| Şema | 8 MB | 2 MB'tan büyük dosyaları yüklemek için kullandığınız [URI blob](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Harita (XSLT dosyası) | 2 MB | | 
| Çalışma zamanı uç noktası: çağrılar 5 dakika başına okuma | 60,000 | İş yükü, gerektiğinde birden fazla hesap genelinde dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrıları | 45,000 | İş yükü, gerektiğinde birden fazla hesap genelinde dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: 5 dakika başına çağrıları izleme | 45,000 | İş yükü, gerektiğinde birden fazla hesap genelinde dağıtabilirsiniz. | 
| Çalışma zamanı uç noktası: eş zamanlı çağrılar engelleme | ~1,000 | Eş zamanlı istek sayısını azaltın veya gerektiğinde süresini azaltın. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B Protokolü (AS2, EDIFACT olan X12) ileti boyutu

B2B protokolleri için uygulanan limitler şunlardır:

| Ad | Sınır | Notlar | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Kod çözme ve kodlama için geçerlidir | 
| X12 | 50 MB | Kod çözme ve kodlama için geçerlidir | 
| EDIFACT | 50 MB | Kod çözme ve kodlama için geçerlidir | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Yapılandırması: IP adresleri

### <a name="azure-logic-apps-service"></a>Azure Logic Apps hizmetinin

Bir bölgede tüm logic apps, aynı IP adresi aralıklarını kullanın. Logic apps ile doğrudan olun çağrıları desteklemek için [HTTP](../connectors/connectors-native-http.md), [da HTTP + Swagger](../connectors/connectors-native-http-swagger.md)ve içerirler göre bu giden ve gelen IP adresleri, bu nedenle, güvenlik duvarı yapılandırmalarını ayarlamak diğer HTTP istekleri logic apps mevcut şirket burada:

| Logic Apps bölgesi | Giden IP |
|-------------------|-------------|
| Avustralya Doğu | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Avustralya Güneydoğu | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
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

| Logic Apps bölgesi | Gelen IP |
|-------------------|------------|
| Avustralya Doğu | 3.75.153.66, 104.210.89.222 104.210.89.244 |
| Avustralya Güneydoğu | 13.73.115.153, 40.115.78.70 40.115.78.237 |
| Güney Brezilya | 191.235.86.199, 191.235.95.229 191.235.94.220 |
| Orta Kanada | 13.88.249.209, 52.233.30.218 52.233.29.79 |
| Doğu Kanada | 52.232.129.143, 52.229.125.57 52.232.133.109 |
| Orta Hindistan | 52.172.157.194, 52.172.184.192 52.172.191.194 |
| Orta ABD | 13.67.236.76, 40.77.111.254 40.77.31.87 |
| Doğu Asya | 168.63.200.173, 13.75.89.159 23.97.68.172 |
| Doğu ABD | 137.135.106.54, 40.117.99.79 40.117.100.228 |
| Doğu ABD 2 | 40.84.25.234, 40.79.44.7 40.84.59.136 |
| Japonya Doğu | 13.71.146.140, 13.78.84.187 13.78.62.130 |
| Japonya Batı | 40.74.140.173, 40.74.81.13 40.74.85.215 |
| Orta Kuzey ABD | 168.62.249.81, 157.56.12.202 65.52.211.164 |
| Kuzey Avrupa | 13.79.173.49, 52.169.218.253 52.169.220.174 |
| Orta Güney ABD | 13.65.98.39, 13.84.41.46 13.84.43.45 |
| Güney Hindistan | 52.172.9.47, 52.172.49.43 52.172.51.140 |
| Güneydoğu Asya | 52.163.93.214, 52.187.65.81 52.187.65.155 |
| Batı Orta ABD | 52.161.26.172, 52.161.8.128 52.161.19.82 |
| Batı Avrupa | 13.95.155.53, 52.174.54.218 52.174.49.6 |
| Batı Hindistan | 104.211.164.112, 104.211.165.81 104.211.164.25 |
| Batı ABD | 52.160.90.237, 138.91.188.137 13.91.252.184 |
| Batı ABD 2 | 13.66.224.169, 52.183.30.10 52.183.39.67 |
| Birleşik Krallık Güney | 51.140.79.109, 51.140.78.71 51.140.84.39 |
| Birleşik Krallık Batı | 51.141.48.98, 51.141.51.145 51.141.53.164 |
| | |

### <a name="connectors"></a>Bağlayıcılar

Çağrıları desteklemek için [Bağlayıcılar](../connectors/apis-list.md) içerirler bu giden IP adresleri için güvenlik duvarı yapılandırmalarınızı ayarlama yapma, logic apps bulunduğu bölgelerine bağlı.

> [!IMPORTANT]
>
> Var olan yapılandırmaları varsa, lütfen güncelleştirmeniz **olabildiğince çabuk 1 Eylül 2018'den önce** içerir ve logic apps bulunduğu bölge için bu listedeki IP adresleriyle eşleşen. 

| Logic Apps bölgesi | Giden IP | 
|-------------------|-------------|  
| Avustralya Doğu | 13.70.72.192 - 13.70.72.207, 13.72.243.10 40.126.251.213 | 
| Avustralya Güneydoğu | 13.77.50.240 - 13.77.50.255, 13.70.136.174 40.127.80.34 | 
| Güney Brezilya | 191.233.203.192 - 191.233.203.207, 104.41.59.51 191.232.38.129 | 
| Orta Kanada | -13.71.170.223, 13.71.170.224 - 13.71.170.208 13.71.170.239, 52.237.24.126, 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13, 52.233.26.83 | 
| Doğu Kanada | 40.69.106.240 - 40.69.106.255, 52.242.35.152, 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52, 52.229.120.131 | 
| Orta Hindistan | 104.211.81.192 - 104.211.81.207, 52.172.211.12 104.211.98.164 | 
| Orta ABD | 13.89.171.80 - 13.89.171.95, 52.173.245.164 40.122.49.51 | 
| Doğu Asya | 13.75.36.64 - 13.75.36.79, 52.175.23.169 23.99.116.181 | 
| Doğu ABD | 40.71.11.80 - 40.71.11.95, 40.71.249.205 191.237.41.52 | 
| Doğu ABD 2 | 40.70.146.208 - 40.70.146.223, 52.232.188.154 104.208.233.100 | 
| Japonya Doğu | 13.78.108.0 - 13.78.108.15, 13.71.153.19 40.115.186.96 | 
| Japonya Batı | 40.74.100.224 - 40.74.100.239, 104.215.61.248 40.74.130.77 | 
| Orta Kuzey ABD | 52.162.107.160 - 52.162.107.175, 52.162.242.161 65.52.218.230 | 
| Kuzey Avrupa | 13.69.227.208 - 13.69.227.223, 52.178.150.68 104.45.93.9 | 
| Orta Güney ABD | 104.214.19.48 - 104.214.19.63, 13.65.86.57 104.214.70.191 | 
| Güney Hindistan | 40.78.194.240 - 40.78.194.255, 13.71.125.22 104.211.227.225 | 
| Güneydoğu Asya | 13.67.8.240 - 13.67.8.255, 52.187.68.19 13.76.231.68 | 
| Batı Orta ABD | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 52.161.27.108, 52.161.30.5, 52.161.29.35, 52.161.26.212, 52.161.128 | 
| Batı Avrupa | 13.69.64.208 - 13.69.64.223, 52.174.88.118 40.115.50.13 | 
| Batı Hindistan | 104.211.146.224 - 104.211.146.239, 104.211.189.218 104.211.161.203 | 
| Batı ABD | 40.112.243.160 - 40.112.243.175, 104.42.122.49 104.40.51.248 | 
| Batı ABD 2 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 13.66.225.219, 13.66.218.78, 13.66.220.135, 13.66.219.14, 13.66.221.19 | 
| Birleşik Krallık Güney | 51.140.148.0 - 51.140.148.15, 51.140.80.51 | 
| Birleşik Krallık Batı | 51.140.211.0 - 51.140.211.15, 51.141.47.105 | 
| | | 

## <a name="next-steps"></a>Sonraki adımlar  

* Bilgi edinmek için nasıl [ilk mantıksal uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Hakkında bilgi edinin [yayın örnekleri ve senaryoları](../logic-apps/logic-apps-examples-and-scenarios.md)
