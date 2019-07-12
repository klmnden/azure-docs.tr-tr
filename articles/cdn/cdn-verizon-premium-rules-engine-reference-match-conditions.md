---
title: Premium Verizon'dan Azure CDN kural altyapısı eşleştirme koşulları | Microsoft Docs
description: Azure Content Delivery Network Premium Verizon için başvuru belgeleri altyapısı eşleştirme koşulları kuralları.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 1660dca34b2f128ef5889145fcdeed0d2523b9bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593211"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Verizon özel kurallar altyapısı Azure CDN'den eşleşen koşulları

Bu makalede ayrıntılı açıklamaları için Azure Content Delivery Network (CDN) Verizon Premium'dan kullanılabilir eşleştirme koşulları listeler [kurallar altyapısı](cdn-verizon-premium-rules-engine.md).

İkinci bir kuralı bir eşleşme koşulu parçasıdır. Belirli türde istekler bir özellikler kümesi gerçekleştirilecek bir eşleşme koşulu tanımlar.

Örneğin, bir eşleşme koşulu için kullanabilirsiniz:

- Belirli bir konumdaki içerik isteklerini filtre.
- İstek Filtreleme belirli IP adresi ya da ülke/bölge oluşturulur.
- Üst bilgileri tarafından istek filtreleme.

## <a name="always-match-condition"></a>Her zaman eşleşme koşulu

Her zaman eşleşme koşulu, tüm istekler için varsayılan bir özellikler kümesi uygular.

Ad | Amaç
-----|--------
[Her zaman](#always) | Varsayılan bir özellik kümesi, tüm istekler için geçerlidir.

## <a name="device-match-condition"></a>Cihaz eşleşme koşulu

Cihaz eşleşme koşulu, kendi özelliklerine bağlı olarak bir mobil CİHAZDAN istekleri tanımlar.  

Ad | Amaç
-----|--------
[cihaz](#device) | Kendi özelliklerine bağlı olarak bir mobil CİHAZDAN istekleri tanımlar.

## <a name="location-match-conditions"></a>Konum eşleşme koşulları

İstek sahibinin konumuna göre konum eşleşme koşulları tanımlayın.

Ad | Amaç
-----|--------
[Sayı olarak](#as-number) | Belirli bir ağdan kaynaklanan istekler tanımlar.
[Ülke](#country) | Belirtilen ülkelerden/bölgelerden kökenli isteklerine tanımlar.

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
[İstemci IP adresi](#client-ip-address) | Belirli bir IP adresinden kaynaklanan istekler tanımlar.
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
[URL Path Regex](#url-path-regex) | Bir isteğin belirtilen normal ifade için göreli yol karşılaştırır.
[URL yolu joker karakter](#url-path-wildcard) | Bir isteğin göreli yolu belirtilen desen ile karşılaştırır.
[URL sorgu değişmez değeri](#url-query-literal) | Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.
[URL sorgu parametresi](#url-query-parameter) | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
[Sorgu Regex URL'si](#url-query-regex) | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
[URL sorgu joker karakter](#url-query-wildcard) | Belirtilen isteğin sorgu dizesi değeri karşılaştırır.

## <a name="reference-for-rules-engine-match-conditions"></a>Kural altyapısı eşleştirme koşulları için başvuru

---

### <a name="always"></a>Her zaman

Her zaman eşleşme koşulu, tüm istekler için varsayılan bir özellikler kümesi uygular.

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="client-ip-address"></a>İstemci IP adresi

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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
  - Bir tanımlama bilgisi değeri yararlanabilirsiniz [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Ardından yalnızca tam olarak eşleşen bir joker karakter değeri belirtilmediği takdirde bu eşleşme koşulu eşleşecektir. Örneğin, "Value" belirtme "Value" ancak "Value1" veya "Value2" eşleşir
  - Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılır olup olmadığını denetlemek için seçenek.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#reference-for-rules-engine-match-conditions)
</br>

---

### <a name="cookie-parameter-regex"></a>Tanımlama bilgisi parametre normal ifade

Tanımlama bilgisi parametresi Regex eşleşme koşulu, bir tanımlama bilgisi adı ve değeri tanımlar. Kullanabileceğiniz [normal ifadeler](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) istenen tanımlama bilgisi değeri tanımlamak için.

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

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="country"></a>Country

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

- Erişim (403) özelliği Reddet: Etkinleştirme [erişimi engelle (403) özellik](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) ülke filtreleme özelliği bir izin verilenler veya Engellenenler kısmı çoğaltmak için.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="customer-origin"></a>Müşteri kaynağı

Anahtar bilgileri:

- Müşteri kaynağı eşleşme koşulu olup içerik CDN URL ya da bir kenar için seçilen müşteri kaynağı işaret eden bir CNAME URL aracılığıyla istenmeden bağımsız olarak karşılanır.
- Müşteri kaynağı sayfasından bir kuralı tarafından başvurulan bir müşteri kaynağı yapılandırması silinemiyor. Bir müşteri kaynağı yapılandırması silme girişiminde bulunmadan önce aşağıdaki yapılandırmalar da başvurmadığından emin olun:
  - Bir müşteri kaynağı eşleşme koşulu
  - Bir edge CNAME yapılandırma
- Belirli eşleşme koşulları birleştirmek ve IF deyimini kullanmayın. Örneğin, bir müşteri kaynağı eşleşme koşulu CDN Origin eşleşme koşulu ile birleştirerek, hiç eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki müşteri kaynağı eşleşme koşulu ve IF deyimini ile birleştirilemez.

[Başa dön](#reference-for-rules-engine-match-conditions)

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
değişmez değer  | Çoğu karakter kullanarak özel bir anlamı fotoğrafını çekmenizi engellemek için bu seçeneği, [değişmez değer](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Joker karakter | [Joker karakterler] tüm avantajlarından yararlanmak için bu seçeneği belirleyin ([değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Kullanmak için bu seçeneği belirleyin [normal ifadeler](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Normal ifadeleri desen karakter tanımlamak için yararlıdır.

#### <a name="wurfl-capabilities"></a>WURFL özellikleri

WURFL özelliği, mobil cihazları açıklayan bir kategoriye ifade eder. Seçilen özellik istekleri tanımlamak için kullanılan mobil cihaz açıklaması türünü belirler.

Aşağıdaki tabloda WURFL özelliklerini ve onların değişkenleri kurallar altyapısı için listeler.

> [!NOTE]
> Aşağıdaki değişkenleri desteklenir **istemci isteği üst bilgisi değiştirme** ve **istemci yanıt üst bilgisi değiştirme** özellikleri.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Marka adı | %{wurfl_cap_brand_name} | Marka cihazın adını gösteren bir dize. | Samsung
Cihaz işletim sistemi | %{wurfl_cap_device_os} | Cihazda yüklü işletim sistemi gösteren bir dize. | İOS
Cihaz işletim sistemi sürümü | %{wurfl_cap_device_os_version} | Cihazda yüklü işletim sisteminin sürüm numarasını belirten bir dize. | 1.0.1
Çift yönü | %{wurfl_cap_dual_orientation} | Cihaz çift yönlendirme destekleyip desteklemediğini belirten bir Boole değeri. | true
HTML DTD'nin tercih edilir. | %{wurfl_cap_html_preferred_dtd} | Mobil cihazın tercih edilen belge türü tanımı (DTD'nin) için HTML içeriğini belirten bir dize. | Yok<br/>xhtml_basic<br/>HTML5
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
Model adı | %{wurfl_cap_model_name} | Cihazın model adını belirten dize. | S3
Aşamalı indirme | %{wurfl_cap_progressive_download} | Hala yüklenirken cihazın ses ve video kayıttan yürütme destekleyip desteklemediğini belirten bir Boole değeri. | true
Yayınlanma Tarihi | %{wurfl_cap_release_date} | Cihaz WURFL veritabanına eklendikten sonra ay ve yıl gösteren bir dize.<br/><br/>Biçim: `yyyy_mm` | 2013_december
Çözüm yükseklik | %{wurfl_cap_resolution_height} | Cihazın yüksekliğini piksel cinsinden belirten bir tamsayı. | 768
Çözüm genişliği | %{wurfl_cap_resolution_width} | Cihazın genişliğini piksel cinsinden belirten bir tamsayı. | 1024

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="referring-domain"></a>Etki alanı başvuran

Ana bilgisayar adı, etki alanı başvuran koşulu karşılayıp karşılamadığını belirler, içerik istendi başvuran ile ilişkili.

**Eşleşme**/**eşleşmiyor** seçeneği başvuran etki alanı altında koşul eşleşen Koşullar karşılanıyorsa belirler:

- **Eşleşme**: Belirtilen değerleriyle eşleşecek şekilde başvuran ana bilgisayar adı gerektirir. 
- **Eşleşmeyen**: Başvurulan ana bilgisayar adı belirtilen değerle eşleşmiyor gerektirir.

Anahtar bilgileri:

- Birden çok ana bilgisayar adları, her biri tek bir boşluk ile sınırlayan tarafından belirtin.
- Bu eşleşme koşulu destekler [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- Belirtilen değer bir yıldız işareti içermiyorsa başvuran'ın ana bilgisayar adı için tam bir eşleşme olması gerekir. Örneğin, "etkialanım.com" belirtme "www.mydomain.com." eşleşmez
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma yapılan olup olmadığını denetlemek için seçenek.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-regex"></a>İstek üst bilgisi normal ifade

**Eşleşme**/**eşleşmiyor** seçeneği istek üst bilgisi Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.

- **Eşleşme**: Belirtilen üstbilgiyi içerecek şekilde istek gerektirir. Değeri tanımlı belirtilen desenle eşleşmelidir belirtilen [normal ifade](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
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

[Başa dön](#reference-for-rules-engine-match-conditions)

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
  - Bir üstbilgi değerini yararlanabilirsiniz [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Kullanım **yoksay çalışması** üst bilgi değeri karşılaştırma büyük küçük harf duyarlılığı denetleme seçeneği.
  - Belirtilen desenle en az biri için bir üstbilgi değerini tam olarak eşleşen olduğunda bu eşleşme koşulu karşılanır.
  - Her biri tek bir boşluk ile sınırlayan tarafından birden çok değer belirtin.
- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Maksimum yaş
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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

- URL yol belirtimi yararlanabilirsiniz [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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

- Bir dosya adı değeri yararlanabilirsiniz [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Örneğin, her dosya adı deseni bir veya daha fazla yıldız işareti (*), her bir yıldız işareti, bir veya daha fazla karakter dizisi eşleştiği oluşabilir.

- Joker karakterler belirtilmezse, yalnızca tam bir eşleşme bu eşleşme koşulu yerine getirecek.

    Örneğin, "sunusu.ppt" belirtme "sunusu.ppt" ancak değil bir adlandırılmış "presentation.pptx." adlı bir varlığı eşleşir

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-regex"></a>URL yolu normal ifade

Belirtilen bir isteğin URL yolu karşılaştırır [normal ifade](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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
     - CDN URL'si: http://wpc.0001.&lt ; etki alanı&gt; /800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL'si: http:\//&lt;uç nokta&gt;.azureedge.net/path/asset.htm
    
    Ek bilgi:
    
     - URL yolu (köküne): /800001/CustomerOrigin/path/asset.htm
    
     - (Kaynak) göreli URL yolu: /path/asset.htm
    
- Her biri tek bir boşluk ile sınırlayan tarafından birden çok URL yolu belirtin.

   Örneğin: /marketing/asset.* /sales/*.htm

- URL'de bulunan sorgu dizelerini göz ardı edilir.
    
- Kullanım **yoksay çalışması** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
    
- URL yolu boşluk "yerine % 20."
    
- Bir URL yolu yararlanabilirsiniz için belirtilen değer [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Her URL yol deseni her yıldız en az bir karakter dizisi eşleştiği bir veya daha fazla yıldız işareti (*) içerebilir.

#### <a name="sample-scenarios"></a>Örnek senaryolar

Aşağıdaki tabloda örnek yapılandırmaları bir istek belirtilen URL deseni eşleştiğinde bu eşleşme koşulu karşılanır varsayın:

Value                   | Göreli    | Sonuç 
------------------------|----------------|-------
*/test.html */test.php  | Kök veya kaynağı | Bu desen, "örnek test.html" ya da herhangi bir klasörde "test.php" adlı varlıklar istekleriyle eşleştirilir.
/ 80ABCD/kaynak/metin / *   | Kök           | Bu düzen, istenen varlık aşağıdaki ölçütleri karşıladığında eşleştirilir: <br />-Bu "Başlangıç" adlı bir müşteri kaynağı üzerinde bulunmalıdır <br />-Göreli yolu "metin" adlı bir klasör ile başlamalıdır Diğer bir deyişle, istenen varlık "metin" klasörü veya özyinelemeli klasörlerinden birinde bulunabilir.
*/CSS/* */js/*          | Kök veya kaynağı | Bu düzen tüm CDN veya edge css veya js bir klasör içeren CNAME URL'ler göre eşleştirilir.
*.jpg *.gif *.png       | Kök veya kaynağı | Bu düzen tüm CDN veya edge CNAME URL'lerin ilgili .jpg, .gif veya .png ile biten eşleştirilir. Bu düzen belirtmek için alternatif bir yolu olan [URL yolu uzantısı, koşul ile eşleşen](#url-path-extension).
/ Resimler / * / ortam / *      | Kaynak         | Bu düzen, CDN veya edge CNAME bir "Görüntü" veya "ortam" klasörü ile göreli yolu başlar URL'leri eşleştirilir. <br />-CDN URL'si: http:\//wpc.0001.&lt; etki alanı&gt;/800001/myorigin/images/sales/event1.png<br />-Örnek edge CNAME URL'si: http:\//cdn.mydomain.com/images/sales/event1.png

[Başa dön](#reference-for-rules-engine-match-conditions)

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

[Başa dön](#reference-for-rules-engine-match-conditions)

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
- Parametre değerlerini içerebilir [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
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

Ad  | Value |  Sonuç
------|-------|--------
Kullanıcı  | ALi   | İstenen URL sorgu dizesi olduğunda bu deseni eşleştirilen "? kullanıcı = joe."
Kullanıcı  | *     | Bir kullanıcı parametresi için istenen URL'ye sorgu dizesi içerdiğinde, bu düzen eşleştirilir.
Email | ALi\* | İstenen URL'ye sorgu dizesi "ALi" ile başlayan bir e-posta parametresini içerdiğinde bu düzen eşleşiyor

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-regex"></a>Sorgu Regex URL'si

Belirtilen sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirtilen bir eşleşen bir değere ayarlanır [normal ifade](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

**Eşleşme**/**eşleşmiyor** seçeneği URL'si sorgu Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.

- **Eşleşme**: Belirtilen normal ifadeyle eşleşen bir URL sorgu dizesini içeren isteği gerektirir.
- **Eşleşmeyen**: Belirtilen normal ifadeyle eşleşmez bir URL sorgu dizesini içeren isteği gerektirir.

Anahtar bilgileri:

- Yalnızca tam eşleşme belirtilen normal ifade için bu eşleşme koşulu karşılaması.
    
- Kullanım **yoksay çalışması** sorgu dize karşılaştırmaları duyarlılığını denetleme seçeneği.
    
- Bu seçeneğin amacı doğrultusunda, sorgu dizesi için soru işareti (?) sınırlandırıcıdan sonra gelen ilk karakter ile bir sorgu dizesi başlatır.
    
- URL kodlaması belirli karakter gerektirir. Kullanım yüzde simgesi URL şu karakterleri kodlamak:

   Karakter | URL kodlaması | Value
   ----------|--------------|------
   Uzay     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`20
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`25
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Yüzde sembolleri atlanması gereken unutmayın.

- Çift kaçış özel normal ifade karakterleri (örneğin, \^$. +) normal ifadede ters eğik çizgi dahil etmek için.

   Örneğin:

   Value | Yorumlanan 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Hangi önbellek ayarları izlenen şekilde nedeniyle bu eşleşme koşulu aşağıdaki özellikleri ile uyumsuzdur:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Maksimum yaş
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-wildcard"></a>URL sorgu joker karakter

Belirtilen değerleri isteğin sorgu dizesi karşı karşılaştırır.

**Eşleşme**/**eşleşmiyor** seçeneği URL'si sorgu joker karakterin altında koşul eşleşen Koşullar karşılanıyorsa belirler.

- **Eşleşme**: Belirtilen bir joker karakter değeri ile eşleşen bir URL sorgu dizesini içeren isteği gerektirir.
- **Eşleşmeyen**: Belirtilen bir joker karakter değeri eşleşmiyor bir URL sorgu dizesini içeren isteği gerektirir.

Anahtar bilgileri:

- Bu seçeneğin amacı doğrultusunda, sorgu dizesi için soru işareti (?) sınırlandırıcıdan sonra gelen ilk karakter ile bir sorgu dizesi başlatır.
- Parametre değerlerini içerebilir [değerleri joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
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

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Content Delivery Network genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kural altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-verizon-premium-rules-engine.md)
