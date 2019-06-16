---
title: Azure Application Gateway web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların
description: Bu sayfa web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralları hakkında bilgi sağlar.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 4/11/2019
ms.author: victorh
ms.openlocfilehash: 0ad5cc76c0f4631fd60eea7d0a57e4740b6a9db3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60832923"
---
# <a name="web-application-firewall-crs-rule-groups-and-rules"></a>Web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların

Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamalarını yaygın güvenlik açıklarına ve açıklardan yararlanmaya karşı korur. Bu, OWASP çekirdek kural kümeleri üzerinde 3.0 veya 2.2.9'daki tanımlanan kurallara dayalı aracılığıyla gerçekleştirilir. Bu kurallar kural kural olarak devre dışı bırakılabilir. Bu makale, sunulan rulesets ve geçerli kurallarını içerir.

Application Gateway web uygulaması güvenlik duvarıyla kullanırken aşağıdaki kural gruplarının ve kuralların kullanılabilir.

# <a name="owasp-30tabowasp3"></a>[OWASP 3.0](#tab/owasp3)

## <a name="owasp30"></a> Kural kümeleri

### <a name="General"></a> <p x-ms-format-detection="none">Genel</p>

|RuleId|Açıklama|
|---|---|
|200004|Olası çok bölümlü eşleşmeyen sınır.|

### <a name="crs911"></a> <p x-ms-format-detection="none">İSTEK 911 YÖNTEMİ ZORLAMA</p>

|RuleId|Açıklama|
|---|---|
|911100|Yöntemine ilke tarafından izin verilmiyor|


### <a name="crs913"></a> <p x-ms-format-detection="none">İSTEK 913 TARAYICI ALGILAMA</p>

|RuleId|Açıklama|
|---|---|
|913100|Güvenlik tarayıcısı ile ilişkili kullanıcı aracısı bulundu|
|913110|Güvenlik tarayıcısı ile ilişkili istek üst bilgisi bulunamadı|
|913120|İstek dosya adı/güvenlik tarayıcısı ile ilişkili bağımsız değişken bulunamadı|
|913101|Betik/genel HTTP istemcisi ile ilişkili kullanıcı aracısı bulundu|
|913102|Web Gezgin/bot ile ilişkili kullanıcı aracısı bulundu|

### <a name="crs920"></a> <p x-ms-format-detection="none">REQUEST-920-PROTOCOL-ENFORCEMENT</p>

|RuleId|Açıklama|
|---|---|
|920100|HTTP isteği geçersiz satır|
|920130|İstek gövdesi ayrıştırılamadı.|
|920140|Çok parçalı istek gövdesi katı doğrulama başarısız oldu|
|920160|Content-Length HTTP üstbilgisi sayısal değil.|
|920170|GET veya HEAD ile gövde içeriği isteyin.|
|920180|POST isteği Content-Length üst bilgisi eksik.|
|920190|Aralık = son bayt değeri geçersiz.|
|920210|Birden çok ve çakışan bağlantı üstbilgi verileri bulunamadı.|
|920220|URL kodlaması kötüye saldırı denemesi|
|920240|URL kodlaması kötüye saldırı denemesi|
|920250|UTF8 Uygunsuz kullanım saldırı girişimini kodlaması|
|920260|Unicode tam/yarım genişlik kötüye saldırı denemesi|
|920270|Geçersiz karakter isteğinde (boş karakter için)|
|920280|İstek ana bilgisayar üst bilgisi eksik|
|920290|Boş bir ana bilgisayar üstbilgisi|
|920310|İstek boş olan üst bilgisi kabul edin|
|920311|İstek boş olan üst bilgisi kabul edin|
|920330|Boş Kullanıcı Aracısı üstbilgisi|
|920340|İçeren içerik ancak eksik Content-Type üst bilgi isteği|
|920350|Ana bilgisayar üst bilgisi bir sayısal IP adresidir|
|920380|İstek çok fazla bağımsız değişken|
|920360|Bağımsız değişken adı çok uzun|
|920370|Bağımsız değişken değeri çok uzun|
|920390|Toplam bağımsız değişkenleri boyutu aşıldı|
|920400|Karşıya yüklenen dosya boyutu çok büyük|
|920410|Karşıya yüklenen toplam dosya boyutu çok büyük|
|920420|İstek içerik türü ilke tarafından izin verilmiyor|
|920430|HTTP protokolü sürümünü ilke tarafından izin verilmiyor|
|920440|URL uzantısına İlkesi tarafından kısıtlanıyor|
|920450|HTTP üst bilgisi (%@{MATCHED_VAR}) ilkesi tarafından kısıtlanıyor|
|920200|Aralık = çok fazla alan (6 ya da daha fazla)|
|920201|Aralık = çok fazla alan için pdf isteği (35 veya daha fazla)|
|920230|Birden çok URL kodlaması algılandı|
|920300|İstek eksik bir üst bilgisi kabul edin|
|920271|Geçersiz karakter isteğinde (olmayan yazdırılabilir karakter)|
|920320|Kullanıcı Aracısı üst bilgisi eksik|
|920272|(Dışında yazdırılabilir karakter ASCII 127 aşağıda) isteğinde geçersiz karakter|
|920202|Aralık = çok fazla alan için pdf isteği (6 ya da daha fazla)|
|920273|İstek (dışında çok sıkı kümesi) içinde geçersiz karakter|
|920274|İstek üstbilgilerini (dışında çok sıkı kümesi) içinde geçersiz karakter|
|920460|Olağan dışı kaçış karakterleri|

### <a name="crs921"></a> <p x-ms-format-detection="none">REQUEST-921-PROTOCOL-ATTACK</p>

|RuleId|Açıklama|
|---|---|
|921100|HTTP isteği kaçakçılığı saldırı.|
|921110|HTTP isteği kaçakçılığı saldırı|
|921120|HTTP yanıtı bölme saldırı|
|921130|HTTP yanıtı bölme saldırı|
|921140|HTTP üstbilgisi ekleme saldırısına aracılığıyla üst bilgileri|
|921150|HTTP üstbilgisi ekleme saldırısına Yükü (CR/LF algılandı) aracılığıyla|
|921160|HTTP üstbilgisi ekleme saldırısına Yükü (CR/LF ve üst bilgi adı algılandı) aracılığıyla|
|921151|HTTP üstbilgisi ekleme saldırısına Yükü (CR/LF algılandı) aracılığıyla|
|921170|HTTP parametresi kirliliği|
|921180|HTTP parametresi kirlilik (% @{TX.1})|

### <a name="crs930"></a> <p x-ms-format-detection="none">İSTEK-930-UYGULAMASI-SALDIRI-LFI</p>

|RuleId|Açıklama|
|---|---|
|930100|Yol çapraz geçiş saldırısı (/.. /)|
|930110|Yol çapraz geçiş saldırısı (/.. /)|
|930120|İşletim sistemi dosya erişim girişimi|
|930130|Kısıtlanmış dosya erişim denemesi|

### <a name="crs931"></a> <p x-ms-format-detection="none">İSTEK-931-UYGULAMASI-SALDIRI-RFI</p>

|RuleId|Açıklama|
|---|---|
|931100|Uzaktan dosya eklemeye (RFI) olası bir saldırı IP adresini kullanarak URL parametresi =|
|931110|Uzaktan dosya eklemeye (RFI) olası bir saldırı ortak RFI savunmasız kullanılan parametre w/URL yükü =|
|931120|Olası bir uzak dosya ekleme (RFI) Saldırıcı URL kullanılan yükü w/sondaki soru işareti karakteri (?) =|
|931130|Olası bir uzak dosya ekleme (RFI) Saldırıcı etki başvuru/Link|

### <a name="crs932"></a> <p x-ms-format-detection="none">İSTEK-932-UYGULAMASI-SALDIRI-NAK</p>

|RuleId|Açıklama|
|---|---|
|932120|Uzaktan komut yürütme Windows PowerShell komutunu bulundu =|
|932130|Uzaktan komut yürütme UNIX Kabuk ifadesi bulundu =|
|932140|Uzaktan komut yürütme için / komutu BULUNAMAZSA Windows =|
|932160|Uzaktan komut yürütme = UNIX Kabuk kodu bulundu|
|932170|Uzaktan komut yürütme Shellshock (CVE-2014-6271) =|
|932171|Uzaktan komut yürütme Shellshock (CVE-2014-6271) =|

### <a name="crs933"></a> <p x-ms-format-detection="none">REQUEST-933-APPLICATION-ATTACK-PHP</p>

|RuleId|Açıklama|
|---|---|
|933100|PHP ekleme saldırısına açma/kapatma etiketi bulundu =|
|933110|PHP ekleme saldırısına PHP betik dosyasını karşıya yükleme bulundu =|
|933120|PHP ekleme saldırısına yapılandırma yönergesi bulundu =|
|933130|PHP ekleme saldırısına değişkenleri bulundu =|
|933150|PHP ekleme saldırısına = yüksek riskli bir PHP işlevi adı bulunamadı|
|933160|PHP ekleme saldırısına yüksek riskli PHP işlev çağrısı bulundu =|
|933180|PHP ekleme saldırısına değişken işlev çağrısı bulundu =|
|933151|PHP ekleme saldırısına = orta riskli PHP işlev adı bulunamadı|
|933131|PHP ekleme saldırısına değişkenleri bulundu =|
|933161|PHP ekleme saldırısına değeri düşük PHP işlev çağrısı bulundu =|
|933111|PHP ekleme saldırısına PHP betik dosyasını karşıya yükleme bulundu =|

### <a name="crs941"></a> <p x-ms-format-detection="none">İSTEK-941-UYGULAMASI-SALDIRI-XSS</p>

|RuleId|Açıklama|
|---|---|
|941100|XSS saldırı algılandığında libinjection aracılığıyla|
|941110|XSS filtre - kategori 1 = betiği etiketi vektör|
|941130|XSS filtre - 3 Kategori özniteliği vektör =|
|941140|XSS filtre - kategori 4 Javascript URI vektör =|
|941150|XSS filtre - kategori 5 izin verilmeyen HTML özniteliklerini =|
|941180|Düğüm Doğrulayıcı kara liste anahtar sözcükleri|
|941190|XSS stil sayfaları kullanma|
|941200|XSS VML çerçevelerini kullanarak|
|941210|Karıştırılmış Javascript kullanarak XSS|
|941220|XSS karıştırılmış VB komut dosyası kullanma|
|941230|Kullanarak XSS 'etiket ekleme'|
|941240|'İmport' veya 'uygulama' özniteliğini kullanarak XSS|
|941260|'Meta' etiketini kullanarak XSS|
|941270|XSS 'link' href ='ı kullanma|
|941280|'Base' etiketini kullanarak XSS|
|941290|'Uygulaması' etiketini kullanarak XSS|
|941300|'Nesnesi' etiketini kullanarak XSS|
|941310|US-ASCII hatalı kodlama XSS filtre - saldırısı algılandı.|
|941330|IE XSS filtreleri - saldırısı algılandı.|
|941340|IE XSS filtreleri - saldırısı algılandı.|
|941350|UTF-7 kodlama IE XSS - saldırısı algılandı.|
|941320|Olası XSS saldırısı algılandı - HTML etiketi işleyicisi|

### <a name="crs942"></a> <p x-ms-format-detection="none">REQUEST-942-APPLICATION-ATTACK-SQLI</p>

|RuleId|Açıklama|
|---|---|
|942100|SQL ekleme saldırısı algılandı libinjection aracılığıyla|
|942110|SQL ekleme saldırısına: Algılanan test ortak ekleme|
|942130|SQL ekleme saldırısına: SQL Tautology algılandı.|
|942140|SQL ekleme saldırısına ortak DB adları algılandı =|
|942160|Sleep() veya benchmark() kullanarak görme sqli testlerini algılar.|
|942170|Koşullu sorguları da dahil olmak üzere SQL Kıyaslama ve uyku ekleme denemesi algılar|
|942190|MSSQL kod yürütme ve bilgi toplama girişimleri algılar.|
|942200|MySQL yorum olarak algılar / alanı gizlenmiş eklemelerini ve vurgulamasını belirtir sonlandırma|
|942230|Koşullu SQL ekleme girişimlerini algılar|
|942260|2/3 temel SQL kimlik doğrulaması atlama çalışır algılar|
|942270|Temel sql ekleme aranıyor. Mysql oracle ve diğerleri için ortak saldırı dizesi.|
|942290|Temel MongoDB SQL ekleme girişimlerini bulur|
|942300|MySQL açıklamalar, koşullar ve ch (a) r eklemelerini algılar|
|942320|MySQL ve PostgreSQL algılar depolanan yordam/işlev ekleme|
|942330|Klasik SQL ekleme probings 1/2 algılar|
|942340|3/3 temel SQL kimlik doğrulaması atlama çalışır algılar|
|942350|MySQL UDF ekleme ve diğer veri/yapısı düzenleme algılar çalışır|
|942360|Birleştirilmiş temel SQL ekleme ve SQLLFI girişimleri algılar.|
|942370|Klasik SQL ekleme probings 2/2 algılar|
|942150|SQL ekleme saldırısına|
|942410|SQL ekleme saldırısına|
|942430|SQL karakter Anomali algılama (args) kısıtlı: özel karakter sayısı aşıldı (12)|
|942440|SQL açıklama dizisi algılandı.|
|942450|Belirtilen SQL onaltılık kodlama|
|942251|HAVING eklemelerini algılar|
|942460|Meta karakter Anomali algılama uyarısı - yinelenen sözcük olmayan karakter|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Açıklama|
|---|---|
|943100|Olası oturum sabitleme saldırı = tanımlama bilgisi değerleri HTML olarak ayarlama|
|943110|Olası oturum sabitleme saldırı SessionID parametre adıyla etki alanı kapalı başvuran =|
|943120|Olası oturum sabitleme saldırı SessionID parametre adıyla hiçbir başvuran =|

# <a name="owasp-229tabowasp2"></a>[OWASP 2.2.9](#tab/owasp2)

## <a name="owasp229"></a> Kural kümeleri

### <a name="crs20"></a> crs_20_protocol_violations

|RuleId|Açıklama|
|---|---|
|960911|HTTP isteği geçersiz satır|
|981227|Apache hata = isteğinde geçersiz bir URI.|
|960912|İstek gövdesi ayrıştırılamadı.|
|960914|Çok parçalı istek gövdesi katı doğrulama başarısız oldu|
|960915|Çok bölümlü ayrıştırıcı olası eşleşmeyen bir sınır algılandı.|
|960016|Content-Length HTTP üstbilgisi sayısal değil.|
|960011|GET veya HEAD ile gövde içeriği isteyin.|
|960012|POST isteği Content-Length üst bilgisi eksik.|
|960902|Kimlik kodlama geçersiz kullanımı.|
|960022|HTTP 1.0 için izin verilmiyor üstbilgi bekler.|
|960020|Pragma üst bilgisi Cache-Control üst bilgisi, HTTP/1.1 istekleri için gerektirir.|
|958291|Aralık = alan varsa ve 0 ile başlar.|
|958230|Aralık = son bayt değeri geçersiz.|
|958295|Birden çok ve çakışan bağlantı üstbilgi verileri bulunamadı.|
|950107|URL kodlaması kötüye saldırı denemesi|
|950109|Birden çok URL kodlaması algılandı|
|950108|URL kodlaması kötüye saldırı denemesi|
|950801|UTF8 Uygunsuz kullanım saldırı girişimini kodlaması|
|950116|Unicode tam/yarım genişlik kötüye saldırı denemesi|
|960901|İstek içinde geçersiz karakter|
|960018|İstek içinde geçersiz karakter|

### <a name="crs21"></a> crs_21_protocol_anomalies

|RuleId|Açıklama|
|---|---|
|960008|İstek ana bilgisayar üst bilgisi eksik|
|960007|Boş bir ana bilgisayar üstbilgisi|
|960015|İstek eksik bir üst bilgisi kabul edin|
|960021|İstek boş olan üst bilgisi kabul edin|
|960009|İstek bir kullanıcı aracısı üst bilgisi eksik|
|960006|Boş Kullanıcı Aracısı üstbilgisi|
|960904|İçeren içerik ancak eksik Content-Type üst bilgi isteği|
|960017|Ana bilgisayar üst bilgisi bir sayısal IP adresidir|

### <a name="crs23"></a> crs_23_request_limits

|RuleId|Açıklama|
|---|---|
|960209|Bağımsız değişken adı çok uzun|
|960208|Bağımsız değişken değeri çok uzun|
|960335|İstek çok fazla bağımsız değişken|
|960341|Toplam bağımsız değişkenleri boyutu aşıldı|
|960342|Karşıya yüklenen dosya boyutu çok büyük|
|960343|Karşıya yüklenen toplam dosya boyutu çok büyük|

### <a name="crs30"></a> crs_30_http_policy

|RuleId|Açıklama|
|---|---|
|960032|Yöntemine ilke tarafından izin verilmiyor|
|960010|İstek içerik türü ilke tarafından izin verilmiyor|
|960034|HTTP protokolü sürümünü ilke tarafından izin verilmiyor|
|960035|URL uzantısına İlkesi tarafından kısıtlanıyor|
|960038|HTTP üst bilgisi ilkesi tarafından kısıtlanıyor|

### <a name="crs35"></a> crs_35_bad_robots

|RuleId|Açıklama|
|---|---|
|990002|Site bir güvenlik tarayıcısı taranan isteğini belirtir.|
|990901|Site bir güvenlik tarayıcısı taranan isteğini belirtir.|
|990902|Site bir güvenlik tarayıcısı taranan isteğini belirtir.|
|990012|Sahte web sitesi Gezgini|

### <a name="crs40"></a> crs_40_generic_attacks

|RuleId|Açıklama|
|---|---|
|960024|Meta karakter Anomali algılama uyarısı - yinelenen sözcük olmayan karakter|
|950008|Belgelenmemiş ColdFusion etiket ekleme|
|950010|LDAP ekleme saldırısına|
|950011|Saldırı SSI ekleme|
|950018|Evrensel PDF XSS URL algılandı.|
|950019|E-posta ekleme saldırısına|
|950012|HTTP isteği kaçakçılığı saldırı.|
|950910|HTTP yanıtı bölme saldırı|
|950911|HTTP yanıtı bölme saldırı|
|950117|Uzak dosya ekleme Saldırıcı|
|950118|Uzak dosya ekleme Saldırıcı|
|950119|Uzak dosya ekleme Saldırıcı|
|950120|Olası bir uzak dosya ekleme (RFI) Saldırıcı etki başvuru/Link|
|981133|Kural 981133|
|981134|Kural 981134|
|950009|Oturum sabitleme saldırı|
|950003|Oturum sabitleme|
|950000|Oturum sabitleme|
|950005|Uzak dosya erişim denemesi|
|950002|Sistem komut erişimi|
|950006|Sistem komut ekleme|
|959151|PHP ekleme saldırısına|
|958976|PHP ekleme saldırısına|
|958977|PHP ekleme saldırısına|

### <a name="crs41sql"></a> crs_41_sql_injection_attacks

|RuleId|Açıklama|
|---|---|
|981231|SQL açıklama dizisi algılandı.|
|981260|Belirtilen SQL onaltılık kodlama|
|981320|SQL ekleme saldırısına ortak DB adları algılandı =|
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
|950007|Görme engelli SQL ekleme saldırısına|
|950001|SQL ekleme saldırısına|
|950908|SQL ekleme saldırısına.|
|959073|SQL ekleme saldırısına|
|981272|Sleep() veya benchmark() kullanarak görme sqli testlerini algılar.|
|981250|Koşullu sorguları da dahil olmak üzere SQL Kıyaslama ve uyku ekleme denemesi algılar|
|981241|Koşullu SQL ekleme girişimlerini algılar|
|981276|Temel sql ekleme aranıyor. Mysql oracle ve diğerleri için ortak saldırı dizesi.|
|981270|Temel MongoDB SQL ekleme girişimlerini bulur|
|981253|MySQL ve PostgreSQL algılar depolanan yordam/işlev ekleme|
|981251|MySQL UDF ekleme ve diğer veri/yapısı düzenleme algılar çalışır|

### <a name="crs41xss"></a> crs_41_xss_attacks

|RuleId|Açıklama|
|---|---|
|973336|XSS filtre - kategori 1 = betiği etiketi vektör|
|973338|XSS filtre - kategori 3 Javascript URI vektör =|
|981136|Kural 981136|
|981018|Kural 981018|
|958016|Siteler arası betik (XSS) saldırı|
|958414|Siteler arası betik (XSS) saldırı|
|958032|Siteler arası betik (XSS) saldırı|
|958026|Siteler arası betik (XSS) saldırı|
|958027|Siteler arası betik (XSS) saldırı|
|958054|Siteler arası betik (XSS) saldırı|
|958418|Siteler arası betik (XSS) saldırı|
|958034|Siteler arası betik (XSS) saldırı|
|958019|Siteler arası betik (XSS) saldırı|
|958013|Siteler arası betik (XSS) saldırı|
|958408|Siteler arası betik (XSS) saldırı|
|958012|Siteler arası betik (XSS) saldırı|
|958423|Siteler arası betik (XSS) saldırı|
|958002|Siteler arası betik (XSS) saldırı|
|958017|Siteler arası betik (XSS) saldırı|
|958007|Siteler arası betik (XSS) saldırı|
|958047|Siteler arası betik (XSS) saldırı|
|958410|Siteler arası betik (XSS) saldırı|
|958415|Siteler arası betik (XSS) saldırı|
|958022|Siteler arası betik (XSS) saldırı|
|958405|Siteler arası betik (XSS) saldırı|
|958419|Siteler arası betik (XSS) saldırı|
|958028|Siteler arası betik (XSS) saldırı|
|958057|Siteler arası betik (XSS) saldırı|
|958031|Siteler arası betik (XSS) saldırı|
|958006|Siteler arası betik (XSS) saldırı|
|958033|Siteler arası betik (XSS) saldırı|
|958038|Siteler arası betik (XSS) saldırı|
|958409|Siteler arası betik (XSS) saldırı|
|958001|Siteler arası betik (XSS) saldırı|
|958005|Siteler arası betik (XSS) saldırı|
|958404|Siteler arası betik (XSS) saldırı|
|958023|Siteler arası betik (XSS) saldırı|
|958010|Siteler arası betik (XSS) saldırı|
|958411|Siteler arası betik (XSS) saldırı|
|958422|Siteler arası betik (XSS) saldırı|
|958036|Siteler arası betik (XSS) saldırı|
|958000|Siteler arası betik (XSS) saldırı|
|958018|Siteler arası betik (XSS) saldırı|
|958406|Siteler arası betik (XSS) saldırı|
|958040|Siteler arası betik (XSS) saldırı|
|958052|Siteler arası betik (XSS) saldırı|
|958037|Siteler arası betik (XSS) saldırı|
|958049|Siteler arası betik (XSS) saldırı|
|958030|Siteler arası betik (XSS) saldırı|
|958041|Siteler arası betik (XSS) saldırı|
|958416|Siteler arası betik (XSS) saldırı|
|958024|Siteler arası betik (XSS) saldırı|
|958059|Siteler arası betik (XSS) saldırı|
|958417|Siteler arası betik (XSS) saldırı|
|958020|Siteler arası betik (XSS) saldırı|
|958045|Siteler arası betik (XSS) saldırı|
|958004|Siteler arası betik (XSS) saldırı|
|958421|Siteler arası betik (XSS) saldırı|
|958009|Siteler arası betik (XSS) saldırı|
|958025|Siteler arası betik (XSS) saldırı|
|958413|Siteler arası betik (XSS) saldırı|
|958051|Siteler arası betik (XSS) saldırı|
|958420|Siteler arası betik (XSS) saldırı|
|958407|Siteler arası betik (XSS) saldırı|
|958056|Siteler arası betik (XSS) saldırı|
|958011|Siteler arası betik (XSS) saldırı|
|958412|Siteler arası betik (XSS) saldırı|
|958008|Siteler arası betik (XSS) saldırı|
|958046|Siteler arası betik (XSS) saldırı|
|958039|Siteler arası betik (XSS) saldırı|
|958003|Siteler arası betik (XSS) saldırı|
|973300|Olası XSS saldırısı algılandı - HTML etiketi işleyicisi|
|973301|XSS saldırısı algılandı|
|973302|XSS saldırısı algılandı|
|973303|XSS saldırısı algılandı|
|973304|XSS saldırısı algılandı|
|973305|XSS saldırısı algılandı|
|973306|XSS saldırısı algılandı|
|973307|XSS saldırısı algılandı|
|973308|XSS saldırısı algılandı|
|973309|XSS saldırısı algılandı|
|973311|XSS saldırısı algılandı|
|973313|XSS saldırısı algılandı|
|973314|XSS saldırısı algılandı|
|973331|IE XSS filtreleri - saldırısı algılandı.|
|973315|IE XSS filtreleri - saldırısı algılandı.|
|973330|IE XSS filtreleri - saldırısı algılandı.|
|973327|IE XSS filtreleri - saldırısı algılandı.|
|973326|IE XSS filtreleri - saldırısı algılandı.|
|973346|IE XSS filtreleri - saldırısı algılandı.|
|973345|IE XSS filtreleri - saldırısı algılandı.|
|973324|IE XSS filtreleri - saldırısı algılandı.|
|973323|IE XSS filtreleri - saldırısı algılandı.|
|973348|IE XSS filtreleri - saldırısı algılandı.|
|973321|IE XSS filtreleri - saldırısı algılandı.|
|973320|IE XSS filtreleri - saldırısı algılandı.|
|973318|IE XSS filtreleri - saldırısı algılandı.|
|973317|IE XSS filtreleri - saldırısı algılandı.|
|973329|IE XSS filtreleri - saldırısı algılandı.|
|973328|IE XSS filtreleri - saldırısı algılandı.|

### <a name="crs42"></a> crs_42_tight_security

|RuleId|Açıklama|
|---|---|
|950103|Yol çapraz geçiş saldırısı|

### <a name="crs45"></a> crs_45_trojans

|RuleId|Açıklama|
|---|---|
|950110|Arka kapı erişimi|
|950921|Arka kapı erişimi|
|950922|Arka kapı erişimi|

---

## <a name="next-steps"></a>Sonraki adımlar

WAF kurallarını devre dışı bırakma hakkında bilgi edinin: [WAF kurallarını özelleştirme](application-gateway-customize-waf-rules-portal.md)