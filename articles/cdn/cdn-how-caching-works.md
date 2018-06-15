---
title: Önbelleğe alma nasıl çalışır | Microsoft Docs
description: Önbelleğe alma verileri daha hızlı bir şekilde erişilebilir için gelecekte istekleri böylece verilerini yerel olarak depolamak işlemidir.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: v-deasim
ms.openlocfilehash: bb0824995972b49febdb1695e41f45fbd0966cd1
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33765799"
---
# <a name="how-caching-works"></a>Önbelleğe alma nasıl işler?

Bu makalede genel önbelleğe alma kavramlara genel bakış sağlar ve nasıl [Azure içerik teslim ağı (CDN)](cdn-overview.md) önbelleğe alma performansını artırmak için kullanır. CDN uç noktanız önbelleğe alma davranışını özelleştirmek için bkz: hakkında bilgi almak isterseniz [denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md) ve [denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle](cdn-query-string.md).

## <a name="introduction-to-caching"></a>Önbelleğe alma giriş

Önbelleğe alma verileri daha hızlı bir şekilde erişilebilir için gelecekte istekleri böylece verilerini yerel olarak depolamak işlemidir. Önbelleğe alma, web tarayıcısı önbelleğe alma, bir web en yaygın türü tarayıcı statik verileri kopyasını yerel bir sabit sürücüde yerel olarak depolar. Web tarayıcısı, önbelleğe alma kullanarak sunucuya birden çok gidiş dönüş yapmaktan kaçınmak ve bunun yerine zaman ve kaynak vermeyip aynı verilere yerel olarak erişim. Önbelleğe alma, statik görüntüler, CSS dosyaları ve JavaScript dosyaları gibi küçük, statik verileri yerel olarak yönetmek için çok uygundur.

Benzer şekilde, önbelleğe alma tarafından bir içerik teslim ağına kullanıcı yakın uç sunucularda kaynağa kadar seyahat ve son kullanıcı gecikme istekleri önlemek için kullanılır. Yalnızca tek bir kullanıcı için kullanılan bir web tarayıcısının önbelleğine CDN paylaşılan bir önbelleğe sahiptir. Bir paylaşılan CDN önbellekte bir kullanıcı tarafından istenen bir dosya daha sonra diğer kullanıcılar tarafından hangi kaynak sunucuya istek sayısı büyük ölçüde azaltır erişilebilir.

Dinamik kaynaklara sık sık değişen veya tek bir kullanıcıya özgü önbelleğe alınamaz. Bu türdeki kaynağı ancak, dinamik site Hızlandırma (DSA) en iyi duruma getirme için performans iyileştirmeleri Azure içerik teslim ağı üzerinde yararlanabilir.

Önbelleğe alma, kaynak sunucu ve son kullanıcı arasında birden çok düzeyde oluşabilir:

- Web sunucusu: (birden çok kullanıcı için) bir paylaşılan önbelleği kullanır.
- İçerik teslim ağı: (birden çok kullanıcı için) bir paylaşılan önbelleği kullanır.
- Internet servis sağlayıcısı (ISS): (birden çok kullanıcı için) bir paylaşılan önbelleği kullanır.
- Web tarayıcısı: özel bir önbelleği (bir kullanıcı için) kullanır.

Her önbellek genellikle kendi kaynak yenilik yönetir ve bir dosya eski olduğunda doğrulama gerçekleştirir. Bu davranış belirtimi, önbelleğe alma HTTP tanımlanan [RFC 7234](https://tools.ietf.org/html/rfc7234).

### <a name="resource-freshness"></a>Kaynak yenilik

Önbelleğe alınan kaynak potansiyel olarak güncel olmayan ya da (karşılaştırıldığında, kaynak sunucuda karşılık gelen kaynak) eski olabileceğinden, içerik yenilendiğinde tüm önbelleğe alma mekanizması denetlemek için önemlidir. Her erişildiğinde süresi ve bant genişliği tüketimini kaydetmek için önbelleğe alınan kaynak sürümü kaynak sunucusu üzerindeki karşılaştırılır değil. Bunun yerine, önbelleğe alınan kaynak yeni olarak kabul edilir sürece, en güncel sürümünü olduğu varsayılır ve doğrudan istemciye gönderilir. Önbelleğe alınan kaynak kendi yaşı yaş veya bir önbellek ayarı tarafından tanımlanan dönem az olduğunda yeni olarak kabul edilir. Örneğin, bir tarayıcı bir web sayfası yüklediğinde, önbelleğe alınan her kaynak için sabit diskinizde yeni olduğunu ve yüklerken doğrular. Kaynak yeni değilse (eski), güncel bir kopyasını sunucudan da yüklenir.

### <a name="validation"></a>Doğrulama

Bir kaynak eski olarak değerlendirilir, kaynak sunucunun istedi, yani doğrulamak, önbellekteki verileri hala kaynak sunucuda nedir eşleşip eşleşmediğini belirlemek için. Kaynak sunucuda dosya güncellendiyse önbelleğini kaynak sürümünü güncelleştirir. Kaynak yeni ise, aksi takdirde, verileri doğrudan önbellekten ilk önce doğrulamadan teslim edilir.

### <a name="cdn-caching"></a>CDN önbelleğe alma

Önbelleğe alma teslimat hızlandırmak ve görüntüleri, yazı tipleri ve videolar gibi statik varlıklar için kaynak yükü azaltmak için bir CDN çalışır şekilde tam sayı. CDN önbelleğe alma, statik kaynakları seçmeli olarak kullanıcıya daha yerel stratejik olarak yerleştirilmiş sunucularda depolanır ve aşağıdaki avantajları sağlar:

- Çoğu web trafiği (örneğin, görüntüler, yazı tipleri ve videolar) statik olduğundan, CDN önbelleğe alma taşıyarak ağ gecikmesini içerik yakın böylece veri geçen uzaklığı azaltma kullanıcıya azaltır.

- Bir CDN çalışmaya boşaltarak, önbelleğe alma ağ trafiği ve kaynak sunucu üzerindeki yükü azaltabilir. Bunun yapılması, çok sayıda kullanıcı olduğunda bile uygulaması için maliyet ve kaynak gereksinimlerini azaltır.

Önbelleğe alma nasıl uygulandığını benzeyen bir web tarayıcısında nasıl önbelleğe alma bir CDN önbellek yönergesi üstbilgileri göndererek gerçekleştirildiğini denetleyebilirsiniz. Önbellek yönergesi üstbilgileri genellikle kaynak sunucu tarafından eklenen HTTP üstbilgileri ' dir. Bu üstbilgileri çoğunu ilk olarak istemci tarayıcılarında önbelleğe alma adresi için tasarlanmış olsa da, şimdi de CDN'ler gibi tüm ara önbellekleri tarafından kullanılır. 

İki üstbilgi önbellek yenilik tanımlamak için kullanılabilir: `Cache-Control` ve `Expires`. `Cache-Control` Daha fazla geçerli olduğundan ve önceliklidir `Expires`, ikisi de varsa. Ayrıca üstbilgileri (doğrulayıcıları denir) doğrulama için kullanılan iki tür vardır: `ETag` ve `Last-Modified`. `ETag` Daha fazla geçerli olduğundan ve önceliklidir `Last-Modified`, her ikisi de tanımlanır.  

## <a name="cache-directive-headers"></a>Önbellek yönergesi üstbilgileri

> [!IMPORTANT]
> Varsayılan olarak, DSA için en iyi hale getirilmiş bir Azure CDN uç noktası önbellek yönergesi üstbilgileri yoksayar ve önbelleğe alma atlar. İçin **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri, nasıl Azure CDN uç kullanarak bu üstbilgileri değerlendirir ayarlayabilirsiniz [CDN önbelleği kurallarını](cdn-caching-rules.md)önbelleğe almayı etkinleştirmek için. İçin **verizon'dan Azure CDN Premium** yalnızca profilleri, kullandığınız [kurallar altyapısı](cdn-rules-engine.md) önbelleğe almayı etkinleştirmek için.

Azure CDN önbellek süresi ve önbellek paylaşımı tanımlayın aşağıdaki HTTP önbellek yönergesi üstbilgileri destekler.

**Cache-Control:**
- HTTP 1.1 web yayımcılar içeriklerini üzerinde daha fazla denetime ve sınırlamaları yönelik olarak sunulan `Expires` üstbilgi.
- Geçersiz kılmaları `Expires` başlığı, her iki onu ve `Cache-Control` tanımlanır.
- İstemci bir HTTP isteği CDN POP kullanıldığında `Cache-Control` tüm Azure CDN profili tarafından varsayılan olarak sayılır.
- İstemci bir HTTP yanıtının CDN POP kullanıldığında:
     - **Verizon'dan Azure CDN standart/Premium** ve **Azure CDN standart Microsoft** tüm destek `Cache-Control` yönergeleri.
     - **Azure CDN standart akamai'den** yalnızca aşağıdakileri destekler `Cache-Control` yönergeleri; tüm diğerleri yoksayılır:
         - `max-age`: Bir önbellek içeriği için belirtilen saniye sayısı depolayabilirsiniz. Örneğin, `Cache-Control: max-age=5`. Bu yönerge içeriği yeni olarak kabul edilir en uzun süreyi belirtir.
         - `no-cache`: İçeriği önbelleğe ancak içeriği önce önbellekten teslim etmeden her zaman doğrulayın. Eşdeğer `Cache-Control: max-age=0`.
         - `no-store`: Hiçbir zaman önbellek içeriği. Daha önce depolanmışsa içeriğini kaldırın.

**Zaman aşımı:**
- HTTP 1.0 sunulan eski başlığı; Geriye dönük uyumluluk desteklenen.
- Tarih temelli bir süre ile ikinci duyarlık kullanır. 
- Benzer şekilde `Cache-Control: max-age`.
- Kullanılabilir `Cache-Control` yok.

**Pragma:**
   - Azure CDN tarafından varsayılan olarak dikkate alınır değil.
   - HTTP 1.0 sunulan eski başlığı; Geriye dönük uyumluluk desteklenen.
   - Bir istemci istek üstbilgisi aşağıdaki yönergesi olarak kullanılan: `no-cache`. Bu yönerge kaynak yeni bir sürümünü sunmak için sunucusuna bildirir.
   - `Pragma: no-cache` eşdeğer olan `Cache-Control: no-cache`.

## <a name="validators"></a>Doğrulayıcıları

Önbellek eski olduğunda, HTTP önbellek doğrulayıcıları önbelleğe alınan bir dosya sürümü kaynak sunucu sürüm ile karşılaştırmak için kullanılır. **Verizon'dan Azure CDN standart/Premium** destekler `ETag` ve `Last-Modified` varsayılan olarak, doğrulayıcıları sırada **Azure CDN standart Microsoft** ve **akamai'denAzureCDNstandart** yalnızca destekler `Last-Modified` varsayılan olarak.

**ETag:**
- **Verizon'dan Azure CDN standart/Premium** destekleyen `ETag` varsayılan olarak, sırada **Azure CDN standart Microsoft** ve **akamai'den Azure CDN standart** desteklemez.
- `ETag` her dosya ve bir dosya sürümü için benzersiz bir dizeye tanımlar. Örneğin, `ETag: "17f0ddd99ed5bbe4edffdd6496d7131f"`.
- HTTP 1.1 içinde sunulmuştur ve daha güncel `Last-Modified`. Son değiştirilme tarihini belirlemek zor olduğu durumlarda faydalıdır.
- Güçlü doğrulama ve zayıf doğrulama destekler; Ancak, Azure CDN yalnızca güçlü doğrulama destekler. Güçlü doğrulama için iki kaynak Beyanları bayt için bayt olmalıdır aynı. 
- Bir önbellek kullanan bir dosyayı doğrular `ETag` göndererek bir `If-None-Match` üstbilgi bir veya daha fazla `ETag` istekte doğrulayıcıları. Örneğin, `If-None-Match: "17f0ddd99ed5bbe4edffdd6496d7131f"`. Sunucunun sürüm eşleşiyorsa bir `ETag` Doğrulayıcı listesinde, gönderdiği durum kodu 304 (değişiklik) yanıt olarak. Sürüm farklıysa, sunucu durum kodu 200 (Tamam) ve güncelleştirilmiş kaynak yanıt verir.

**Son değiştirilen:**
- İçin **Azure CDN standart/Premium verizon'dan** yalnızca `Last-Modified` kullanılır `ETag` HTTP yanıtı parçası değil. 
- Kaynak son değiştirilen kaynak sunucunun belirledi saat ve tarihi belirtir. Örneğin, `Last-Modified: Thu, 19 Oct 2017 09:28:00 GMT`.
- Bir önbellek dosyası kullanarak doğrular `Last-Modified` göndererek bir `If-Modified-Since` sahip bir tarih ve saat istekteki üstbilgi. Kaynak sunucu, tarihle karşılaştırır `Last-Modified` son kaynak üstbilgisi. Kaynağın belirtilen zamanından bu yana değiştirilmemiş, sunucu durum kodu 304 (değişiklik) yanıt olarak döndürür. Kaynak değiştirilirse sunucu durumu döndürür kod 200 (Tamam) ve güncelleştirilmiş kaynak.

## <a name="determining-which-files-can-be-cached"></a>Hangi dosyaların önbelleğe belirleme

Tüm kaynaklar önbelleğe alınabilir. Aşağıdaki tabloda, hangi kaynaklara, HTTP yanıtı türüne göre önbelleğe alınabilir gösterir. Tüm bu koşulların uymayan ile HTTP yanıtlarını teslim kaynakları önbelleğe alınamaz. İçin **verizon'dan Azure CDN Premium** yalnızca, bu koşullar bazıları özelleştirmek için kurallar altyapısı kullanabilirsiniz.

|                   | Microsoft Azure CDN          | Verizon'dan Azure CDN | Akamai'den Azure CDN        |
|-------------------|-----------------------------------|------------------------|------------------------------|
| HTTP durum kodları | 200, 203, 206, 300, 301, 410, 416 | 200                    | 200, 203, 300, 301, 302, 401 |
| HTTP yöntemleri      | GET, HEAD                         | GET                    | GET                          |
| Dosya boyutu sınırları  | 300 GB                            | 300 GB                 | -Genel web teslim en iyi duruma getirme: 1,8 GB<br />-En iyi duruma getirme medya: 1,8 GB<br />-Büyük dosya en iyi duruma getirme: 150 GB |

İçin **Azure CDN standart Microsoft** kaynağın üzerinde çalışmak için önbelleğe alma, kaynak sunucunun tüm HEAD desteklemesi gerekir ve HTTP GET istekleri ve içerik-uzunluk değerleri varlık için HEAD ve GET HTTP yanıtları için aynı olmalıdır. HEAD isteği için kaynak sunucunun HEAD isteği desteklemesi gerekir ve bir GET isteği almış gibi aynı üst bilgilerle yanıt vermelidir.

## <a name="default-caching-behavior"></a>Varsayılan önbelleğe alma davranışı

Aşağıdaki tabloda, önbelleğe alma davranışı Azure CDN ürünü ve bunların en iyi duruma getirme için varsayılan açıklanmaktadır.

|    | Microsoft: Genel web teslim | Verizon: Genel web teslim | Verizon: DSA | Akamai: Genel web teslim | Akamai: DSA | Akamai: Büyük dosya indirme | Akamai: genel veya medya VOD akışı |
|------------------------|--------|-------|------|--------|------|-------|--------|
| **Uy kaynağı**       | Evet    | Evet   | Hayır   | Evet    | Hayır   | Evet   | Evet    |
| **CDN önbellek süresi** | 2 gün |7 gün | None | 7 gün | None | 1 gün | 1 yıl |

**Kaynak dikkate**: vermenizin belirtir [önbellek yönergesi üstbilgileri desteklenen](#http-cache-directive-headers) kaynak sunucudan HTTP yanıt varsa.

**CDN önbellek süresi**: Azure CDN kaynak önbelleğe alınmış süreyi belirtir. Ancak, varsa **dikkate kaynak** Evet ve önbellek yönergesi üstbilgi kaynak sunucudan gelen HTTP yanıtı içerir `Expires` veya `Cache-Control: max-age`, Azure CDN bunun yerine üstbilgisi tarafından belirtilen süre değeri kullanır. 

## <a name="next-steps"></a>Sonraki adımlar

- Özelleştirme ve önbelleğe alma davranışı kuralları önbelleğe alma yoluyla CDN üzerinde varsayılan geçersiz kılma öğrenmek için bkz: [denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md). 
- Sorgu dizelerini önbelleğe alma davranışı denetlemek için kullanmayı öğrenmek için bkz: [denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle](cdn-query-string.md).



