---
title: Önbelleğe alma azure ön kapısı hizmeti - | Microsoft Docs
description: Bu makale, nasıl Azure ön kapısı hizmet uçlarınıza izler anlamanıza yardımcı olur.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 42ee1dea8c9735592f6d6c9e0542ca094a6be383
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962906"
---
# <a name="caching-with-azure-front-door-service"></a>Azure ön kapısı hizmetiyle önbelleğe alma
Belgesinde, ön kapısı önbelleğe almanın etkin yönlendirme kurallarıyla davranışını belirtir.

## <a name="delivery-of-large-files"></a>Büyük dosyaların teslimini
Dosya boyutu büyük dosyaları bir sınır olmaksızın Azure ön kapısı hizmeti sunar. Ön kapısı nesne parçalama olarak adlandırılan tekniği kullanır. Büyük dosya isteği gönderildiğinde Front Door dosyayı arka uçtan küçük parçalar halinde alır. Front Door ortamı tam veya bayt aralığı belirtilen bir dosya isteği aldıktan sonra dosyayı 8 MB'lık parçalar halinde arka uçtan ister.

</br>Öbek ön kapısı ortam ulaştıktan sonra önbelleğe alınmış ve hemen kullanıcıya sunulan. Ön kapısı ardından paralel sonraki öbek önceden getirir. Bu önceden getirme içeriği gecikmesini azaltır kullanıcı önceden bir öbek kalmasını sağlar. Bu işlem tüm kadar devam eder (istenirse) dosyasını indirdiğiniz, tüm bayt aralıkları (istenirse) kullanılabilir, veya istemci bağlantıyı sonlandırır.

</br>Bayt aralığı istek üzerine daha fazla bilgi için okuma [RFC 7233](https://web.archive.org/web/20171009165003/ http://www.rfc-base.org/rfc-7233.html).
Ön kapısı alındığında ve dosyanın tamamı ön kapısı önbellekte önbelleğe alınması gerekmez tüm öbekleri önbelleğe alır. Dosya veya bayt aralıkları için sonraki istekleri önbellekten sunulur. Aksi takdirde tüm öbekleri önbelleğe alınır, öbekleri arka ucundan istemek için önceden getirme kullanılır. Bu iyileştirme, bayt aralığı isteklerini desteklemek için arka uç yeteneklerini üzerinde kullanır; Bu iyileştirme, arka bayt aralığı isteklerini desteklemiyorsa, etkili değildir.

## <a name="file-compression"></a>Dosya sıkıştırma
Ön kapısı, müşterileriniz için daha küçük ve daha hızlı yanıt bunun sonucunda içerik edge üzerinde dinamik olarak sıkıştırabilirsiniz. Tüm dosyaları sıkıştırma için uygundur. Ancak, bir dosya sıkıştırma listesi için uygun bir MIME türü olmalıdır. Şu anda ön kapısı değiştirilmesi bu listeye izin vermez. Geçerli bir listesi verilmiştir:</br>
- "application/eot"
- "application/yazı tipi"
- "application/yazı tipi-sfnt"
- "application/javascript"
- "application/json"
- "application/opentype"
- "application/otf"
- "application/pkcs7-MIME"
- "application/truetype"
- "application/fot adı",
- "uygulama/vnd.ms-fontobject"
- "application/xhtml + xml"
- "application/xml"
- "application/xml+rss"
- "application/x-font-opentype"
- "application/x-font-truetype"
- "application/x-font-fot adı"
- "application/x-httpd-cgi"
- "application/x-mpegurl"
- "application/x-opentype"
- "application/x-otf"
- "application/x-perl"
- "application/x-fot"adı
- "application/x-javascript"
- "yazı tipi/eot"
- "yazı tipi/fot adı"
- "yazı tipi/otf"
- "yazı tipi/opentype"
- "görüntü/svg + xml"
- "metin/css"
- "text/csv"
- "text/html"
- "text/javascript"
- "text/js", "text/plain"
- "metin/zengin metin"
- "metin/sekmesini ayrılmış değerler"
- "metin/xml"
- "metin/x-betik"
- "metin/x-component"
- "metin/x-java-source"

Ayrıca, dosya, ayrıca 1 KB ve 8 MB boyutunda, kapsamlı arasında olmalıdır.

Bu profiller, aşağıdaki sıkıştırma kodlamaları destekler:
- [gzip (GNU zip)](https://en.wikipedia.org/wiki/Gzip)
- [brotli](https://en.wikipedia.org/wiki/Brotli)

Bir istek, gzip ve Brotli sıkıştırmayı destekliyorsa, Brotli sıkıştırma önceliklidir.</br>
Bir varlık için bir istek önbellek isabetsizliği sıkıştırma ve istek sonuçları belirttiğinde, ön kapısı doğrudan POP sunucusunda varlığın sıkıştırma gerçekleştirir. Daha sonra sıkıştırılmış dosyayı önbellekten sunulur. Bir transfer-encoding ile elde edilen öğe döndürülür: öbekli.

## <a name="query-string-behavior"></a>Sorgu dize davranışı
Ön kapısı ile bir web isteği için bir sorgu dizesi içeren dosyaların önbelleğe nasıl kontrol edebilirsiniz. Bir sorgu dizesi ile bir web isteğinde sorgu dizesi bir soru işareti (?) sonra gerçekleşen istek, bölümüdür. Bir sorgu dizesi alan adını ve değerini bir eşittir işareti (=) tarafından ayrılır, bir veya daha fazla anahtar-değer çiftleri içerir. Her anahtar-değer çifti bir ampersan ile ayrılır (&). Örneğin, http://www.contoso.com/content.mov?field1=value1&field2=value2. Bir isteğin sorgu dizesinde birden fazla anahtar-değer çifti varsa, bunların sırası önemli değildir.
- **Sorgu dizelerini Yoksay**: Varsayılan modu. Bu modda, ön kapısı sorgu dizelerini istek sahibi uç ilk istek üzerine geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi dolana kadar ön kapısı ortamından sunulan tüm sonraki istekleri varlık için sorgu dizelerini yoksay.

- **Her benzersiz URL'yi önbelleğe al**: Bu modda, benzersiz bir URL, sorgu dizesi dahil olmak üzere her bir istekle kendi önbellek ile benzersiz bir varlık olarak kabul edilir. Örneğin, istek için arka uç yanıtı `www.example.ashx?q=test1` ön kapısı ortam önbelleğe alınmış ve sonraki önbelleklerle aynı sorgu dizesi için döndürdü. Bir istek için `www.example.ashx?q=test2` ayrı bir varlık kendi yaşam süresi ayarı ile önbelleğe alınır.

## <a name="cache-purge"></a>Önbellek Temizleme
Varlığın yaşam süresi (TTL) dolmadan ön kapısı varlıklar önbelleğe alır. Bir istemci varlık istediğinde varlığın TTL süresi dolduğunda, ön kapısı ortamı istemci isteğe hizmet vermek için varlığı yeni güncelleştirilmiş bir kopyasını alır ve depolama önbelleği yenileyin.
</br>Kullanıcılarınızın her zaman en son kopyasını varlıklarınızı Al emin olmak için en iyi her güncelleştirme için varlıklarınızı sürümüdür ve bunları yeni URL olarak yayımlayın. Ön kapısı hemen sonraki istemci istekleri için yeni varlıkları alır. Bazen tüm kenar düğümlerinden önbelleğe alınmış içeriği temizlemek ve bunları tüm yeni güncelleştirilmiş varlıkları almak için zorlamak isteyebilirsiniz. Bu, web uygulamanıza veya hızlı bir şekilde yanlış bilgiler içeren güncelleştirme varlıklarına güncelleştirmeleri nedeniyle olabilir.

</br>Kenar düğümlerinden temizlemek istediğiniz hangi varlıkları seçin. Tüm varlıkları silmek isterseniz, temizleme tüm onay kutusuna tıklayın. Aksi takdirde, yol metin kutusu içinde temizlemek istediğiniz her varlık yolunu yazın. Yolu aşağıdaki biçimleri desteklenir.
1. **Tek URL Temizleme**: Dosya uzantısı, örneğin, /pictures/strasbourg.png tam bir URL belirterek tek varlığı Temizle;
2. **Joker Temizleme**: Yıldız işareti (\*) joker karakter olarak kullanılabilir. Tüm klasörleri, alt klasörler ve dosyaları bir uç nokta altında Temizleme /\* tüm alt klasörleri ve klasör belirterek belirli bir klasör altındaki dosyalar ardında yer alan yolu ya da Temizleme /\*, örneğin, / resimler\*.
3. **Kök etki alanı Temizleme**: Kök yolda "/" ile uç nokta temizleme.

Önbellek temizler ön kapısı üzerinde büyük/küçük harfe duyarsızdır. Ayrıca, sorgu dizesi bağımsız bir URL temizleme, tüm sorgu dizesi çözümlenmeyebileceği temizler anlamına gelir, kullanılabilir. 

## <a name="cache-expiration"></a>Önbellek süre sonu
Üst bilgi aşağıdaki sırayla bir öğe ne kadar olacağını belirlemek için kullanılan bizim önbellekte depolanır:</br>
1. Cache-Control: s-maxage =\<saniye >
2. Cache-Control:, max-age =\<saniye >
3. Süre sonu: \<http tarih >

Yanıt Cache-Control gibi önbelleğe alınabilir olmaz gösteren bir cache-Control yanıt üstbilgileri: özel, Cache-Control: no-cache ve Cache-Control: no-store dikkate alınır. Ancak, varsa birden çok istek uçuşan aynı URL POP sırasında yanıt paylaşabilir. Cache-Control varsa varsayılan AFD kaynak süreyi X önbelleğe alacağı burada X 1 ila 3 gün arasında rastgele seçilir davranışıdır.


## <a name="request-headers"></a>İstek üst bilgileri

Şu istek üst bilgilerini arka uca önbelleğe alma kullanılırken iletilir değil.
- Yetkilendirme
- İçerik uzunluğu
- Transfer-Encoding

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
