---
title: Azure CDN kurallar altyapısı Verizon özgü HTTP üstbilgilerini | Microsoft Docs
description: Bu makalede, Azure CDN kuralları altyapısıyla Verizon özgü HTTP üstbilgileri kullanmayı açıklar.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: v-deasim
ms.openlocfilehash: 1a4bb48efe2a91c85b773341bb38b0f3ce4c7d9b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı Verizon özgü HTTP üst bilgileri

İçin **verizon'dan Azure CDN Premium** ürünler, bir HTTP isteğinin zaman, kaynak sunucuya bulunma noktası (POP) sunucusu ekleyebilirsiniz bir veya daha fazla ayrılmış üstbilgileri (veya proxy özel üstbilgi) istemci istek POP'a gönderilir. Bu üstbilgileri alınan üstbilgileri iletme standart ek olarak mevcuttur. Standart istek üstbilgileri hakkında daha fazla bilgi için bkz: [isteği alanları](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Bu ayrılmış üstbilgileri birini (içerik teslim ağı) Azure CDN POP istekte kaynak sunucuya eklenmesini önlemek istiyorsanız, bir kural oluşturmanız gerekir [Proxy özel üstbilgi özelliği](cdn-rules-engine-reference-features.md#proxy-special-headers) kurallar altyapısı. Bu kuralda üstbilgileri üstbilgi alanında varsayılan listesinden kaldırmak istediğiniz üstbilgi dışlayın. Etkinleştirdiyseniz [önbellek yanıt üstbilgilerini hata ayıklama özelliği](cdn-rules-engine-reference-features.md#debug-cache-response-headers), gerekli eklediğinizden emin olun `X-EC-Debug` üstbilgileri. 

Örneğin, kaldırmak için `Via` üstbilgisi, kural üstbilgi alan üstbilgilerinin aşağıdaki listede içermelidir: *X-iletilen-için X-iletilen-Proto, X-ana bilgisayar, X-Midgress, X-Gateway-liste X EC adı, ana bilgisayar*. 

![Proxy özel üstbilgi kuralı](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

Aşağıdaki tabloda, istekteki Verizon CDN POP tarafından eklenen üstbilgileri açıklanmaktadır:

İstek üstbilgisi | Açıklama | Örnek
---------------|-------------|--------
[aracılığıyla](#via-request-header) | Bu yönlendirilirken POP sunucu tanımlayan bir kaynak sunucu isteği. | HTTP/1.1 ECS (dca/1A2B)
X-iletilen-için | Sahibinin IP adresini gösterir.| 10.10.10.10
X iletilen Proto | İsteğin protokolü gösterir. | http
X-ana bilgisayar | İsteğin ana bilgisayar adını belirtir. | CDN.mydomain.com
X-Midgress | İstek ek bir CDN Sunucu yönlendirilirken olup olmadığını gösterir. Örneğin, POP Sunucusu kaynağı kalkan sunucu veya bir POP sunucu ADN ağ geçidi sunucusu. <br />Yalnızca midgress trafiği gerçekleştiğinde bu başlığı isteğine eklenir. Bu durumda, üstbilgi isteği ek bir CDN Sunucu yönlendirilirken olduğunu belirtmek için 1 olarak ayarlanır.| 1
[Ana Bilgisayar](#host-request-header) | Ana bilgisayar ve istenen içeriğin bulunduğu bağlantı noktasını tanımlar. | Marketing.mydomain.com:80
[X-Gateway-liste](#x-gateway-list-request-header) | ADN: Müşteri kaynak için atanmış ADN Ağ Geçidi sunucuları yük devretme listesini tanımlar. <br />Kaynak kalkan: Müşteri kaynak için atanmış kaynak kalkan sunucuları kümesini gösterir. | `icn1,hhp1,hnd1`
X-EC -_&lt;adı&gt;_ | İle başlayan istek üstbilgileri *X EC* (örneğin, X-EC-etiketi, [X EC Debug](cdn-http-debug-headers.md)) CDN tarafından kullanılmak üzere ayrılmıştır.| waf üretim

## <a name="via-request-header"></a>İstek üstbilgisi
İçinden biçimi `Via` üstbilgisi tanımlar POP sunucu isteği aşağıdaki sözdizimi tarafından belirtilen:

`Via: Protocol from Platform (POP/ID)` 

Sözdiziminde kullanılan terimler şu şekilde tanımlanır:
- Protokol: istek proxy ile kullanılan Protokolü (örneğin, HTTP/1.1) sürümü gösterir. 

- Platform: içeriği istendi platform gösterir. Aşağıdaki kodları Bu alan için geçerlidir: 
    Kod | Platform
    -----|---------
    ECAcc | HTTP büyük
    ECS   | HTTP küçük
    ECD   | Uygulama teslim ağı (ADN)

- POP: Gösterir [POP](cdn-pop-abbreviations.md) istek işlenmiş. 

- Kimliği: yalnızca iç kullanım için.

### <a name="example-via-request-header"></a>Örnek aracılığıyla istek üstbilgisi

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Ana bilgisayar istek üstbilgisi
POP sunucuları üzerine yazar `Host` aşağıdaki koşulların her ikisi de doğruysa, üstbilgi:
- İstenen içerik için bir müşteri kaynak sunucu kaynağıdır.
- Karşılık gelen müşteri kaynağın HTTP ana bilgisayar üstbilgisi seçeneği boş değil.

`Host` İstek üstbilgisi, HTTP ana bilgisayar üstbilgisi seçeneğinde tanımlanan değer yansıtacak şekilde yazılır.
Müşteri kaynağın HTTP ana bilgisayar üstbilgisi seçeneği boş, ardından ayarlarsanız `Host` istek sahibi tarafından gönderilen istek üstbilgisi Müşteri'nin kaynak sunucuya iletilir.

## <a name="x-gateway-list-request-header"></a>X ağ geçidi listesi istek üstbilgisi
POP sunucu ekleme/kılacak ' aşağıdaki koşullardan biri karşılandığında X ağ geçidi listesi istek üstbilgisi:
- İstek ADN platform işaret eder.
- İstek kaynak kalkan özelliği tarafından korunan bir müşteri kaynak sunucuya iletilir.

