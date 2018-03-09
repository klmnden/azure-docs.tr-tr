---
title: "Azure CDN kurallar altyapısı eşleşme koşullar | Microsoft Docs"
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
ms.openlocfilehash: e4b7113f27e5e15d69dfdd1efd13e255ef4a8ab7
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Azure CDN kurallar altyapısı eşleşen koşulları 
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
[Cookie Parameter](#cookie-parameter) | Belirtilen değer için her istek ile ilişkili tanımlama bilgilerini denetler.
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
[URL yolu dizini](#url-path-directory) | İstekleri kendi göreli yoluna göre tanımlar.
[URL yolu genişletme](#url-path-extension) | İstekleri, dosya adı uzantılarına göre tanımlar.
[URL yolu dosya](#url-path-filename) | İstekleri, dosya adına göre tanımlar.
[URL yolu değişmez değeri](#url-path-literal) | Bir isteğin göreli yol belirtilen değerle karşılaştırır.
[URL yolu Regex](#url-path-regex) | Bir isteğin göreli yolu için belirtilen normal ifade karşılaştırır.
[URL Path Wildcard](#url-path-wildcard) | Bir isteğin göreli yolu için belirtilen deseni karşılaştırır.
[URL sorgu değişmez değeri](#url-query-literal) | Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.
[URL sorgu parametresi](#url-query-parameter) | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
[URL sorgu Regex](#url-query-regex) | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen sorgu dizesi parametresi içeren istekleri tanımlar.
[URL Query Wildcard](#url-query-wildcard) | Belirtilen isteğin sorgu dizesi değerleri karşılaştırır.


## <a name="reference-for-rules-engine-match-conditions"></a>Kurallar altyapısı eşleşme koşulları için başvurusu

---
### <a name="always"></a>Her Zaman

Her zaman eşleştirme koşulu varsayılan bir özellik için tüm istekleri geçerlidir.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>Sayı olarak 
AS numarası ağ kendi Otonom sistem numarası (ASN) tarafından tanımlanır. 

**Eşleşmeleri**/**eşleşmiyor** seçenek, AS numarası altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşen**: istemci ağı ASN belirtilen Asn'ler biriyle eşleşen gerektirir. 
- **Eşleşmiyor mu**: istemci ağı ASN belirtilen Asn'ler hiçbirini eşleşmediğini gerektirir.

Anahtar bilgileri:
- Birden çok Asn'ler her biri tek bir boşluk ile sınırlandırma tarafından belirtin. Örneğin, istekleri 64514 64515 eşleşen 64514 veya 64515 ulaşır.
- Belirli isteklerini geçerli bir ASN döndürmeyebilir. Bir soru işareti (?), geçerli bir ASN belirlenemedi istekleri ile eşleşir.
- İstenen ağı için tüm ASN belirtin. Kısmi değerler eşleşen değil.
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
- CDN depolama içerikten istendi.
- İstek URI'si bu eşleşme koşulundaki içerik erişim noktası (örneğin, /000001) türünü kullanır:
  - CDN URL'si: İstek URI'si seçili içerik erişimi nokta içermesi gerekir.
  - Edge CNAME URL'si: Karşılık gelen sınır CNAME yapılandırması seçili içerik erişim noktasına işaret etmelidir.
  
Anahtar bilgileri:
 - İçerik erişim noktası istenen içerik kullanılmalıdır hizmeti tanımlar.
 - Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanmayın. Örneğin, bir müşteri kaynak eşleşme koşul CDN kaynak eşleşme koşul birleştirme hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki CDN kaynak eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>İstemci IP Adresi
**Eşleşmeleri**/**eşleşmiyor** seçenek, istemci IP adresi altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşen**: istemcinin IP adresi belirtilen IP adresleri biriyle eşleşen gerektirir. 
- **Eşleşmiyor mu**: istemcinin IP adresi belirtilen IP adreslerinden herhangi birini eşleşmediğini gerektirir. 

Anahtar bilgileri:
- CIDR gösterimini kullanın.
- Birden çok IP adresi ve/veya IP adres blokları her biri tek bir boşluk ile sınırlandırma tarafından belirtin. Örneğin:
  - **IPv4 örnek**: 1.2.3.4 10.20.30.40 ya da adresinden 1.2.3.4 veya 10.20.30.40 gelen tüm istekler eşleşir.
  - **IPv6 örnek**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 adresi 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80 gelen tüm istekler eşleşir.
- IP adres bloğu sözdizimi, ardından bir eğik ve önek boyutu temel IP adresidir. Örneğin:
  - **IPv4 örnek**: 5.5.5.64/26 5.5.5.64 5.5.5.127 aracılığıyla adreslerinden gelen tüm istekler eşleşir.
  - **IPv6 örnek**: 1:2:3: / 48 adresleri 1:2:3:0:0:0:0:0 1:2:3:ffff:ffff:ffff:ffff:ffff aracılığıyla öğesinden gelen istekleri ile eşleşir.
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
**Eşleşmeleri**/**eşleşmiyor** seçenek, tanımlama bilgisi parametresi altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: Bu eşleşme koşulundaki değerleri en az biriyle eşleşen bir değeri ile belirtilen tanımlama bilgisi içeren bir istek gerektirir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değerini bu eşleşme koşulundaki değerlerden herhangi birini eşleşmiyor.
  
Anahtar bilgileri:
- Tanımlama bilgisi adı: 
  - Tanımlama bilgisi adı belirtirken joker karakter değerleri, yıldız işareti (*) de dahil olmak üzere desteklenmediğinden, yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygundur.
  - Bu eşleme koşul örneği başına yalnızca bir tek tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
- Tanımlama bilgisi değeri: 
  - Her biri tek bir boşluk ile sınırlandırma tarafından birden çok tanımlama bilgisi değerlerini belirtin.
  - Bir tanımlama bilgisi değerini yararlanabilir [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values). 
  - Bir joker karakter değeri belirtilmediği takdirde, yalnızca tam bir eşleşme bu eşleşme koşul karşılayacaktır. Örneğin, "Değeri" belirtme "Değeri" ancak "Value1" veya "Value2." ile eşleşir
  - Kullanım **yoksay durumda** büyük küçük harfe duyarlı karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılır olup olmadığını denetlemek için seçenek.
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
Bir tanımlama bilgisi adı ve değeri tanımlama bilgisi parametresi Regex eşleşme koşulu tanımlar. Kullanabileceğiniz [normal ifadeler](cdn-rules-engine-reference.md#regular-expressions) istenen tanımlama bilgisi değerini tanımlamak için. 

**Eşleşmeleri**/**eşleşmiyor** seçenek, tanımlama bilgisi parametresi Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: bir istek belirtilen tanımlama bilgisi belirtilen normal ifadeyle eşleşen bir değeri ile içermesini gerektirir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen tanımlama bilgisi içermiyor.
  - Belirtilen tanımlama bilgisi içeriyor, ancak değerini belirtilen normal ifade eşleşmiyor.
  
Anahtar bilgileri:
- Tanımlama bilgisi adı: 
  - Tanımlama bilgisi adı belirtirken normal ifadeler ve joker karakter değerleri, yıldız işareti (*) de dahil olmak üzere desteklenmediğinden, yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygundur.
  - Bu eşleme koşul örneği başına yalnızca bir tek tanımlama bilgisi adı belirtilebilir.
  - Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
- Tanımlama bilgisi değeri: 
  - Bir tanımlama bilgisi değer normal ifadeler yararlanabilir.
  - Kullanım **yoksay durumda** büyük küçük harfe duyarlı karşılaştırma isteğin tanımlama bilgisi değerini karşı yapılır olup olmadığını denetlemek için seçenek.
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
Ülke kodu aracılığıyla bir ülke belirtebilirsiniz. 

**Eşleşmeleri**/**eşleşmiyor** seçenek, ülke altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: belirlenen ülke kodu değerlerini içeren istek gerektirir. 
- **Eşleşmiyor**: İstek belirlenen ülke kodu değerleri içermiyor gerektirir.

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

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Ülke filtreleme kuralları altyapısını kullanarak uygulama
Bu eşleme koşul özelleştirmeleri bir isteğin kaynaklandığı konumuna göre çok sayıda gerçekleştirmenizi sağlar. Örneğin, aşağıdaki yapılandırma ile ülke filtreleme özelliği davranışını çoğaltılabilir:

- URL yolu joker karakter eşleştirme: ayarlamak [URL yolu joker karakter eşleşmesi koşulu](#url-path-wildcard) güvenli hale dizinine. 
    Bu kural tarafından tüm alt erişimi kısıtlanmış emin olmak için göreli yol sonuna bir yıldız işareti ekleyin.

- Ülke eşleştir: ülkelerde istenen kümesine ülke eşleşme koşulu belirtin.
   - İzin ver: Ayarlamak ülke eşleşen koşulu **eşleşmiyor** URL yolu joker karakter eşleştirme koşul tarafından tanımlanan konumda depolanan içeriği yalnızca belirtilen ülkelerde erişmesine izin vermek için.
   - Engelle: Ayarlamak ülke eşleşen koşulu **eşleşmeleri** belirtilen ülkelerin URL yolu joker karakter eşleştirme koşul tarafından tanımlanan konumunda depolanan içeriğe erişmesini engellemek için.

- Reddetme erişim (403) özelliği: Etkinleştirmek [erişimi engelle (403) özellik](cdn-rules-engine-reference-features.md#deny-access-403) ülke filtreleme özelliği izin verilenler veya Engellenenler kısmı çoğaltmak için.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Müşteri kaynağı

Anahtar bilgileri: 
- Müşteri kaynak eşleşme koşulu olup içerik CDN URL'sine veya seçili müşteri kaynağa işaret eden bir CNAME URL kenar aracılığıyla istenmeden bağımsız olarak karşılanır.
- Müşteri başlangıç sayfasından kuralı tarafından başvurulan bir müşteri kaynağı yapılandırması silinemiyor. Müşteri kaynak yapılandırması silmeye çalışmadan önce aşağıdaki yapılandırmalar, başvurmadığından emin olun:
  - Bir müşteri kaynak eşleşme koşulu
  - Bir sınır CNAME yapılandırma
- Bir ve IF deyimi, belirli bir eşleşme koşulu birleştirmek için kullanmayın. Örneğin, bir müşteri kaynak eşleşme koşul CDN kaynak eşleşme koşulu ile birleştirerek hiçbir zaman eşleşen bir eşleşme deseni oluşturursunuz. Bu nedenle, iki müşteri kaynak eşleşme koşul ve IF deyimi aracılığıyla birleştirilemez.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Cihaz

Cihaz eşleşme koşulu, bir mobil aygıttan kendi özelliğe göre yapılan istekleri tanımlar. Mobil cihaz algılama aracılığıyla gerçekleştirilir [WURFL](http://wurfl.sourceforge.net/). 

**Eşleşmeleri**/**eşleşmiyor** seçenek, cihaz altında koşul eşleşen Koşullar karşılanıyorsa belirler:
- **Eşleşme**: belirtilen değerle eşleşecek şekilde sahibinin aygıt gerektirir. 
- **Eşleşmiyor mu**: sahibinin aygıt belirtilen değerle eşleşmiyor gerektirir.

Anahtar bilgileri:

- Kullanım **yoksay durumda** seçeneği belirtilen değeri büyük küçük harfe duyarlı olup olmadığını belirtin.
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

#### <a name="string-type"></a>Dize türü
Bir WURFL özelliği, genellikle rakam, harf ve sembol herhangi bir bileşimini kabul eder. Bu özellik esnek yapısı nedeniyle, bu eşleşme koşulu ile ilişkili değer nasıl yorumlanacağını seçmeniz gerekir. Aşağıdaki tabloda kullanılabilir seçenekler açıklar:

Tür     | Açıklama
---------|------------
Değişmez değer  | Çoğu karakter kullanarak özel bir anlamı almayı önlemek için bu seçeneği belirleyin, [değişmez değer](cdn-rules-engine-reference.md#literal-values).
Joker karakter | Tüm [joker karakterler] yararlanmak için bu seçeneği belirleyin ([joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values).
Regex    | Kullanmak için bu seçeneği belirleyin [normal ifadeler](cdn-rules-engine-reference.md#regular-expressions). Normal ifadeleri desen karakter tanımlamak için faydalıdır.

#### <a name="wurfl-capabilities"></a>WURFL özellikleri
Bir WURFL özelliği mobil aygıtları açıklayan bir kategoriye başvuruyor. Seçilen yetenek istekleri tanımlamak için kullanılan mobil aygıt açıklaması türünü belirler.

Aşağıdaki tabloda WURFL yetenekleri ve onların değişkenleri kurallar altyapısı için listeler.
<br>
> [!NOTE] 
> Aşağıdaki değişkenleri desteklenir **değiştirmek istemci isteği üstbilgisi** ve **değiştirme istemci yanıt üstbilgisi** özellikleri.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Marka adı | %{wurfl_cap_brand_name} | Aygıtın marka adını belirten bir dize. | Samsung
Aygıt işletim sistemi | %{wurfl_cap_device_os} | Cihazda yüklü olan işletim sisteminin gösteren bir dize. | IOS
Cihaz İşletim Sistemi Sürümü | %{wurfl_cap_device_os_version} | Cihazda yüklü olan işletim sisteminin sürüm numarasını gösteren bir dize. | 1.0.1
Çift yönü | %{wurfl_cap_dual_orientation} | Cihaz çift orientation destekleyip desteklemediğini belirten bir Boole değeri. | true
HTML DTD tercih edilen | %{wurfl_cap_html_preferred_dtd} | HTML içeriği için mobil cihazın tercih edilen belge türü tanımı (DTD) gösteren bir dize. | yok<br/>xhtml_basic<br/>html5
Satır içi kullanım görüntüsü | %{wurfl_cap_image_inlining} | Cihaz Base64 destekleyip desteklemediğini belirten bir Boole değeri görüntüleri kodlanmış. | false
Android olduğu | %{wurfl_vcap_is_android} | Cihaz Android işletim sistemi kullanıp kullanmadığını belirten bir Boole değeri. | true
İOS | %{wurfl_vcap_is_ios} | Cihaz iOS kullanıp kullanmadığını belirten bir Boole değeri. | false
Akıllı TV | %{wurfl_cap_is_smarttv} | Cihaz akıllı TV olup olmadığını belirten bir Boole değeri. | false
Smartphone olduğu | %{wurfl_vcap_is_smartphone} | Cihaz akıllı olup olmadığını belirten bir Boole değeri. | true
Tablet olduğu | %{wurfl_cap_is_tablet} | Cihaz tablet olup olmadığını belirten bir Boole değeri. Bu açıklama OS bağımsızdır. | true
Kablosuz aygıt | %{wurfl_cap_is_wireless_device} | Cihazın kablosuz aygıt olarak kabul edilip edilmediğini belirten bir Boole değeri. | true
Pazarlama adı | %{wurfl_cap_marketing_name} | Cihazın pazarlama adı belirten bir dize. | BlackBerry 8100 Pearl
Mobil tarayıcı | %{wurfl_cap_mobile_browser} | İçerik aygıttan istemek için kullanılan tarayıcı gösteren bir dize. | Chrome
Mobil tarayıcı sürümü | %{wurfl_cap_mobile_browser_version} | İçerik aygıttan istemek için kullanılan tarayıcı sürümünü gösteren bir dize. | 31
Model adı | %{wurfl_cap_model_name} | Cihazın model adını belirten dize. | s3
Aşamalı indirme | %{wurfl_cap_progressive_download} | Hala yüklenirken cihazın ses ve video oynatma destekleyip desteklemediğini belirten bir Boole değeri. | true
Yayınlanma Tarihi | %{wurfl_cap_release_date} | Yıl ve ay aygıtı WURFL veritabanına eklendi gösteren bir dize.<br/><br/>Biçimi: `yyyy_mm` | 2013_december
Çözümleme yüksekliği | %{wurfl_cap_resolution_height} | Cihazın yüksekliğini piksel cinsinden belirten bir tamsayı. | 768
Çözümleme genişliği | %{wurfl_cap_resolution_width} | Cihazın genişliğini piksel cinsinden belirten bir tamsayı. | 1024

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Edge Cname
Anahtar bilgileri: 
- Kullanılabilir kenar CNAME'ler listesi kurallar altyapısı yapılandırılmakta olan bir platform için sınır CNAME'ler sayfasında yapılandırılan bu sınır CNAME'ler sınırlıdır.
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
Ana bilgisayar adı üzerinden içerik başvuran etki alanı koşulun yerine getirilip getirilmediğini belirler istenen başvuran ile ilişkilendirilmiş. 

**Eşleşmeleri**/**eşleşmiyor** seçeneğini belirler başvuran etki alanı altında koşul eşleşen Koşullar karşılanıyorsa:
- **Eşleşme**: Belirtilen değerlerle eşleşecek şekilde başvuran ana bilgisayar adı gerektirir. 
- **Eşleşmiyor mu**: başvuran ana bilgisayar adı belirtilen değerle eşleşmiyor gerektirir.

Anahtar bilgileri:
- Her biri tek bir boşluk ile sınırlandırma tarafından birden çok ana bilgisayar adı belirtin.
- Bu eşleşme koşul destekleyen [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values).
- Belirtilen değer bir yıldız işareti içermiyorsa başvuran'ın ana bilgisayar adı için tam bir eşleşme olmalıdır. Örneğin, "etkialanım.com" belirtme "www.mydomain.com." eşleşmez
- Kullanım **yoksay durumda** büyük küçük harfe duyarlı karşılaştırma yapılan olup olmadığını denetlemek için seçenek.
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
**Eşleşmeleri**/**eşleşmiyor** seçeneği altında istek üstbilgisi değişmez değer eşleşen koşulu Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen üstbilgi içerecek şekilde istek gerektirir. Değeri bu eşleşme koşulunda tanımlanan adla aynı olmalıdır.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini bu eşleşme koşulundaki bir eşleşmiyor.
  
Anahtar bilgileri:
- Üstbilgi adı karşılaştırmaları her zaman büyük/küçük harf duyarlıdır. Kullanım **yoksay durumda** üstbilgi değer karşılaştırmaları duyarlılık denetlemek için seçeneği.
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
**Eşleşmeleri**/**eşleşmiyor** seçenek, istek üstbilgisi Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen üstbilgi içerecek şekilde istek gerektirir. Değeri tanımlı kalıbıyla eşleşmelidir belirtilen [normal ifade](cdn-rules-engine-reference.md#regular-expressions).
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini belirtilen normal ifade eşleşmiyor.

Anahtar bilgileri:
- Üstbilgi adı: 
  - Üstbilgi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
  - Üstbilgi adında boşluk "yerine % 20." 
- Üstbilgi değeri: 
  - Bir üstbilgi değeri normal ifadeler yararlanabilir.
  - Kullanım **yoksay durumda** üstbilgi değer karşılaştırmaları duyarlılık denetlemek için seçeneği.
  - Yalnızca bir üstbilgi değeri tam olarak en az belirtilen desenleri eşleştiğinde eşleşme koşul karşılanır.
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
**Eşleşmeleri**/**eşleşmiyor** seçenek, istek üstbilgisi joker altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşme**: Belirtilen üstbilgi içerecek şekilde istek gerektirir. Değeri bu eşleşme koşulundaki değerleri en az biriyle eşleşmelidir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirttiğiniz üstbilgi içermiyor.
  - Belirttiğiniz üstbilgi içeriyor, ancak değerini belirtilen değerlerden herhangi birini eşleşmiyor.
  
Anahtar bilgileri:
- Üstbilgi adı: 
  - Üstbilgi adı karşılaştırmaları büyük/küçük harfe duyarsızdır.
  - "% 20." Başlık adında boşluk değiştirilmesi gereken Bu değer, bir üstbilgi değeri alanları belirtmek için de kullanabilirsiniz.
- Üstbilgi değeri: 
  - Bir üstbilgi değeri yararlanabilir [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values).
  - Kullanım **yoksay durumda** üstbilgi değer karşılaştırmaları duyarlılık denetlemek için seçeneği.
  - Bir üstbilgi değeri tam olarak belirtilen desenleri en az birine eşleşen olduğunda bu eşleşme koşul karşılanır.
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
Yalnızca seçili istek yöntemle varlıklar istendiğinde istek yöntemi eşleşme koşul karşılanır. Kullanılabilir istek yöntemler şunlardır:
- AL
- HEAD 
- YAYINLA 
- SEÇENEKLER 
- PUT 
- DELETE 
- TRACE 
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
Yalnızca seçili protokolüyle varlıklar istendiğinde istek düzenini Eşleştir koşul karşılanır. Kullanılabilir protokoller şunlardır: 
- HTTP
- HTTPS

Anahtar bilgileri:
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
  - Önbellek dolgu tamamlayın
  - Varsayılan iç Max-Age
  - İç Max-Age zorla
  - Kaynak No-Cache yoksay
  - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-directory"></a>URL yolu dizini
Bir istek, istenen varlık dosya adını hariç, göreli yol tanımlar.

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL yolu dizini altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: isteği belirtilen URL deseni ile eşleşen dosya adı hariç göreli bir URL yolu içermesi gerekir.
- **Eşleşmiyor mu**: isteği belirtilen URL deseni eşleşmiyor dosya adı, hariç göreli bir URL yolu içermesi gerekir.

Anahtar bilgileri:
- Kullanım **için göreli** URL karşılaştırma içerik erişim noktasından önce veya sonra başlayıp başlamadığını belirlemek için seçeneği. Verizon CDN ana bilgisayar adı ve göreli yolu (örneğin, /800001/CustomerOrigin) istenen varlık arasında görünen yol kısmı içerik erişimi noktasıdır. Sunucu türü (örneğin, CDN veya müşteri kaynağını) tarafından bir konum ve müşteri hesabı numaranız tanımlar.

   Aşağıdaki değerler için kullanılabilir **için göreli** seçeneği:
   - **Kök**: URL karşılaştırma noktası doğrudan CDN hostname sonra başlayacağını belirtir. 

     Örneğin: http:\//wpc.0001.&lt; etki alanı&gt;/**800001/myorigin/Klasörüm'ün**/index.htm

   - **Kaynak**: URL karşılaştırma noktası içerik erişim noktası (örneğin, /000001 veya/800001/myorigin) sonra başlayacağını belirtir. Çünkü \*. azureedge.net CNAME oluşturulur Verizon CDN ana bilgisayar adı kaynak dizine göre varsayılan olarak, Azure CDN kullanıcıların kullanması gereken **kaynak** değeri. 

     Örneğin: https:\//&lt;endpoint&gt;.azureedge.net/**Klasörüm'ün**/index.htm 

     Bu URL aşağıdaki Verizon CDN ana bilgisayar adı işaret eder: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/myorigin/**Klasörüm'ün**/index.htm

- CNAME URL kenar URL karşılaştırma önce CDN URL'ye yeniden yazılmıştır.

    Örneğin, aşağıdaki URL'ler her ikisi de aynı varlık noktası ve bu nedenle aynı URL yolu sahiptir.
    - CDN URL'si: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/CustomerOrigin/path/asset.htm
    
    - Edge CNAME URL'si: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Ek bilgiler:
    - Özel etki alanı: https:\//my.domain.com/path/asset.htm
    
    - (Kök) göreli URL yolu: / 800001/CustomerOrigin/yol /
    
    - (Kaynak) göreli URL yolu: /path/

- URL karşılaştırma bitmeden yalnızca İstenen varlık dosya adı için kullanılan URL'nin kısmı. Sondaki eğik bu tür bir yolu son karakterdir.
    
- URL yolu deseni boşluklar "yerine % 20."
    
- Her URL yolu deseni her yıldız bir veya daha fazla karakter dizisi eşleştiği bir veya daha fazla yıldız işareti (*) içerebilir.
    
- Her biri tek bir boşluk ile sınırlandırma tarafından düzende birden fazla URL yolu belirtin.

    Örneğin: * /sales/ * /marketing/

- Bir URL yolu belirtimi yararlanabilir [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values).

- Kullanım **yoksay durumda** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-extension"></a>URL yolu genişletme
İstekleri istenen varlık dosya uzantısıyla tanımlar.

**Eşleşmeleri**/**eşleşmiyor** seçeneği altında URL yolu uzantısı ile eşleşen koşulu Koşullar karşılanıyorsa belirler.
- **Eşleşen**: tam olarak belirtilen desenle eşleşen bir dosya uzantısı içerecek şekilde istek URL'si gereklidir.

   "Htm" belirtirseniz, örneğin, "htm" varlıklar, değil "html" varlıklar eşleştirilir.  

- **Eşleşmiyor mu**: belirtilen desenle eşleşmiyor bir dosya uzantısı içerecek şekilde bir URL isteği gerektirir.

Anahtar bilgileri:
- İçinde eşleştirilecek dosya uzantılarını belirtmek **değeri** kutusu. Başında nokta içermez; Örneğin, htm .htm yerine kullanın.

- Kullanım **yoksay durumda** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

- Birden çok dosya uzantıları, tek bir alana sahip her bir uzantı sınırlandırma tarafından belirtin. 

    Örneğin: htm html

- Örneğin, "htm" belirtme "htm" varlıklar, ancak değil "html" varlıklar eşleşir.


#### <a name="sample-scenario"></a>Örnek senaryo

Aşağıdaki örnek yapılandırma, bir istek belirtilen uzantılara biriyle eşleştiğinde bu eşleşme koşul karşılanır varsayar.

Değer belirtimi: asp aspx php html

Aşağıdaki uzantılarına sahip end URL'leri bulduğunda bu eşleşme koşul karşılanır:
- .asp
- .aspx
- .php
- .html

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-filename"></a>URL yolu dosya
İstekleri istenen varlık dosya adıyla tanımlar. Bu eşleşme koşul amaçları doğrultusunda, istenen varlık, bir süre ve dosya uzantısını (örneğin, index.html) adı bir dosya adı oluşur.

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL yolu dosya altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: İstek URL'si yolundaki belirtilen desenle eşleşen bir dosya adı içermesi gerekiyor.
- **Eşleşmiyor mu**: bir dosya adı belirtilen desenle eşleşen değil, URL yolunu içeren isteği gerektirir.

Anahtar bilgileri:
- Kullanım **yoksay durumda** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.

- Birden çok dosya uzantılarını belirtmek için her bir uzantı tek bir boşlukla ayırın.

    Örneğin: index.htm index.html

- Bir dosya adı değeri alanları "yerine % 20."
    
- Bir dosya adı değeri yararlanabilir [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values). Örneğin, her dosya adı deseni bir veya daha fazla yıldız işareti (*), her yıldız bir veya daha fazla karakter dizisi eşleştiği oluşabilir.
    
- Joker karakterler belirtilmezse, yalnızca tam bir eşleşme bu eşleşme koşul karşılayacaktır.

    Örneğin, "sunusu.ppt" belirtme "sunusu.ppt" ancak olmayan bir adlandırılmış "presentation.pptx." adlı bir varlık eşleşir

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-literal"></a>URL yolu değişmez değeri
Belirtilen değer için dosya adını içeren bir isteğin URL yolunu karşılaştırır.

**Eşleşmeleri**/**eşleşmiyor** seçeneği altında URL yolu değişmez değer eşleşen koşulu Koşullar karşılanıyorsa belirler.
- **Eşleşen**: isteği belirtilen desenle eşleşen bir URL yolu içermesi gerekir.
- **Eşleşmiyor mu**: belirtilen desenle eşleşmiyor bir URL yolu içerecek şekilde istek gerektirir.

Anahtar bilgileri:
- Kullanım **için göreli** URL karşılaştırma noktası içerik erişim noktasından önce veya sonra başlayan olup olmadığını belirtmek için seçeneği. 

    Aşağıdaki değerler için kullanılabilir **için göreli** seçeneği:
     - **Kök**: URL karşılaştırma noktası doğrudan CDN hostname sonra başlayacağını belirtir.

       Örneğin: http:\//wpc.0001.&lt; Etki alanı&gt;/**800001/myorigin/myfolder/index.htm**

     - **Kaynak**: URL karşılaştırma noktası içerik erişim noktası (örneğin, /000001 veya/800001/myorigin) sonra başlayacağını belirtir. Çünkü \*. azureedge.net CNAME oluşturulur Verizon CDN ana bilgisayar adı kaynak dizine göre varsayılan olarak, Azure CDN kullanıcıların kullanması gereken **kaynak** değeri. 

       Örneğin: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Bu URL aşağıdaki Verizon CDN ana bilgisayar adı işaret eder: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/myorigin/**myfolder/index.htm**

- Bir URL karşılaştırma önce CDN URL'sine bir kenar CNAME URL yeniden yazılmıştır.

   Örneğin, aşağıdaki URL'ler her ikisi de aynı varlık noktası ve bu nedenle aynı URL yolu sahiptir:
    - CDN URL'si: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/CustomerOrigin/path/asset.htm
    - Edge CNAME URL'si: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

   Ek bilgiler:
    
    - (Kök) göreli URL yolu: /800001/CustomerOrigin/path/asset.htm
   
    - (Kaynak) göreli URL yolu: /path/asset.htm

- URL'deki sorgu dizelerini göz ardı edilir.
- Kullanım **yoksay durumda** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
- Bu eşleme koşul için belirtilen değer göreli yolu, istemci tarafından yapılan tam istek karşı karşılaştırılır.

- Belirli bir dizin için yapılan tüm istekleri eşleşecek şekilde kullanmak [URL yolu dizini](#url-path-directory) veya [URL yolu joker](#url-path-wildcard) eşleşen koşulu.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-regex"></a>URL yolu Regex
Belirtilen bir isteğin URL yolu karşılaştırır [normal ifade](cdn-rules-engine-reference.md#regular-expressions).

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL yolu Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: isteği belirtilen normal ifadeyle eşleşen bir URL yolu içermesi gerekir.
- **Eşleşmiyor mu**: isteği belirtilen normal ifadeyle eşleşmez bir URL yolu içermesi gerekir.

Anahtar bilgileri:
- CNAME URL kenar URL karşılaştırma önce CDN URL'ye yeniden yazılmıştır. 
 
   Örneğin, her iki URL'leri aynı varlık noktası ve bu nedenle aynı URL yolu sahiptir.

     - CDN URL'si: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL'si: http:\//my.domain.com/path/asset.htm

   Ek bilgiler:
    
     - URL yolu: /800001/CustomerOrigin/path/asset.htm

- URL'deki sorgu dizelerini göz ardı edilir.
    
- Kullanım **yoksay durumda** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
    
- URL yolunu alanları "% 20." değiştirilmelidir

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-wildcard"></a>URL Path Wildcard
Bir isteğin göreli URL yolu için belirtilen joker karakter deseni karşılaştırır.

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL yolu joker altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: isteği belirtilen joker karakter deseni ile eşleşen bir URL yolu içermesi gerekir.
- **Eşleşmiyor mu**: isteği belirtilen joker karakter deseni eşleşmiyor bir URL yolu içermesi gerekir.

Anahtar bilgileri:
- **Bağıntı** seçeneği: Bu seçenek URL karşılaştırma noktası içerik erişim noktasından önce veya sonra başlayan olup olmadığını belirler.

   Bu seçenek aşağıdaki değerlere sahip olabilir:
     - **Kök**: URL karşılaştırma noktası doğrudan CDN hostname sonra başlayacağını belirtir.

       Örneğin: http:\//wpc.0001.&lt; Etki alanı&gt;/**800001/myorigin/myfolder/index.htm**

     - **Kaynak**: URL karşılaştırma noktası içerik erişim noktası (örneğin, /000001 veya/800001/myorigin) sonra başlayacağını belirtir. Çünkü \*. azureedge.net CNAME oluşturulur Verizon CDN ana bilgisayar adı kaynak dizine göre varsayılan olarak, Azure CDN kullanıcıların kullanması gereken **kaynak** değeri. 

       Örneğin: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Bu URL aşağıdaki Verizon CDN ana bilgisayar adı işaret eder: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/myorigin/**myfolder/index.htm**

- CNAME URL kenar URL karşılaştırma önce CDN URL'ye yeniden yazılmıştır.

   Örneğin, aşağıdaki URL'ler her ikisi de aynı varlık noktası ve bu nedenle aynı URL yolu sahiptir:
     - CDN URL: http://wpc.0001.&lt;Domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL'si: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

   Ek bilgiler:
    
     - (Kök) göreli URL yolu: /800001/CustomerOrigin/path/asset.htm
    
     - (Kaynak) göreli URL yolu: /path/asset.htm
    
- Her biri tek bir boşluk ile sınırlandırma tarafından birden fazla URL yolu belirtin.

   Örneğin: /marketing/asset.* /sales/*.htm

- URL'deki sorgu dizelerini göz ardı edilir.
    
- Kullanım **yoksay durumda** büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirilir olup olmadığını denetlemek için seçenek.
    
- URL yolunu alanları "yerine % 20."
    
- Bir URL yolu yararlanabilir için belirtilen değer [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values). Her URL yolu deseni her yıldız bir veya daha fazla karakter dizisi eşleştiği bir veya daha fazla yıldız işareti (*) içerebilir.

#### <a name="sample-scenarios"></a>Örnek senaryolar

Aşağıdaki tabloda örnek yapılandırması, bir istek belirtilen URL desenle eşleştiğinde bu eşleşme koşul karşılanır varsayın:

Değer                   | Göreceli olarak    | Sonuç 
------------------------|----------------|-------
*/test.html */test.php  | Kök veya kaynak | Bu deseni "örnek test.html" ya da herhangi bir klasörde "test.php" adlı varlıklar için istekleri ile eşleştirilir.
/ 80ABCD/kaynak/metin / *   | Kök           | İstenen varlık aşağıdaki ölçütleri karşıladığında bu deseni eşleştirilir: <br />-, "Kaynak" olarak adlandırılan bir müşteri kaynak üzerinde bulunmalıdır <br />-Göreli yol "metin" adlı bir klasör ile başlamalıdır Diğer bir deyişle, istenen varlık "metin" klasörü veya özyinelemeli klasörlerinden birinde bulunabilir.
*/css/* */js/*          | Kök veya kaynak | Bu desen tüm CDN veya kenarı css veya js bir klasör içeren CNAME URL'ler ile eşleştirilir.
*.jpg *.gif *.png       | Kök veya kaynak | Bu desen tüm CDN veya edge CNAME URL'lerin ilgili .jpg, .gif veya .png bitiş eşleştirilir. Bu deseni belirlemek için alternatif bir yolu [URL yolu uzantısı ile eşleşen koşulu](#url-path-extension).
/ görüntüleri / * / medya / *      | Kaynak         | Bu desen CDN veya edge CNAME, göreli yolu "görüntüleri" veya "ortam" bir klasör ile başlayan URL'ler ile eşleştirilir. <br />-CDN URL'sine: http:\//wpc.0001.&lt; Etki alanı&gt;/800001/myorigin/images/sales/event1.png<br />-Edge CNAME URL örneği: http:\//cdn.mydomain.com/images/sales/event1.png

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-literal"></a>URL sorgu değişmez değeri
Bir isteğin sorgu dizesi belirtilen değerle karşılaştırır.

**Eşleşmeleri**/**eşleşmiyor** seçeneği altında URL sorgu değişmez değer eşleşen koşulu Koşullar karşılanıyorsa belirler.
- **Eşleşen**: Belirtilen sorgu dizesiyle eşleşen bir URL sorgu dizesi içeren isteği gerektirir.
- **Eşleşmiyor mu**: Belirtilen sorgu dizesi eşleşmiyor bir URL sorgu dizesi içeren isteği gerektirir.

Anahtar bilgileri:

- Yalnızca tam sorgu dize eşleşmeleri bu eşleşme koşulu karşılıyor.
    
- Kullanım **yoksay durumda** sorgu dize karşılaştırmaları duyarlılık denetlemek için seçeneği.
    
- Önde gelen soru işareti (?) sorgu dizesi değeri metinde dahil etmeyin.
    
- URL kodlaması belirli karakterler gerektirir. Kullanım yüzdesi simgenin URL'ye şu karakterleri kodlayın:

   Karakter | URL Encoding
   ----------|---------
   Boşluk     | %20
   &         | %25

- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Max-Age
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-parameter"></a>URL sorgu parametresi
Belirtilen sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirtilen desenle eşleşen bir değere ayarlanır. Sorgu dizesi parametreleri (örneğin, parametre değeri =) URL'sini belirlemek bu koşul yerine getirilip getirilmediğini isteği. Bu eşleşme koşul adını kullanarak bir sorgu dizesi parametresi tanımlar ve bir veya daha fazla değer parametre değeri kabul eder. 

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL sorgu parametresi altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: Bu eşleşme koşulundaki değerleri en az biriyle eşleşen bir değeri ile belirtilen parametre içeren bir istek gerektirir.
- **Eşleşmiyor**: istek ya da aşağıdaki ölçütleri karşılayan gerektirir:
  - Belirtilen parametre içermiyor.
  - Belirtilen parametre içeriyor ancak değeri bu eşleşme koşulundaki değerlerden herhangi birini eşleşmiyor.

Bu eşleme koşul parametre ad/değer birleşimlerini belirtmek için kolay bir yol sağlar. Bir sorgu dizesi parametresi eşleşen daha fazla esneklik için kullanmayı [URL sorgu joker](#url-query-wildcard) eşleşen koşulu.

Anahtar bilgileri:
- Bu eşleme koşul örneği başına yalnızca bir tek URL sorgu parametresi adı belirtilebilir.
    
- Bir parametre adı belirtildiğinde joker karakter değerleri desteklenmediğinden, yalnızca tam parametre adı eşleşmeleri karşılaştırma için uygundur.
- Parametre değerlerini içerebilir [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values).
   - Her parametre değeri desen bir veya daha fazla karakter dizisi her yıldız eşleştiği bir veya daha fazla yıldız işareti (*) oluşabilir.
   - URL kodlaması belirli karakterler gerektirir. Kullanım yüzdesi simgenin URL'ye şu karakterleri kodlayın:

       Karakter | URL Encoding
       ----------|---------
       Boşluk     | %20
       &         | %25

- Her biri tek bir boşluk ile sınırlandırma tarafından birden çok sorgu dizesi parametre değerlerini belirtin. Belirtilen ad/değer birleşimlerinden biri, bir istek içerdiğinde, bu eşleşme koşul karşılanır.

   - Örnek 1:

     - Yapılandırma:

       ValueA ValueB

     - Bu yapılandırma aşağıdaki sorgu dizesi parametreleri eşleşir:

       Parameter1=ValueA
    
       Parametre1 ValueB =

   - Örnek 2:

     - Yapılandırma: 

        Value%20A Value%20B

     - Bu yapılandırma aşağıdaki sorgu dizesi parametreleri eşleşir:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Yalnızca belirtilen sorgu dizesi ad/değer birleşimleri en az biri için tam bir eşleşme olduğunda bu eşleşme koşul karşılanır.

   Örneğin, önceki örnekte yapılandırma kullanırsanız, parametre ad/değer bileşimi "parametre1 ValueAdd =" bir eşleşme olarak kabul edilir değil. Ancak, aşağıdaki değerlerden birini belirtirseniz, bu ad/değer bileşimi ile eşleşir:

   - ValueA ValueB ValueAdd
   - ValueA * ValueB

- Kullanım **yoksay durumda** sorgu dize karşılaştırmaları duyarlılık denetlemek için seçeneği.
    
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Max-Age
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

#### <a name="sample-scenarios"></a>Örnek senaryolar
Aşağıdaki örnek, bu seçenek belirli durumlarda nasıl çalıştığını gösterir:

Ad      | Değer |  Sonuç
----------|-------|--------
Kullanıcı      | Joe   | İstenen URL için sorgu dizesi olduğunda bu deseni eşleşen "? kullanıcı joe =."
Kullanıcı      | *     | İstenen URL için sorgu dizesi kullanıcı parametresini içerdiğinde bu deseni eşleştirilir.
E-posta CAN | *     | İstenen URL için sorgu dizesi "Can" ile başlayan bir e-posta parametresini içerdiğinde bu deseni eşleşir

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-regex"></a>URL sorgu Regex
Belirtilen sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirtilen bir eşleşen bir değere ayarlayın [normal ifade](cdn-rules-engine-reference.md#regular-expressions).

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL sorgu Regex altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: Belirtilen normal ifadeyle eşleşen bir URL sorgu dizesi içeren isteği gerektirir.
- **Eşleşmiyor mu**: Belirtilen normal ifadeyle eşleşmez bir URL sorgu dizesi içeren isteği gerektirir.

Anahtar bilgileri:
- Yalnızca belirtilen normal ifade için tam eşleşme bu eşleşme koşulu karşılıyor.
    
- Kullanım **yoksay durumda** sorgu dize karşılaştırmaları duyarlılık denetlemek için seçeneği.
    
- Bu seçenek amaçları doğrultusunda, sorgu dizesi için soru işareti (?) sınırlayıcı sonra ilk karakteri ile bir sorgu dizesi başlatır.
    
- URL kodlaması belirli karakterler gerektirir. Kullanım yüzdesi simgenin URL'ye şu karakterleri kodlayın:

   Karakter | URL Encoding | Değer
   ----------|--------------|------
   Boşluk     | %20          | \%20
   &         | %25          | \%25

   Yüzde simgeleri kaçış uygulanmalıdır unutmayın.

- Çift kaçış özel normal ifade karakterler (örneğin, \^$. +) normal ifadede bir ters eğik çizgi eklenecek.

   Örneğin:

   Değer | Yorumlanan 
   ------|---------------
   \\+    | +
   \\\+   | \\+

- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Max-Age
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski


[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-wildcard"></a>URL sorgu joker karakter
İsteğin sorgu dizesi karşı belirtilen değerler karşılaştırır.

**Eşleşmeleri**/**eşleşmiyor** seçenek, URL sorgu joker altında koşul eşleşen Koşullar karşılanıyorsa belirler.
- **Eşleşen**: Belirtilen joker karakter değeri ile eşleşen bir URL sorgu dizesi içeren isteği gerektirir.
- **Eşleşmiyor mu**: Belirtilen joker değerle eşleşmiyor bir URL sorgu dizesi içeren isteği gerektirir.

Anahtar bilgileri:
- Bu seçenek amaçları doğrultusunda, sorgu dizesi için soru işareti (?) sınırlayıcı sonra ilk karakteri ile bir sorgu dizesi başlatır.
- Parametre değerleri içerebilir [joker karakter değerleri](cdn-rules-engine-reference.md#wildcard-values):
   - Her parametre değeri desen bir veya daha fazla karakter dizisi her yıldız eşleştiği bir veya daha fazla yıldız işareti (*) oluşabilir.
   - URL kodlaması belirli karakterler gerektirir. Kullanım yüzdesi simgenin URL'ye şu karakterleri kodlayın:

     Karakter | URL Encoding
     ----------|---------
     Boşluk     | %20
     &         | %25

- Her biri tek bir boşluk ile sınırlandırma tarafından birden çok değer belirtin.

   Örneğin: *parametre1 ValueA =* *ValueB* *parametre1 = ValueC & parametre2 ValueD =*

- Yalnızca belirtilen sorgu dizesi desenleri en az biri için tam eşleşme bu eşleşme koşulu karşılıyor.
    
- Kullanım **yoksay durumda** sorgu dize karşılaştırmaları duyarlılık denetlemek için seçeneği.
    
- Hangi önbelleğinde ayarları izlenen şekilde nedeniyle bu eşleşme koşul aşağıdaki özelliklerle uyumlu değil:
   - Önbellek dolgu tamamlayın
   - Varsayılan iç Max-Age
   - İç Max-Age zorla
   - Kaynak No-Cache yoksay
   - İç Max-eski

#### <a name="sample-scenarios"></a>Örnek senaryolar
Aşağıdaki örnek, bu seçenek belirli durumlarda nasıl çalıştığını gösterir:

 Ad                 | Açıklama
 ---------------------|------------
user=joe              | İstenen URL için sorgu dizesi olduğunda bu deseni eşleşen "? kullanıcı joe =."
\*user=\* \*optout=\* | CDN URL sorgusu kullanıcı veya optout parametresini içerdiğinde bu deseni eşleştirilir.

[Başa dön](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure içerik teslim ağı genel bakış](cdn-overview.md)
* [Kuralları altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kurallar altyapısı koşullu ifadeler](cdn-rules-engine-reference-conditional-expressions.md)
* [Kurallar altyapısı özellikleri](cdn-rules-engine-reference-features.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)

