---
title: "Eşleşen Azure CDN kurallar altyapısı için koşullar | Microsoft Docs"
description: "Azure içerik teslim ağı başvuru belgelerine altyapısı eşleşme koşullar kuralları."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 9986e654b076df099e3912f9da628728723b5c3d
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="match-conditions-for-the-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı için koşullara uyan
Bu makalede kullanılabilir eşleşme koşullar için Azure içerik teslim ağı (CDN) ayrıntılı açıklamaları listelenmektedir [kurallar altyapısı](cdn-rules-engine.md).

İkinci bir kural eşleşen koşulu parçasıdır. Bir eşleşme koşulu, belirli türde bir özellik kümesi gerçekleştirilecek istekler tanımlar.

Örneğin, bir eşleşme koşulu kullanabilirsiniz:
- Belirli bir konumdaki içerik filtre ister.
- Belirli IP adresi ya da ülke oluşturulan filtre istekler.
- Üst bilgileri tarafından filtre istek sayısı.

## <a name="always-match-condition"></a>Her zaman bir koşulla eşleşmesi

Her zaman eşleştirme koşulu varsayılan bir özellik için tüm istekleri geçerlidir.

Ad | Amaç
-----|--------
[Her zaman](#always) | Varsayılan bir özellikler kümesi tüm isteklere uygulanır.

## <a name="device-match-condition"></a>Cihaz eşleşme koşulu

Cihaz eşleşme koşulu, bir mobil aygıttan kendi özelliğe göre yapılan istekleri tanımlar.  

Ad | Amaç
-----|--------
[Cihazı](#device) | Bir mobil aygıttan kendi özelliğe göre yapılan istekleri tanımlar.

## <a name="location-match-conditions"></a>Konum eşleme koşulları

Konum eşleme koşulları sahibinin konum temelinde istekleri tanımlayın.

Ad | Amaç
-----|--------
[Sayı olarak](#as-number) | Belirli bir ağdan kaynaklanan istekleri tanımlar.
[Ülke](#country) | Belirtilen ülkelerden kökenli isteklerine tanımlar.

## <a name="origin-match-conditions"></a>Kaynak eşleşme koşulları

Kaynak eşleşme koşullar içerik teslim ağı depolama veya müşteri kaynak sunucuya işaret istekleri tanımlayın.

Ad | Amaç
-----|--------
[CDN kaynak](#cdn-origin) | İçerik teslim ağı depolamada depolanan içerik isteklerini tanımlar.
[Müşteri kaynağı](#customer-origin) | Belirli bir müşteri kaynak sunucuda depolanan içerik isteklerini tanımlar.

## <a name="request-match-conditions"></a>İsteği eşleşme koşulları

Özelliklerine göre istekleri isteği eşleşme koşulları tanımlayın.

Ad | Amaç
-----|--------
[İstemci IP adresi](#client-ip-address) | Belirli bir IP adresinden kaynaklanan istekleri tanımlar.
[Tanımlama bilgisi parametresi](#cookie-parameter) | Belirtilen değer için her istek ile ilişkili tanımlama bilgilerini denetler.
[Tanımlama bilgisi parametresi Regex](#cookie-parameter-regex) | Belirtilen normal ifade için her istek ile ilişkili tanımlama bilgilerini denetler.
[Edge Cname](#edge-cname) | Belirli bir kenar CNAME noktası istekleri tanımlar.
[Başvuran etki alanı](#referring-domain) | Belirtilen ana bilgisayar adlarından başvurulan istekleri tanımlar.
[İstek üstbilgisi değişmez değeri](#request-header-literal) | Belirtilen değerine belirtilen üstbilgi içeren istekleri tanımlar.
[İstek üstbilgisi Regex](#request-header-regex) | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen üstbilgi içeren istekleri tanımlar.
[İstek üstbilgisi joker karakter](#request-header-wildcard) | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen üstbilgi içeren istekleri tanımlar.
[İstek yöntemi](#request-method) | İstekleri tarafından HTTP yöntemleri tanımlar.
[İstek düzeni](#request-scheme) | İstekleri kendi HTTP protokolü tarafından tanımlar.

## <a name="url-match-conditions"></a>URL eşleşme koşulları

URL eşleşme koşullar kendi URL tabanlı istekleri tanımlayın.

Ad | Amaç
-----|--------
URL yolu dizini | İstekleri kendi göreli yoluna göre tanımlar.
URL yolu genişletme | İstekleri, dosya adı uzantılarına göre tanımlar.
URL yolu dosya | İstekleri, dosya adına göre tanımlar.
URL yolu değişmez değeri | Bir isteğin göreli yol belirtilen değerle karşılaştırır.
URL yolu Regex | Bir isteğin göreli yolu için belirtilen normal ifade karşılaştırır.
URL yolu joker karakter | Bir isteğin göreli yolu için belirtilen deseni karşılaştırır.
URL sorgu değişmez değeri | Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.
URL sorgu parametresi | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
URL sorgu Regex | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
URL sorgu joker karakter | Belirtilen isteğin sorgu dizesi değerleri karşılaştırır.


## <a name="reference-for-rules-engine-match-conditions"></a>Kurallar altyapısı eşleşme koşulları için başvurusu

---
### <a name="always"></a>Her zaman

Her zaman eşleştirme koşulu varsayılan bir özellik için tüm istekleri geçerlidir.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>Sayı olarak 
AS numarası ağ kendi Otonom sistem numarası (ASN) tarafından tanımlanır. Bir seçenek, bir istemcinin ağ belirtilen ASN "Eşleşen" veya "Mu eşleşmediğinden" olduğunda bu koşul yerine olup olmadığını belirtmek için sağlanır.

Anahtar bilgileri:
- Birden çok Asn'ler her biri tek bir boşluk ile sınırlandırma tarafından belirtin. Örneğin, istekleri 64514 64515 eşleşen 64514 veya 64515 ulaşır.
- Belirli isteklerini geçerli bir ASN döndürmeyebilir. Bir soru işareti (?), geçerli bir ASN belirlenemedi istekleri ile eşleşir.
- İstenen ağı için tüm ASN belirtmeniz gerekir. Kısmi değerler eşleşen değil.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cdn-origin"></a>CDN kaynak
Aşağıdaki koşulların her ikisi de karşılandığında CDN kaynak eşleşme koşul karşılanır:
- İçerik teslim ağı depolama içerikten istendi.
- İstek URI'si bu eşleşme koşulundaki içerik erişim noktası (örneğin, /000001) kullanır.
  - İçerik teslim ağı URL'si: İstek URI'si seçili içerik erişimi nokta içermesi gerekir.
  - Edge CNAME URL'si: Karşılık gelen sınır CNAME yapılandırması seçili içerik erişim noktasına işaret etmelidir.
  
Anahtar bilgileri:
 - İçerik erişim noktası istenen içerik kullanılmalıdır hizmeti tanımlar.
 - Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanmayın. Örneğin, bir müşteri kaynak eşleşme koşul CDN kaynak eşleşme koşul birleştirme hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki CDN kaynak eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>İstemci IP Adresi
Bir seçenek, bir istemcinin IP adresi "Eşleşmeleri" veya "Mu eşleşmediğinden" belirtilen IP adresleri kullanılırken istemci IP adresi koşul karşılanır olup olmadığını belirtmek için sağlanır.

Anahtar bilgileri:
- CIDR gösterimini kullandığınızdan emin olun.
- Birden çok IP adresi ve/veya IP adres blokları her biri tek bir boşluk ile sınırlandırma tarafından belirtin.
  - **IPv4 örnek**: 1.2.3.4 10.20.30.40 1.2.3.4 veya 10.20.30.40 gelen tüm istekler eşleşir.
  - **IPv6 örnek**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80 gelen tüm istekler eşleşir.
- IP adres bloğu sözdizimi, ardından bir eğik ve önek boyutu temel IP adresidir.
  - **IPv4 örnek**: 5.5.5.64/26 5.5.5.64 5.5.5.127 aracılığıyla öğesinden gelen istekleri eşleşir.
  - **IPv6 örnek**: 1:2:3: / 48 1:2:3:ffff:ffff:ffff:ffff:ffff aracılığıyla 1:2:3:0:0:0:0:0 öğesinden gelen istekleri ile eşleşir.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter"></a>Tanımlama bilgisi parametresi
**Eşleşmeleri**/**eşleşmiyor** seçeneğini belirler tanımlama bilgisi parametresi altında koşulu aynı koşulların yerine getirildiği.
- **Eşleşen**: Bu eşleşme koşulundaki değerleri en az biriyle eşleşen bir değeri ile belirtilen tanımlama bilgisi içeren bir istek gerektirir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değerini bu eşleşme koşulundaki değerlerden herhangi birini eşleşmiyor.
  
Anahtar bilgileri:
- Tanımlama bilgisi adı: 
  - Tanımlama bilgisi adı belirtirken bir yıldız işareti gibi özel karakterler desteklenmez. Bu, yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygun olmadığını anlamına gelir.
  - Bu eşleme koşul örneği başına yalnızca bir tek tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
- Tanımlama bilgisi değeri: 
  - Her biri tek bir boşluk ile sınırlandırma tarafından birden çok tanımlama bilgisi değerlerini belirtin.
  - Bir tanımlama bilgisi değerini özel karakterlerin yararlanabilir. 
  - Bir joker karakter belirtilmediği takdirde, yalnızca tam bir eşleşme bu eşleşme koşul karşılayacaktır. Örneğin, "Değeri" belirtme "Değeri" ancak "Value1" veya "Value2." ile eşleşir
  - **Yoksay durumda** seçenek, büyük küçük harfe duyarlı karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılan olup olmadığını belirler.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter-regex"></a>Tanımlama bilgisi parametresi Regex
Bir tanımlama bilgisi adı ve değeri tanımlama bilgisi parametresi Regex eşleşme koşulu tanımlar. Normal ifadeler istenen tanımlama bilgisi değerini tanımlamak için kullanabilirsiniz. 

**Eşleşmeleri**/**eşleşmiyor** seçeneği altında bu koşulu eşleşen memnun koşulları belirler.
- **Eşleşen**: bir istek belirtilen tanımlama bilgisi belirtilen normal ifadeyle eşleşen bir değeri ile içermesini gerektirir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değerini belirtilen normal ifade eşleşmiyor.
  
Anahtar bilgileri:
- Tanımlama bilgisi adı: 
  - Tanımlama bilgisi adı belirtirken, normal ifadeler ve bir yıldız işareti gibi özel karakterler desteklenmez. Bu, yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygun olmadığını anlamına gelir.
  - Bu eşleme koşul örneği başına yalnızca bir tek tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
- Tanımlama bilgisi değeri: 
  - Bir tanımlama bilgisi değer normal ifadeler yararlanabilir.
  - **Yoksay durumda** seçenek, büyük küçük harfe duyarlı karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılan olup olmadığını belirler.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

--- 
### <a name="country"></a>Ülke
Ülke kodu aracılığıyla bir ülke belirtebilirsiniz. Bu durum mi olacağını belirtmek için bir seçenek sağlanan ne zaman yerine bir isteğin kaynaklandığı "Eşleşmeleri" veya "Mu eşleşmediğinden" belirtilen değerleri ülke.

Anahtar bilgileri:
- Her biri tek bir boşluk ile sınırlandırma tarafından birden fazla ülke kodunu belirtin.
- Bir ülke kodu belirtirken joker karakterler desteklenmez.
- "AB" ve "AP" Ülke kodlarına bu bölgelerdeki tüm IP adreslerini kapsayacak değil.
- Belirli isteklerini geçerli bir ülke kodu döndürmeyebilir. Bir soru işareti (?), geçerli bir ülke kodu belirlenemedi istekleri ile eşleşir.
- Ülke kodlarına büyük/küçük harfe duyarlıdır.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Müşteri kaynağı

Anahtar bilgileri: 
- Müşteri kaynak eşleşme koşul olup içerik bir içerik teslim ağı URL veya seçili müşteri kaynağa işaret eden bir CNAME URL kenar aracılığıyla istenmeden bağımsız olarak gerçekleşmiş.
- Müşteri başlangıç sayfasından kuralı tarafından başvurulan bir müşteri kaynağı yapılandırması silinemiyor. Müşteri kaynak yapılandırması silmeye çalışmadan önce aşağıdaki yapılandırmalar, başvurmadığından emin olun:
  - Bir müşteri kaynak eşleşme koşulu
  - Bir sınır CNAME yapılandırma
- Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanmayın. Örneğin, bir müşteri kaynak eşleşme koşul CDN kaynak eşleşme koşulu ile birleştirerek hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki müşteri kaynak eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Cihaz

Cihaz eşleşme koşulu, bir mobil aygıttan kendi özelliğe göre yapılan istekleri tanımlar. Mobil cihaz algılama aracılığıyla gerçekleştirilir [WURFL](http://wurfl.sourceforge.net/). Aşağıdaki tabloda WURFL yetenekleri ve onların değişkenleri içerik teslim ağı kurallar altyapısı için listeler.
<br>
> [!NOTE] 
> Aşağıdaki değişkenleri desteklenir **değiştirmek istemci isteği üstbilgisi** ve **değiştirme istemci yanıt üstbilgisi** özellikleri.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Marka adı | % {wurfl_cap_brand_name} | Aygıtın marka adını belirten bir dize. | Samsung
Aygıt işletim sistemi | % {wurfl_cap_device_os} | Cihazda yüklü olan işletim sisteminin gösteren bir dize. | IOS
Cihaz İşletim Sistemi Sürümü | % {wurfl_cap_device_os_version} | Cihazda yüklü olan işletim sisteminin sürüm numarasını gösteren bir dize. | 1.0.1
Çift yönü | % {wurfl_cap_dual_orientation} | Cihaz çift orientation destekleyip desteklemediğini belirten bir Boole değeri. | doğru
HTML DTD tercih edilen | % {wurfl_cap_html_preferred_dtd} | HTML içeriği için mobil cihazın tercih edilen belge türü tanımı (DTD) gösteren bir dize. | yok<br/>xhtml_basic<br/>HTML5
Satır içi kullanım görüntüsü | % {wurfl_cap_image_inlining} | Cihaz Base64 destekleyip desteklemediğini belirten bir Boole değeri görüntüleri kodlanmış. | yanlış
Android olduğu | % {wurfl_vcap_is_android} | Cihaz Android işletim sistemi kullanıp kullanmadığını belirten bir Boole değeri. | doğru
İOS | % {wurfl_vcap_is_ios} | Cihaz iOS kullanıp kullanmadığını belirten bir Boole değeri. | yanlış
Akıllı TV | % {wurfl_cap_is_smarttv} | Cihaz akıllı TV olup olmadığını belirten bir Boole değeri. | yanlış
Smartphone olduğu | % {wurfl_vcap_is_smartphone} | Cihaz akıllı olup olmadığını belirten bir Boole değeri. | doğru
Tablet olduğu | % {wurfl_cap_is_tablet} | Cihaz tablet olup olmadığını belirten bir Boole değeri. Bu bir işletim Sisteminden bağımsız açıklamasıdır. | doğru
Kablosuz aygıt | % {wurfl_cap_is_wireless_device} | Cihazın kablosuz aygıt olarak kabul edilip edilmediğini belirten bir Boole değeri. | doğru
Pazarlama adı | % {wurfl_cap_marketing_name} | Cihazın pazarlama adı belirten bir dize. | BlackBerry 8100 inci
Mobil tarayıcı | % {wurfl_cap_mobile_browser} | İçerik aygıttan istemek için kullanılan tarayıcı gösteren bir dize. | Chrome
Mobil tarayıcı sürümü | % {wurfl_cap_mobile_browser_version} | İçerik aygıttan istemek için kullanılan tarayıcı sürümünü gösteren bir dize. | 31
Model adı | % {wurfl_cap_model_name} | Cihazın model adını belirten dize. | S3
Aşamalı indirme | % {wurfl_cap_progressive_download} | Hala yüklenirken cihazın ses ve video oynatma destekleyip desteklemediğini belirten bir Boole değeri. | doğru
Yayınlanma Tarihi | % {wurfl_cap_release_date} | Yıl ve ay aygıtı WURFL veritabanına eklendi gösteren bir dize.<br/><br/>Biçimi:`yyyy_mm` | 2013_december
Çözümleme yüksekliği | % {wurfl_cap_resolution_height} | Cihazın yüksekliğini piksel cinsinden belirten bir tamsayı. | 768
Çözümleme genişliği | % {wurfl_cap_resolution_width} | Cihazın genişliğini piksel cinsinden belirten bir tamsayı. | 1024

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Edge Cname
Anahtar bilgileri: 
- Kullanılabilir kenar CNAME'ler listesi HTTP kurallar altyapısı yapılandırılmakta olan platform karşılık gelen sınır CNAME'ler sayfasında yapılandırılmış sınırlıdır.
- Bir sınır CNAME yapılandırmasını silmeye çalışmadan önce bir sınır Cname eşleşme koşulu, başvurmuyor emin emin olun. Bir kuralda tanımlanan kenar CNAME yapılandırmaları kenar CNAME'ler sayfasından silinemiyor. 
- Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanmayın. Örneğin, bir müşteri kaynak eşleşme koşul bir kenar Cname eşleşme koşulunu birleştirme hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki sınır Cname eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="referring-domain"></a>Başvuran etki alanı
Ana bilgisayar adı üzerinden içerik başvuran etki alanı koşulun yerine getirilip getirilmediğini belirler istenen başvuran ile ilişkilendirilmiş. Bir seçenek başvuran konak belirtilen değerleri "Eşleşen" veya "Mu eşleşmediğinden" adlandırdığınızda bu koşul yerine olup olmadığını belirtmek için sağlanır.

Anahtar bilgileri:
- Her biri tek bir boşluk ile sınırlandırma tarafından birden çok ana bilgisayar adı belirtin.
- Bu eşleme koşul özel karakterleri destekler.
- Belirtilen değer bir yıldız işareti içermiyorsa başvuran'ın ana bilgisayar adı için tam bir eşleşme olmalıdır. Örneğin, "etkialanım.com" belirtme "www.mydomain.com." eşleşmez
- **Yoksay durumda** seçenek, büyük küçük harfe duyarlı karşılaştırma gerçekleştirilen olup olmadığını belirler.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-literal"></a>İstek üstbilgisi değişmez değeri
**Eşleşmeleri**/**eşleşmiyor** seçeneği altında bu koşulu eşleşen memnun koşulları belirler.
- **Eşleşme**: Belirtilen üstbilgi içerecek şekilde istek gerektirir. Değeri bu eşleşme koşulunda tanımlanan adla aynı olmalıdır.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini bu eşleşme koşulundaki bir eşleşmiyor.
  
Anahtar bilgileri:
- Üstbilgi adı karşılaştırmaları her zaman büyük/küçük harf duyarlıdır. **Yoksay durumda** seçeneği üstbilgi değer karşılaştırmaları duyarlılık belirler.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-regex"></a>İstek üstbilgisi Regex
**Eşleşmeleri**/**eşleşmiyor** seçeneğini belirler istek üstbilgisi Regex altında koşul eşleşen koşullar yerine getirildiği.
- **Eşleşme**: Belirtilen üstbilgi içerecek şekilde istek gerektirir. Değeri, belirtilen normal ifadede tanımlanan desen eşleşmesi gerekir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini belirtilen normal ifade eşleşmiyor.

Anahtar bilgileri:
- Üstbilgi adı: 
  - Üstbilgi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
  - "% 20." Başlık adında boşluk değiştirilmesi gereken 
- Üstbilgi değeri: 
  - Bir üstbilgi değeri normal ifadeler yararlanabilir.
  - **Yoksay durumda** seçeneği üstbilgi değer karşılaştırmaları duyarlılık belirler.
  - Yalnızca tam başlık değeri eşleşmeleri belirtilen desenleri en az biri için bu koşul karşılayacaktır.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski 

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-header-wildcard"></a>İstek üstbilgisi joker karakter
**Eşleşmeleri**/**eşleşmiyor** seçeneğini belirler istek üstbilgisi joker altında koşul eşleşen koşullar yerine getirildiği.
- **Eşleşme**: Belirtilen üstbilgi içerecek şekilde istek gerektirir. Değeri bu eşleşme koşulundaki değerleri en az biriyle eşleşmelidir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini belirtilen değerlerden herhangi birini eşleşmiyor.
  
Anahtar bilgileri:
- Üstbilgi adı: 
  - Üstbilgi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
  - "% 20." Başlık adında boşluk değiştirilmesi gereken Bu değer, bir üstbilgi değeri alanları belirtmek için de kullanabilirsiniz.
- Üstbilgi değeri: 
  - Bir üstbilgi değeri özel karakterlerin yararlanabilir.
  - **Yoksay durumda** seçeneği üstbilgi değer karşılaştırmaları duyarlılık belirler.
  - Yalnızca tam başlık değeri eşleşmeleri belirtilen desenleri en az biri için bu koşul karşılayacaktır.
  - Her biri tek bir boşluk ile sınırlandırma tarafından birden çok değer belirtin.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-method"></a>İstek yöntemi
Seçilen istek yöntemle istenen varlıklar istek yöntemi koşul karşılayacaktır. Kullanılabilir istek yöntemler şunlardır:
- GET
- HEAD 
- POST 
- SEÇENEKLER 
- PUT 
- DELETE 
- İZLEME 
- BAĞLANMA 

Anahtar bilgileri:
- Varsayılan olarak, önbelleğe alınan içeriği ağ üzerinde yalnızca GET isteği yöntemi oluşturabilir. Diğer istek yöntemleri, ağ üzerinden taşınır.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-scheme"></a>İstek düzeni
Yalnızca seçili protokolüyle istenen varlıklar istek düzeni koşul karşılayacaktır. Kullanılabilir HTTP ve HTTPS protokollerdir.

Anahtar bilgileri:
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure içerik teslim ağı genel bakış](cdn-overview.md)
* [Kuralları altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-conditional-expressions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)

