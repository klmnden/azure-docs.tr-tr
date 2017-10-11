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
ms.openlocfilehash: f4886b1d78dfa87cf25737fb46c12b5963034f27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Azure CDN kurallar altyapısı eşleşen koşulları
Bu konu kullanılabilir eşleşme koşullar Azure içerik teslim ağı (CDN) için ayrıntılı açıklamaları listeler [kurallar altyapısı](cdn-rules-engine.md).

İkinci bir kural eşleşen koşulu parçasıdır. Bir eşleşme koşulu, belirli türde bir özellik kümesi gerçekleştirilecek istekler tanımlar.

Örneğin, belirli bir konumdaki içerik isteklerini belirli bir IP adresi ya da ülke veya üst bilgileri tarafından oluşturulan istekler filtrelemek için kullanılabilir.

## <a name="always"></a>Her zaman

Her zaman eşleştirme koşul, varsayılan bir özellikler kümesi tüm isteklere uygulanmasını için tasarlanmıştır.

## <a name="device"></a>Cihaz

Cihaz eşleşme koşulu, bir mobil aygıttan kendi özelliğe göre yapılan istekleri tanımlar.  Mobil cihaz algılama aracılığıyla gerçekleştirilir [WURFL](http://wurfl.sourceforge.net/).  WURFL yetenekleri ve onların CDN kurallar altyapısı değişkenleri aşağıda listelenmiştir.

> [!NOTE] 
> Değişkenleri desteklenir **değiştirmek istemci isteği üstbilgisi** ve **değiştirme istemci yanıt üstbilgisi** özellikleri.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Marka adı | % {wurfl_cap_brand_name} | Aygıtın marka adını belirten bir dize. | Samsung
Aygıt işletim sistemi | % {wurfl_cap_device_os} | Cihazda yüklü olan işletim sisteminin gösteren bir dize. | İOS
Cihaz işletim sistemi sürümü | % {wurfl_cap_device_os_version} | Cihazda yüklü işletim sisteminin sürüm numarasını gösteren bir dize. | 1.0.1
Çift yönü | % {wurfl_cap_dual_orientation} | Cihaz çift orientation destekleyip desteklemediğini belirten bir Boole değeri. | TRUE
HTML DTD tercih edilen | % {wurfl_cap_html_preferred_dtd} | HTML içeriği için mobil cihazın tercih edilen belge türü tanımı (DTD) gösteren bir dize. | Yok<br/>xhtml_basic<br/>HTML5
Satır içi kullanım görüntüsü | % {wurfl_cap_image_inlining} | Cihaz Base64 destekleyip desteklemediğini belirten bir Boole değeri görüntüleri kodlanmış. | False
Android olduğu | % {wurfl_vcap_is_android} | Cihaz Android işletim sistemi kullanıp kullanmadığını belirten bir Boole değeri. | TRUE
İOS | % {wurfl_vcap_is_ios} | Cihaz iOS kullanıp kullanmadığını belirten bir Boole değeri. | False
Akıllı TV | % {wurfl_cap_is_smarttv} | Cihaz akıllı TV olup olmadığını belirten bir Boole değeri. | False
Smartphone olduğu | % {wurfl_vcap_is_smartphone} | Cihaz akıllı olup olmadığını belirten bir Boole değeri. | TRUE
Tablet olduğu | % {wurfl_cap_is_tablet} | Cihaz tablet olup olmadığını belirten bir Boole değeri. Bu bir işletim Sisteminden bağımsız açıklamasıdır. | TRUE
Kablosuz aygıt | % {wurfl_cap_is_wireless_device} | Cihazın kablosuz aygıt olarak kabul edilip edilmediğini belirten bir Boole değeri. | TRUE
Pazarlama adı | % {wurfl_cap_marketing_name} | Cihazın pazarlama adı belirten bir dize. | BlackBerry 8100 inci
Mobil tarayıcı | % {wurfl_cap_mobile_browser} | İçerik aygıttan istemek için kullanılan tarayıcı gösteren bir dize. | Chrome
Mobil tarayıcı sürümü | % {wurfl_cap_mobile_browser_version} | İçerik aygıttan istemek için kullanılan tarayıcı sürümünü belirten bir dize. | 31
Model adı | % {wurfl_cap_model_name} | Cihazın model adını belirten dize. | S3
Aşamalı indirme | % {wurfl_cap_progressive_download} | Hala yüklenirken cihazın ses/video oynatma destekleyip desteklemediğini belirten bir Boole değeri. | TRUE
Sürüm tarihi | % {wurfl_cap_release_date} | Yıl ve ay aygıtı WURFL veritabanına eklendi gösteren bir dize.<br/><br/>Biçimi:`yyyy_mm` | 2013_december
Çözümleme yüksekliği | % {wurfl_cap_resolution_height} | Cihazın yüksekliğini piksel cinsinden belirten bir tamsayı. | 768
Çözümleme genişliği | % {wurfl_cap_resolution_width} | Cihazın genişliğini piksel cinsinden belirten bir tamsayı. | 1024


## <a name="location"></a>Konum

Bu eşleme koşulları sahibinin konum temelinde istekleri belirlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
Sayı olarak | Belirli bir ağdan kaynaklanan istekleri tanımlar.
Ülke | Belirtilen ülkelerden kökenli isteklerine tanımlar.


## <a name="origin"></a>Kaynak

Koşulları tanımlamak üzere tasarlanmış bu eşleşme CDN depolama o noktaya veya müşteri kaynak sunucu istekleri.

Ad | Amaç
-----|--------
CDN kaynak | CDN depolama alanında depolanan içerik isteklerini tanımlar.
Müşteri kaynağı | Belirli bir müşteri kaynak sunucuda depolanan içerik isteklerini tanımlar.


## <a name="request"></a>İstek

Bu eşleme koşulları özelliklerine göre istekleri belirlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
İstemci IP adresi | Belirli bir IP adresinden kaynaklanan istekleri tanımlar.
Tanımlama bilgisi parametresi | Belirtilen değer için her istek ile ilişkili tanımlama bilgilerini denetler.
Tanımlama bilgisi parametresi Regex | Belirtilen normal ifade için her istek ile ilişkili tanımlama bilgilerini denetler.
Edge Cname | Belirli bir kenar CNAME noktası istekleri tanımlar.
Başvuran etki alanı | Belirtilen hostname(s) başvurulan istekleri tanımlar.
İstek üstbilgisi değişmez değeri | Belirtilen değer için ayarlanmış belirtilen üstbilgi içeren istekleri tanımlar.
İstek üstbilgisi Regex | Belirtilen normal ifadeyle eşleşen bir değere ayarlayın belirtilen üstbilgi içeren istekleri tanımlar.
İstek üstbilgisi joker karakter | Belirtilen desenle eşleşen bir değere ayarlayın belirtilen üstbilgi içeren istekleri tanımlar.
İstek yöntemi | İstekleri tarafından HTTP yöntemleri tanımlar.
İstek düzeni | İstekleri kendi HTTP protokolü tarafından tanımlar.

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

