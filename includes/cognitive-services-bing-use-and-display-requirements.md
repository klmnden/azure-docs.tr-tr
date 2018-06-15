---
title: include dosyası
description: include dosyası
services: cognitive-services
author: MikeDodaro
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 04/19/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 174af83686eba665a729246be7a477b9a5054f30
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356077"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Bing arama API kullanın ve gereksinimleri görüntüleme

Kullanım ve görüntü gereksinimleri ilgili bilgiler ve içerik herhangi bir uygulaması için geçerlidir. Örneğin, ilişkileri, meta verileri ve diğer sinyaller gereksinimleri geçerlidir. Bunlar aşağıdaki API çağrıları aracılığıyla kullanılabilir:

- Bing Özel Arama
- Bing Varlık Arama
- Bing Resim Arama
- Bing Haber Arama
- Bing Video Arama
- Bing Görsel Arama
- Bing Web Araması
- Bing Yazım Denetimi
- Bing Otomatik Öneri

Bu gereksinimleri belirli özellikleri ve sonuçları belgelerindeki ilgili uygulama ayrıntılarını bulabilirsiniz.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>Bing yazım denetimi ve Bing otomatik öneri API'leri

Yapma:

- Kopyalama, saklamak veya Bing yazım denetimi veya Bing otomatik öneri API'leri almaya herhangi bir veriyi önbelleğe.
- Bing yazım denetimi veya Bing otomatik öneri API'leri herhangi bir machine learning veya benzer algoritmik etkinlik bir parçası olarak almaya verileri kullanın. Bu veriler, eğitme, değerlendirmek veya siz veya üçüncü tarafların sunabilir yeni veya var olan hizmetleri geliştirmek için kullanmayın.

## <a name="definitions"></a>Tanımlar

- *Yanıt* bir yanıtta döndürülen sonuçları kategorisine başvuruyor. Örneğin, Bing Web arama API yanıtından Web sayfası sonuçları, resim, video, visual ve haber kategorilerde yanıtlar dahil edebilirsiniz.   
- *Yanıt* ve yanıt bir arama API için tek bir çağrı olarak alınan verileri ilişkili tüm yanıtlar anlamına gelir.
- *Sonuç* yanıt bilgilerinin bir öğeye başvuruyor. Örneğin, bir tek haber makaleyle bağlantılı veri kümesi bir haber yanıtında bir sonucudur.
- *Arama API'leri* , topluca Bing özel arama, varlık arama, görüntü arama, haber arama, Video arama, Visual arama ve Web arama API'leri anlamına gelir. 


## <a name="search-apis"></a>API arama

Bu bölümdeki gereksinimleri arama API'leri için geçerlidir. Arama API'leri Bing yazım denetimi veya Bing otomatik öneri içermez. Bu iki API gereksinimleri önceki bölümde ele alınmıştır.

### <a name="internet-search-experience"></a>Internet arama deneyimi

Yanıtları döndürülen tüm veriler yalnızca Internet arama deneyimleri kullanılabilir. Bir internet arama deneyimi görüntülenen, olarak geçerli içerik anlamına gelir: 
- İlgili ve son kullanıcının doğrudan sorgu veya diğer göstergesi kullanıcının arama faiz ve hedefi (örneğin, bir kullanıcı belirtilen arama sorgusu) duyarlı olur. 
- Kullanıcıların bulmasına ve veri kaynakları için gidin yardımcı olur (örneğin, sağlanan URL'leri içerik veya attribution conspicuously verilerle görüntülenen tıklatılabilir bir bağlantı olması için köprü olarak uygulanır). Ya da görünür şekilde Bing varlık arama API kullanıyorsanız, aratıp ilgili sorguyu için arama sonuçlarını gitmek kullanıcının sağlayan yanıt sağlanan aratıp URL bağlantı.
- Aralarından seçim yapabileceğiniz kullanıcı için birden çok sonuç içerir (örneğin, haber yanıt birkaç sonuçlarından görüntülenir veya daha az birkaç varsa tüm sonuçları döndüren). 
- Arama (örneğin, görüntü küçük resimleri küçük resim-kullanıcının görünen orantılı olarak boyutlandırılır) amaca uygun bir miktarıyla sınırlıdır. 
- İçerik Internet arama sonuçları (örneğin, içerik "Web'den" olduğunu bir deyim) olduğunu kullanıcıya görünür bir gösterge içerir.
- Arama API'lerden alınan verileri kullanımınız herhangi bir yasaların veya üçüncü taraf haklarını ihlal emin olmak uygun önlemleri, herhangi bir birleşimini içerir. Örneğin, bir Creative Commons lisansı FQDN'yi kullanıyorsanız, geçerli lisans koşullarına uymanız. Ne ölçüleri olabilir belirlemek için yasal danışmanlar uygun belgelere bakın.
Yalnızca Internet arama Deneyimi gereksinimi URL bulma işlemi için bu makalenin sonraki bölümlerinde açıklandığı gibi istisnadır. 

### <a name="restrictions"></a>Kısıtlamalar

Yapma:

- Kopyalama, saklamak veya yanıtlardan (hariç tutma "Hizmet sürekliliği" bölümünde bu makalenin sonraki bölümlerinde izin verdiği ölçüde) herhangi bir veri önbellek. 
- Arama API'lerden herhangi bir machine learning veya benzer algoritmik etkinlik bir parçası olarak alınan verileri kullanın. Bu veriler, eğitme, değerlendirmek veya siz veya üçüncü tarafların sunabilir yeni veya var olan hizmetleri geliştirmek için kullanmayın.
- Sonuçları (diğer herhangi bir gereksinim ihlal olmayan bir şekilde yeniden biçimlendirmek üzere daha), içeriğini değiştirme yasalar tarafından gerekli veya Microsoft tarafından kabul sürece. 
- Attribution ve sonuç içerikle ilişkili URL'lerin atlayın.
- Sipariş atlandığını tarafından da dahil olmak üzere, sipariş veya derecelendirme sağlandığında yasalar tarafından gerekli kılınmadıkça hiçbir bir yanıt görüntülenen veya Microsoft tarafından kabul sonuçlanır. (Bing özel arama API için bu kural customsearch.ai Portalı aracılığıyla uygulanan yeniden sıralama için geçerli değildir.)
- İçinde bir yanıt herhangi bir kısmını diğer içeriği diğer içerik yanıtın bir parçası olduğunu düşünmek için bir kullanıcı sunulmasını bir biçimde görüntüler. 
- Bir yanıt herhangi bir bölümünü görüntüler herhangi bir sayfasında Microsoft tarafından sağlanmayan reklam görüntüler. 
- Bing görüntüsünden, haber arama, Video arama veya Visual arama API'leri yanıtları (i) ile tüm tanıtım görüntüler; ya da (II) olan veya filtre öncelikle (veya yalnızca) sınırlı görüntü, haber ve/veya video veya görsel sonuçları.

### <a name="notices-and-branding"></a>Bildirimler ve markalama 

- Göze çarpacak şekilde işlev köprü dahil [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=521839), her bir kullanıcı bir arama sorgusu giriş olanağı sunar kullanıcı deneyimini (UX) noktasında yakınında. Köprü etiket **Microsoft gizlilik bildirimi**.
- Marka, tutarlı Bing öne [Bing ticari marka kullanım yönergeleri](https://go.microsoft.com/fwlink/?linkid=833278), her bir kullanıcı bir arama sorgusu giriş olanağı sunar UX noktasında yakınında. Bu tür markalama açıkça kullanıcıya Microsoft Internet arama deneyimi başlatırken olduğunu belirtmek gerekir.
- Microsoft, aksi takdirde kullanımınız için yazma belirtmediği sürece her yanıt (veya bir yanıt kısmı) Bing Web araması, görüntü arama, haber arama, Video arama ve görsel arama API'leri Microsoft'a görüntülenen öznitelik. Bu açıklanan [Bing ticari marka kullanım yönergeleri](https://go.microsoft.com/fwlink/?linkid=833278). 
- Microsoft aksini belirtmedikçe yanıtları (özniteliği ya da yanıtları bölümlerini) Microsoft'a, Bing özel arama API'SİNDEN görüntülenen belirli kullanımınız için yazma değil.

### <a name="transferring-responses"></a>Yanıt aktarma

İleti bir uygulamanın veya sosyal medya nakil, aşağıdaki durumlardan gibi bir arama API yanıtından başka bir kullanıcı, aktarmak bir kullanıcı etkinleştirirseniz: 
- Aktarılan yanıtları gerekir:
  - Dosya aktarımı kullanıcıya görüntülenen yanıtların içerikten değiştirilmemiş olan içerik oluşur. Biçimlendirme değişiklikleri verilebilir.
  - Herhangi bir veri meta verileri formunda eklemeniz gerekmez.
  - Bing Web, görüntü, haber, Video ve görsel API'leri yanıtlar için Bing ile güçlendirilmiştir bir internet arama deneyimi aracılığıyla görüntüleme dilini gösteren yanıt alındı. Örneğin, Bing üzerinde bu görüntüyü hakkında daha fazla bilgi "Bing tarafından desteklenen" veya "Öğrenin" gibi dil görüntüleyebilir veya Bing logo kullanabilirsiniz.
  - Bing özel arama API'sinden yanıtlar için görüntüleme dilini yanıt belirten bir internet arama deneyimi alındı. Örneğin, "Hakkında daha fazla bilgi bu arama sonucu." gibi dil görüntüleyebilir
  - Yanıtı oluşturmak için kullanılan tam sorgu öne.
  - Belirgin bağlantı veya benzer attribution yanıtının temel alınan kaynak için doğrudan veya arama motoru (aratıp, m.bing.com veya geçerli olarak özel arama hizmetinizi) aracılığıyla içerir.
- Yanıtları aktarımını otomatikleştirmek değil. Bir aktarım açıkça bir yanıt aktarmak için bir hedefi evidencing bir kullanıcı eylemi tarafından başlatılması gerekir.
- Dosya aktarımı kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmak bir kullanıcı yalnızca sağlayabilir.

### <a name="continuity-of-service"></a>Kesintisiz hizmet devamlılığı 

Kopyalama, depolamaz veya arama API yanıtlardan herhangi bir veri önbelleğe alma. Ancak, hizmet erişim ve veri işleme sürekliliği etkinleştirmek için aşağıdaki koşullarda yalnızca sonuçları tutabilir:

**aygıt.** Tutulan sonuçları yalnızca kullanılabilir olması koşuluyla sonuçlarına (i) 24 saat zamandan daha düşük bir aygıtın veya sorgunun kadar (II) başka bir sorgu güncelleştirilmiş sonuçlar için bir kullanıcının gönderdiğini korumak bir kullanıcı sağlayabilir:

- Kullanıcı daha önce o kullanıcı için (örneğin, durumunda hizmet kesintisi) cihazdaki döndürülen sonuçları erişebilir etkinleştirmek için kullanılır.
- Öngörülü sorgunuz için döndürülen Sonuçların depolanacağı, kullanıcının gereksinimlerinin, o kullanıcının sinyaller (örneğin, durumunda beklenen hizmet kesintisi) göre kapatıldığını kişiselleştirilmiş.

**Sunucu.** Sonuçları güvenli bir şekilde denetim bir sunucuda tek bir kullanıcı için belirli tutabilir ve yalnızca tutulan sonuçları görüntülemek:

- Çözümünüzdeki bu kullanıcıya daha önce döndürülen sonuçları geçmiş raporu erişmek kullanıcı etkinleştirmek için kullanılır. Sonuçları değil (i) 21 günden fazla bir süre son kullanıcının ilk sorgu zaman tutulur ve (II) kullanıcının yeni veya yinelenen sorgusuna yanıt olarak görüntülenir.
- Öngörülü sorgunuz için döndürülen Sonuçların depolanacağı, kullanıcının gereksinimlerinin, o kullanıcının sinyalleri üzerinde temel kapatıldığını kişiselleştirilmiş. Sorgu, veya bir kullanıcı güncelleştirilmiş sonuçları için başka bir sorgu gönderdiğinde (II) kadar küçük (i) 24 saatlik zaman için bu sonuçları depolayabilirsiniz.

Korunan her sonuçları belirli bir kullanıcı için başka bir kullanıcı için sonuçlarla commingled olamaz. Diğer bir deyişle, her kullanıcı sonuçlarını korunur ve gerekir ayrı olarak teslim.

### <a name="general"></a>Genel 

Tutulan sonuçları tüm sunumu için:

- Bir sorgu gönderildiği saati Temizle, görünen duyuru içerir.
- Yeniden sorgulama ve almak için bir düğmeye veya benzer kullanıcı anlamına gelir mevcut sonuçları güncelleştirildi. 
- Sonuçları sunuya markalama Bing korur.
- Silin (ve gerekirse yeni bir sorgu ile yenileme) belirtilen üretildikten içinde depolanan sonuçları.

### <a name="non-display-url-discovery"></a>Olmayan görüntü URL bulma 

Kullanıcı ya da müşteri bir sorguya yanıt veren bilgi kaynakları URL'lerini keşfetme birinde tek amacı için bir internet olmayan arama deneyimi arama yanıtları yalnızca kullanabilirsiniz. Bir rapor veya benzeri bir yanıt sağladığınız böyle URL'leri kopyalamak:

- Yalnızca bu kullanıcı veya bu sorgusuna yanıt olarak müşteri için.
- Yalnızca önemli ek değerli içerik, sorgu ile ilgili içeriyorsa.

Görüntü gereksinimleri aşağıdakiler dışında bu görüntü olmayan kullanım için geçerli değildir ve arama API'leri, önceki bölümlerde kullanın: 

- Önbellek, kopyalamayın veya herhangi bir veri ya da, içeriği depolamak ya da, daha önce açıklanan kopyalama, sınırlı URL dışında arama yanıtı uyarlanmıştır.
- (URL'leri dahil) veri arama API'lerden alınan kullanımınız herhangi bir yasaların veya üçüncü taraf haklarını ihlal emin olun.
- Arama API'lerden herhangi bir arama dizini ya da makine öğrenme veya benzer algoritmik etkinlik bir parçası olarak alınan (URL'leri dahil) verileri kullanmayın. Bu veriler tren oluşturun, değerlendirmek ya da siz veya üçüncü tarafların sunabilir hizmetleri geliştirmek için kullanmayın.

## <a name="gdpr-compliance"></a>GDPR uyumluluk  

Genel veri koruma düzenleme (GDPR) ve arama API'leri, Bing yazım denetleme API veya Bing otomatik öneri API çağrıları bağlantılı olarak işlenir Avrupa Birliği tabi kişisel verilerin göre Microsoft olduğunu anlama bağımsız veri denetleyicileri GDPR altında. GDPR uyduğunuzdan bağımsız olarak sorumlu.  

