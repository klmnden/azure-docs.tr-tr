---
title: "Azure uygulama ağ geçidi web uygulaması güvenlik duvarı CRS Kural gruplarını ve kurallarını | Microsoft Docs"
description: "Bu sayfa, web uygulaması güvenlik duvarı CRS Kural gruplarını ve kurallarını bilgi sağlar."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: davidmu
ms.openlocfilehash: 9265be4ac4258115c9302189d84b20e4894d42bb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="list-of-web-application-firewall-crs-rule-groups-and-rules-offered"></a>Web uygulaması güvenlik duvarı CRS Kural gruplarını ve kurallarını listesi sunulan

Uygulama ağ geçidi web uygulaması Güvenlik Duvarı (WAF) web uygulamaları, Ortak Güvenlik Açıkları ve güvenlik açıklarına korur. Bu OWASP çekirdek kural kümeleri üzerinde 2.2.9 veya 3.0 göre tanımlanan kurallar yoluyla yapılır. Bu kurallar bir kuralı tarafından temelinde devre dışı bırakılabilir. Bu makale, sunulan rulesets ve geçerli kurallarını içerir.

Aşağıdaki tablolarda Kural gruplarını ve uygulama ağ geçidi ile web uygulaması güvenlik duvarı kullanırken kullanılabilir olan kuralları olur.  Her tablo belirli bir CRS sürümü için bir kural grubu bulunan kuralları gösterir.

##<a name="owasp30"></a>OWASP_3.0

### <a name="crs910"></a>  <p x-ms-format-detection="none">İSTEK 910 IP SAYGINLIĞI</p>

|RuleId|Açıklama|
|---|---|
|910011|Kural 910011|
|910012|Kural 910012|
|910000|Bilinen kötü amaçlı istemciden (önceki trafiği ihlalleri dayalı) isteyin.|
|910100|İstemci IP yüksek Risk ülke konumdan değil.|
|910120|Kural 910120|
|910130|Kural 910130|
|910150|HTTP kara liste eşleştirmek için arama motoru IP|
|910160|HTTP kara liste eşleşme göndericisi IP|
|910170|Şüpheli IP için HTTP kara liste eşleşme|
|910180|Harvester IP için HTTP kara liste eşleşme|
|910013|Kural 910013|
|910014|Kural 910014|
|910015|Kural 910015|
|910016|Kural 910016|
|910017|Kural 910017|
|910018|Kural 910018|

### <a name="crs911"></a> <p x-ms-format-detection="none">911 YÖNTEMİ ZORLAMA İSTEĞİ</p>

|RuleId|Açıklama|
|---|---|
|911011|Kural 911011|
|911012|Kural 911012|
|911100|Yöntemi İlkesi tarafından izin verilmiyor|
|911013|Kural 911013|
|911014|Kural 911014|
|911015|Kural 911015|
|911016|Kural 911016|
|911017|Kural 911017|
|911018|Kural 911018|

### <a name="crs912"></a> <p x-ms-format-detection="none">İSTEK 912 DOS KORUMASI</p>

|RuleId|Açıklama|
|---|---|
|912100|Kural 912100|
|912012|Kural 912012|
|912120|% Tanımlanan hizmet reddi (DoS) saldırısı @{tx.real_ip} (% @{tx.dos_block_counter} isabet son uyarı itibaren)|
|912130|Kural 912130|
|912140|Kural 912140|
|912150|Kural 912150|
|912160|Kural 912160|
|912170|Olası hizmet reddi (DoS) saldırısı % @{tx.real_ip} - istek Bursts sayısı = % @{ip.dos_burst_counter}|
|912013|Kural 912013|
|912014|Kural 912014|
|912019|Kural 912019|
|912171|Olası hizmet reddi (DoS) saldırısı % @{tx.real_ip} - istek Bursts sayısı = % @{ip.dos_burst_counter}|
|912015|Kural 912015|
|912016|Kural 912016|
|912017|Kural 912017|
|912018|Kural 912018|

### <a name="crs913"></a> <p x-ms-format-detection="none">İSTEK 913 TARAYICI ALGILAMA</p>

|RuleId|Açıklama|
|---|---|
|913011|Kural 913011|
|913012|Kural 913012|
|913100|User-Agent Güvenlik tarayıcıyla ilişkilendirilmiş bulundu|
|913110|Güvenlik tarayıcısı ile ilişkili istek üst bilgisi bulunamadı|
|913120|İstek dosyaadı/güvenlik tarayıcısı ile ilişkili bağımsız değişken bulundu|
|913013|Kural 913013|
|913014|Kural 913014|
|913101|User-Agent scripting genel HTTP istemciyle ilişkili bulundu|
|913102|User-Agent web Gezgin/bot ile ilişkili bulundu|
|913015|Kural 913015|
|913016|Kural 913016|
|913017|Kural 913017|
|913018|Kural 913018|

### <a name="crs920"></a> <p x-ms-format-detection="none">920 PROTOKOLÜ ZORLAMA İSTEĞİ</p>

|RuleId|Açıklama|
|---|---|
|920011|Kural 920011|
|920012|Kural 920012|
|920100|Geçersiz bir HTTP istek satır|
|920130|İstek gövdesi ayrıştırılamadı.|
|920140|Çok bölümlü istek gövdesi katı doğrulama başarısız PE %@{REQBODY_PROCESSOR_ERROR =} BQ %@{MULTIPART_BOUNDARY_QUOTED} BW %@{MULTIPART_BOUNDARY_WHITESPACE} DB %@{MULTIPART_DATA_BEFORE} DA %@{MULTIPART_DATA_AFTER} HF %@{MULTIPART_HEADER_FOLDING} LF % @ {MULTIPART_LF_LINE}     SM %@{MULTIPART_SEMICOLON_MISSING} IQ %@{MULTIPART_INVALID_QUOTING} BULUNUR %@{MULTIPART_INVALID_HEADER_FOLDING} FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED}|
|920160|Content-Length HTTP üstbilgisi sayısal değil.|
|920170|GET veya HEAD gövde içerikle isteyin.|
|920180|POST isteğini Content-Length üstbilgisi eksik.|
|920190|Aralık = son bayt değeri geçersiz.|
|920210|Birden çok ve çakışan bağlantı üstbilgi veri bulundu.|
|920220|URL kodlaması kötüye saldırı girişimi|
|920240|URL kodlaması kötüye saldırı girişimi|
|920250|UTF8 Uygunsuz kullanım saldırı girişiminde kodlaması|
|920260|Unicode tam/Yarı genişlik kötüye saldırı girişimi|
|920270|(Null karakter) isteğinde geçersiz bir karakter|
|920280|İstek ana bilgisayar üstbilgisi eksik|
|920290|Boş ana bilgisayar üstbilgisi|
|920310|İsteği boş olduğundan üstbilgi kabul et|
|920311|İsteği boş olduğundan üstbilgi kabul et|
|920330|Boş Kullanıcı Aracısı üstbilgisi|
|920340|İçeren içerik ancak eksik Content-Type üstbilgisi isteği|
|920350|Ana bilgisayar üstbilgisi sayısal bir IP adresidir.|
|920380|İstek çok fazla bağımsız değişken|
|920360|Bağımsız değişken adı çok uzun|
|920370|Bağımsız değişken değeri çok uzun|
|920390|Toplam bağımsız değişkenleri boyutu aşıldı|
|920400|Karşıya yüklenen dosya boyutu çok büyük|
|920410|Karşıya yüklenen dosyaların toplam boyutu çok büyük|
|920420|İsteğin içerik türü ilke tarafından izin verilmiyor|
|920430|HTTP protokolü sürümünü ilke tarafından izin verilmiyor|
|920440|URL dosya uzantısı İlkesi tarafından kısıtlanıyor|
|920450|HTTP üstbilgisi (%@{MATCHED_VAR}) ilkesi tarafından kısıtlanıyor|
|920013|Kural 920013|
|920014|Kural 920014|
|920200|Aralık = çok fazla alan (6 veya daha fazla)|
|920201|Aralık = çok fazla alan pdf istek (35 ya da daha fazla) için|
|920230|Birden çok URL kodlaması algılandı|
|920300|Eksik isteği bir üst bilgisi kabul et|
|920271|Geçersiz bir karakter isteğinde (olmayan yazdırılabilir karakter)|
|920320|Kullanıcı Aracısı üstbilgisi eksik|
|920015|Kural 920015|
|920016|Kural 920016|
|920272|Geçersiz bir karakter isteğinde (dışında yazdırılabilir karakter ASCII 127 aşağıda)|
|920017|Kural 920017|
|920018|Kural 920018|
|920202|Aralık = çok fazla alan pdf istek (6 veya daha fazla) için|
|920273|(Dışında çok sıkı kümesi) isteğinde geçersiz bir karakter|
|920274|Geçersiz karakter (dışında çok sıkı kümesi) istek üstbilgileri|
|920460|Kural 920460|

### <a name="crs921"></a> <p x-ms-format-detection="none">921 PROTOKOLÜ SALDIRI İSTEĞİ</p>

|RuleId|Açıklama|
|---|---|
|921011|Kural 921011|
|921012|Kural 921012|
|921100|HTTP isteği saldırı kaçakçılığı.|
|921110|HTTP isteği kaçakçılığı saldırısı|
|921120|HTTP yanıtı bölme saldırısı|
|921130|HTTP yanıtı bölme saldırısı|
|921140|HTTP üstbilgisi ekleme saldırısı üstbilgileri aracılığıyla|
|921150|HTTP üstbilgisi ekleme saldırısı Yükü (CR/LF algılanan) aracılığıyla|
|921160|HTTP üstbilgisi ekleme saldırısı Yükü (CR/LF ve üstbilgi adı algılandı) aracılığıyla|
|921013|Kural 921013|
|921014|Kural 921014|
|921151|HTTP üstbilgisi ekleme saldırısı Yükü (CR/LF algılanan) aracılığıyla|
|921015|Kural 921015|
|921016|Kural 921016|
|921170|Kural 921170|
|921180|HTTP parametresini kirliliği (% @{TX.1})|
|921017|Kural 921017|
|921018|Kural 921018|

### <a name="crs930"></a> <p x-ms-format-detection="none">İSTEK-930-UYGULAMA-SALDIRI-LFI</p>

|RuleId|Açıklama|
|---|---|
|930011|Kural 930011|
|930012|Kural 930012|
|930100|Yol çapraz geçiş saldırısı (/.. /)|
|930110|Yol çapraz geçiş saldırısı (/.. /)|
|930120|İşletim sistemi dosya erişim girişimi|
|930130|Kısıtlanmış dosya erişim girişimi|
|930013|Kural 930013|
|930014|Kural 930014|
|930015|Kural 930015|
|930016|Kural 930016|
|930017|Kural 930017|
|930018|Kural 930018|

### <a name="crs931"></a> <p x-ms-format-detection="none">İSTEK-931-UYGULAMA-SALDIRI-RFI</p>

|RuleId|Açıklama|
|---|---|
|931011|Kural 931011|
|931012|Kural 931012|
|931100|Olası bir uzak dosya ekleme (RFI) saldırı IP adresini kullanarak URL parametresi =|
|931110|Olası bir uzak dosya ekleme (RFI) saldırı = ortak RFI savunmasız parametre kullanılan ad w/URL yükü|
|931120|Olası uzak dosya ekleme (RFI) saldırı URL kullanılan yükü w ve sondaki soru işareti karakteri (?) =|
|931013|Kural 931013|
|931014|Kural 931014|
|931130|Olası uzak dosya ekleme (RFI) saldırı etki alanı dışı başvuru/bağlantısı =|
|931015|Kural 931015|
|931016|Kural 931016|
|931017|Kural 931017|
|931018|Kural 931018|

### <a name="crs932"></a> <p x-ms-format-detection="none">İSTEK-932-UYGULAMA-SALDIRI-RCE</p>

|RuleId|Açıklama|
|---|---|
|932011|Kural 932011|
|932012|Kural 932012|
|932120|Uzaktan komut yürütme = bulunan Windows PowerShell komutu|
|932130|Uzaktan komut yürütme UNIX Kabuk ifadesi bulundu =|
|932140|Uzaktan komut yürütme / komut BULUNURSA = Windows|
|932160|Uzaktan komut yürütme bulunan UNIX Kabuk kodu =|
|932170|Uzaktan komut yürütme Shellshock (CVE-2014-6271) =|
|932171|Uzaktan komut yürütme Shellshock (CVE-2014-6271) =|
|932013|Kural 932013|
|932014|Kural 932014|
|932015|Kural 932015|
|932016|Kural 932016|
|932017|Kural 932017|
|932018|Kural 932018|

### <a name="crs933"></a> <p x-ms-format-detection="none">İSTEK-933-UYGULAMA-SALDIRI-PHP</p>

|RuleId|Açıklama|
|---|---|
|933011|Kural 933011|
|933012|Kural 933012|
|933100|PHP ekleme saldırı açma/kapatma etiketi bulundu =|
|933110|PHP ekleme saldırı PHP komut dosyasını karşıya yükleme bulundu =|
|933120|PHP ekleme saldırı yapılandırma yönergesi bulundu =|
|933130|PHP ekleme saldırı değişkenleri bulundu =|
|933150|PHP ekleme saldırı yüksek riskli PHP işlev adı bulundu =|
|933160|PHP ekleme saldırı yüksek riskli PHP işlev çağrısı bulundu =|
|933180|PHP ekleme saldırı değişken işlev çağrısı bulundu =|
|933013|Kural 933013|
|933014|Kural 933014|
|933151|PHP ekleme saldırı orta riskli PHP işlev adı bulundu =|
|933015|Kural 933015|
|933016|Kural 933016|
|933131|PHP ekleme saldırı değişkenleri bulundu =|
|933161|PHP ekleme saldırı düşük değer PHP işlev çağrısı bulundu =|
|933111|PHP ekleme saldırı PHP komut dosyasını karşıya yükleme bulundu =|
|933017|Kural 933017|
|933018|Kural 933018|

### <a name="crs941"></a> <p x-ms-format-detection="none">İSTEK-941-UYGULAMA-SALDIRI-XSS</p>

|RuleId|Açıklama|
|---|---|
|941011|Kural 941011|
|941012|Kural 941012|
|941100|XSS saldırının algılandığı libinjection aracılığıyla|
|941110|XSS filtresi - kategori 1 komut dosyası etiketi vektör =|
|941130|XSS filtresi - kategori 3 özniteliği vektör =|
|941140|XSS filtresi - kategori 4 Javascript URI vektör =|
|941150|XSS filtresi - kategori 5 izin verilmeyen HTML özniteliklerini =|
|941180|Düğüm Doğrulayıcı kara liste anahtar sözcükler|
|941190|IE XSS - saldırının algılandığı filtreler.|
|941200|IE XSS - saldırının algılandığı filtreler.|
|941210|IE XSS - saldırının algılandığı filtreler.|
|941220|IE XSS - saldırının algılandığı filtreler.|
|941230|IE XSS - saldırının algılandığı filtreler.|
|941240|IE XSS - saldırının algılandığı filtreler.|
|941260|IE XSS - saldırının algılandığı filtreler.|
|941270|IE XSS - saldırının algılandığı filtreler.|
|941280|IE XSS - saldırının algılandığı filtreler.|
|941290|IE XSS - saldırının algılandığı filtreler.|
|941300|IE XSS - saldırının algılandığı filtreler.|
|941310|US-ASCII hatalı biçimlendirilmiş kodlama XSS filtresi - saldırı algılandı.|
|941350|UTF-7 kodlama IE XSS - saldırı algılandı.|
|941013|Kural 941013|
|941014|Kural 941014|
|941320|Olası XSS saldırının algılandığı - HTML etiketi işleyicisi|
|941015|Kural 941015|
|941016|Kural 941016|
|941017|Kural 941017|
|941018|Kural 941018|

### <a name="crs942"></a> <p x-ms-format-detection="none">İSTEK-942-UYGULAMA-SALDIRI-SQLI</p>

|RuleId|Açıklama|
|---|---|
|942011|Kural 942011|
|942012|Kural 942012|
|942100|SQL ekleme saldırının algılandığı libinjection aracılığıyla|
|942140|SQL ekleme saldırısında algılanan ortak DB adları =|
|942160|Sleep() veya benchmark() kullanarak görme sqli testleri algılar.|
|942170|Koşullu sorguları da dahil olmak üzere SQL Kıyaslama ve uyku ekleme girişimleri algılar|
|942230|Koşullu SQL ekleme girişimlerini algılar|
|942270|Temel sql ekleme işlemi için aranıyor. Mysql oracle ve diğerleri için ortak saldırı dizesi.|
|942290|Temel MongoDB SQL ekleme girişimlerini bulur|
|942320|MySQL ve PostgreSQL algılar saklı yordam/işlev eklemelerini|
|942350|MySQL UDF ekleme ve diğer veri/yapısı düzenleme algılar çalışır|
|942013|Kural 942013|
|942014|Kural 942014|
|942150|SQL ekleme saldırısı|
|942410|SQL ekleme saldırısı|
|942440|SQL açıklama dizisi algılandı.|
|942450|Tanımlanan SQL onaltılık kodlama|
|942015|Kural 942015|
|942016|Kural 942016|
|942251|HAVING eklemelerini algılar|
|942460|Meta karakterler Anomali algılama uyarısı - yinelenen sözcük olmayan karakterleri|
|942017|Kural 942017|
|942018|Kural 942018|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Açıklama|
|---|---|
|943011|Kural 943011|
|943012|Kural 943012|
|943100|Olası oturum Fixation saldırı HTML'de tanımlama bilgisi değerleri ayarlama =|
|943110|Olası oturum Fixation saldırı = etki alanı dışı başvuran ile SessionID parametre adı|
|943120|Olası oturum Fixation saldırı hiçbir başvuran ile SessionID parametre adı =|
|943013|Kural 943013|
|943014|Kural 943014|
|943015|Kural 943015|
|943016|Kural 943016|
|943017|Kural 943017|
|943018|Kural 943018|

##<a name="owasp229"></a>OWASP_2.2.9

### <a name="crs20"></a>crs_20_protocol_violations

|RuleId|Açıklama|
|---|---|
|960911|Geçersiz bir HTTP istek satır|
|981227|Apache hata = isteğinde geçersiz bir URI.|
|960912|İstek gövdesi ayrıştırılamadı.|
|960914|Çok bölümlü istek gövdesi katı doğrulama başarısız PE %@{REQBODY_PROCESSOR_ERROR =} BQ %@{MULTIPART_BOUNDARY_QUOTED} BW %@{MULTIPART_BOUNDARY_WHITESPACE} DB %@{MULTIPART_DATA_BEFORE} DA %@{MULTIPART_DATA_AFTER} HF %@{MULTIPART_HEADER_FOLDING} LF % @ {MULTIPART_LF_LINE}     SM %@{MULTIPART_SEMICOLON_MISSING} IQ %@{MULTIPART_INVALID_QUOTING} BULUNUR %@{MULTIPART_INVALID_HEADER_FOLDING} FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED}|
|960915|Çok bölümlü ayrıştırıcı olası eşleşmeyen sınır algıladı.|
|960016|Content-Length HTTP üstbilgisi sayısal değil.|
|960011|GET veya HEAD gövde içerikle isteyin.|
|960012|POST isteğini Content-Length üstbilgisi eksik.|
|960902|Kimlik kodlaması geçersiz kullanımı.|
|960022|HTTP 1.0 için izin verilmiyor üstbilgi bekler.|
|960020|Pragma üstbilgi Cache-Control üstbilgisinin HTTP/1.1 istekleri için gerektirir.|
|958291|Aralık = alan varsa ve 0 ile başlar.|
|958230|Aralık = son bayt değeri geçersiz.|
|958295|Birden çok ve çakışan bağlantı üstbilgi veri bulundu.|
|950107|URL kodlaması kötüye saldırı girişimi|
|950109|Birden çok URL kodlaması algılandı|
|950108|URL kodlaması kötüye saldırı girişimi|
|950801|UTF8 Uygunsuz kullanım saldırı girişiminde kodlaması|
|950116|Unicode tam/Yarı genişlik kötüye saldırı girişimi|
|960901|İstekte geçersiz bir karakter|
|960018|İstekte geçersiz bir karakter|

### <a name="crs21"></a>crs_21_protocol_anomalies

|RuleId|Açıklama|
|---|---|
|960008|İstek ana bilgisayar üstbilgisi eksik|
|960007|Boş ana bilgisayar üstbilgisi|
|960015|Eksik isteği bir üst bilgisi kabul et|
|960021|İsteği boş olduğundan üstbilgi kabul et|
|960009|Bir kullanıcı aracısı üstbilgisi eksik isteği|
|960006|Boş Kullanıcı Aracısı üstbilgisi|
|960904|İçeren içerik ancak eksik Content-Type üstbilgisi isteği|
|960017|Ana bilgisayar üstbilgisi sayısal bir IP adresidir.|

### <a name="crs23"></a>crs_23_request_limits

|RuleId|Açıklama|
|---|---|
|960209|Bağımsız değişken adı çok uzun|
|960208|Bağımsız değişken değeri çok uzun|
|960335|İstek çok fazla bağımsız değişken|
|960341|Toplam bağımsız değişkenleri boyutu aşıldı|
|960342|Karşıya yüklenen dosya boyutu çok büyük|
|960343|Karşıya yüklenen dosyaların toplam boyutu çok büyük|

### <a name="crs30"></a>crs_30_http_policy

|RuleId|Açıklama|
|---|---|
|960032|Yöntemi İlkesi tarafından izin verilmiyor|
|960010|İsteğin içerik türü ilke tarafından izin verilmiyor|
|960034|HTTP protokolü sürümünü ilke tarafından izin verilmiyor|
|960035|URL dosya uzantısı İlkesi tarafından kısıtlanıyor|
|960038|HTTP üstbilgisi İlkesi tarafından kısıtlanıyor|

### <a name="crs35"></a>crs_35_bad_robots

|RuleId|Açıklama|
|---|---|
|990002|Site güvenlik tarayıcısı taranan isteği gösterir|
|990901|Site güvenlik tarayıcısı taranan isteği gösterir|
|990902|Site güvenlik tarayıcısı taranan isteği gösterir|
|990012|Sahte web sitesi Gezgin|

### <a name="crs40"></a>crs_40_generic_attacks

|RuleId|Açıklama|
|---|---|
|960024|Meta karakterler Anomali algılama uyarısı - yinelenen sözcük olmayan karakterleri|
|950008|Belgelenmemiş ColdFusion etiketler ekleme|
|950010|LDAP ekleme saldırısı|
|950011|Saldırı SSI ekleme|
|950018|Evrensel PDF XSS URL algılandı.|
|950019|E-posta ekleme saldırısı|
|950012|HTTP isteği saldırı kaçakçılığı.|
|950910|HTTP yanıtı bölme saldırısı|
|950911|HTTP yanıtı bölme saldırısı|
|950117|Uzak dosya ekleme saldırısı|
|950118|Uzak dosya ekleme saldırısı|
|950119|Uzak dosya ekleme saldırısı|
|950120|Olası uzak dosya ekleme (RFI) saldırı etki alanı dışı başvuru/bağlantısı =|
|981133|Kural 981133|
|981134|Kural 981134|
|950009|Oturum Fixation saldırısı|
|950003|Oturum Fixation|
|950000|Oturum Fixation|
|950005|Uzak dosya erişim girişimi|
|950002|Sistem komut erişimi|
|950006|Sistem komut ekleme|
|959151|PHP ekleme saldırısı|
|958976|PHP ekleme saldırısı|
|958977|PHP ekleme saldırısı|

### <a name="crs41sql"></a>crs_41_sql_injection_attacks

|RuleId|Açıklama|
|---|---|
|981231|SQL açıklama dizisi algılandı.|
|981260|Tanımlanan SQL onaltılık kodlama|
|981320|SQL ekleme saldırısında algılanan ortak DB adları =|
|981300|Kural 981300|
|981301|Kural 981301|
|981302|Kural 981302|
|981303|Kural 981303|
|981304|Kural 981304|
|981305|Kural 981305|
|981306|Kural 981306|
|981307|Kural 981307|
|981308|Kural 981308|
|981309|Kural 981309|
|981310|Kural 981310|
|981311|Kural 981311|
|981312|Kural 981312|
|981313|Kural 981313|
|981314|Kural 981314|
|981315|Kural 981315|
|981316|Kural 981316|
|981317|SQL SELECT deyimi Anomali algılama Uyarısı|
|950007|Görme engelli SQL ekleme saldırısı|
|950001|SQL ekleme saldırısı|
|950908|SQL ekleme saldırısında.|
|959073|SQL ekleme saldırısı|
|981272|Sleep() veya benchmark() kullanarak görme sqli testleri algılar.|
|981250|Koşullu sorguları da dahil olmak üzere SQL Kıyaslama ve uyku ekleme girişimleri algılar|
|981241|Koşullu SQL ekleme girişimlerini algılar|
|981276|Temel sql ekleme işlemi için aranıyor. Mysql oracle ve diğerleri için ortak saldırı dizesi.|
|981270|Temel MongoDB SQL ekleme girişimlerini bulur|
|981253|MySQL ve PostgreSQL algılar saklı yordam/işlev eklemelerini|
|981251|MySQL UDF ekleme ve diğer veri/yapısı düzenleme algılar çalışır|

### <a name="crs41xss"></a>crs_41_xss_attacks

|RuleId|Açıklama|
|---|---|
|973336|XSS filtresi - kategori 1 komut dosyası etiketi vektör =|
|973338|XSS filtresi - kategori 3 Javascript URI vektör =|
|981136|Kural 981136|
|981018|Kural 981018|
|958016|Siteler arası (XSS) saldırısı|
|958414|Siteler arası (XSS) saldırısı|
|958032|Siteler arası (XSS) saldırısı|
|958026|Siteler arası (XSS) saldırısı|
|958027|Siteler arası (XSS) saldırısı|
|958054|Siteler arası (XSS) saldırısı|
|958418|Siteler arası (XSS) saldırısı|
|958034|Siteler arası (XSS) saldırısı|
|958019|Siteler arası (XSS) saldırısı|
|958013|Siteler arası (XSS) saldırısı|
|958408|Siteler arası (XSS) saldırısı|
|958012|Siteler arası (XSS) saldırısı|
|958423|Siteler arası (XSS) saldırısı|
|958002|Siteler arası (XSS) saldırısı|
|958017|Siteler arası (XSS) saldırısı|
|958007|Siteler arası (XSS) saldırısı|
|958047|Siteler arası (XSS) saldırısı|
|958410|Siteler arası (XSS) saldırısı|
|958415|Siteler arası (XSS) saldırısı|
|958022|Siteler arası (XSS) saldırısı|
|958405|Siteler arası (XSS) saldırısı|
|958419|Siteler arası (XSS) saldırısı|
|958028|Siteler arası (XSS) saldırısı|
|958057|Siteler arası (XSS) saldırısı|
|958031|Siteler arası (XSS) saldırısı|
|958006|Siteler arası (XSS) saldırısı|
|958033|Siteler arası (XSS) saldırısı|
|958038|Siteler arası (XSS) saldırısı|
|958409|Siteler arası (XSS) saldırısı|
|958001|Siteler arası (XSS) saldırısı|
|958005|Siteler arası (XSS) saldırısı|
|958404|Siteler arası (XSS) saldırısı|
|958023|Siteler arası (XSS) saldırısı|
|958010|Siteler arası (XSS) saldırısı|
|958411|Siteler arası (XSS) saldırısı|
|958422|Siteler arası (XSS) saldırısı|
|958036|Siteler arası (XSS) saldırısı|
|958000|Siteler arası (XSS) saldırısı|
|958018|Siteler arası (XSS) saldırısı|
|958406|Siteler arası (XSS) saldırısı|
|958040|Siteler arası (XSS) saldırısı|
|958052|Siteler arası (XSS) saldırısı|
|958037|Siteler arası (XSS) saldırısı|
|958049|Siteler arası (XSS) saldırısı|
|958030|Siteler arası (XSS) saldırısı|
|958041|Siteler arası (XSS) saldırısı|
|958416|Siteler arası (XSS) saldırısı|
|958024|Siteler arası (XSS) saldırısı|
|958059|Siteler arası (XSS) saldırısı|
|958417|Siteler arası (XSS) saldırısı|
|958020|Siteler arası (XSS) saldırısı|
|958045|Siteler arası (XSS) saldırısı|
|958004|Siteler arası (XSS) saldırısı|
|958421|Siteler arası (XSS) saldırısı|
|958009|Siteler arası (XSS) saldırısı|
|958025|Siteler arası (XSS) saldırısı|
|958413|Siteler arası (XSS) saldırısı|
|958051|Siteler arası (XSS) saldırısı|
|958420|Siteler arası (XSS) saldırısı|
|958407|Siteler arası (XSS) saldırısı|
|958056|Siteler arası (XSS) saldırısı|
|958011|Siteler arası (XSS) saldırısı|
|958412|Siteler arası (XSS) saldırısı|
|958008|Siteler arası (XSS) saldırısı|
|958046|Siteler arası (XSS) saldırısı|
|958039|Siteler arası (XSS) saldırısı|
|958003|Siteler arası (XSS) saldırısı|
|973300|Olası XSS saldırının algılandığı - HTML etiketi işleyicisi|
|973301|Algılanan XSS saldırısı|
|973302|Algılanan XSS saldırısı|
|973303|Algılanan XSS saldırısı|
|973304|Algılanan XSS saldırısı|
|973305|Algılanan XSS saldırısı|
|973306|Algılanan XSS saldırısı|
|973307|Algılanan XSS saldırısı|
|973308|Algılanan XSS saldırısı|
|973309|Algılanan XSS saldırısı|
|973311|Algılanan XSS saldırısı|
|973313|Algılanan XSS saldırısı|
|973314|Algılanan XSS saldırısı|
|973331|IE XSS - saldırının algılandığı filtreler.|
|973315|IE XSS - saldırının algılandığı filtreler.|
|973330|IE XSS - saldırının algılandığı filtreler.|
|973327|IE XSS - saldırının algılandığı filtreler.|
|973326|IE XSS - saldırının algılandığı filtreler.|
|973346|IE XSS - saldırının algılandığı filtreler.|
|973345|IE XSS - saldırının algılandığı filtreler.|
|973324|IE XSS - saldırının algılandığı filtreler.|
|973323|IE XSS - saldırının algılandığı filtreler.|
|973348|IE XSS - saldırının algılandığı filtreler.|
|973321|IE XSS - saldırının algılandığı filtreler.|
|973320|IE XSS - saldırının algılandığı filtreler.|
|973318|IE XSS - saldırının algılandığı filtreler.|
|973317|IE XSS - saldırının algılandığı filtreler.|
|973329|IE XSS - saldırının algılandığı filtreler.|
|973328|IE XSS - saldırının algılandığı filtreler.|

### <a name="crs42"></a>crs_42_tight_security

|RuleId|Açıklama|
|---|---|
|950103|Yol çapraz geçiş saldırısı|

### <a name="crs45"></a>crs_45_trojans

|RuleId|Açıklama|
|---|---|
|950110|Arka kapı erişimi|
|950921|Arka kapı erişimi|
|950922|Arka kapı erişimi|

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek WAF kuralları devre dışı bırakma hakkında bilgi edinin: [özelleştirme WAF kuralları](application-gateway-customize-waf-rules-portal.md)

[1]: ./media/application-gateway-integration-security-center/figure1.png