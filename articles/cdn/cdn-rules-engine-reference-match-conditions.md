---
title: Azure CDN, kural altyapısı eşleştirme koşulları | Microsoft Docs
description: Azure Content Delivery Network için başvuru belgeleri altyapısı eşleştirme koşulları kuralları.
services: cdn
documentationcenter: ''
author: Lichard
manager: akucer
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 877d994968dbc575c8baa7ac4c8a40b76f6d617f
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59283486"
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Azure CDN, kural altyapısı eşleşen koşulları 
Bu makalede ayrıntılı açıklamaları için Azure Content Delivery Network (CDN) kullanılabilir eşleştirme koşulları listeler [kurallar altyapısı](cdn-rules-engine.md).

İkinci bir kuralı bir eşleşme koşulu parçasıdır. Belirli türde istekler bir özellikler kümesi gerçekleştirilecek bir eşleşme koşulu tanımlar.

Örneğin, bir eşleşme koşulu için kullanabilirsiniz:
- Belirli bir konumdaki içerik isteklerini filtre.
- İstek Filtreleme belirli IP adresi ya da ülke oluşturulur.
- Üst bilgileri tarafından istek filtreleme.

## <a name="always-match-condition"></a>Her zaman eşleşme koşulu

Her zaman eşleşme koşulu, tüm istekler için varsayılan bir özellikler kümesi uygular.

Ad | Amaç
-----|--------
[Her Zaman](#always) | Varsayılan bir özellik kümesi, tüm istekler için geçerlidir.

## <a name="device-match-condition"></a>Cihaz eşleşme koşulu

Cihaz eşleşme koşulu, kendi özelliklerine bağlı olarak bir mobil CİHAZDAN istekleri tanımlar.  

Ad | Amaç
-----|--------
[Cihaz](#device) | Kendi özelliklerine bağlı olarak bir mobil CİHAZDAN istekleri tanımlar.

## <a name="location-match-conditions"></a>Konum eşleşme koşulları

İstek sahibinin konumuna göre konum eşleşme koşulları tanımlayın.

Ad | Amaç
-----|--------
[Sayı olarak](#as-number) | Belirli bir ağdan kaynaklanan istekler tanımlar.
[Ülke](#country) | Belirtilen ülkelerden kökenli isteklerine tanımlar.

## <a name="origin-match-conditions"></a>Kaynak eşleştirme koşulları

Content Delivery Network depolama veya müşteri kaynak sunucuya işaret istekleri kaynak eşleştirme koşulları tanımlayın.

Ad | Amaç
-----|--------
[CDN kaynağı](#cdn-origin) | Content Delivery Network depolanan içerik isteklerini tanımlar.
[Müşteri kaynağı](#customer-origin) | Belirli bir müşteri kaynak sunucuda depolanan içerik isteklerini tanımlar.

## <a name="request-match-conditions"></a>İstek eşleştirme koşulları

Özelliklerine göre istekleri isteği eşleşme koşulları tanımlayın.

Ad | Amaç
-----|--------
[İstemci IP Adresi](#client-ip-address) | Belirli bir IP adresinden kaynaklanan istekler tanımlar.
[Tanımlama bilgisi parametresi](#cookie-parameter) | Belirtilen değer için her bir istekle ilişkili tanımlama bilgilerini denetler.
[Tanımlama bilgisi parametre normal ifade](#cookie-parameter-regex) | Belirtilen normal ifade için her bir istekle ilişkili tanımlama bilgilerini denetler.
[Edge Cname](#edge-cname) | Belirli bir kenar CNAME noktası istekleri tanımlar.
[Etki alanı başvuran](#referring-domain) | Belirtilen ana bilgisayar adlarından adı veriliyordu istekleri tanımlar.
[İstek üst bilgisi değişmez değeri](#request-header-literal) | Belirtilen bir değere ayarlanmış belirtilen üst bilgisi içeren istekleri tanımlar.
[İstek üst bilgisi normal ifade](#request-header-regex) | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen üst bilgisi içeren istekleri tanımlar.
[İstek üst bilgisi joker karakter](#request-header-wildcard) | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen üst bilgisi içeren istekleri tanımlar.
[İstek yöntemi](#request-method) | İstek tarafından kendi HTTP yöntemini tanımlar.
[İstek düzeni](#request-scheme) | İstek, HTTP protokolü tarafından tanımlar.

## <a name="url-match-conditions"></a>URL eşleştirme koşulları

URL eşleştirme koşulları kendi URL'lere göre istekleri belirleyin.

Ad | Amaç
-----|--------
[URL yol dizini](#url-path-directory) | İstek, göreli yol tanımlar.
[URL yolu genişletme](#url-path-extension) | Dosya adı uzantılarına göre istekleri tanımlar.
[URL yol dosyaadı](#url-path-filename) | Dosya adına göre istekleri tanımlar.
[URL yolu değişmez değeri](#url-path-literal) | Bir isteğin göreli yolu belirtilen değerle karşılaştırır.
[URL yolu normal ifade](#url-path-regex) | Bir isteğin belirtilen normal ifade için göreli yol karşılaştırır.
[URL yolu joker karakter](#url-path-wildcard) | Bir isteğin göreli yolu belirtilen desen ile karşılaştırır.
[URL sorgu değişmez değeri](#url-query-literal) | Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.
[URL sorgu parametresi](#url-query-parameter) | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
[Sorgu Regex URL'si](#url-query-regex) | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
[URL sorgu joker karakter](#url-query-wildcard) | Belirtilen isteğin sorgu dizesi değeri karşılaştırır.


## <a name="reference-for-rules-engine-match-conditions"></a>Kural altyapısı eşleştirme koşulları için başvuru
<a name="main"></a>
---
### <a name="always"></a>Her Zaman

Her zaman eşleşme koşulu, tüm istekler için varsayılan bir özellikler kümesi uygular.

[Başa dön](#main)

</br>

---
### <a name="as-number"></a>Sayı olarak 
AS numarası ağ, Otonom sistem numarası (ASN) tarafından tanımlanır. 

**Eşleşme**/**eşleşmiyor** seçeneği AS numarasını altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: İstemci ağ ASN'si belirtilen Asn'ler biriyle eşleşen gerektirir. 
- **Eşleşmeyen**: İstemci ağ ASN'si belirtilen Asn'ler hiçbirini eşleşmediğini gerektirir.

Anahtar bilgileri:
- Her biri tek bir boşluk ile sınırlayan tarafından birden çok Asn'ler belirtin. Örneğin, istekleri 64514 64515 eşleşen 64514 veya 64515 ulaşır.
- Belirli istek geçerli bir ASN döndürmeyebilir. Bir soru işareti (?) geçerli bir ASN belirlenemedi istekleri eşleşir.
- İstenen ağ için tüm ASN'yi belirtin. Kısmi değerler eşleşen değil.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="cdn-origin"></a>CDN kaynağı
CDN Origin eşleşme koşulu karşılandığında, aşağıdaki koşulların her ikisinin de karşılanır:
- CDN Depolama'dan içerik isteğinde bulunuldu.
- İstek URI'si bu eşleşme koşulu içinde tanımlanan içerik erişim noktası (örneğin, /000001) türünü kullanır:
  - CDN URL'Sİ: İstek URI'si seçili içerik erişim noktası içermelidir.
  - Edge CNAME URL'si: Karşılık gelen uç CNAME yapılandırma seçili içerik erişim noktasına işaret etmelidir.
  
Anahtar bilgileri:
 - İçerik erişim noktası talep edilen içeriği kullanılmalıdır hizmeti tanımlar.
 - Belirli eşleşme koşulları birleştirmek ve IF deyimini kullanmayın. Örneğin, CDN Origin eşleşme koşulu ile müşteri kaynağı eşleşme koşulu birleştirme hiç eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki CDN Origin eşleşme koşulu ve IF deyimini ile birleştirilemez.

[Başa dön](#main)

</br>

---
### <a name="client-ip-address"></a>İstemci IP Adresi
**Eşleşme**/**eşleşmiyor** seçeneği, istemci IP adresi altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: İstemcinin IP adresini belirtilen IP adreslerinden birini eşleşmesini gerektirir. 
- **Eşleşmeyen**: İstemcinin IP adresini belirtilen IP adreslerini hiçbirini eşleşmediğini gerektirir. 

Anahtar bilgileri:
- CIDR gösterimini kullanın.
- Birden çok IP adresi ve/veya IP adresi blokları her biri tek bir boşluk ile sınırlayan tarafından belirtin. Örneğin:
  - **IPv4 örnek**: 1.2.3.4 10.20.30.40 ya da adresinden 1.2.3.4 veya 10.20.30.40 gelen tüm istekleri eşleşir.
  - **IPv6 örnek**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 adresi 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80 gelen tüm istekleri eşleşir.
- Bir IP adres bloğu sözdizimi ardından bir eğik çizgi ve önek boyutundan temel IP adresidir. Örneğin:
  - **IPv4 örnek**: 5.5.5.64/26 5.5.5.127 aracılığıyla 5.5.5.64 adreslerinden gelen tüm istekleri eşleşir.
  - **IPv6 örnek**: 1:2:3: / 48 adresleri 1:2:3:0:0:0:0:0 1:2:3:ffff:ffff:ffff:ffff:ffff aracılığıyla öğesinden gelen tüm istekleri eşleşir.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="cookie-parameter"></a>Tanımlama bilgisi parametresi
**Eşleşme**/**eşleşmiyor** seçenek, tanımlama bilgisi parametresi altında koşul aynı Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Bir istek belirtilen tanımlama bilgisi ile bu eşleşme koşulu içinde tanımlanan değerleri en az biriyle eşleşen bir değer içermesi gerekir.
- **Eşleşmeyen**: İstek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değeri hiçbir bu eşleşme koşulu içinde tanımlanan değerleri eşleşmiyor.
  
Anahtar bilgileri:
- Tanımlama bilgisi adı: 
  - Tanımlama bilgisi adı belirtilirken yıldız işareti (*) joker karakter değerleri desteklenmediğinden, yalnızca tam tanımlama bilgisi adı eşleşiyor karşılaştırma için uygundur.
  - Bu eşleşme koşulu örneği başına yalnızca tek bir tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmalar büyük/küçük harfe duyarsızdır.
- Tanımlama bilgisi değeri: 
  - Birden fazla tanımlama bilgisi değerleri, her biri tek bir boşluk ile sınırlayan tarafından belirtin.
  - Bir tanımlama bilgisi değeri yararlanabilirsiniz [değerleri joker](cdn-rules-engine-reference.md#wildcard-values). 
  - Ardından yalnızca tam olarak eşleşen bir joker karakter değeri belirtilmediği takdirde bu eşleşme koşulu eşleşecektir. Örneğin, "Value" belirtme "Value" ancak "Value1" veya "Value2" eşleşir
  - Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılır olup olmadığını denetlemek için seçenek.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="cookie-parameter-regex"></a>Tanımlama bilgisi parametre normal ifade
Tanımlama bilgisi parametresi Regex eşleşme koşulu, bir tanımlama bilgisi adı ve değeri tanımlar. Kullanabileceğiniz [normal ifadeler](cdn-rules-engine-reference.md#regular-expressions) istenen tanımlama bilgisi değeri tanımlamak için. 

**Eşleşme**/**eşleşmiyor** seçenek, tanımlama bilgisi parametresi Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Bir istek belirtilen tanımlama bilgisi ile belirtilen normal ifadeyle eşleşen bir değer içermesi gerekir.
- **Eşleşmeyen**: İstek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak belirtilen normal ifade değeri eşleşmiyor.
  
Anahtar bilgileri:
- Tanımlama bilgisi adı: 
  - Tanımlama bilgisi adı belirtilirken normal ifadeler ve yıldız işareti (*) joker karakter değerleri desteklenmediğinden, yalnızca tam tanımlama bilgisi adı eşleşiyor karşılaştırma için uygundur.
  - Bu eşleşme koşulu örneği başına yalnızca tek bir tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmalar büyük/küçük harfe duyarsızdır.
- Tanımlama bilgisi değeri: 
  - Bir tanımlama bilgisi değeri normal ifadeler yararlanabilirsiniz.
  - Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılır olup olmadığını denetlemek için seçenek.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="country"></a>Ülke
Bir ülkeye, ülke kodu aracılığıyla belirtebilirsiniz. 

**Eşleşme**/**eşleşmiyor** seçeneği ülke altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: Belirtilen ülke kodu değerler içerecek şekilde istek gerektirir. 
- **Eşleşmeyen**: İstek belirtilen ülke kodu değerleri içermiyor gerektirir.

Anahtar bilgileri:
- Birden fazla ülke kodu, her biri tek bir boşluk ile sınırlayan tarafından belirtin.
- Bir ülke kodu belirtirken joker karakterler desteklenmez.
- "AB" ve "AP" ülke kodları bu bölgelerdeki tüm IP adreslerini kapsayacak değil.
- Belirli istek geçerli bir ülke kodu döndürmeyebilir. Bir soru işareti (?) geçerli bir ülke kodu belirlenemedi istekleri eşleşir.
- Ülke Kodları büyük/küçük harfe duyarlıdır.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Kural altyapısı kullanarak uygulama ülke filtrelemesi
Bu eşleşme koşulu özelleştirmeleri bir isteğin kaynaklandığı konumuna göre çok sayıda yapmanıza olanak tanır. Örneğin, aşağıdaki yapılandırma ile ülke filtreleme özelliği davranışını çoğaltılabilir:

- URL yolu joker karakter eşleşmesi: Ayarlama [URL yolu joker karakter eşleşmesi koşul](#url-path-wildcard) alınmayacaksa dizine. 
    Bu kural tarafından tüm alt öğelerini erişimi engellenir emin olmak için göreli yol sonuna bir yıldız işareti ekleyin.

- Ülke eşleşme: Ülke eşleşme koşulu ülkeler istenen kümesi için ayarlayın.
   - İzin ver: Ülke eşleşme koşulu kümesine **eşleşmiyor** URL yolu joker karakter eşleşmesi koşul tarafından tanımlanan bir konumda depolanan içeriğe yalnızca belirtilen ülke erişmesine izin vermek için.
   - Engelleme: Ülke eşleşme koşulu kümesine **eşleşme** belirtilen ülkeleri URL yolu joker karakter eşleşmesi koşul tarafından tanımlanan bir konumda depolanan içeriğe erişimini engellemek için.

- Erişim (403) özelliği Reddet: Etkinleştirme [erişimi engelle (403) özellik](cdn-rules-engine-reference-features.md#deny-access-403) ülke filtreleme özelliği bir izin verilenler veya Engellenenler kısmı çoğaltmak için.

[Başa dön](#main)

</br>

---
### <a name="customer-origin"></a>Müşteri kaynağı

Anahtar bilgileri: 
- Müşteri kaynağı eşleşme koşulu olup içerik CDN URL ya da bir kenar için seçilen müşteri kaynağı işaret eden bir CNAME URL aracılığıyla istenmeden bağımsız olarak karşılanır.
- Müşteri kaynağı sayfasından bir kuralı tarafından başvurulan bir müşteri kaynağı yapılandırması silinemiyor. Bir müşteri kaynağı yapılandırması silme girişiminde bulunmadan önce aşağıdaki yapılandırmalar da başvurmadığından emin olun:
  - Bir müşteri kaynağı eşleşme koşulu
  - Bir edge CNAME yapılandırma
- Belirli eşleşme koşulları birleştirmek ve IF deyimini kullanmayın. Örneğin, bir müşteri kaynağı eşleşme koşulu CDN Origin eşleşme koşulu ile birleştirerek, hiç eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki müşteri kaynağı eşleşme koşulu ve IF deyimini ile birleştirilemez.

[Başa dön](#main)

</br>

---
### <a name="device"></a>Cihaz

Cihaz eşleşme koşulu, kendi özelliklerine bağlı olarak bir mobil CİHAZDAN istekleri tanımlar. Mobil cihaz algılaması aracılığıyla gerçekleştirilir [WURFL](http://wurfl.sourceforge.net/). 

**Eşleşme**/**eşleşmiyor** seçeneği, cihaz altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: Belirtilen değerle eşleşecek şekilde sahibinin cihaz gerektirir. 
- **Eşleşmeyen**: Cihaz sahibinin belirtilen değerle eşleşmiyor gerektirir.

Anahtar bilgileri:

- Kullanım **yoksay çalışması** belirtilen değerin büyük küçük harfe duyarlı olup olmadığını belirtmek için seçeneği.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

#### <a name="string-type"></a>Dize türü
WURFL yeteneği, sayı, harf ve semboller herhangi bir birleşimini genellikle kabul eder. Bu özellik esnek yapısı nedeniyle, bu eşleşme koşulu ile ilişkili değer nasıl yorumlanacağını seçmeniz gerekir. Aşağıdaki tabloda kullanılabilir seçenekleri açıklar:

Type     | Açıklama
---------|------------
değişmez değer  | Çoğu karakter kullanarak özel bir anlamı fotoğrafını çekmenizi engellemek için bu seçeneği, [değişmez değer](cdn-rules-engine-reference.md#literal-values).
Joker karakter | [Joker karakterler] tüm avantajlarından yararlanmak için bu seçeneği belirleyin ([değerleri joker](cdn-rules-engine-reference.md#wildcard-values).
Regex    | Kullanmak için bu seçeneği belirleyin [normal ifadeler](cdn-rules-engine-reference.md#regular-expressions). Normal ifadeleri desen karakter tanımlamak için yararlıdır.

#### <a name="wurfl-capabilities"></a>WURFL özellikleri
WURFL özelliği, mobil cihazları açıklayan bir kategoriye ifade eder. Seçilen özellik istekleri tanımlamak için kullanılan mobil cihaz açıklaması türünü belirler.

Aşağıdaki tabloda WURFL özelliklerini ve onların değişkenleri kurallar altyapısı için listeler.
<br>
> [!NOTE] 
> Aşağıdaki değişkenleri desteklenir **istemci isteği üst bilgisi değiştirme** ve **istemci yanıt üst bilgisi değiştirme** özellikleri.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Marka adı | %{wurfl_cap_brand_name} | Marka cihazın adını gösteren bir dize. | Samsung
Cihaz işletim sistemi | %{wurfl_cap_device_os} | Cihazda yüklü işletim sistemi gösteren bir dize. | IOS
Cihaz İşletim Sistemi Sürümü | %{wurfl_cap_device_os_version} | Cihazda yüklü işletim sisteminin sürüm numarasını belirten bir dize. | 1.0.1
Çift yönü | %{wurfl_cap_dual_orientation} | Cihaz çift yönlendirme destekleyip desteklemediğini belirten bir Boole değeri. | true
HTML DTD'nin tercih edilir. | %{wurfl_cap_html_preferred_dtd} | Mobil cihazın tercih edilen belge türü tanımı (DTD'nin) için HTML içeriğini belirten bir dize. | yok<br/>xhtml_basic<br/>HTML5
Satır içi görüntüsü | %{wurfl_cap_image_inlining} | Cihaz Base64 destekleyip desteklemediğini belirten bir Boole değeri görüntüleri kodlanmış. | false
Android olduğu | %{wurfl_vcap_is_android} | Cihazın Android işletim sistemi kullanıp kullanmadığını gösteren bir Boole değeri. | true
İOS | %{wurfl_vcap_is_ios} | Cihazın iOS kullanıp kullanmadığını gösteren bir Boole değeri. | false
Akıllı TV | %{wurfl_cap_is_smarttv} | Cihaz akıllı TV olup olmadığını belirten bir Boole değeri. | false
Smartphone olduğu | %{wurfl_vcap_is_smartphone} | Bir akıllı cihaz olup olmadığını belirten bir Boole değeri. | true
Tablet olduğu | %{wurfl_cap_is_tablet} | Cihaz bir tablet olup olmadığını belirten bir Boole değeri. Bu açıklama, işletim sistemi-bağımsızdır. | true
Kablosuz cihazdır | %{wurfl_cap_is_wireless_device} | Cihaz bir kablosuz cihaz olarak kabul edilip edilmediğini belirten bir Boole değeri. | true
Pazarlama adı | %{wurfl_cap_marketing_name} | Pazarlama cihazın adını gösteren bir dize. | BlackBerry 8100 Pearl
Mobil tarayıcı | %{wurfl_cap_mobile_browser} | İçerik CİHAZDAN istemek için kullanılan tarayıcı gösteren bir dize. | Chrome
Mobil tarayıcı sürümü | %{wurfl_cap_mobile_browser_version} | İçerik CİHAZDAN istemek için kullanılan tarayıcı sürümünü gösteren bir dize. | 31
Model Adı | %{wurfl_cap_model_name} | Cihazın model adını belirten dize. | S3
Aşamalı indirme | %{wurfl_cap_progressive_download} | Hala yüklenirken cihazın ses ve video kayıttan yürütme destekleyip desteklemediğini belirten bir Boole değeri. | true
Yayınlanma Tarihi | %{wurfl_cap_release_date} | Cihaz WURFL veritabanına eklendikten sonra ay ve yıl gösteren bir dize.<br/><br/>Biçim: `yyyy_mm` | 2013_december
Çözüm yükseklik | %{wurfl_cap_resolution_height} | Cihazın yüksekliğini piksel cinsinden belirten bir tamsayı. | 768
Çözüm genişliği | %{wurfl_cap_resolution_width} | Cihazın genişliğini piksel cinsinden belirten bir tamsayı. | 1024

[Başa dön](#main)

</br>

---
### <a name="edge-cname"></a>Edge Cname
Anahtar bilgileri: 
- Kullanılabilir edge CNAME'ler listesini kurallar altyapısı yapılandırılmakta olan bir platform için Edge CNAME'ler sayfasında yapılandırılan bu edge CNAME'ler sınırlıdır.
- Bir edge CNAME yapılandırmasını silme girişiminde bulunmadan önce bir kenar Cname eşleşme koşulu, başvurmuyor emin emin olun. Bir kuralda tanımlanan edge CNAME yapılandırmaları Edge CNAME'ler sayfasından silinemiyor. 
- Belirli eşleşme koşulları birleştirmek ve IF deyimini kullanmayın. Örneğin, bir kenar Cname eşleşme koşulu ile müşteri kaynağı eşleşme koşulu birleştirme hiç eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki Edge Cname eşleşme koşulu ve IF deyimini ile birleştirilemez.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="referring-domain"></a>Etki alanı başvuran
Ana bilgisayar adı, etki alanı başvuran koşulu karşılayıp karşılamadığını belirler, içerik istendi başvuran ile ilişkili. 

**Eşleşme**/**eşleşmiyor** seçeneği başvuran etki alanı altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: Belirtilen değerleriyle eşleşecek şekilde başvuran ana bilgisayar adı gerektirir. 
- **Eşleşmeyen**: Başvurulan ana bilgisayar adı belirtilen değerle eşleşmiyor gerektirir.

Anahtar bilgileri:
- Birden çok ana bilgisayar adları, her biri tek bir boşluk ile sınırlayan tarafından belirtin.
- Bu eşleşme koşulu destekler [değerleri joker](cdn-rules-engine-reference.md#wildcard-values).
- Belirtilen değer bir yıldız işareti içermiyorsa başvuran'ın ana bilgisayar adı için tam bir eşleşme olması gerekir. Örneğin, "etkialanım.com" belirtme "www.mydomain.com." eşleşmez
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma yapılan olup olmadığını denetlemek için seçenek.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="request-header-literal"></a>İstek üst bilgisi değişmez değeri
**Eşleşme**/**eşleşmiyor** seçeneği altında istek üst bilgisi değişmez değer eşleşen koşulu koşul karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen üstbilgiyi içerecek şekilde istek gerektirir. Değeri, bu eşleşme koşulu içinde tanımlanan bir eşleşmesi gerekir.
- **Eşleşmeyen**: İstek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen üstbilgiyi içermiyor.
  - Belirtilen üstbilgiyi içerir, ancak bu eşleşme koşulu içinde tanımlanan bir değeri ile eşleşmiyor.
  
Anahtar bilgileri:
- Üst bilgi adı karşılaştırmalar her zaman büyük/küçük harf duyarsızdır. Kullanım **yoksay çalışması** üst bilgi değeri karşılaştırma büyük küçük harf duyarlılığı denetleme seçeneği.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="request-header-regex"></a>İstek üst bilgisi normal ifade
**Eşleşme**/**eşleşmiyor** seçeneği istek üst bilgisi Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen üstbilgiyi içerecek şekilde istek gerektirir. Değeri tanımlı belirtilen desenle eşleşmelidir belirtilen [normal ifade](cdn-rules-engine-reference.md#regular-expressions).
- **Eşleşmeyen**: İstek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen üstbilgiyi içermiyor.
  - Belirtilen üstbilgiyi içerir, ancak değeri, belirtilen normal ifadeyle eşleşmiyor.

Anahtar bilgileri:
- Üst bilgi adı: 
  - Üst bilgi adı karşılaştırmalar büyük/küçük harfe duyarsızdır.
  - Üst bilgi adı boşluk "yerine % 20." 
- Üst bilgi değeri: 
  - Normal ifadeler bir üstbilgi değerini yararlanabilirsiniz.
  - Kullanım **yoksay çalışması** üst bilgi değeri karşılaştırma büyük küçük harf duyarlılığı denetleme seçeneği.
  - Yalnızca bir üst bilgi değeri tam olarak en az belirtilen desenle eşleştiğinde eşleşme koşulu karşılandı.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski 

[Başa dön](#main)

</br>

---
### <a name="request-header-wildcard"></a>İstek üst bilgisi joker karakter
**Eşleşme**/**eşleşmiyor** seçeneği istek üst bilgisi joker altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen üstbilgiyi içerecek şekilde istek gerektirir. Değeri bu eşleşme koşulu içinde tanımlanan değerleri en az biriyle eşleşmelidir.
- **Eşleşmeyen**: İstek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen üstbilgiyi içermiyor.
  - Belirtilen üstbilgiyi içerir, ancak değeri belirtilen değerler eşleşmiyor.
  
Anahtar bilgileri:
- Üst bilgi adı: 
  - Üst bilgi adı karşılaştırmalar büyük/küçük harfe duyarsızdır.
  - Üst bilgi adında boşluk bulunan "% 20." ile değiştirilmelidir Bu değer, boşluk bir üst bilgi değeri belirtmek için de kullanabilirsiniz.
- Üst bilgi değeri: 
  - Bir üstbilgi değerini yararlanabilirsiniz [değerleri joker](cdn-rules-engine-reference.md#wildcard-values).
  - Kullanım **yoksay çalışması** üst bilgi değeri karşılaştırma büyük küçük harf duyarlılığı denetleme seçeneği.
  - Belirtilen desenle en az biri için bir üstbilgi değerini tam olarak eşleşen olduğunda bu eşleşme koşulu karşılanır.
  - Her biri tek bir boşluk ile sınırlayan tarafından birden çok değer belirtin.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="request-method"></a>İstek yöntemi
Yalnızca seçili istek yöntemi aracılığıyla varlıklar istendiğinde istek yöntemi eşleşme koşulu karşılandı. Kullanılabilir istek yöntemler şunlardır:
- GET
- HEAD 
- POST 
- SEÇENEKLER 
- PUT 
- DELETE 
- TRACE 
- BAĞLANMA 

Anahtar bilgileri:
- Varsayılan olarak, önbelleğe alınan içeriği ağ üzerinde yalnızca GET isteği yöntemi oluşturur. Diğer tüm istek yöntemleri, ağ üzerinden taşınır.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="request-scheme"></a>İstek düzeni
Yalnızca seçili protokolüyle varlıklar istendiğinde istek düzeni eşleşme koşulu karşılanır. Kullanılabilir protokoller şunlardır: 
- HTTP
- HTTPS

Anahtar bilgileri:
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="url-path-directory"></a>URL yol dizini
Bir istek, istenen varlık dosya adı dahil değildir, göreli yol tanımlar.

**Eşleşme**/**eşleşmiyor** seçenek, URL yolu dizini altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen URL deseni ile eşleşen dosya adı dışında bir göreli URL yolu içerecek şekilde istek gerektirir.
- **Eşleşmeyen**: Belirtilen URL deseni eşleşmeyen dosya adı dışında bir göreli URL yolu içerecek şekilde istek gerektirir.

Anahtar bilgileri:
- Kullanım **bağıntı** URL karşılaştırma önce veya sonra içerik erişim noktası başlayıp başlamadığını belirtmek için seçeneği. Verizon CDN ana bilgisayar adını ve yolunu istenen varlığa (örneğin, /800001/CustomerOrigin) arasında görünür yolunun bir bölümü içerik erişimi noktasıdır. Bu, sunucu türü (örneğin, CDN veya müşteri kaynak) bir yer ve müşterinin hesap numaranızı tanımlar.

   Aşağıdaki değerler kullanılabilir **bağıntı** seçeneği:
   - **Kök**: URL karşılaştırma noktası doğrudan CDN ana bilgisayar sonra başlayacağını belirtir. 

     Örneğin: http:\//wpc.0001.&lt; etki alanı&gt;/**800001/myorigin/Klasörüm'ün**/index.htm

   - **Kaynak**: URL karşılaştırma noktası içerik erişim noktası (örneğin, /000001 veya/800001/myorigin) sonra başlayacağını belirtir. Çünkü \*. azureedge.net CNAME oluşturulur Verizon CDN ana bilgisayar üzerinde kaynak dizine göreli varsayılan olarak, Azure CDN kullanıcıların kullanması gereken **kaynak** değeri. 

     Örneğin: https:\//&lt;uç nokta&gt;.azureedge.net/**Klasörüm'ün**/index.htm 

     Bu URL aşağıdaki Verizon CDN ana bilgisayar adına işaret eder: http:\//wpc.0001.&lt; etki alanı&gt;/800001/myorigin/**Klasörüm'ün**/index.htm

- Bir edge CNAME URL URL karşılaştırma önce bir CDN URL'sine yeniden.

    Örneğin, aşağıdaki URL'ler her ikisi de aynı varlığa işaret ve bu nedenle URL yolunun aynısını sahiptir.
  - CDN URL'si: http:\//wpc.0001.&lt; etki alanı&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL'si: http:\//&lt;uç nokta&gt;.azureedge.net/path/asset.htm
    
    Ek bilgi:
  - Özel etki alanı: https:\//my.domain.com/path/asset.htm
    
  - URL yolu (köküne): / 800001/CustomerOrigin/path /
    
  - (Kaynak) göreli URL yolu: /path/

- URL karşılaştırma sona erer hemen önce istenen varlık dosya adı için kullanılan URL kısmı. Sondaki eğik çizgi, bu tür bir yol son karakterdir.
    
- URL yol deseni boşluk "yerine % 20."
    
- Her URL yol deseni, her bir yıldız işareti, bir veya daha fazla karakter dizisi eşleştiği bir veya daha fazla yıldız işareti (*) içerebilir.
    
- Birden çok URL yolu, her biri tek bir boşluk ile sınırlayan tarafından desen belirtin.

    Örneğin: * /sales/ * /marketing/

- URL yol belirtimi yararlanabilirsiniz [değerleri joker](cdn-rules-engine-reference.md#wildcard-values).

- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

[Başa dön](#main)

</br>

---
### <a name="url-path-extension"></a>URL yolu genişletme
İstek, istenen varlık dosya uzantısıyla tanımlar.

**Eşleşme**/**eşleşmiyor** seçeneği altında URL yolu uzantısı ile eşleşen koşulu koşul karşılanıyorsa belirler.
- **Eşleşme**: Tam olarak belirtilen desenle eşleşen bir dosya uzantısı içerecek şekilde istek URL'sini gerektirir.

   "Htm" belirtirseniz, örneğin, "htm" varlıklar, "html" varlıklar değil eşleştirilir.  

- **Eşleşmeyen**: URL isteği belirtilen deseniyle eşleşmeyen bir dosya uzantısını içermesi gerekir.

Anahtar bilgileri:
- Uzantıları ile eşleşen belirtin **değer** kutusu. Başında nokta dahil değildir; Örneğin, htm .htm yerine kullanın.

- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

- Tek bir boşluk ile her bir uzantısı sınırlayan tarafından birden çok dosya uzantısı belirtin. 

    Örneğin: htm html

- Örneğin, "htm" belirtme "htm" varlıklar, ancak değil "html" varlıklar eşleşir.


#### <a name="sample-scenario"></a>Örnek senaryo

Aşağıdaki örnek yapılandırma, bir istek belirtilen uzantılara biriyle eşleştiğinde bu eşleşme koşulu karşılanır varsayar.

Değer belirtimi: asp aspx php html

Aşağıdaki Uzantılar ile biten URL bulduğunda bu eşleşme koşulu karşılandı:
- .asp
- .aspx
- .php
- .HTML

[Başa dön](#main)

</br>

---
### <a name="url-path-filename"></a>URL yol dosyaadı
İstek, istenen varlığın dosya adını tanımlar. Bu eşleşme koşulu amaçları için bir dosya adı, istenen varlık, bir süre ve dosya uzantısı (örneğin, index.html) adını oluşur.

**Eşleşme**/**eşleşmiyor** seçenek, URL yolu dosya adı altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: İstek URL yolundaki belirtilen desenle eşleşen bir dosya adı içermesi gerekir.
- **Eşleşmeyen**: Dosya adında belirtilen desenle eşleşmez, URL yolu içerecek şekilde istek gerektirir.

Anahtar bilgileri:
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

- Birden çok dosya uzantısı belirtmek için her bir uzantı tek bir boşluk ile ayırın.

    Örneğin: index.htm index.html

- Bir dosya adı değeri boşluk "yerine % 20."
    
- Bir dosya adı değeri yararlanabilirsiniz [değerleri joker](cdn-rules-engine-reference.md#wildcard-values). Örneğin, her dosya adı deseni bir veya daha fazla yıldız işareti (*), her bir yıldız işareti, bir veya daha fazla karakter dizisi eşleştiği oluşabilir.
    
- Joker karakterler belirtilmezse, yalnızca tam bir eşleşme bu eşleşme koşulu yerine getirecek.

    Örneğin, "sunusu.ppt" belirtme "sunusu.ppt" ancak değil bir adlandırılmış "presentation.pptx." adlı bir varlığı eşleşir

[Başa dön](#main)

</br>

---
### <a name="url-path-literal"></a>URL yolu değişmez değeri
Bir isteğin URL yolu, dosya adı, belirtilen değerle karşılaştırır.

**Eşleşme**/**eşleşmiyor** seçeneği altında URL yolu değişmez eşleşen koşulu koşul karşılanıyorsa belirler.
- **Eşleşme**: İsteği belirtilen desenle eşleşen bir URL yolu içermesi gerekir.
- **Eşleşmeyen**: İsteği belirtilen desenle eşleşmiyor bir URL yolu içermesi gerekir.

Anahtar bilgileri:
- Kullanım **bağıntı** URL karşılaştırma noktası öncesinde veya sonrasında içerik erişim noktası başlayıp başlamayacağını belirtmek için seçeneği. 

    Aşağıdaki değerler kullanılabilir **bağıntı** seçeneği:
  - **Kök**: URL karşılaştırma noktası doğrudan CDN ana bilgisayar sonra başlayacağını belirtir.

    Örneğin: http:\//wpc.0001.&lt; etki alanı&gt;/**800001/myorigin/myfolder/index.htm**

  - **Kaynak**: URL karşılaştırma noktası içerik erişim noktası (örneğin, /000001 veya/800001/myorigin) sonra başlayacağını belirtir. Çünkü \*. azureedge.net CNAME oluşturulur Verizon CDN ana bilgisayar üzerinde kaynak dizine göreli varsayılan olarak, Azure CDN kullanıcıların kullanması gereken **kaynak** değeri. 

    Örneğin: https:\//&lt;uç nokta&gt;.azureedge.net/**myfolder/index.htm**

    Bu URL aşağıdaki Verizon CDN ana bilgisayar adına işaret eder: http:\//wpc.0001.&lt; etki alanı&gt;/800001/myorigin/**myfolder/index.htm**

- Bir edge CNAME URL bir URL karşılaştırma önce bir CDN URL'sine yeniden.

    Örneğin, aşağıdaki URL'ler her ikisi de aynı varlığa işaret ve bu nedenle URL yolunun aynısını sahiptir:
  - CDN URL'si: http:\//wpc.0001.&lt; etki alanı&gt;/800001/CustomerOrigin/path/asset.htm
  - Edge CNAME URL'si: http:\//&lt;uç nokta&gt;.azureedge.net/path/asset.htm
    
    Ek bilgi:
    
  - URL yolu (köküne): /800001/CustomerOrigin/path/asset.htm
   
  - (Kaynak) göreli URL yolu: /path/asset.htm

- URL'de bulunan sorgu dizelerini göz ardı edilir.
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
- Bu eşleşme koşulu için belirtilen değer göreli yolu istemci tarafından yapılan tam istek karşılaştırılır.

- Belirli bir dizin için yapılan tüm istekleri eşleştirmek için kullanın [URL yolu dizini](#url-path-directory) veya [URL yolu joker](#url-path-wildcard) eşleşen koşul.

[Başa dön](#main)

</br>

---
### <a name="url-path-regex"></a>URL yolu normal ifade
Belirtilen bir isteğin URL yolu karşılaştırır [normal ifade](cdn-rules-engine-reference.md#regular-expressions).

**Eşleşme**/**eşleşmiyor** seçenek, URL yolu Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: İstek belirtilen normal ifadeyle eşleşen bir URL yolu içermesini gerektirir.
- **Eşleşmeyen**: Belirtilen normal ifadeyle eşleşmez bir URL yolu içerecek şekilde istek gerektirir.

Anahtar bilgileri:
- Bir edge CNAME URL URL karşılaştırma önce bir CDN URL'sine yeniden. 
 
    Örneğin, her iki URL'leri aynı varlığa işaret ve bu nedenle URL yolunun aynısını sahiptir.

  - CDN URL'si: http:\//wpc.0001.&lt; etki alanı&gt;/800001/CustomerOrigin/path/asset.htm

  - Edge CNAME URL'si: http:\//my.domain.com/path/asset.htm
    
    Ek bilgi:
    
  - URL yolu: /800001/CustomerOrigin/path/asset.htm

- URL'de bulunan sorgu dizelerini göz ardı edilir.
    
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
    
- URL yolu boşluk "% 20." ile değiştirilmelidir

[Başa dön](#main)

</br>

---
### <a name="url-path-wildcard"></a>URL yolu joker karakter
Bir isteğin göreli URL yolu için belirtilen bir joker karakter deseni karşılaştırır.

**Eşleşme**/**eşleşmiyor** seçenek, URL yolu joker karakterin altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: İsteği belirtilen joker karakter deseni ile eşleşen bir URL yolu içermesi gerekir.
- **Eşleşmeyen**: İsteği belirtilen joker karakter deseni eşleşmeyen bir URL yolu içermesi gerekir.

Anahtar bilgileri:
- **Göreli yolu** seçeneği: Bu seçenek, URL karşılaştırma noktası öncesinde veya sonrasında içerik erişim noktası başlayıp başlamayacağını belirler.

   Bu seçenek, aşağıdaki değerlere sahip olabilir:
  - **Kök**: URL karşılaştırma noktası doğrudan CDN ana bilgisayar sonra başlayacağını belirtir.

    Örneğin: http:\//wpc.0001.&lt; etki alanı&gt;/**800001/myorigin/myfolder/index.htm**

  - **Kaynak**: URL karşılaştırma noktası içerik erişim noktası (örneğin, /000001 veya/800001/myorigin) sonra başlayacağını belirtir. Çünkü \*. azureedge.net CNAME oluşturulur Verizon CDN ana bilgisayar üzerinde kaynak dizine göreli varsayılan olarak, Azure CDN kullanıcıların kullanması gereken **kaynak** değeri. 

    Örneğin: https:\//&lt;uç nokta&gt;.azureedge.net/**myfolder/index.htm**

    Bu URL aşağıdaki Verizon CDN ana bilgisayar adına işaret eder: http:\//wpc.0001.&lt; etki alanı&gt;/800001/myorigin/**myfolder/index.htm**

- Bir edge CNAME URL URL karşılaştırma önce bir CDN URL'sine yeniden.

    Örneğin, aşağıdaki URL'ler her ikisi de aynı varlığa işaret ve bu nedenle URL yolunun aynısını sahiptir:
  - CDN URL'si: http://wpc.0001.&lt; etki alanı&gt;/800001/CustomerOrigin/path/asset.htm
  - Edge CNAME URL'si: http:\//&lt;uç nokta&gt;.azureedge.net/path/asset.htm
    
    Ek bilgi:
    
  - URL yolu (köküne): /800001/CustomerOrigin/path/asset.htm
    
  - (Kaynak) göreli URL yolu: /path/asset.htm
    
- Her biri tek bir boşluk ile sınırlayan tarafından birden çok URL yolu belirtin.

   Örneğin: /marketing/asset.* /sales/*.htm

- URL'de bulunan sorgu dizelerini göz ardı edilir.
    
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
    
- URL yolu boşluk "yerine % 20."
    
- Bir URL yolu yararlanabilirsiniz için belirtilen değer [değerleri joker](cdn-rules-engine-reference.md#wildcard-values). Her URL yol deseni her yıldız en az bir karakter dizisi eşleştiği bir veya daha fazla yıldız işareti (*) içerebilir.

#### <a name="sample-scenarios"></a>Örnek senaryolar

Aşağıdaki tabloda örnek yapılandırmaları bir istek belirtilen URL deseni eşleştiğinde bu eşleşme koşulu karşılanır varsayın:

Değer                   | Göreli    | Sonuç 
------------------------|----------------|-------
*/test.html */test.php  | Kök veya kaynağı | Bu desen, "örnek test.html" ya da herhangi bir klasörde "test.php" adlı varlıklar istekleriyle eşleştirilir.
/ 80ABCD/kaynak/metin / *   | Kök           | Bu düzen, istenen varlık aşağıdaki ölçütleri karşıladığında eşleştirilir: <br />-Bu "Başlangıç" adlı bir müşteri kaynağı üzerinde bulunmalıdır <br />-Göreli yolu "metin" adlı bir klasör ile başlamalıdır Diğer bir deyişle, istenen varlık "metin" klasörü veya özyinelemeli klasörlerinden birinde bulunabilir.
*/CSS/* */js/*          | Kök veya kaynağı | Bu düzen tüm CDN veya edge css veya js bir klasör içeren CNAME URL'ler göre eşleştirilir.
*.jpg *.gif *.png       | Kök veya kaynağı | Bu düzen tüm CDN veya edge CNAME URL'lerin ilgili .jpg, .gif veya .png ile biten eşleştirilir. Bu düzen belirtmek için alternatif bir yolu olan [URL yolu uzantısı, koşul ile eşleşen](#url-path-extension).
/ Resimler / * / ortam / *      | Kaynak         | Bu düzen, CDN veya edge CNAME bir "Görüntü" veya "ortam" klasörü ile göreli yolu başlar URL'leri eşleştirilir. <br />-CDN URL'si: http:\//wpc.0001.&lt; etki alanı&gt;/800001/myorigin/images/sales/event1.png<br />-Örnek edge CNAME URL'si: http:\//cdn.mydomain.com/images/sales/event1.png

[Başa dön](#main)

</br>

---
### <a name="url-query-literal"></a>URL sorgu değişmez değeri
Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.

**Eşleşme**/**eşleşmiyor** seçeneği altında URL'si sorgu değişmez eşleşen koşulu koşul karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen sorgu dizesi ile eşleşen bir URL sorgu dizesini içeren isteği gerektirir.
- **Eşleşmeyen**: Belirtilen sorgu dizesini eşleşmiyor bir URL sorgu dizesini içeren isteği gerektirir.

Anahtar bilgileri:

- Yalnızca tam sorgu dize eşleşmeleri bu eşleşme koşulu karşılayan.
    
- Kullanım **yoksay çalışması** sorgu dize karşılaştırmaları duyarlılığını denetleme seçeneği.
    
- Önde gelen soru işareti (?), sorgu dizesi değeri metinde içermez.
    
- URL kodlaması belirli karakter gerektirir. Kullanım yüzde simgesi URL şu karakterleri kodlamak:

   Karakter | URL kodlaması
   ----------|---------
   Uzay     | %20
   &         | %25

- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Maksimum yaş
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

[Başa dön](#main)

</br>

---
### <a name="url-query-parameter"></a>URL sorgu parametresi
Belirtilen sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirtilen desenle eşleşen bir değere ayarlanır. Sorgu dizesi parametreleri (örneğin, parametre = value) istek URL'si, bu koşulu karşılayıp karşılamadığını belirlemek. Bu eşleşme koşulu, bir sorgu dizesi parametresi adını kullanarak tanımlar ve bir veya daha fazla değerler için parametre değeri kabul eder. 

**Eşleşme**/**eşleşmiyor** seçeneği URL'si sorgu parametresi altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Bir istek bu eşleşme koşulu içinde tanımlanan değerleri en az biriyle eşleşen bir değer ile belirtilen parametre içermesini gerektirir.
- **Eşleşmeyen**: İstek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen parametre içermiyor.
  - Belirtilen parametre içerir, ancak değeri hiçbir bu eşleşme koşulu içinde tanımlanan değerleri eşleşmiyor.

Bu eşleşme koşulu, parametre adı/değer birleşimleri belirtmek için kolay bir yol sağlar. Bir sorgu dizesi parametresi eşleşen daha fazla esneklik için kullanmayı [URL'si sorgu joker](#url-query-wildcard) eşleşen koşul.

Anahtar bilgileri:
- Bu eşleşme koşulu örneği başına yalnızca bir tek URL sorgu parametresi adı belirtilebilir.
    
- Parametre adı belirtildiğinde değerleri joker desteklenmediğinden, yalnızca tam parametre adı eşleşiyor karşılaştırma için uygundur.
- Parametre değerlerini içerebilir [değerleri joker](cdn-rules-engine-reference.md#wildcard-values).
   - Her parametre değeri desen, bir veya daha fazla karakter dizisi her yıldız işareti eşleştiği bir veya daha fazla yıldız işaretleri (*) oluşabilir.
   - URL kodlaması belirli karakter gerektirir. Kullanım yüzde simgesi URL şu karakterleri kodlamak:

       Karakter | URL kodlaması
       ----------|---------
       Uzay     | %20
       &         | %25

- Her biri tek bir boşluk ile sınırlayan tarafından birden çok sorgu dizesi parametresi değerlerini belirtin. Bir istek belirtilen ad/değer birleşimleri içerdiğinde bu eşleşme koşulu karşılanır.

   - Örnek 1:

     - Yapılandırma:

       Değera Değerb

     - Bu yapılandırma aşağıdaki sorgu dizesi parametreleri eşleşen:

       Parametre1 Değera =
    
       Parametre1 Değerb =

   - Örnek 2:

     - Yapılandırma: 

        % 20A değeri % 20B değeri

     - Bu yapılandırma aşağıdaki sorgu dizesi parametreleri eşleşen:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Yalnızca belirtilen sorgu dizesi ad/değer birleşimleri en az biri için tam bir eşleşme olduğunda, bu eşleşme koşulu karşılanır.

   Örneğin, önceki örnekte yapılandırmayı kullanıyorsanız, parametre ad/değer birleşimi "parametre1 ValueAdd =" bir eşleşme olarak değerlendirilebilecek değil. Ancak, aşağıdaki değerlerden birini belirtirseniz, bu ad/değer bileşimi eşleşir:

   - Değera Değerb ValueAdd
   - Değera * Değerb

- Kullanım **yoksay çalışması** sorgu dize karşılaştırmaları duyarlılığını denetleme seçeneği.
    
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Maksimum yaş
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

#### <a name="sample-scenarios"></a>Örnek senaryolar
Aşağıdaki örnek, bu seçenek belirli durumlarda nasıl çalıştığını gösterir:

Ad  | Değer |  Sonuç
------|-------|--------
Kullanıcı  | ALi   | İstenen URL sorgu dizesi olduğunda bu deseni eşleştirilen "? kullanıcı = joe."
Kullanıcı  | *     | Bir kullanıcı parametresi için istenen URL'ye sorgu dizesi içerdiğinde, bu düzen eşleştirilir.
Email | ALi\* | İstenen URL'ye sorgu dizesi "ALi" ile başlayan bir e-posta parametresini içerdiğinde bu düzen eşleşiyor

[Başa dön](#main)

</br>

---
### <a name="url-query-regex"></a>Sorgu Regex URL'si
Belirtilen sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirtilen bir eşleşen bir değere ayarlanır [normal ifade](cdn-rules-engine-reference.md#regular-expressions).

**Eşleşme**/**eşleşmiyor** seçeneği URL'si sorgu Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen normal ifadeyle eşleşen bir URL sorgu dizesini içeren isteği gerektirir.
- **Eşleşmeyen**: Belirtilen normal ifadeyle eşleşmez bir URL sorgu dizesini içeren isteği gerektirir.

Anahtar bilgileri:
- Yalnızca tam eşleşme belirtilen normal ifade için bu eşleşme koşulu karşılaması.
    
- Kullanım **yoksay çalışması** sorgu dize karşılaştırmaları duyarlılığını denetleme seçeneği.
    
- Bu seçeneğin amacı doğrultusunda, sorgu dizesi için soru işareti (?) sınırlandırıcıdan sonra gelen ilk karakter ile bir sorgu dizesi başlatır.
    
- URL kodlaması belirli karakter gerektirir. Kullanım yüzde simgesi URL şu karakterleri kodlamak:

   Karakter | URL kodlaması | Değer
   ----------|--------------|------
   Uzay     | %20          | \%20
   &         | %25          | \%25

   Yüzde sembolleri atlanması gereken unutmayın.

- Çift kaçış özel normal ifade karakterleri (örneğin, \^$. +) normal ifadede ters eğik çizgi dahil etmek için.

   Örneğin:

   Değer | Yorumlanan 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Maksimum yaş
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski


[Başa dön](#main)

</br>

---
### <a name="url-query-wildcard"></a>URL sorgu joker karakter
Belirtilen değerleri isteğin sorgu dizesi karşı karşılaştırır.

**Eşleşme**/**eşleşmiyor** seçeneği URL'si sorgu joker karakterin altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen bir joker karakter değeri ile eşleşen bir URL sorgu dizesini içeren isteği gerektirir.
- **Eşleşmeyen**: Belirtilen bir joker karakter değeri eşleşmiyor bir URL sorgu dizesini içeren isteği gerektirir.

Anahtar bilgileri:
- Bu seçeneğin amacı doğrultusunda, sorgu dizesi için soru işareti (?) sınırlandırıcıdan sonra gelen ilk karakter ile bir sorgu dizesi başlatır.
- Parametre değerlerini içerebilir [değerleri joker](cdn-rules-engine-reference.md#wildcard-values):
   - Her parametre değeri desen, bir veya daha fazla karakter dizisi her yıldız işareti eşleştiği bir veya daha fazla yıldız işaretleri (*) oluşabilir.
   - URL kodlaması belirli karakter gerektirir. Kullanım yüzde simgesi URL şu karakterleri kodlamak:

     Karakter | URL kodlaması
     ----------|---------
     Uzay     | %20
     &         | %25

- Her biri tek bir boşluk ile sınırlayan tarafından birden çok değer belirtin.

   Örneğin: *Parametre1 Değera =* *Değerb* *parametre1 = ValueC & parametre2 = değerli*

- Yalnızca bu eşleşme koşulu için belirtilen sorgu dizesi desenleri en az bir tam eşleşme karşılar.
    
- Kullanım **yoksay çalışması** sorgu dize karşılaştırmaları duyarlılığını denetleme seçeneği.
    
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Maksimum yaş
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

#### <a name="sample-scenarios"></a>Örnek senaryolar
Aşağıdaki örnek, bu seçenek belirli durumlarda nasıl çalıştığını gösterir:

 Ad                 | Açıklama
 ---------------------|------------
user=joe              | İstenen URL sorgu dizesi olduğunda bu deseni eşleştirilen "? kullanıcı = joe."
\*Kullanıcı =\* \*geri çevirme =\* | CDN URL'sine sorgu kullanıcı veya geri çevirme parametresini içerdiğinde bu düzen eşleştirilir.

[Başa dön](#main)

</br>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Content Delivery Network genel bakış](cdn-overview.md)
* [Kural altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kural altyapısı koşullu ifadeleri](cdn-rules-engine-reference-conditional-expressions.md)
* [Kural altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kural altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)

