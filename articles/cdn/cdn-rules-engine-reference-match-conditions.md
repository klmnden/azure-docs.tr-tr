---
title: "Azure CDN kurallar altyapısı eşleşme koşullar | Microsoft Docs"
description: "Azure CDN başvuru belgelerine altyapısı eşleşme koşulları ve özellikleri kuralları."
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
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 0abbcf8508cb95d0fb8a9c8e5243426752efe590
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Azure CDN kurallar altyapısı eşleşen koşulları
Bu konu kullanılabilir eşleşme koşullar Azure içerik teslim ağı (CDN) için ayrıntılı açıklamaları listeler [kurallar altyapısı](cdn-rules-engine.md).

İkinci bir kural eşleşen koşulu parçasıdır. Bir eşleşme koşulu, belirli türde bir özellik kümesi gerçekleştirilecek istekler tanımlar.

Örneğin, belirli bir konumdaki içerik isteklerini belirli bir IP adresi ya da ülke veya üst bilgileri tarafından oluşturulan istekler filtrelemek için kullanılabilir.

## <a name="always"></a>Her Zaman

Her zaman eşleştirme koşul, varsayılan bir özellikler kümesi tüm isteklere uygulanmasını için tasarlanmıştır.

## <a name="device"></a>Cihaz

Cihaz eşleşme koşulu, bir mobil aygıttan kendi özelliğe göre yapılan istekleri tanımlar.  Mobil cihaz algılama aracılığıyla gerçekleştirilir [WURFL](http://wurfl.sourceforge.net/).  WURFL yetenekleri ve onların CDN kurallar altyapısı değişkenleri aşağıda listelenmiştir.
<br>
> [!NOTE] 
> Değişkenleri desteklenir **değiştirmek istemci isteği üstbilgisi** ve **değiştirme istemci yanıt üstbilgisi** özellikleri.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Marka adı | % {wurfl_cap_brand_name} | Aygıtın marka adını belirten bir dize. | Samsung
Aygıt işletim sistemi | % {wurfl_cap_device_os} | Cihazda yüklü olan işletim sisteminin gösteren bir dize. | IOS
Cihaz İşletim Sistemi Sürümü | % {wurfl_cap_device_os_version} | Cihazda yüklü işletim sisteminin sürüm numarasını gösteren bir dize. | 1.0.1
Çift yönü | % {wurfl_cap_dual_orientation} | Cihaz çift orientation destekleyip desteklemediğini belirten bir Boole değeri. | true
HTML DTD tercih edilen | % {wurfl_cap_html_preferred_dtd} | HTML içeriği için mobil cihazın tercih edilen belge türü tanımı (DTD) gösteren bir dize. | yok<br/>xhtml_basic<br/>HTML5
Satır içi kullanım görüntüsü | % {wurfl_cap_image_inlining} | Cihaz Base64 destekleyip desteklemediğini belirten bir Boole değeri görüntüleri kodlanmış. | yanlış
Android olduğu | % {wurfl_vcap_is_android} | Cihaz Android işletim sistemi kullanıp kullanmadığını belirten bir Boole değeri. | true
İOS | % {wurfl_vcap_is_ios} | Cihaz iOS kullanıp kullanmadığını belirten bir Boole değeri. | yanlış
Akıllı TV | % {wurfl_cap_is_smarttv} | Cihaz akıllı TV olup olmadığını belirten bir Boole değeri. | yanlış
Smartphone olduğu | % {wurfl_vcap_is_smartphone} | Cihaz akıllı olup olmadığını belirten bir Boole değeri. | true
Tablet olduğu | % {wurfl_cap_is_tablet} | Cihaz tablet olup olmadığını belirten bir Boole değeri. Bu bir işletim Sisteminden bağımsız açıklamasıdır. | true
Kablosuz aygıt | % {wurfl_cap_is_wireless_device} | Cihazın kablosuz aygıt olarak kabul edilip edilmediğini belirten bir Boole değeri. | true
Pazarlama adı | % {wurfl_cap_marketing_name} | Cihazın pazarlama adı belirten bir dize. | BlackBerry 8100 inci
Mobil tarayıcı | % {wurfl_cap_mobile_browser} | İçerik aygıttan istemek için kullanılan tarayıcı gösteren bir dize. | Chrome
Mobil tarayıcı sürümü | % {wurfl_cap_mobile_browser_version} | İçerik aygıttan istemek için kullanılan tarayıcı sürümünü belirten bir dize. | 31
Model adı | % {wurfl_cap_model_name} | Cihazın model adını belirten dize. | S3
Aşamalı indirme | % {wurfl_cap_progressive_download} | Hala yüklenirken cihazın ses/video oynatma destekleyip desteklemediğini belirten bir Boole değeri. | true
Yayınlanma Tarihi | % {wurfl_cap_release_date} | Yıl ve ay aygıtı WURFL veritabanına eklendi gösteren bir dize.<br/><br/>Biçimi:`yyyy_mm` | 2013_december
Çözümleme yüksekliği | % {wurfl_cap_resolution_height} | Cihazın yüksekliğini piksel cinsinden belirten bir tamsayı. | 768
Çözümleme genişliği | % {wurfl_cap_resolution_width} | Cihazın genişliğini piksel cinsinden belirten bir tamsayı. | 1024


## <a name="location"></a>Konum

Bu eşleme koşulları sahibinin konum temelinde istekleri belirlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
Sayı olarak | Belirli bir ağdan kaynaklanan istekleri tanımlar.
Ülke | Belirtilen ülkelerden kökenli isteklerine tanımlar.

### <a name="as-number"></a>Sayı olarak 
Bu ağ, Otonom sistem numarası (ASN) tarafından tanımlanır. Bir seçenek, bir istemcinin "Eşleşen" veya "Mu eşleşmediğinden" belirtilen sayı olarak ağ olduğunda bu koşul yerine olup olmadığını belirtmek için sağlanır.

**Anahtar bilgileri**
- Her biri tek bir boşluk ile sınırlandırma tarafından birden çok AS numarası belirtin. Örneğin, 64514 64515 64514 veya 64515 gelen istekleri eşleşir.
- Belirli isteklerini geçerli bir sayı olarak döndürmeyebilir. Bir soru işareti (?) istekleri için geçerli bir sayı olarak belirlenemiyor eşleştirmek.
- Tüm gibi istenen ağ numarası belirtilmelidir. Kısmi değerler eşleşen değil.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

### <a name="country"></a>Ülke
Bir ülke kendi ülke kodu belirtilebilir. Bu durum mi olacağını belirtmek için bir seçenek sağlanan ne zaman yerine bir isteğin kaynaklandığı "Eşleşmeleri" veya "Mu eşleşmediğinden" belirtilen değerler ülke.


**Anahtar bilgileri**
- Her biri tek bir boşluk ile sınırlandırma tarafından birden fazla ülke kodunu belirtin.
- Bir ülke kodu belirtirken joker karakterler desteklenmez.
- "AB" ve "AP" Ülke kodlarına bu bölgelerdeki tüm IP adreslerini kapsayacak değil.
- Belirli isteklerini geçerli bir ülke kodu döndürmeyebilir. Bir soru işareti (?) istekleri için geçerli bir ülke kodu belirlenemiyor eşleştirmek.
- Ülke kodlarına büyük/küçük harfe duyarlıdır.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

## <a name="origin"></a>Kaynak

Koşulları tanımlamak üzere tasarlanmış bu eşleşme CDN depolama o noktaya veya müşteri kaynak sunucu istekleri.

Ad | Amaç
-----|--------
CDN kaynak | CDN depolama alanında depolanan içerik isteklerini tanımlar.
Müşteri kaynağı | Belirli bir müşteri kaynak sunucuda depolanan içerik isteklerini tanımlar.

### <a name="cdn-origin"></a>CDN kaynak
Aşağıdaki koşulların her ikisi de karşılandıktan sonra bu eşleşme koşul karşılanır:
- CDN depolama içerikten istendi.
- İstek URI'si bu eşleşme koşulundaki içerik erişim noktası (örneğin, /000001) yararlanır.
  - CDN URL'si: İstek URI'si seçili içerik erişimi nokta içermesi gerekir.
  - Edge CNAME URL'si: Karşılık gelen sınır CNAME yapılandırması seçili içerik erişim noktasına işaret etmelidir.
  
*Notlar:*
 - İçerik erişim noktası istenen içerik kullanılmalıdır hizmeti tanımlar.
 - Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanılmamalıdır. Örneğin, bir müşteri kaynak eşleşme koşul CDN kaynak eşleşme koşul birleştirme hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu çok aynı nedenden dolayı iki CDN kaynak eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.
 
### <a name="customer-origin"></a>Müşteri kaynağı

**Anahtar bilgileri** 
- Bu eşleşme koşul olup içerik CDN veya seçili müşteri kaynağa işaret eden bir CNAME URL kenar kullanarak istenmeden bağımsız olarak uyulmuş.
- Bir kuralı tarafından başvurulan bir müşteri kaynağı yapılandırması müşteri başlangıç sayfasından silinemez. Bir müşteri kaynağı yapılandırması silmeye çalışmadan önce aşağıdaki yapılandırmalar, başvurmadığından emin olun:
  - Müşteri kaynak eşleşme koşulu
  - Bir sınır CNAME yapılandırma.
- Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanılmamalıdır. Örneğin, bir müşteri kaynak eşleşme koşul CDN kaynak eşleşme koşulu ile birleştirerek hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu çok aynı nedenden dolayı iki müşteri kaynak eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.

## <a name="request"></a>İstek

Bu eşleme koşulları özelliklerine göre istekleri belirlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
İstemci IP Adresi | Belirli bir IP adresinden kaynaklanan istekleri tanımlar.
Tanımlama bilgisi parametresi | Belirtilen değer için her istek ile ilişkili tanımlama bilgilerini denetler.
Tanımlama bilgisi parametresi Regex | Belirtilen normal ifade için her istek ile ilişkili tanımlama bilgilerini denetler.
Edge Cname | Belirli bir kenar CNAME noktası istekleri tanımlar.
Başvuran etki alanı | Belirtilen hostname(s) başvurulan istekleri tanımlar.
İstek üstbilgisi değişmez değeri | Belirtilen değer için ayarlanmış belirtilen üstbilgi içeren istekleri tanımlar.
İstek üstbilgisi Regex | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen üstbilgi içeren istekleri tanımlar.
İstek üstbilgisi joker karakter | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen üstbilgi içeren istekleri tanımlar.
İstek yöntemi | İstekleri tarafından HTTP yöntemleri tanımlar.
İstek düzeni | İstekleri kendi HTTP protokolü tarafından tanımlar.

### <a name="client-ip-address"></a>İstemci IP Adresi
İstemci IP kullanıcının olduğunda bu koşul yerine olup olmadığını belirtmek için bir seçenek sağlanan "Eşleşen" veya "Mu eşleşmediğinden" belirtilen IP adresleri adres.

**Anahtar bilgileri:**
- CIDR gösterimini kullandığınızdan emin olun.
- Birden çok IP adresi ve/veya IP adres blokları her biri tek bir boşluk ile sınırlandırma tarafından belirtin.
  - **IPv4 örnek:** 1.2.3.4 10.20.30.40 1.2.3.4 veya 10.20.30.40 gelen tüm istekleri eşleşir.
  - **IPv6 örnek:** 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80 gelen tüm istekleri eşleşir.
- IP adres bloğu sözdizimi, ardından bir eğik ve önek boyutu temel IP adresidir.
  - **IPv4 örnek:** 5.5.5.64/26 5.5.5.64 5.5.5.127 aracılığıyla öğesinden gelen istekleri eşleşir.
  - **IPv6 örnek:** 1:2:3: / 48 1:2:3:ffff:ffff:ffff:ffff:ffff aracılığıyla 1:2:3:0:0:0:0:0 öğesinden gelen istekleri ile eşleşir.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

### <a name="cookie-parameter"></a>Tanımlama bilgisi parametresi
**Eşleşen / eşleşmiyor** seçeneği altında bu koşulu eşleşen memnun koşulları belirler.
- **Eşleşme:** bu eşleşme koşulundaki değerleri en az biriyle eşleşen bir değeri ile belirtilen tanımlama bilgisi içeren bir istek gerektirir.
- **Eşleşmiyor:** istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değerini bu eşleşme koşulundaki değerlerden herhangi birini eşleşmiyor.
  
**Anahtar bilgileri:**
- **Tanımlama bilgisi adı:** 
  - Tanımlama bilgisi adı belirtirken bir yıldız işareti gibi özel karakterler desteklenmez. Bu, yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygun olmadığını anlamına gelir.
  - Bu eşleme koşul örneği başına yalnızca bir tek tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
- **Tanımlama bilgisi değeri:** 
  - Her biri tek bir boşluk ile sınırlandırma tarafından birden çok tanımlama bilgisi değerlerini belirtin.
  - Bir tanımlama bilgisi değerini özel karakterlerin yararlanabilir. 
  - Bir joker karakter belirtilmediği takdirde, yalnızca tam bir eşleşme bu eşleşme koşul karşılayacaktır. 
   - **Örnek:** "Değeri" belirtme eşleşir "Değeri" ancak "Value1" veya "Value2."
  - **Yoksay durumda** seçenek, büyük küçük harfe duyarlı karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılan olup olmadığını belirler.
  - Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Max-Age
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

### <a name="cookie-parameter-regex"></a>Tanımlama bilgisi parametresi Regex
Bu eşleme durum tanımlama bilgisi adı ve değeri tanımlar. Normal ifadeler istenen tanımlama bilgisi değerini tanımlamak için kullanılabilir. 

**Eşleşen / eşleşmiyor** seçeneği altında bu eşleşme koşulun karşılanıp koşulları belirler.
- **Eşleşme:** içeren belirtilen tanımlama bilgisi belirtilen normal ifadeyle eşleşen bir değeri ile isteği gerektirir.
- **Eşleşmiyor:** istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değerini belirtilen normal ifade eşleşmiyor.
  
**Anahtar bilgileri:**
- **Tanımlama bilgisi adı:** 
  - Normal ifadeler ve özel karakterler bir yıldız işareti de dahil olmak üzere, tanımlama bilgisi adı belirtirken desteklenmez. Bu, yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygun olmadığını anlamına gelir.
  - Bu eşleme koşul örneği başına yalnızca bir tek tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
- **Tanımlama bilgisi değeri:** 
  - Bir tanımlama bilgisi değer normal ifadeler yararlanabilir.
  - **Yoksay durumda** seçenek, büyük küçük harfe duyarlı karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılan olup olmadığını belirler.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

### <a name="edge-cname"></a>Edge Cname
**Anahtar bilgileri** 
- Kullanılabilir kenar CNAME'ler listesi HTTP kurallar altyapısı yapılandırılmakta olan platform karşılık gelen sınır CNAME'ler sayfasında yapılandırılmış sınırlıdır.
- Bir sınır CNAME yapılandırma silmeye çalışmadan önce bir kenar Cname eşleşme koşulu, başvurmuyor emin emin olun. Bir kuralda tanımlanan kenar CNAME yapılandırmaları kenar CNAME'ler sayfasından silinemiyor. 
- Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanılmamalıdır. Örneğin, bir müşteri kaynak eşleşme koşul bir kenar Cname eşleşme koşulunu birleştirme hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu çok aynı nedenden dolayı iki sınır Cname eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

### <a name="referring-domain"></a>Başvuran etki alanı
Ana bilgisayar üzerinden içerik bu koşul yerine getirilip getirilmediğini belirler istenen başvuran ile ilişkilendirilmiş. Bir seçenek başvuran ana bilgisayar adı "Eşleşen" veya "Eşleşmiyor" olduğunda bu koşul yerine olup olmadığını belirtmek için sağlanan belirtilen değerler.
**Anahtar bilgileri:**
- Birden çok ana bilgisayar adları, her biri tek bir boşluk ile sınırlandırma tarafından belirtin.
- Bu eşleme koşul özel karakterleri destekler.
- Belirtilen değer bir yıldız işareti içermiyorsa başvuran'ın ana bilgisayar adı için tam bir eşleşme olmalıdır. Örneğin, "etkialanım.com" belirtme "www.mydomain.com." eşleşmez
- Servis talebi yoksay seçeneği büyük küçük harfe duyarlı karşılaştırma gerçekleştirilen olup olmadığını belirler.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski
  
 ### <a name="request-header-literal"></a>İstek üstbilgisi değişmez değeri
**Eşleşen / eşleşmiyor** seçeneği altında bu koşulu eşleşen memnun koşulları belirler.
- **Eşleşme:** belirtilen içerecek şekilde istek gerekiyor üstbilgi ve değerini bu eşleşme koşulundaki bir eşleşmesi gerekir.
- **Eşleşmiyor:** istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini bu eşleşme koşulundaki eşleşmiyor.
  
**Anahtar bilgileri:**
- Üstbilgi adı karşılaştırmaları her zaman büyük/küçük harf duyarlıdır. Üstbilgi değer karşılaştırmaları duyarlılık çalışması yoksay seçeneği tarafından belirlenir.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski
  
### <a name="request-header-regex"></a>İstek üstbilgisi Regex
**Not:** Gelişmiş ayrı olarak satın alınması kurallarını kurallar altyapısı - bu yetenek gereklidir. Etkinleştirmek için CDN hesap yöneticinize başvurun. 

**Eşleşen / eşleşmiyor** seçeneği altında bu koşulu eşleşen memnun koşulları belirler.
- **Eşleşme:** belirtilen içerecek şekilde istek gerekiyor üstbilgi ve değerini belirtilen normal ifadede tanımlanan örnekle eşleşmesi gerekir.
- **Eşleşmiyor:** istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini belirtilen normal ifade eşleşmiyor.

**Anahtar bilgileri:**
- Üstbilgi adı: 
  - Üstbilgi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
  - "% 20." Başlık adında boşluk değiştirilmesi gereken 
- Üstbilgi değeri: 
  - Bir üstbilgi değeri normal ifadeler yararlanmak.
  - Üstbilgi değer karşılaştırmaları duyarlılık çalışması yoksay seçeneği tarafından belirlenir.
  - Yalnızca tam başlık değeri eşleşmeleri belirtilen desenleri en az biri için bu koşul karşılayacaktır.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski 

### <a name="request-header-wildcard"></a>İstek üstbilgisi joker karakter
**Eşleşen / eşleşmiyor** seçeneği altında bu koşulu eşleşen memnun koşulları belirler.
- **Eşleşme:** belirtilen içerecek şekilde istek gerekiyor üstbilgi ve değerini eşleşmelidir bu eşleşme koşulundaki değerleri en az biri.
- **Eşleşmiyor:** istek ya da aşağıdaki ölçütleri karşılaması gerekir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini belirtilen değerlerden herhangi birini eşleşmiyor.
  
**Anahtar bilgileri:**
- Üstbilgi adı: 
  - Üstbilgi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
  - "% 20." Başlık adında boşluk değiştirilmesi gereken Bu değer, bir üstbilgi değeri alanları belirtmek için de kullanılabilir.
- Üstbilgi değeri: 
  - Bir üstbilgi değeri özel karakterlerin yararlanabilir.
  - Üstbilgi değer karşılaştırmaları duyarlılık çalışması yoksay seçeneği tarafından belirlenir.
  - Yalnızca tam başlık değeri eşleşmeleri belirtilen desenleri en az biri için bu koşul karşılayacaktır.
  - Her biri tek bir boşluk ile sınırlandırma tarafından birden çok değer belirtin.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

### <a name="request-method"></a>İstek yöntemi
Seçilen istek yöntemi kullanılarak istenen yalnızca varlıklar, bu durum karşılayacaktır. Kullanılabilir istek yöntemler şunlardır:
- AL
- HEAD 
- YAYINLA 
- SEÇENEKLER 
- PUT 
- SİL 
- İZLEME 
- BAĞLANMA 

**Anahtar bilgileri:**
- Varsayılan olarak, önbelleğe alınmış içeriği bizim ağ üzerinde yalnızca GET isteği yöntemi oluşturabilir. Diğer istek yöntemleri yalnızca bizim ağ üzerinden taşınır.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

### <a name="request-scheme"></a>İstek düzeni
Seçili protokolü kullanılarak istenen yalnızca varlıklar, bu durum karşılayacaktır. Kullanılabilir HTTP ve HTTPS protokollerdir.

**Anahtar bilgileri:**
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

## <a name="url"></a>URL

Bu eşleme koşulları kendi URL tabanlı istekleri belirlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
URL yolu dizini | İstekleri kendi göreli yoluna göre tanımlar.
URL yolu genişletme | İstekleri, dosya adı uzantısına göre tanımlar.
URL yolu dosya | İstekleri, dosya tanımlar.
URL yolu değişmez değeri | Bir isteğin göreli yol belirtilen değerle karşılaştırır.
URL yolu Regex | Bir isteğin göreli yolu için belirtilen normal ifade karşılaştırır.
URL yolu joker karakter | Bir isteğin göreli yolu için belirtilen deseni karşılaştırır.
URL sorgu değişmez değeri | Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.
URL sorgu parametresi | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
URL sorgu Regex | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
URL sorgu joker karakter | İsteğin sorgu dizesi karşı belirtilen değerler karşılaştırır.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure CDN'ye genel bakış](cdn-overview.md)
* [Kuralları altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-conditional-expressions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)

