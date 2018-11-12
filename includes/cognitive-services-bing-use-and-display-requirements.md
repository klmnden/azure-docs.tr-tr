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
ms.openlocfilehash: f166ceac1ae848565f861a94781ce0500c24747e
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289164"
---
Kullanım ve görüntü gereksinimleri ilgili bilgiler ve içerik herhangi bir uygulama için geçerlidir. Örneğin, gereksinimleri, ilişkileri, meta verileri ve diğer sinyaller için geçerlidir. Bunlar şu API çağrıları üzerinden kullanılabilir:

- Bing Özel Arama
- Bing Varlık Arama
- Bing Resim Arama
- Bing Haber Arama
- Bing Video Arama
- Bing Görsel Arama
- Bing Web Araması
- Bing Yazım Denetimi
- Bing Otomatik Öneri

Belirli özellikler ve sonuçlar için belgelerinde bu gereksinimleri için ilgili uygulama ayrıntıları bulabilirsiniz.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>API'leri, Bing yazım denetimi ve Bing otomatik öneri

Yapma:

- Kopyalama, saklamak veya Bing yazım denetimi veya Bing otomatik öneri API'leri aldığınız herhangi bir veri önbelleği.
- Bing yazım denetimi veya Bing otomatik öneri API'leri machine learning'e ya da benzer algoritmik etkinliği bir parçası olarak almaya verileri kullanın. Bu veriler eğitmek, değerlendirmek ya da siz veya üçüncü taraflara sunabilir yeni veya var olan hizmetleri geliştirmek için kullanmayın.

## <a name="definitions"></a>Tanımlar

- *Yanıt* bir kategoriye ait bir yanıtta döndürülen sonuçların ifade eder. Örneğin, Bing Web araması API'si yanıtı yanıtlar Web sonuçları, resim, video, visual ve haber kategorileri içerebilir.   
- *Yanıt* tüm yanıtlar anlamına gelir ve ilişkili verileri tek bir arama API'sine çağrıda yanıt alındı.
- *Sonuç* yanıt bilgilerinin bir öğeye başvuruyor. Örneğin, bir tek haber makaleyle bağlantılı veri kümesini haber yanıtında bir sonucudur.
- *Arama API'leri* , toplu olarak, Bing özel arama, varlık araması, resim arama, haber arama, Video arama, görsel arama ve Web arama API'leri anlamına gelir. 


## <a name="search-apis"></a>API arama

Bu bölümdeki gereksinimleri, arama API'leri için geçerlidir. Arama API'leri, Bing yazım denetimi veya Bing otomatik öneri içermez. Bu iki API gereksinimlerini ve önceki bölümde ele alınmıştır.

### <a name="internet-search-experience"></a>Internet arama deneyimi

Yanıtları döndürülen tüm veriler, yalnızca internet arama deneyimleri kullanılabilir. Bir internet arama deneyimi görüntülenen, uygun içerik anlamına gelir: 
- İlgili olduğundan ve yanıt verdiğinden son kullanıcının doğrudan sorgu ya da kullanıcının arama ilgi amaç (örneğin, bir kullanıcı belirtilen arama sorgusu) ve diğer göstergesi. 
- Kullanıcıların bulmasına ve veri kaynağına gidin yardımcı olur (örneğin, sağlanan URL tıklanabilir bir bağlantıya conspicuously verilerle görüntülenen içerik veya atıf, bu nedenle köprü olarak uygulanır). Ya da görünüşte Bing varlık arama API'si kullanıyorsanız, bing.com adresindeki dizinlerde ilgili sorgu için arama sonuçlarını gitmek kullanıcının sağlayan yanıttaki sağlanan bing.com URL'nin bağlantı.
- Aralarından seçim yapabileceğiniz kullanıcı için birden çok sonuç içerir (örneğin, birkaç sonuçlardan haber yanıt görüntülenir veya birkaç'den az ise tüm sonuçlar döndürülür). 
- Arama (örneğin, küçük resimler küçük resim-kullanıcının ekran derlemekten boyutlandırılır görüntü) amaca uygun bir miktar ile sınırlıdır. 
- İçeriğin internet arama sonuçları (örneğin, içeriğin "web" olduğu bir deyimi) olduğunu kullanıcıya görünür bir göstergesi içerir.
- Herhangi bir arama API'lerinden alınan veriler kullanımınız herhangi bir uygulanabilir yasalar veya üçüncü taraf haklarını ihlal etmemesini sağlamak uygun önlemleri birleşimini içerir. Örneğin, bir Creative Commons lisansı FQDN'yi kullanıyorsanız, geçerli lisans koşullarına uymanız. Ölçüler ne olabilir, yasal danışmanlar uygun belgelere bakın.
Yalnızca internet arama Deneyimi gereksinimi için keşif URL'si, bu makalenin sonraki bölümlerinde açıklandığı istisnadır. 

### <a name="restrictions"></a>Kısıtlamalar

Yapma:

- Kopyalama, saklamak veya tüm veriler (hariç tutma "Hizmet sürekliliği" bölümünde bu makalenin devamındaki izin verdiği ölçüde) yanıtları önbelleğe alma. 
- Machine learning'e ya da benzer algoritmik etkinlik kapsamında arama API'lerinden alınan veriler kullanın. Bu veriler eğitmek, değerlendirmek ya da siz veya üçüncü taraflara sunabilir yeni veya var olan hizmetleri geliştirmek için kullanmayın.
- (Dışındaki herhangi bir gereksinim ihlal etmemesini şekilde yeniden biçimlendirmek üzere), sonuçları içerik değiştirme yasaların gerektirdiği durumlar ya da Microsoft tarafından kabul sürece. 
- Attribution ve sonuç içerikle ilişkili URL'leri atlayın.
- Atlama, bir sipariş veya derecelendirme yasaların gerektirdiği durumlar haricinde, sağlanan veya Microsoft tarafından kabul bir yanıtında gösterilen sonuçları dahil olmak üzere yeniden sıralayın. (Bing özel arama API'si için bu kural customsearch.ai Portalı aracılığıyla uygulanan yeniden sıralama için geçerli değildir.)
- Diğer içerik yanıt herhangi bir bölümü içinde bir kullanıcının diğer içerik yanıtın bir parçası olduğunu düşünüyorsanız sunulmasını şekilde görüntüleyin. 
- Bir yanıt herhangi bir bölümünü görüntüleyen herhangi bir sayfa üzerinde Microsoft tarafından sağlanmayan reklamları görüntüleyin. 
- Bing görüntü, haber arama, Video araması ve görsel arama API'leri alınan yanıtları (i) ile herhangi bir reklam görüntüleme; veya (ii), filtre veya birincil (veya yalnızca) sınırlı görüntü, haber ve/veya video ya da görsel sonuçları.

### <a name="notices-and-branding"></a>Bildirimler ve markalama 

- İşlevsel bir köprü göze çarpacak şekilde dahil [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=521839), neredeyse her noktasında bir kullanıcı bir arama sorgusu giriş imkanı kullanıcı deneyimi (UX). Bağ etiketi **Microsoft gizlilik bildirimi**.
- Bing, tutarlı marka öne [Bing olan marka kullanım kılavuzuna](https://go.microsoft.com/fwlink/?linkid=833278), neredeyse her bir kullanıcı bir arama sorgusu giriş imkanı UX noktası. Böyle bir marka açıkça kullanıcıya Microsoft internet arama deneyimini güçlendiren olduğunu belirtmek gerekir.
- Microsoft, aksi takdirde, kullanımınız için yazma belirtmediği sürece her yanıt (veya bir yanıt bölümü) Microsoft'a, Bing Web araması, resim arama, haber arama, Video araması ve görsel arama API'leri görüntülenen özniteliği. Bu açıklanan [Bing olan marka kullanım kılavuzuna](https://go.microsoft.com/fwlink/?linkid=833278). 
- Microsoft aksini belirtmedikçe yanıtları (özniteliği ya da yanıtları bölümlerini), Microsoft Bing özel arama API'si görüntülenen belirli kullanımınız için yazma değil.

### <a name="transferring-responses"></a>Yanıtları aktarma

Bir Mesajlaşma uygulaması veya sosyal medya posta, aşağıdaki durumlardan gibi başka bir kullanıcı, arama API'si yanıt aktarmak için bir kullanıcı devre dışı bırakırsanız: 
- Aktarılan yanıtları gerekir:
  - Aktarma kullanıcıya görüntülenen yanıtların içeriğinden değiştirilmemiş olan içerik oluşur. Biçimlendirme değişikliklerini verilebilir.
  - Herhangi bir veri meta veri biçiminde dahil.
  - Bing Web, görüntü, Haberler, Video ve görsel API'ler yanıtlar için Bing tarafından desteklenen bir internet arama deneyimini aracılığıyla görüntüleme dilini gösteren yanıt alındı. Örneğin, Bing bu görüntü ile ilgili daha fazla "Bing tarafından desteklenen" veya "Bilgi" gibi dil görüntüleyebilir veya Bing logosu kullanabilirsiniz.
  - Bing özel arama API'si yanıtlar almak için bir internet arama deneyimi görüntüleme dilini gösteren yanıt alındı. Örneğin, "Hakkında daha fazla bilgi bu arama sonucunda." gibi dil görüntüleyebilir
  - Yanıtı oluşturmak için kullanılan tam sorgu çarpacak şekilde görüntüleyin.
  - Tanınmış bir bağlantı veya benzer attribution yanıtının temel alınan kaynağa doğrudan veya bir arama motoru (bing.com, m.bing.com veya özel arama hizmetinizi uygunsa) üzerinden içerir.
- Yanıtları aktarımını otomatik değildir. Aktarım NET bir şekilde yanıt aktarmak için bir hedefi evidencing bir kullanıcı eylemi tarafından başlatılmalıdır.
- Aktarma kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmak için bir kullanıcı yalnızca sağlayabilir.

### <a name="continuity-of-service"></a>Kesintisiz hizmet devamlılığı 

Kopyalamayın, depolamak veya tüm veriler arama API yanıtları önbelleğe alma. Ancak, sürekliliği ve veri işleme hizmeti erişimi etkinleştirmek için sonuçları yalnızca aşağıdaki koşullarda Koru:

**Cihaz.** Tutulan sonuçlar yalnızca kullanılabilir olması koşuluyla, sonuçları bir aygıtta saati (i) 24 saatten daha az için sorgu veya (ii) şirketin başka bir sorgu için güncelleştirilmiş sonuçları, bir kullanıcının gönderdiğini kadar korumak bir kullanıcı sağlayabilir:

- Kullanıcının erişim sağlamak için daha önce bu cihazda (örneğin, durumunda hizmet kesintisini) söz konusu kullanıcı için döndürülen sonuçlanır.
- Kullanıcının sinyaller (örneğin, durumunda beklenen hizmet kesintisi) göre kullanıcının gereksinimlerinin olasılığına kişiselleştirilmiş, proaktif sorgu için döndürülen sonuçlarını depolamak için.

**Sunucu.** Güvenli bir şekilde sizin denetlediğiniz bir sunucuda tek bir kullanıcı için belirli sonuçları tutabilir ve yalnızca tutulan sonuçları görüntüleyin:

- Çözümünüzdeki bu kullanıcıya daha önce döndürülen sonuçların geçmiş bir rapora erişmek kullanıcı etkinleştirmek için. Sonuçları değil (i) 21 günden fazla bir süre zamanından sonra son kullanıcının ilk sorgu korunur ve (ii) bir kullanıcının yeni veya yinelenen sorgusuna yanıt olarak görüntülenir.
- Kullanıcı gereksinimlerini, kullanıcının sinyalleri üzerinde temel olasılığına proaktif sorgunuz için döndürülen sonuçlarını depolamak için kişiselleştirilmiş. Bu sonuçları saati (i) 24 saatten daha az için sorgu veya (ii) şirketin başka bir sorgu için güncelleştirilmiş sonuçları bir kullanıcının gönderdiğini kadar depolayabilirsiniz.

Korunan her sonuçları belirli bir kullanıcı için başka bir kullanıcı için sonuçlarla commingled olamaz. Diğer bir deyişle, her kullanıcı sonuçlarını korunur ve gerekir ayrı olarak teslim.

### <a name="general"></a>Genel 

Tutulan sonuçları tüm sunumu için:

- Sorgunun gönderildiği zaman açık, görünür bir bildirim içerir.
- Mevcut bir düğme veya benzer bir kullanıcı için yeniden sorgular ve almak için anlamına gelir, sonuçları güncelleştirildi. 
- Sonuçları sunuda marka Bing korur.
- silin (ve gerekirse yeni bir sorgu ile yenileme) belirtilen zaman dilimlerine içindeki depolanmış sonuç.

### <a name="non-display-url-discovery"></a>Görüntü olmayan URL'yi bulma 

Yalnızca, kullanıcı ya da müşteri için bir sorgu duyarlı bilgi kaynakları URL'lerini keşfetme uygulamalarınızdaki için bir internet olmayan arama deneyimini araması yanıtlarında kullanabilirsiniz. Bir rapor ya da benzer yanıt sağladığınız böyle URL'leri kopyalayabilirsiniz:

- Yalnızca bu kullanıcı veya müşteri, yanıt olarak sorgu.
- Yalnızca bu önemli ek değerli içerik, sorgu ile ilgili içeriyorsa.

Arama API'lerinin önceki bölümlerde kullanın ve görüntü gereksinimleri aşağıdaki istisnalar dışında bu görüntü olmayan kullanım için geçerli değildir: 

- Değil önbellek, kopyalama veya herhangi bir veri veya içerik, saklamak veya daha önce açıklanan kopyalama, sınırlı URL dışındaki arama yanıt öğesinden türetilen.
- Arama API'lerinden alınan (URL'leri dahil) veri kullanımını herhangi bir uygulanabilir yasalar veya üçüncü taraf haklarını ihlal etmemesini sağlamak.
- Herhangi bir arama dizini veya machine learning veya benzer algoritmik etkinlik kapsamında arama API'lerinden alınan (URL'leri dahil) veri kullanmayın. Bu veri train oluşturma, değerlendirmek veya siz veya üçüncü taraflara sunabilir hizmetlerini geliştirmek için kullanmayın.

## <a name="gdpr-compliance"></a>GDPR uyumluluğu  

Genel veri koruma yönetmeliği (GDPR) ve arama API'leri, Bing yazım denetimi API'si ve Bing otomatik öneri API'si çağrılarını bağlantılı olarak işlenir Avrupa Birliği tabi kişisel verileri göre siz ve Microsoft olduğunu biliyoruz. bağımsız veri denetleyicilerine GDPR altında. Bağımsız olarak, GDPR ile uyum için sorumlu olursunuz.  

